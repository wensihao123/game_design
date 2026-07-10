# Godot 场景树、节点生命周期与对象管理

> 核心目标：正确理解节点何时存在、何时可访问、何时真正销毁，以及场景如何安全复用。

---

## 1. 节点生命周期不是简单的从上到下

常见阶段包括：

```text
_init()
_enter_tree()
_ready()
_process / physics / input
_exit_tree()
free
```

典型顺序：

```text
父节点 _enter_tree()
子节点 _enter_tree()
子节点 _ready()
父节点 _ready()
```

因此：

- `_enter_tree()` 通常是父到子；
- `_ready()` 通常是子到父；
- `_init()` 时场景节点结构可能尚未准备完成；
- `_ready()` 只保证当前节点及其子节点已准备，不代表整个游戏所有系统都完成初始化。

---

## 2. 不要在 `_init()` 中访问场景节点

错误：

```gdscript
func _init() -> void:
    $AnimationPlayer.play("idle")
```

`_init()` 可能发生在节点尚未进入场景树时，此时 `$AnimationPlayer` 不可靠。

推荐：

```gdscript
@onready var animation_player: AnimationPlayer = $AnimationPlayer

func _ready() -> void:
    animation_player.play("idle")
```

### `_init()` 更适合做什么

- 初始化纯数据；
- 设置无场景依赖的默认值；
- 构造 `RefCounted` 或 Resource；
- 接收构造参数。

不要在其中依赖：

- `get_tree()`；
- 父节点；
- 子节点；
- Viewport；
- 输入状态；
- 场景中的其他系统。

---

## 3. `@onready` 不是永久安全保证

```gdscript
@onready var health: HealthComponent = $HealthComponent
```

它只在 `_ready()` 前解析一次。

如果之后：

- 节点被替换；
- 子节点被删除；
- 场景结构动态变化；
- 引用目标被 `queue_free()`；

变量仍可能指向失效对象。

使用前可检查：

```gdscript
if is_instance_valid(health):
    health.take_damage(10)
```

更好的方式是避免在运行中随意替换核心组件，或者在替换时显式更新引用。

---

## 4. `_ready()` 默认只执行一次

节点移出场景树后重新加入，`_ready()` 默认不会自动再次调用。

```gdscript
remove_child(enemy)
add_child(enemy)
```

如果确实需要重新触发 ready 流程，可以调用：

```gdscript
enemy.request_ready()
```

但不要把 `request_ready()` 当成常规重置机制。对象池中的节点更适合实现明确的：

```gdscript
func activate(data: SpawnData) -> void:
    pass

func deactivate() -> void:
    pass

func reset_state() -> void:
    pass
```

---

## 5. 避免依赖固定父节点层级

危险写法：

```gdscript
get_parent().get_parent().get_node("HUD")
```

以及：

```gdscript
get_node("/root/Main/GameWorld/Player")
```

这种代码会导致：

- 子场景无法单独运行；
- 改层级就报错；
- 难以测试；
- 场景复用困难；
- 模块之间产生隐藏依赖。

### 更推荐的依赖方式

#### 方式一：Inspector 注入

```gdscript
@export var target: Node2D
```

#### 方式二：初始化函数

```gdscript
func setup(player_ref: Player) -> void:
    player = player_ref
```

#### 方式三：Signal 通知上层

```gdscript
signal died(unit: Node)

func die() -> void:
    died.emit(self)
```

#### 方式四：由父场景完成组装

```gdscript
func _ready() -> void:
    enemy.setup($Player)
    enemy.died.connect(_on_enemy_died)
```

---

## 6. 子场景应尽量自包含

一个可复用敌人场景不应假设外部一定存在：

```text
/root/Main/HUD
/root/Main/Player
/root/Main/AudioManager
```

更合理的敌人场景：

```text
Enemy
├── VisualRoot
├── HealthComponent
├── MovementComponent
├── AttackComponent
├── HurtBox
└── HitBox
```

它只负责自身逻辑，对外通过信号和公开方法通信。

---

## 7. `queue_free()`、`free()` 与 `remove_child()`

### `remove_child()`

```gdscript
remove_child(enemy)
```

只是把节点移出当前父节点，节点仍存在。

适合：

- 暂时移出场景；
- 放入对象池；
- 转移到其他父节点。

### `queue_free()`

```gdscript
enemy.queue_free()
```

在当前帧结束后安全删除节点及子节点。大多数游戏对象删除场景应优先使用它。

### `free()`

```gdscript
enemy.free()
```

