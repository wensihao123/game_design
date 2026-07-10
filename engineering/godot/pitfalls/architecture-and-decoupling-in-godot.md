# Godot 架构、组件化与解耦设计

> 核心目标：避免 Manager 泛滥、场景彼此硬引用、Signal 失控和过深继承。

---

## 1. Godot 更适合组合，而不是深层继承

不推荐：

```text
Node
└── Unit
    └── LivingUnit
        └── CombatUnit
            └── HumanoidUnit
                └── Hero
                    └── Warrior
```

继承层级越深，越容易出现：

- 父类职责过多；
- 子类被迫继承无关行为；
- 修改父类影响所有角色；
- 特殊角色需要大量 override；
- 难以复用某个单独能力。

更推荐：

```text
Warrior
├── HealthComponent
├── MovementComponent
├── MeleeAttackComponent
├── StatusComponent
├── TargetDetector
└── AnimationController
```

角色通过“拥有能力”组成，而不是通过“属于某个越来越具体的父类”获得所有功能。

---

## 2. 一个组件只负责一个领域

### HealthComponent

负责：

- 当前生命；
- 最大生命；
- 扣血；
- 治疗；
- 死亡事件。

不负责：

- 播放受击动画；
- 显示伤害数字；
- 掉落物品；
- 切换游戏结束界面；
- 保存游戏。

```gdscript
class_name HealthComponent
extends Node

signal changed(current: int, maximum: int)
signal died

@export var max_health: int = 100
var current_health: int

func _ready() -> void:
    current_health = max_health

func take_damage(amount: int) -> void:
    if amount <= 0 or current_health <= 0:
        return

    current_health = maxi(current_health - amount, 0)
    changed.emit(current_health, max_health)

    if current_health == 0:
        died.emit()
```

其他系统通过 Signal 响应死亡。

---

## 3. Scene、Node、Resource、RefCounted 的职责

### Scene / Node

适合：

- 场景树生命周期；
- 显示；
- 输入；
- 碰撞；
- 物理；
- Timer；
- AnimationPlayer；
- 与其他节点交互。

### Resource

适合：

- 角色配置；
- 技能定义；
- 装备数据；
- 状态效果配置；
- AI 策略；
- 掉落表；
- 曲线和数值表。

### RefCounted

适合：

- 不需要进入场景树的纯逻辑；
- 伤害结果；
- 战斗上下文；
- 临时计算对象；
- 存档模型；
- 数据转换器。

### Autoload

适合：

- 场景切换；
- 全局存档入口；
- 音频总线控制；
- 平台服务；
- 跨场景玩家档案；
- 极少量真正全局的服务。

---

## 4. 不要把所有系统都变成 Manager

常见失控结构：

```text
GameManager
CombatManager
PlayerManager
EnemyManager
UIManager
SkillManager
QuestManager
InventoryManager
ItemManager
EffectManager
```

最终任何脚本都可以访问任何 Manager，形成全局耦合。

### 判断是否需要 Autoload

问四个问题：

1. 它是否必须跨越所有场景存在？
2. 它是否拥有唯一且明确的生命周期？
3. 它是否是基础设施，而不是某个局部玩法？
4. 把它放进当前场景是否更容易测试和替换？

例如战斗系统通常属于 BattleScene，而不是全局 Autoload。

---

## 5. 依赖应由上层注入

错误：

```gdscript
func _ready() -> void:
    player = get_tree().get_first_node_in_group("player")
    hud = get_node("/root/Main/HUD")
```

推荐：

```gdscript
class_name Enemy
extends CharacterBody2D

var player: Player

func setup(player_ref: Player) -> void:
    player = player_ref
```

由战斗场景组装：

```gdscript
func spawn_enemy() -> void:
    var enemy := enemy_scene.instantiate() as Enemy
    enemy.setup(player)
    enemies_root.add_child(enemy)
```

高层场景知道整体结构，底层组件不需要自己搜索全局。

---

## 6. Signal 用于事件，不用于所有调用

适合 Signal：

```gdscript
signal died
signal health_changed
signal item_acquired
signal battle_finished
```

这些都表示“某件事已经发生”。

不适合 Signal：

