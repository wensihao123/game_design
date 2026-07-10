# Godot 游戏循环、输入与物理系统常见陷阱

> 核心目标：避免帧率相关 Bug、输入穿透、物理抖动和暂停失效。

---

## 1. `_process()` 与 `_physics_process()` 的职责

### `_process(delta)`

按渲染帧调用，调用频率会随 FPS 变化。

适合：

- 纯视觉插值；
- UI 动画；
- 镜头平滑；
- 不参与物理碰撞的展示逻辑；
- 渲染相关更新。

### `_physics_process(delta)`

按固定物理 Tick 调用。

适合：

- CharacterBody2D 移动；
- 碰撞查询；
- 重力；
- 跳跃；
- 物理状态更新；
- 需要固定时间步长的战斗判定。

不要在两个回调中同时修改同一个物理对象的位置，否则可能出现：

- 抖动；
- 穿透；
- 不稳定速度；
- 不同帧率下行为不同。

---

## 2. 正确使用 `delta`

错误：

```gdscript
func _process(_delta: float) -> void:
    position.x += speed
```

这意味着“每帧移动 speed 像素”，而不是“每秒移动 speed 像素”。

正确：

```gdscript
func _process(delta: float) -> void:
    position.x += speed * delta
```

### CharacterBody2D 的特殊点

```gdscript
func _physics_process(delta: float) -> void:
    velocity.x = input_axis * move_speed
    velocity.y += gravity * delta
    move_and_slide()
```

不要写：

```gdscript
velocity.x = input_axis * move_speed * delta
move_and_slide()
```

`velocity` 表示每秒速度，`move_and_slide()` 会按物理步长处理位移。

---

## 3. 加速度、速度、位移要区分

```text
加速度 × delta = 速度变化
速度 × delta = 位移
```

CharacterBody2D 常见写法：

```gdscript
velocity.y += gravity * delta
move_and_slide()
```

若自己直接控制 Node2D：

```gdscript
velocity += acceleration * delta
position += velocity * delta
```

错误地多乘或少乘一次 delta，会导致：

- 移动极慢；
- 帧率相关；
- 重力异常；
- 跳跃高度不稳定。

---

## 4. 不要把所有逻辑都放进每帧回调

错误：

```gdscript
func _process(_delta: float) -> void:
    search_target()
    recalculate_path()
    update_inventory()
    save_game()
```

优化方式：

| 逻辑 | 推荐机制 |
|---|---|
| 输入 | `_input()` / `_unhandled_input()` |
| AI 扫描 | Timer |
| 自动存档 | Timer 或事件触发 |
| 导航重算 | 条件触发或低频更新 |
| UI 更新 | 数据变化 Signal |
| Buff 到期 | Timer 或统一时间系统 |
| 物理移动 | `_physics_process()` |

### 低频 AI 示例

```gdscript
func _on_scan_timer_timeout() -> void:
    current_target = find_best_target()
```

不需要 60 FPS 扫描目标。

---

## 5. `_input()` 与 `_unhandled_input()`

### `_input(event)`

较早收到输入，适合：

- 全局快捷键；
- 调试控制；
- 截图；
- 控制台；
- 必须先于 UI 处理的输入。

### `_unhandled_input(event)`

UI 和其他节点没有消费事件时才收到，通常更适合游戏操作：

```gdscript
func _unhandled_input(event: InputEvent) -> void:
    if event.is_action_pressed("attack"):
        attack()
```

这样点击 UI 按钮时，不会同时触发角色攻击。

---

## 6. Input 单例与事件输入的区别

连续状态：

```gdscript
var axis := Input.get_axis("move_left", "move_right")
```

适合移动。

单次事件：

```gdscript
func _unhandled_input(event: InputEvent) -> void:
    if event.is_action_pressed("interact"):
        interact()
```

适合交互、菜单、确认。

不要只在 `_input()` 中根据事件做连续移动，因为键盘重复率和事件频率并不等于物理帧率。

---

## 7. UI 可能拦截鼠标输入

一个完全透明的 Control 仍可能接收鼠标事件。

常见症状：

- 按钮点击不了；
- 游戏场景收不到鼠标；
- 局部区域无法交互；
- 弹窗关闭后仍有透明遮罩挡住输入。

装饰性 UI 应考虑：

```gdscript
mouse_filter = Control.MOUSE_FILTER_IGNORE
```

交互面板根据需要使用：

- `STOP`：阻止继续传播；
- `PASS`：自身处理后允许向父 Control 传播；
- `IGNORE`：完全不处理鼠标。

注意：具体传播行为还受到 Control 层级和事件处理函数影响。

---

## 8. 输入 Action 不要硬编码按键

错误：

```gdscript
if Input.is_key_pressed(KEY_SPACE):
    jump()
```

推荐：

```gdscript
if Input.is_action_just_pressed("jump"):
    jump()
```

