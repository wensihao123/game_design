# Godot 性能、资源加载与多线程常见陷阱

> 核心目标：先测量，再优化；避免每帧搜索、同步加载、频繁分配和不安全线程。

---

## 1. 不要先认定 GDScript 是性能瓶颈

常见卡顿来源：

- 数百个节点都运行 `_process()`；
- 每帧遍历场景树；
- 每帧分配大量 Array、Dictionary 和临时对象；
- 高频 instantiate / queue_free；
- AI 每帧重新寻路；
- 同步加载大资源；
- 大量透明 UI 和粒子造成 Overdraw；
- 碰撞层配置导致无意义检测；
- UI 每帧重新布局；
- Shader 或灯光过重。

正确流程：

1. 复现问题；
2. 打开 Profiler；
3. 查看 CPU、Physics、Rendering、Memory；
4. 找到真正热点；
5. 修改单一变量；
6. 再次测量；
7. 确认优化有效。

---

## 2. 避免每帧搜索场景树

不推荐：

```gdscript
func _process(_delta: float) -> void:
    var player := get_tree().get_first_node_in_group("player")
    var health := find_child("HealthComponent", true, false)
```

推荐缓存：

```gdscript
@onready var health: HealthComponent = %HealthComponent

var player: Player

func setup(player_ref: Player) -> void:
    player = player_ref
```

场景树搜索适合：

- 初始化；
- 调试工具；
- 极低频行为；
- 无法直接注入依赖的边缘场景。

不适合核心每帧循环。

---

## 3. 减少无意义的 `_process()`

如果一个节点不需要每帧更新，可以关闭：

```gdscript
set_process(false)
set_physics_process(false)
```

需要时再开启：

```gdscript
set_process(true)
```

例如：

- 休眠中的敌人；
- 屏幕外对象；
- 未展开 UI；
- 已死亡角色；
- 未激活场景模块。

大量小节点的空 `_process()` 也会产生累计成本。

---

## 4. 使用事件更新 UI，而不是每帧轮询

错误：

```gdscript
func _process(_delta: float) -> void:
    health_bar.value = player.health.current_health
```

推荐：

```gdscript
func _ready() -> void:
    health.changed.connect(_on_health_changed)

func _on_health_changed(current: int, maximum: int) -> void:
    health_bar.max_value = maximum
    health_bar.value = current
```

只有数据变化时更新。

---

## 5. 控制资源加载时机

### `preload()`

```gdscript
const HIT_EFFECT := preload("res://effects/hit_effect.tscn")
```

适合：

- 小型固定资源；
- 高频使用；
- 脚本启动时必定需要；
- 不希望运行中卡顿。

### `load()`

```gdscript
var scene := load(path)
```

适合：

- 动态路径；
- 不一定使用；
- 内容量适中；
- 可接受该时机的同步加载。

不要在攻击、碰撞等高频函数中同步加载大型资源。

### 后台加载

大型关卡、高清图片、长音频或大量资源包应考虑线程化加载流程，并提供加载界面。

---

## 6. `preload()` 也可能过度使用

如果主脚本预加载：

- 所有地图；
- 所有角色；
- 所有音效；
- 所有技能特效；
- 所有剧情图片；

可能导致：

- 启动时间增长；
- 内存占用过高；
- 不必要资源长期驻留；
- 首屏加载变慢。

建议按内容组划分：

- 当前战斗使用的资源；
- 当前地图使用的资源；
- 全局小型公共资源；
- 未来场景按需加载。

---

## 7. 高频创建和销毁对象

子弹、伤害数字、粒子、掉落物高频执行：

```gdscript
scene.instantiate()
node.queue_free()
```

可能造成：

- 内存分配压力；
- 瞬时卡顿；
- 场景树频繁变化；
- GC 或引用释放压力。

对象池适用于：

- 创建频率高；
- 生命周期短；
- 结构固定；
- Reset 成本低。

但不要默认所有对象都池化。对象池会带来：

- 状态重置遗漏；
- Signal 重复连接；
- Timer 没有停止；
- 动画残留；
- 引用未清空；
- 调试复杂。

---

## 8. 对象池必须完整 Reset

```gdscript
func activate(data: ProjectileData) -> void:
    visible = true
    set_physics_process(true)
    velocity = data.velocity
    damage = data.damage
    lifetime_timer.start(data.lifetime)

func deactivate() -> void:
    visible = false
    set_physics_process(false)
    lifetime_timer.stop()
    velocity = Vector2.ZERO
    damage = 0
    hit_targets.clear()
```

还需要处理：

