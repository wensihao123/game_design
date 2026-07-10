# Godot 2D 横版游戏：UI、动画、碰撞与分辨率专项注意事项

> 适用于横版 2D、贴纸角色、自动战斗、桌面独立游戏等项目。

---

## 1. 先确定逻辑分辨率与显示策略

不能只说“游戏画布是 1000×300”，还需要定义：

- 1000×300 是逻辑分辨率还是资源设计尺寸；
- 超宽屏如何扩展；
- 窗口缩放时是否保持比例；
- UI 是否贴屏幕边缘；
- 战斗区域是否允许显示更多内容；
- 最小窗口尺寸；
- 全屏和窗口模式；
- 高 DPI 行为。

### 推荐方案

```text
核心战斗构图：1000×300
左右超出部分：扩展纯背景
关键角色与交互：限制在核心安全区域
UI：CanvasLayer + Container + Anchor
角色与 UI：不做非等比拉伸
```

避免把 1000×300 画面直接横向拉伸到 16:9 或 21:9。

---

## 2. 核心内容与扩展背景分离

建议背景拆为：

```text
BackgroundRoot
├── FarBackground
├── MidBackground
├── CoreBattleBackground
└── ForegroundDecoration
```

核心 1000×300 决定：

- 角色站位；
- 摄像机重点；
- 关卡交互；
- 战斗空间。

左右扩展区只承担：

- 天空；
- 山林；
- 云层；
- 无关键交互装饰；
- 可重复或镜像背景。

这样超宽屏不会提前暴露敌人、隐藏区域或关卡外内容。

---

## 3. UI 不要全部使用固定 Position

错误做法：

- 所有按钮手动输入坐标；
- 每个分辨率复制一套 UI；
- 窗口变化后通过脚本逐个修正；
- 依赖设计稿像素位置。

推荐使用：

- Anchor；
- Container；
- Margin；
- `custom_minimum_size`；
- Size Flags；
- Theme；
- CanvasLayer。

示例结构：

```text
CanvasLayer
└── SafeArea
    ├── TopBar
    ├── Content
    └── BottomDock
```

按钮组使用 HBoxContainer、VBoxContainer 或 GridContainer，而不是手动排列。

---

## 4. Theme 与组件状态

不要为每个按钮单独复制样式。

使用 Theme 管理：

- 默认字体；
- 字号；
- Button；
- Panel；
- Label；
- Tooltip；
- ProgressBar；
- Focus 样式；
- Disabled 状态。

需要区分：

- normal；
- hover；
- pressed；
- disabled；
- focus。

独立游戏经常只设计 normal 状态，导致键盘和手柄导航时没有焦点提示。

---

## 5. 高 DPI 与字体清晰度

桌面显示器可能有 125%、150%、200% 缩放。

注意：

- 字体不要烘焙在图片里；
- 使用动态字体；
- 测试小字号；
- 避免非整数缩放造成模糊；
- 图标提供足够分辨率；
- 九宫格面板边缘不要被拉糊；
- 检查 Windows 高 DPI 和 macOS Retina。

UI 文字应在不同语言下测试长度。

---

## 6. 贴纸角色白边不应参与物理判定

贴纸角色通常具有厚白边和透明边距。

如果碰撞盒按照整张 Sprite 尺寸设置，会导致：

- 视觉未接触却发生碰撞；
- 角色之间间距过大；
- 攻击判定不自然；
- 角色站位看起来悬空；
- 不同帧透明边界变化造成抖动。

建议：

```text
CharacterBody2D
├── VisualRoot
│   └── AnimatedSprite2D
├── BodyCollision
├── HurtBox
├── HitBoxRoot
└── GroundMarker
```

碰撞盒根据角色身体核心区域设置，而不是根据白边。

---

## 7. VisualRoot 与物理主体分离

视觉弹跳：

```gdscript
visual_root.position.y = bounce_offset
```

视觉缩放：

```gdscript
visual_root.scale = Vector2(1.1, 0.9)
```

不要直接改变 CharacterBody2D 的主体 Scale 和 Position 来做每帧动画。

原因：

- CollisionShape 跟随缩放；
- 地面检测变化；
- 攻击范围抖动；
- 导航和逻辑坐标污染；
- 角色实际位置与显示位置混在一起。

---

## 8. Sprite Sheet 每帧尺寸必须统一

你的角色动画若依赖逐帧贴图，应保证：

- 每帧画布尺寸一致；
- 角色脚底位置一致；
- 身体中心基准一致；
- 不因武器伸出改变帧格大小；
- 不裁切极端动作；
- 透明边界统一。

可定义：

```text
Frame Size: 256×256
Ground Y: 220
Character Pivot X: 128
Facing Direction: Right
```

每帧中角色脚底固定在 Ground Y，否则播放时会漂移。

---

## 9. 动画中的 Root Motion 要谨慎

如果角色移动由代码控制，动画帧中的角色整体不应自行向前移动，否则会出现：