优点：

- 支持键盘、手柄和触屏；
- 可重绑定；
- 支持多键映射；
- 代码与平台按键分离；
- 更方便无障碍设置。

---

## 9. `is_action_pressed` 与 `is_action_just_pressed`

```gdscript
Input.is_action_pressed("attack")
```

按住期间每帧都为 true。

```gdscript
Input.is_action_just_pressed("attack")
```

按下瞬间为 true。

如果普通技能错误使用 `pressed`，可能每个物理帧释放一次。

建议：

```gdscript
if Input.is_action_just_pressed("attack") and can_attack:
    attack()
```

持续射击则应明确设计射速计时器。

---

## 10. 物理层与 Mask 配置错误

Godot 物理层通常分为：

- Collision Layer：我属于哪些层；
- Collision Mask：我要检测哪些层。

常见错误：

- 玩家和敌人都在同层，但 Mask 没有互相检测；
- HitBox 检测到自己的 HurtBox；
- 地面和角色层混在一起；
- Area2D 同时用于拾取、攻击和范围检测；
- 使用默认第一层导致所有对象互相检测。

建议建立文档：

```text
Layer 1: World
Layer 2: Player Body
Layer 3: Enemy Body
Layer 4: Player HurtBox
Layer 5: Enemy HurtBox
Layer 6: Player HitBox
Layer 7: Enemy HitBox
Layer 8: Interactable
Layer 9: Pickup
```

并给项目设置中的层命名。

---

## 11. HitBox 和 HurtBox 不要共用主体碰撞

角色主体碰撞负责：

- 地面；
- 墙体；
- 阻挡；
- 单位间碰撞。

攻击 HitBox 负责：

- 当前攻击范围；
- 伤害类型；
- 命中目标。

受击 HurtBox 负责：

- 可被攻击区域；
- 受击倍率；
- 无敌状态。

将三者分开，能避免攻击动画修改主体碰撞盒。

---

## 12. 物理查询的时机

刚刚移动、创建或改变碰撞体后，立刻查询物理世界，结果可能尚未反映最新状态。

常见处理方式：

- 在下一个物理帧查询；
- 使用 `await get_tree().physics_frame`；
- 在 `_physics_process()` 中进行；
- 不要在修改碰撞体后立即依赖同步结果。

示例：

```gdscript
await get_tree().physics_frame
perform_query()
```

不要滥用 await；应先确认问题确实来自物理同步时机。

---

## 13. 暂停游戏时的 Process Mode

```gdscript
get_tree().paused = true
```

节点是否继续运行取决于 Process Mode。

暂停菜单常见设置：

```text
Process Mode = When Paused
```

持续运行的全局服务可能使用：

```text
Process Mode = Always
```

游戏世界通常继承暂停状态。

### 暂停时仍需注意

Signal 调用不一定自动停止。即使节点暂停，其他系统仍可能直接调用它的方法。

因此战斗结算代码最好显式检查：

```gdscript
if get_tree().paused:
    return
```

是否需要检查取决于系统职责，不要全局到处复制。

---

## 14. Timer 也受暂停和时间缩放影响

Timer 是否继续运行取决于：

- Process Mode；
- `ignore_time_scale`；
- SceneTree 是否暂停；
- 节点是否仍在树中。

用于 UI 或真实时间逻辑的 Timer，应明确是否忽略游戏速度。

例如：

- 技能冷却通常跟随游戏时间；
- 暂停菜单动画可能不跟随游戏暂停；
- 网络超时可能需要真实时间；
- 受击慢动作期间，某些 UI 提示不应变慢。

---

## 15. 物理移动和视觉移动分离

角色可采用：

```text
CharacterBody2D
├── VisualRoot
├── CollisionShape2D
├── HurtBox
└── HitBox
```

弹跳、 squash and stretch、受击震动只修改 `VisualRoot`：

```gdscript
visual_root.position.y = visual_offset
visual_root.scale = squash_scale
```

不要让视觉动画持续修改 CharacterBody2D 的主体位置，否则可能影响：

- 地面检测；
- 碰撞；
- 导航；
- 攻击判定；
- 网络同步。

---

## 16. 调试清单

- [ ] 物理移动只在 `_physics_process()`；
- [ ] `velocity` 没有错误乘 delta；
- [ ] 重力作为加速度乘 delta；
- [ ] 游戏操作优先使用 Input Map；
- [ ] UI 不会意外拦截输入；
- [ ] 装饰性 Control 使用正确 Mouse Filter；
- [ ] Collision Layer 和 Mask 有统一命名；
- [ ] 主体碰撞、HitBox、HurtBox 分离；
- [ ] AI 和搜索逻辑不是每帧全量执行；
- [ ] 暂停菜单使用正确 Process Mode；
- [ ] Timer 是否受暂停和时间缩放影响已明确；
- [ ] 视觉动画不会破坏物理主体。