- CollisionShape 是否 disabled；
- Area2D Monitoring；
- Shader 参数；
- Tween；
- AnimationPlayer；
- 临时 Signal；
- 父节点；
- Z Index；
- Modulate；
- Scale 和 Rotation。

---

## 9. Array 与 Dictionary 的临时分配

每帧写：

```gdscript
var nearby := []
for enemy in enemies:
    if condition:
        nearby.append(enemy)
```

在大量对象中可能造成频繁分配。

可考虑：

- 复用数组；
- 降低执行频率；
- 分帧处理；
- 维护空间索引；
- 只在对象变化时更新集合；
- 使用 Area2D 维护进入/离开列表。

先测量再优化，普通规模项目不必过早复杂化。

---

## 10. 大量实体不要都使用完整节点树

数千个纯展示对象使用完整场景可能很重。

可考虑：

- MultiMesh；
- 批量 Sprite 绘制；
- TileMap；
- 自定义 `_draw()`；
- GPU 粒子；
- 降低远处对象的逻辑频率；
- LOD 或休眠系统；
- 只让活跃对象拥有完整 AI。

你的游戏若只同时显示少量角色，不需要为“可能的万人战斗”提前设计复杂 ECS。

---

## 11. 多线程不能直接随意操作场景树

推荐线程做：

- 程序化地图数据；
- 路径计算数据；
- 大量纯数学运算；
- 文件解析；
- 数据压缩；
- 不依赖场景树的 AI 计算。

不要默认在子线程中安全地：

- `add_child()`；
- `queue_free()`；
- 修改活动节点；
- 操作 RenderingServer 之外的非线程安全 API；
- 读写共享 Array 而不加锁。

推荐流程：

```text
Worker Thread
    ↓
生成纯数据结果
    ↓
线程安全队列 / 受保护共享状态
    ↓
Main Thread
    ↓
更新节点和场景树
```

---

## 12. Thread 必须正确结束

错误管理线程可能导致：

- 退出项目卡住；
- 场景切换后线程继续访问旧数据；
- 死锁；
- 资源泄漏；
- 随机崩溃。

应设计：

- 停止标志；
- Mutex；
- Semaphore；
- 任务队列；
- `wait_to_finish()`；
- 场景退出时取消任务；
- 结果包含版本号或请求 ID，避免旧结果覆盖新状态。

---

## 13. 不要每个任务创建一个线程

频繁创建销毁线程有成本。

更适合：

- 固定工作线程；
- WorkerThreadPool；
- 任务队列；
- 批处理；
- 异步加载 API。

短小计算放到线程里，可能比主线程直接算更慢。

---

## 14. 分帧处理比多线程更简单

例如一次生成 10,000 个对象，可以分批：

```gdscript
for i in range(total):
    create_item(i)

    if i % 100 == 0:
        await get_tree().process_frame
```

优点：

- 不涉及线程安全；
- 实现简单；
- 可以显示进度；
- 主线程仍保持响应。

缺点是总耗时不一定减少，只是避免单帧卡死。

---

## 15. 渲染与 Overdraw

2D 中也可能出现严重 Overdraw：

- 大量半透明贴图叠加；
- 全屏透明 Control；
- 粒子层数过多；
- 模糊和全屏 Shader；
- 不必要的巨大 CanvasItem；
- 屏幕外对象仍绘制。

优化方向：

- 缩小贴图透明边界；
- 屏幕外隐藏或停用；
- 合并静态背景；
- 减少多层全屏半透明面板；
- 控制粒子数量；
- 检查 Shader 复杂度。

---

## 16. 性能验收原则

不要只在开发电脑上测试。

至少测试：

- 目标最低配置；
- 集成显卡；
- 高 DPI；
- 不同窗口尺寸；
- Debug 与 Release；
- 编辑器内与导出包；
- 场景首次进入和第二次进入；
- 长时间运行后的内存；
- 大量敌人或特效的极限场景。

---

## 17. 检查清单

- [ ] 已使用 Profiler 找到真实热点；
- [ ] 没有每帧搜索场景树；
- [ ] 非活跃节点关闭 Process；
- [ ] UI 通过事件更新；
- [ ] 大资源不在高频逻辑中同步加载；
- [ ] 没有一次性预加载全部游戏内容；
- [ ] 高频临时对象评估过对象池；
- [ ] 对象池拥有完整 reset；
- [ ] 大量实体使用适合的渲染方式；
- [ ] 工作线程不直接操作场景树；
- [ ] 线程能在退出时正确结束；
- [ ] 先考虑分帧，再考虑复杂多线程；
- [ ] 已在真实导出包和低配置设备上测试。