```gdscript
signal request_take_damage(target, amount)
```

明确的同步命令通常直接调用：

```gdscript
target_health.take_damage(amount)
```

### 简单规则

- 命令：直接方法调用；
- 事件：Signal；
- 全局广播：谨慎使用事件总线；
- 需要返回值：优先直接调用；
- 多个未知订阅者：Signal；
- 紧密的父子组件：直接调用即可。

---

## 7. 谨慎使用全局事件总线

事件总线看起来很方便：

```gdscript
Events.enemy_died.emit(enemy)
```

但过度使用后会导致：

- 不知道谁在监听；
- 调用链难追踪；
- 同名事件语义冲突；
- 场景切换后旧连接残留；
- 测试困难；
- 调试只能靠日志。

建议：

- 局部事件直接连接局部节点；
- 只有真正跨场景事件使用全局总线；
- 事件名包含清晰领域；
- 明确参数类型；
- 不要把所有 Signal 都放进单一 `Events.gd`。

---

## 8. Resource 策略替代大量 `match`

不推荐：

```gdscript
func execute_effect(type: StringName, target: Node) -> void:
    match type:
        &"damage":
            pass
        &"heal":
            pass
        &"poison":
            pass
        &"stun":
            pass
```

推荐：

```gdscript
@abstract
class_name SkillEffect
extends Resource

@abstract
func apply(context: SkillContext) -> void
```

不同效果独立实现：

```gdscript
class_name DamageEffect
extends SkillEffect

@export var amount: int = 10

func apply(context: SkillContext) -> void:
    context.target_health.take_damage(amount)
```

新增技能效果时，只增加新类，而不是不断修改核心执行器。

---

## 9. 使用类型标注降低运行时错误

```gdscript
@export var health: HealthComponent
var current_target: Node2D

func calculate_damage(base_damage: int, multiplier: float) -> int:
    return roundi(base_damage * multiplier)
```

类型标注可以：

- 提供编辑器补全；
- 提前发现错误；
- 明确接口；
- 降低错误对象传入；
- 方便重构。

不要为了“灵活”把所有变量都写成 Variant。

---

## 10. 避免字符串驱动核心逻辑

脆弱：

```gdscript
if state == "Attak":
    pass
```

字符串拼错不会提前报错。

可使用：

```gdscript
enum State {
    IDLE,
    CHASE,
    ATTACK,
    DEAD,
}
```

```gdscript
var current_state: State = State.IDLE
```

对于资源 ID，可以使用 `StringName`：

```gdscript
@export var skill_id: StringName
```

并集中管理命名。

---

## 11. 不要过度抽象

SOLID 和组件化不等于每两行代码拆一个类。

不值得拆分的情况：

- 逻辑永远不会单独变化；
- 只有一个调用者；
- 拆分后只增加跳转成本；
- 接口比实现复杂；
- 项目规模很小；
- 尚未出现真实变化点。

推荐策略：

1. 先写清楚；
2. 出现第二个变化版本；
3. 找到共同点；
4. 再抽象。

---

## 12. 推荐的战斗场景结构

```text
BattleScene
├── BattleController
├── SpawnController
├── TargetingSystem
├── Heroes
├── Enemies
├── Effects
├── WorldUI
└── BattleHUD
```

角色：

```text
Hero
├── VisualRoot
├── HealthComponent
├── MovementComponent
├── AttackController
├── SkillController
├── StatusComponent
├── HurtBox
└── HitBox
```

数据：

```text
HeroData
SkillData
SkillEffect
ItemData
StatusEffectData
EnemyData
DropTable
```

---

## 13. 检查清单

- [ ] 场景以组合为主，继承层级较浅；
- [ ] 组件职责明确；
- [ ] 运行时状态与静态 Resource 分离；
- [ ] 局部系统没有无理由做成 Autoload；
- [ ] 依赖由上层场景注入；
- [ ] 命令使用直接方法调用；
- [ ] 事件使用 Signal；
- [ ] 全局事件总线使用范围受控；
- [ ] 没有大量具体类型判断；
- [ ] 核心逻辑不依赖魔法字符串；
- [ ] 公开 API 有类型标注；
- [ ] 没有为了模式而过度拆分。