- 视觉滑步；
- 逻辑位置不变但角色图移动；
- 攻击距离难计算；
- 动画循环回跳。

推荐：

- Sprite 帧围绕固定 Pivot；
- CharacterBody2D 控制世界位移；
- VisualRoot 只负责局部动作；
- 突进技能由代码或明确的 AnimationPlayer 轨道控制逻辑根节点。

---

## 10. 左右翻转与碰撞、武器挂点

使用：

```gdscript
visual_root.scale.x = facing_sign
```

可能同时翻转：

- Sprite；
- 武器挂点；
- HitBox；
- 特效发射点；
- 名字和血条。

UI 不应跟随角色翻转。

推荐结构：

```text
CharacterBody2D
├── FacingRoot
│   ├── VisualRoot
│   ├── WeaponSocket
│   └── HitBoxRoot
├── BodyCollision
├── HurtBox
└── WorldUI
```

只翻转 `FacingRoot`。

---

## 11. HitBox 的启用窗口

攻击 HitBox 不应永久开启。

可以通过 AnimationPlayer：

```text
攻击开始：HitBox.monitoring = true
攻击结束：HitBox.monitoring = false
```

或者在动画事件中调用方法：

```gdscript
func enable_attack_hitbox() -> void:
    hitbox.set_deferred("monitoring", true)

func disable_attack_hitbox() -> void:
    hitbox.set_deferred("monitoring", false)
```

修改物理监测状态时，使用 deferred 往往更安全。

---

## 12. 一次攻击只命中一次

Area2D 在同一攻击窗口中可能多次检测目标。

维护命中集合：

```gdscript
var hit_targets: Dictionary = {}

func begin_attack() -> void:
    hit_targets.clear()

func try_hit(target: HurtBox) -> void:
    if hit_targets.has(target):
        return

    hit_targets[target] = true
    target.receive_hit(current_attack)
```

攻击结束后清空。

对于持续伤害技能，则应明确命中间隔，而不是沿用一次命中逻辑。

---

## 13. 动画回调不要依赖脆弱字符串

AnimationPlayer Call Method 轨道常使用方法名字符串。重命名脚本方法后，动画轨道可能不会自动修复。

建议：

- 动画事件方法集中到单一 AnimationEventReceiver；
- 命名稳定；
- 重构后检查所有动画；
- 避免大量脚本方法直接暴露给动画；
- 关键战斗逻辑不完全依赖动画轨道。

例如：

```gdscript
func animation_event(event_name: StringName) -> void:
    match event_name:
        &"attack_active_start":
            attack_controller.open_hit_window()
        &"attack_active_end":
            attack_controller.close_hit_window()
```

---

## 14. 自动战斗角色的站位与视觉顺序

横版多人战斗容易出现：

- 角色重叠；
- Z 顺序错误；
- 名字和血条互相遮挡；
- 近战单位无法找到站位；
- 角色穿插后朝向频繁抖动。

可以根据 Y 或队列设置 Z Index：

```gdscript
z_index = int(global_position.y)
```

若游戏主要为固定水平站位，也可以使用明确队列：

```text
Frontline Z: 20
Backline Z: 10
Effects Z: 30
World UI Z: 50
```

不要让 Z 规则同时由多个系统控制。

---

## 15. 摄像机与 UI 不要混在同一坐标系

世界 UI：

- 伤害数字；
- 头顶血条；
- 目标标记。

屏幕 UI：

- 技能按钮；
- 背包；
- 设置；
- 战斗状态栏。

屏幕 UI 应放在 CanvasLayer。否则摄像机移动或缩放时，整个 HUD 也会跟着移动。

---

## 16. 音效与动画同步

不要只依赖每帧编号猜音效时机。

更可靠：

- AnimationPlayer 方法轨；
- 明确的攻击事件；
- 状态机进入事件；
- 命中时播放命中音效；
- 挥击时播放挥击音效。

区分：

- 动作音效：出手时；
- 命中音效：真正造成伤害时；
- UI 音效：Control 事件；
- 环境音：场景控制。

---

## 17. 2D 项目验收清单

- [ ] 逻辑分辨率和扩屏策略已明确；
- [ ] 核心战斗区不会因超宽屏变化；
- [ ] UI 使用 Anchor 和 Container；
- [ ] UI 支持键盘、手柄 Focus；
- [ ] 高 DPI 下字体和图标清晰；
- [ ] 白边和透明区域不参与主体碰撞；
- [ ] VisualRoot 与 CharacterBody2D 分离；
- [ ] Sprite Sheet 每帧尺寸和脚底基准一致；
- [ ] 只翻转 FacingRoot，不翻转 UI；
- [ ] HitBox 只在有效帧开启；
- [ ] 一次攻击有命中去重；
- [ ] 动画事件方法重构后经过检查；
- [ ] 世界 UI 与屏幕 UI 分离；
- [ ] Z 顺序有统一规则；
- [ ] 导出包在不同分辨率和 DPI 下测试。