立即销毁对象。使用不当可能导致当前调用链中的引用失效。

一般情况下优先使用 `queue_free()`。

---

## 8. `queue_free()` 后对象并非立刻消失

```gdscript
enemy.queue_free()
enemy.take_damage(10)
```

在当前帧中，第二行仍可能执行，但这种逻辑是不安全的。

推荐：

```gdscript
if (
    is_instance_valid(enemy)
    and not enemy.is_queued_for_deletion()
):
    enemy.take_damage(10)
```

同时，删除对象后应尽快清理集合：

```gdscript
enemies.erase(enemy)
```

不要长期保存失效节点引用。

---

## 9. Signal 与对象生命周期

常见风险：

- 发射方已经销毁；
- 接收方已经销毁；
- 重复连接；
- 对象池复用时旧连接仍存在；
- Lambda 捕获已失效对象。

连接前可检查：

```gdscript
var callback := _on_health_changed

if not health.health_changed.is_connected(callback):
    health.health_changed.connect(callback)
```

断开：

```gdscript
if health.health_changed.is_connected(callback):
    health.health_changed.disconnect(callback)
```

对象池节点在 `deactivate()` 时应处理临时连接。

---

## 10. `owner` 与 `parent` 完全不同

`parent` 表示运行时场景树关系。

`owner` 表示该节点归属于哪个可保存场景。

```gdscript
add_child(new_node)
new_node.owner = self
```

只有在需要把动态创建的节点打包保存进 `PackedScene` 时，通常才需要设置 owner。

### 常见使用场景

- 游戏内关卡编辑器；
- 用户生成地图；
- 自定义场景保存；
- 编辑器插件；
- 程序化生成后保存 `.tscn`。

普通运行时生成的：

- 子弹；
- 特效；
- 敌人；
- 掉落物；

通常不需要设置 owner。

---

## 11. `reparent()` 时注意 Transform

把节点换到其他父节点后，本地坐标系会改变。

```gdscript
node.reparent(new_parent)
```

应明确是否需要保持全局 Transform。

在 2D 中尤其要注意：

- `position` 是相对父节点；
- `global_position` 是世界坐标；
- 父节点缩放会影响子节点；
- CanvasLayer 会改变 UI 所在坐标空间。

移动节点前记录：

```gdscript
var old_global := node.global_transform
node.reparent(new_parent)
node.global_transform = old_global
```

具体是否需要手动恢复，取决于 API 参数和项目版本，但核心原则是：换父节点等于换坐标空间。

---

## 12. 场景切换期间不要保存脆弱引用

切换场景后，原场景中的节点都会被释放。Autoload 或长期对象中若保存：

```gdscript
var player: Player
```

在切换后可能变成失效引用。

更稳妥的做法：

- 场景加载完成后重新注入；
- 使用弱引用；
- 只保存玩家数据，不保存玩家节点；
- 在 `_exit_tree()` 清理注册表；
- 不要让全局单例持有大量场景节点。

---

## 13. 推荐的生成与初始化模式

```gdscript
const ENEMY_SCENE := preload("res://scenes/enemies/slime.tscn")

func spawn_enemy(data: EnemyData, spawn_position: Vector2) -> Enemy:
    var enemy := ENEMY_SCENE.instantiate() as Enemy
    enemy.setup(data)
    enemy.global_position = spawn_position
    enemy.died.connect(_on_enemy_died)
    $Enemies.add_child(enemy)
    return enemy
```

需要注意初始化顺序：

- `setup()` 是否要求节点已进入树；
- `global_position` 是否依赖父节点；
- 子节点是否已经 ready；
- Signal 应在何时连接。

可以将逻辑分为：

```gdscript
func configure(data: EnemyData) -> void:
    # 入树前可设置的数据

func activate() -> void:
    # 入树后启动 AI、动画和计时器
```

---

## 14. 检查清单

- [ ] `_init()` 中不访问场景节点；
- [ ] 子节点引用使用 `@onready` 或明确注入；
- [ ] 不依赖多层 `get_parent()`；
- [ ] 可复用场景能够单独运行；
- [ ] 删除节点优先使用 `queue_free()`；
- [ ] 集合中不会长期保存已销毁节点；
- [ ] 对象池拥有明确 reset 流程；
- [ ] Signal 不会重复连接；
- [ ] 场景切换后重新绑定节点依赖；
- [ ] 动态保存场景时正确理解 owner；
- [ ] Reparent 时检查坐标空间；
- [ ] 生命周期相关逻辑有清晰初始化顺序。
