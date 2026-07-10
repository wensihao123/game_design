# Godot Workflow（Godot 项目工作流）

> 状态：草案  
> 适用范围：使用 Godot Engine 开发的通用 2D、3D、工具型和混合型项目  
> 目的：定义 Godot 项目从环境准备、项目结构、场景与资源管理、脚本开发、调试、测试、导出、升级到发布维护的统一工作方式。

---

## 1. 文档目的

Godot Workflow 用于规范 Godot 项目的日常开发和交付过程，确保项目能够：

- 保持目录结构清晰；
- 控制场景和资源之间的依赖；
- 避免全局状态失控；
- 提高脚本、场景和资源的可复用性；
- 减少导入、导出和平台差异问题；
- 支持稳定的版本控制；
- 提高调试和测试效率；
- 降低引擎升级和插件升级风险；
- 保证正式构建与开发环境行为一致；
- 让项目能够被持续维护和扩展。

本工作流回答以下问题：

- Godot 版本应如何固定；
- 项目目录如何组织；
- Scene、Node、Resource 和 Script 各自承担什么职责；
- Autoload 应如何使用；
- 输入、信号、组和碰撞层如何管理；
- 资源如何导入与命名；
- 如何管理插件和第三方依赖；
- 如何进行 Debug、测试和性能分析；
- 如何验证 Export；
- 如何处理引擎升级；
- 如何进行版本发布和长期维护。

本工作流不规定某一种唯一架构。

它提供的是一组通用约束和决策原则，用于避免 Godot 项目中常见的结构性问题。

---

## 2. 核心原则

### 2.1 固定引擎版本

项目应明确使用的 Godot 版本。

建议记录：

```text
Engine Version
Release Channel
Commit / Build
Renderer
Target Platforms
Required Export Templates
```

例如：

```text
Godot 4.4.1 Stable
Renderer: Forward+
Export Templates: 4.4.1
```

不建议：

- 团队成员使用不同小版本；
- 开发中频繁切换 Stable 与 Dev；
- 未验证就升级主版本；
- Export Templates 与编辑器版本不一致。

---

### 2.2 Scene 表示结构，Resource 表示数据

推荐理解：

```text
Scene = 可实例化的结构和行为组合
Resource = 可复用的数据与配置
Script = 行为和规则
Node = 运行时对象
```

应避免：

- 把大量配置硬编码在 Script；
- 为纯数据创建复杂 Scene；
- 用 Node 充当全局数据库；
- 将所有逻辑都塞进单一根节点。

---

### 2.3 组合优先于深层继承

Godot 的 Scene 和 Node 天然适合组合。

优先使用：

- 子场景；
- 可复用组件；
- Resource 配置；
- 明确接口；
- 信号；
- 依赖注入。

谨慎使用：

- 深层 Script 继承；
- 多级 Scene 继承；
- 超级基类；
- 全局单例；
- 通过节点路径强耦合。

---

### 2.4 显式依赖优先于隐藏依赖

一个节点依赖其他对象时，应尽量通过：

- 导出变量；
- 初始化参数；
- 构造式工厂；
- 明确 NodePath；
- 资源引用；
- 接口；
- 父场景装配；

建立依赖。

避免：

- 任意位置调用 Autoload；
- 依赖固定树路径；
- 使用 `get_node("../../..")`；
- 通过节点名称猜测依赖；
- 在 `_ready()` 中隐式搜索整个场景树。

---

### 2.5 正式导出验证是完成标准的一部分

在 Godot 编辑器中运行成功，不代表正式构建可用。

必须验证：

- Debug Export；
- Release Export；
- 目标平台；
- 资源打包；
- 文件路径；
- 权限；
- 输入设备；
- 配置；
- 存档位置；
- 插件；
- Shader；
- 线程；
- 平台 API。

---

## 3. 项目初始化

## 3.1 创建项目时应确认

```markdown
- [ ] Godot 版本；
- [ ] Renderer；
- [ ] 项目分辨率；
- [ ] 拉伸模式；
- [ ] 目标平台；
- [ ] 输入方式；
- [ ] 默认语言；
- [ ] 版本控制；
- [ ] 忽略文件；
- [ ] 导出模板；
- [ ] 项目目录结构；
```

## 3.2 Renderer 选择

常见选择：

```text
Forward+
Mobile
Compatibility
```

选择应基于：

- 目标平台；
- 2D / 3D 比重；
- Shader 需求；
- 设备性能；
- Web 支持；
- 驱动兼容性。

Renderer 变更可能影响：

- Shader；
- 光照；
- 材质；
- 后处理；
- 性能；
- 平台兼容。

因此应尽早确定。

---

## 4. 推荐目录结构

示例：

```text
project/
├── project.godot
├── addons/
├── assets/
│   ├── art/
│   ├── audio/
│   ├── fonts/
│   ├── shaders/
│   └── video/
├── scenes/
│   ├── core/
│   ├── ui/
│   ├── gameplay/
│   ├── levels/
│   ├── actors/
│   └── tools/
├── scripts/
│   ├── core/
│   ├── systems/
│   ├── components/
│   ├── ui/
│   ├── gameplay/
│   └── utils/
├── resources/
│   ├── config/
│   ├── data/
│   ├── themes/
│   └── localization/
├── tests/
├── tools/
├── docs/
├── export/
└── archive/
```

实际结构可以调整，但应做到：

- 目录职责清楚；
- Runtime 资源与 Source 资源分离；
- Scene、Script、Resource 不混乱堆放；
- 第三方插件独立；
- 测试和工具独立；
- 临时文件不进入正式目录。

---

## 5. 文件与目录命名

推荐统一使用：

```text
snake_case
```

例如：

```text
player_controller.gd
inventory_panel.tscn
enemy_config.tres
```

类名使用：

```text
PascalCase
```

例如：

```gdscript
class_name PlayerController
```

变量和函数使用：

```text
snake_case
```

常量使用：

```text
UPPER_SNAKE_CASE
```

信号建议使用过去式或事件描述：

```gdscript
signal health_changed
signal item_added
signal battle_started
```

避免：

- 空格；
- 中文文件名；
- 大小写混用；
- `new2`；
- `final`；
- `temp`；
- 含糊缩写。

---

## 6. Scene 设计

## 6.1 Scene 的职责

一个 Scene 应表示：

- 一个可独立实例化的对象；
- 一个清楚的 UI 区块；
- 一个可复用组件；
- 一个关卡；
- 一个完整系统入口；
- 一个有独立生命周期的对象。

## 6.2 Scene 应具备

- 清楚的 Root Node；
- 明确输入；
- 明确输出；
- 明确信号；
- 明确依赖；
- 明确销毁方式；
- 可独立预览或测试。

## 6.3 Scene 不应过大

过大 Scene 的信号：

- 节点数极多；
- 一个 Script 管理大量无关行为；
- 修改一个区域影响全场景；
- 无法单独测试；
- 复用困难；
- Merge 冲突频繁。

应考虑拆成：

- 子场景；
- 组件；
- Resource；
- 独立系统；
- 可复用 UI。

---

## 7. Node 选择

应根据职责选择最小合适 Node。

例如：

- `Node`：纯逻辑；
- `Node2D`：2D 空间对象；
- `Control`：UI；
- `Resource`：数据；
- `CharacterBody2D`：角色移动；
- `Area2D`：检测区域；
- `AnimatableBody2D`：可动画移动碰撞体；
- `StaticBody2D`：静态碰撞。

避免因为方便而使用过重节点。

---

## 8. Scene 继承

Scene 继承适合：

- 基础角色模板；
- 共享 UI 框架；
- 少量稳定结构；
- 变体明确的对象。

Scene 继承风险：

- 父场景变化影响大量子场景；
- Override 难追踪；
- 继承层级过深；
- Inspector 状态难理解；
- Merge 冲突复杂。

推荐：

```text
继承层级不宜过深
```

对于复杂变体，优先考虑：

- 子场景组合；
- Resource 配置；
- 组件节点；
- 工厂实例化。

---

## 9. PackedScene 使用

动态创建对象时，应优先使用 `PackedScene`。

示例：

```gdscript
@export var projectile_scene: PackedScene

func spawn_projectile() -> void:
    var projectile := projectile_scene.instantiate()
    add_child(projectile)
```

应避免：

- 在代码中硬编码路径；
- 每次运行时重复加载；
- 实例化后依赖未设置；
- 不检查返回对象类型。

---

## 10. Resource 使用

Resource 适合表示：

- 角色配置；
- 技能配置；
- 道具数据；
- 平衡参数；
- UI Theme；
- 音频配置；
- 关卡配置；
- 状态定义；
- 数据表；
- 行为参数。

## 10.1 自定义 Resource

示例：

```gdscript
class_name ItemData
extends Resource

@export var id: StringName
@export var display_name: String
@export var icon: Texture2D
@export var max_stack: int = 1
```

## 10.2 Resource 风险

Resource 可能被多个实例共享。

运行时修改前应判断：

- 是否应共享；
- 是否需要 `duplicate()`；
- 是否会修改原始资源；
- 是否会污染编辑器资产；
- 是否会影响其他实例。

---

## 11. Script 组织

一个 Script 应有单一清楚职责。

推荐结构：

```gdscript
class_name Example
extends Node

signal state_changed

const MAX_COUNT := 10

@export var config: Resource

var _state: int

func _ready() -> void:
    pass

func public_method() -> void:
    pass

func _private_method() -> void:
    pass
```

## 11.1 脚本职责过大的信号

- 文件极长；
- 处理输入、UI、数据、存档、音频和动画；
- 大量布尔变量；
- 大量相互依赖状态；
- 很难测试；
- 任意修改都影响多个系统。

应考虑拆分：

- Controller；
- Model；
- Component；
- State；
- Service；
- Resource；
- Adapter。

---

## 12. `class_name` 使用

适合：

- 稳定公共类型；
- 可复用组件；
- 公共 Resource；
- 工具类；
- 清楚的系统接口。

谨慎用于：

- 临时脚本；
- 仅某场景使用的内部脚本；
- 大量小型一次性类。

`class_name` 过多会造成全局类型命名空间混乱。

---

## 13. Autoload 使用

Autoload 适合：

- 应用级生命周期；
- 全局路由；
- 持久化服务入口；
- 音频服务；
- 配置服务；
- 平台服务；
- 日志服务。

不适合：

- 任意共享变量；
- 所有系统状态；
- 具体场景对象；
- 大量业务逻辑；
- 可被普通依赖注入解决的服务。

## 13.1 Autoload 风险

- 隐藏依赖；
- 初始化顺序问题；
- 测试困难；
- 状态污染；
- 场景切换后残留；
- 系统耦合。

## 13.2 使用原则

```markdown
- [ ] 职责全局且稳定；
- [ ] 生命周期确实贯穿应用；
- [ ] 接口清楚；
- [ ] 状态可重置；
- [ ] 测试时可替换或清理；
- [ ] 不直接依赖具体场景节点；
```

---

## 14. Signals

Signals 用于解耦事件发送者和接收者。

适合：

- 状态变化；
- UI 通知；
- 生命周期事件；
- 输入结果；
- 数据更新；
- 系统完成事件。

## 14.1 Signal 原则

- 名称表达事件；
- 参数数量适中；
- 参数类型清楚；
- 避免使用信号传递命令；
- 避免全局事件总线滥用；
- 连接和断开生命周期明确。

## 14.2 连接方式

可以使用：

- 编辑器连接；
- 代码连接；
- 中央装配节点连接。

选择应保持一致和可追踪。

---

## 15. Groups

Groups 适合：

- 逻辑分类；
- 广播式查询；
- 调试；
- 场景中同类对象处理。

不应将 Groups 当作：

- 完整类型系统；
- 隐式依赖注入；
- 全局事件系统；
- 复杂权限系统。

使用前应统一命名。

例如：

```text
damageable
interactable
saveable
pause_aware
```

---

## 16. NodePath 与节点引用

推荐：

```gdscript
@onready var label: Label = %Label
```

或通过导出引用：

```gdscript
@export var target: Node2D
```

谨慎使用：

```gdscript
get_node("../../../../Target")
```

风险：

- 场景结构稍变即失效；
- 依赖隐藏；
- 难以复用；
- 难以测试。

---

## 17. Unique Node Name

`%NodeName` 适合场景内部稳定引用。

应注意：

- 名称必须唯一；
- 不要在巨大场景中滥用；
- 不要跨场景依赖；
- 重命名要同步更新引用。

---

## 18. 输入系统

输入动作应统一定义在：

```text
Project Settings → Input Map
```

推荐动作命名：

```text
move_left
move_right
jump
confirm
cancel
pause
ui_primary
ui_secondary
```

避免：

- 直接检查物理按键；
- 不同脚本使用不同命名；
- 将平台键位写死；
- 业务逻辑直接绑定输入设备。

## 18.1 输入分层

推荐：

```text
Device Input
→ Input Mapping
→ Command / Intent
→ Gameplay Logic
```

这样便于：

- 键位重映射；
- 手柄支持；
- 触屏支持；
- AI 控制；
- 回放；
- 自动测试。

---

## 19. UI Workflow

Godot UI 应基于 `Control` 系统。

应优先使用：

- Container；
- Anchor；
- Size Flag；
- Theme；
- Custom Minimum Size；
- Focus；
- Layout Direction。

避免：

- 大量手工像素定位；
- 依赖单一分辨率；
- UI 使用 Node2D 代替 Control；
- 每个组件单独设置重复样式。

## 19.1 UI 状态

每个 UI 组件应考虑：

```text
Default
Hover
Pressed
Focused
Disabled
Loading
Empty
Error
Success
```

## 19.2 Theme

建议使用 Theme 管理：

- 字体；
- 颜色；
- 边距；
- StyleBox；
- 图标；
- 控件状态。

---

## 20. 2D Workflow

应统一：

- 项目基准分辨率；
- Stretch Mode；
- Stretch Aspect；
- 像素密度；
- Texture Filter；
- Camera；
- Z Index；
- Y Sort；
- Pivot；
- Sprite 原点；
- 导入压缩。

像素项目还需固定：

- Integer Scaling；
- Nearest Filter；
- Sub-pixel 行为；
- Camera 移动规则。

---

## 21. 3D Workflow

应统一：

- 单位；
- 坐标；
- Up Axis；
- 模型缩放；
- 材质；
- LOD；
- 碰撞；
- Lightmap；
- Skeleton；
- Animation；
- 导入预设。

不应在每个模型中使用不同缩放逻辑。

---

## 22. Physics Workflow

物理相关应明确：

- Physics Tick；
- 移动放在哪个回调；
- Collision Layer；
- Collision Mask；
- Area 与 Body 职责；
- 速度单位；
- 重力；
- 单位比例；
- 暂停行为。

## 22.1 `_process` 与 `_physics_process`

推荐：

- 视觉和非物理逻辑使用 `_process`；
- 物理移动和碰撞使用 `_physics_process`；
- 不在不同帧率回调中混用同一状态更新。

---

## 23. Collision Layer 管理

碰撞层必须文档化。

示例：

| Layer | Name |
|---:|---|
| 1 | World |
| 2 | Player |
| 3 | Enemy |
| 4 | Projectile |
| 5 | Trigger |
| 6 | Interactable |

避免：

- 凭记忆使用数字；
- 不同场景自行定义；
- Layer 与 Mask 混乱；
- 无用碰撞层长期保留。

---

## 24. State Management

复杂对象应使用明确状态模型。

可使用：

- Enum；
- State Pattern；
- State Machine；
- AnimationTree State Machine；
- Resource 驱动状态。

避免：

- 大量布尔变量组合；
- 隐式状态；
- 状态跳转分散在多个脚本；
- UI 状态与逻辑状态分离。

---

## 25. Animation Workflow

可使用：

- AnimationPlayer；
- AnimationTree；
- Tween；
- AnimatedSprite2D；
- Skeleton2D / Skeleton3D。

应明确：

- 谁控制状态；
- 谁控制播放；
- 是否允许中断；
- 事件点；
- 动画结束行为；
- Root Motion；
- 过渡；
- 暂停。

避免多个系统同时控制同一属性。

---

## 26. Audio Workflow

推荐集中管理：

- Bus；
- Volume；
- SFX；
- Music；
- UI；
- Voice；
- Ambience；
- Ducking；
- Save Setting。

应使用 Audio Bus，而不是单独修改每个播放器音量。

---

## 27. Save / Load Workflow

存档系统应明确：

- 数据模型；
- 版本；
- 路径；
- 序列化；
- 加密；
- 校验；
- 自动保存；
- 手动保存；
- 备份；
- 恢复；
- 迁移；
- 损坏处理。

## 27.1 路径

运行时可写数据应使用：

```text
user://
```

不应写入：

```text
res://
```

## 27.2 版本化

每份存档建议包含：

```text
save_version
app_version
created_at
updated_at
```

---

## 28. Localization Workflow

建议使用 Godot Translation 系统。

应统一：

- Key；
- CSV / PO；
- 默认语言；
- Fallback；
- 字体；
- 字符集；
- 复数；
- 变量插值；
- 运行时切换。

避免在 Scene 中硬编码大量用户可见文本。

---

## 29. Import Workflow

Godot 会生成导入元数据。

应注意：

- 原始资源进入版本控制；
- `.godot/imported` 通常不需要提交；
- `.import` 行为取决于版本；
- 导入设置应稳定；
- 批量资源使用统一预设；
- 修改原文件后验证重导入。

## 29.1 导入检查

```markdown
- [ ] 文件路径正确；
- [ ] 文件名正确；
- [ ] Filter 正确；
- [ ] Compression 正确；
- [ ] Mipmap 正确；
- [ ] Loop 正确；
- [ ] Slice 正确；
- [ ] Scale 正确；
- [ ] 正式构建可加载；
```

---

## 30. External Files

如果运行时需要外部文件，应明确：

- 是否打包；
- 是否保留原扩展名；
- 是否使用 `FileAccess`；
- 是否需要 Export Filter；
- 是否跨平台；
- 是否可写；
- 是否允许用户替换。

---

## 31. Plugin Workflow

插件分为：

- 编辑器插件；
- Runtime 插件；
- 平台插件；
- Native 扩展；
- Asset Library 插件。

## 31.1 使用前检查

```markdown
- [ ] 来源可信；
- [ ] License 明确；
- [ ] 支持当前 Godot 版本；
- [ ] 活跃维护；
- [ ] 依赖明确；
- [ ] 可移除；
- [ ] 不包含不必要功能；
- [ ] 正式导出可用；
```

## 31.2 插件管理

应记录：

```text
Name
Version
Source
License
Purpose
Owner
Godot Compatibility
Upgrade Notes
Removal Plan
```

---

## 32. GDExtension 与原生依赖

使用 GDExtension 时必须关注：

- Godot 版本兼容；
- 平台二进制；
- Debug / Release；
- CPU 架构；
- 导出配置；
- License；
- 崩溃风险；
- 调试符号；
- CI 构建。

---

## 33. Editor Tooling

适合使用：

- `@tool`；
- EditorPlugin；
- 自定义 Inspector；
- 导入工具；
- 内容验证工具；
- 批量命名工具；
- 数据编辑器。

`@tool` 脚本风险：

- 在编辑器中执行；
- 修改资源；
- 引发递归更新；
- 空引用；
- 保存时执行；
- 影响团队编辑器稳定性。

应谨慎隔离和测试。

---

## 34. Debug Workflow

## 34.1 常用工具

- Remote Scene Tree；
- Remote Inspector；
- Breakpoint；
- Debugger；
- Profiler；
- Monitors；
- Visual Profiler；
- Network Profiler；
- Print；
- Push Warning；
- Push Error；
- Debug Draw。

## 34.2 日志原则

日志应包含：

- 系统；
- 对象；
- 状态；
- 关键参数；
- 错误上下文。

避免：

- 每帧打印；
- 敏感信息；
- 无上下文文本；
- 正式版本大量 Debug 输出。

---

## 35. Error Handling

应区分：

- 可恢复错误；
- 不可恢复错误；
- 用户错误；
- 数据错误；
- 配置错误；
- 编程错误。

Godot 中可使用：

- `assert`；
- `push_error`；
- `push_warning`；
- 错误码；
- 结果对象；
- Fallback；
- 重试。

`assert` 不应作为正式用户错误处理。

---

## 36. Testing Workflow

可测试内容包括：

- 纯逻辑；
- Resource；
- 状态机；
- 数据转换；
- 存档迁移；
- 组件；
- 场景集成；
- 输入；
- 导出构建。

测试可以分为：

```text
Unit
Component
Scene
Integration
End-to-End
Export
```

## 36.1 测试原则

- 逻辑尽量与 Scene Tree 解耦；
- 减少测试对 Autoload 的依赖；
- 支持状态重置；
- 使用测试场景；
- 使用固定测试数据；
- 关键 Bug 补回归测试。

---

## 37. Performance Workflow

Godot 性能检查应先测量。

关注：

- Frame Time；
- Physics Time；
- Draw Calls；
- Node Count；
- Object Count；
- Memory；
- Texture Memory；
- Audio；
- Script Time；
- GC；
- Shader Compilation；
- Resource Loading。

## 37.1 常见风险

- 每帧搜索节点；
- 每帧分配；
- 大量信号广播；
- 过多 CanvasItem；
- 频繁实例化和销毁；
- 大纹理；
- 同步加载；
- 复杂 Shader；
- 不必要物理检测；
- 过多全局逻辑。

---

## 38. Threading

使用线程前应确认：

- 任务确实耗时；
- API 是否线程安全；
- Scene Tree 不能任意跨线程修改；
- 结果如何回主线程；
- 取消和销毁如何处理；
- 错误如何传递。

不要为了“优化”提前引入线程复杂度。

---

## 39. Async / Await

使用 `await` 时应检查：

- 对象是否可能被销毁；
- 场景是否可能切换；
- 信号是否一定触发；
- 是否可能重复调用；
- 是否需要取消；
- 状态是否仍然有效。

---

## 40. Scene Change Workflow

场景切换应明确：

- 谁发起；
- 是否有 Loading；
- 旧场景何时释放；
- 全局状态如何保留；
- 音频如何过渡；
- 存档如何处理；
- 失败如何恢复；
- 重复切换如何防止。

---

## 41. Pause Workflow

暂停应统一定义：

- 哪些节点暂停；
- 哪些 UI 仍运行；
- 音频如何处理；
- Tween 如何处理；
- 输入如何处理；
- 网络如何处理；
- Timer 如何处理。

使用 `process_mode` 时应统一规范。

---

## 42. Configuration Workflow

项目配置应区分：

- 开发；
- 测试；
- Staging；
- Production。

不要将：

- 密钥；
- 生产 URL；
- 私密配置；

硬编码进仓库。

---

## 43. Export Presets

每个目标平台应维护独立 Export Preset。

应确认：

- Architecture；
- Debug / Release；
- Icons；
- Permissions；
- Signing；
- Resources；
- Filters；
- Compression；
- Feature Tags；
- Environment；
- Version；
- Package Identifier。

---

## 44. Export Validation

每个正式版本至少验证：

```markdown
- [ ] Release Export 成功；
- [ ] 目标平台启动成功；
- [ ] 主流程可用；
- [ ] 输入可用；
- [ ] 存档可用；
- [ ] 音频可用；
- [ ] 资源完整；
- [ ] Shader 正常；
- [ ] 插件正常；
- [ ] 日志路径正常；
- [ ] 版本号正确；
```

---

## 45. Web Export

应额外检查：

- 浏览器兼容；
- SharedArrayBuffer；
- HTTPS；
- 文件大小；
- 初始加载；
- 缓存；
- 音频自动播放；
- 本地存储；
- 多线程支持；
- CORS；
- 移动浏览器。

---

## 46. Mobile Export

应额外检查：

- 权限；
- 屏幕旋转；
- 安全区域；
- 触屏；
- 后台恢复；
- 内存；
- 电量；
- 包体积；
- 签名；
- 商店配置；
- 不同设备比例。

---

## 47. Desktop Export

应额外检查：

- 安装路径；
- 用户数据路径；
- 多显示器；
- 高 DPI；
- 窗口模式；
- 手柄；
- 文件权限；
- 杀毒软件误报；
- Crash Log；
- 自动更新。

---

## 48. Console Export

应额外关注：

- 平台 SDK；
- 认证；
- TRC / TCR；
- 输入设备；
- 存档规范；
- 用户切换；
- Suspend / Resume；
- 网络；
- 成就；
- 平台服务；
- 性能预算。

---

## 49. Version Control

应提交：

- `.gd`；
- `.tscn`；
- `.tres`；
- `.res`；
- 原始资产；
- `project.godot`；
- Export Preset（不含敏感信息）；
- 插件源码；
- 文档。

通常不提交：

- `.godot/`；
- 临时缓存；
- 本地编辑器设置；
- 导出产物；
- 私密密钥；
- 平台私有证书。

---

## 50. `.gitignore`

应根据 Godot 版本和平台配置。

至少考虑：

```text
.godot/
export/
build/
*.tmp
*.log
.DS_Store
Thumbs.db
```

敏感文件应额外加入忽略列表。

---

## 51. Scene Merge 冲突

`.tscn` 是文本格式，但大型场景仍容易冲突。

降低冲突方式：

- 拆小 Scene；
- 不多人同时编辑同一 Scene；
- 资源独立；
- 避免无关节点重排；
- 提交前检查 Diff；
- 使用统一 Godot 版本；
- 减少自动格式变化。

---

## 52. Commit Workflow

每次提交应：

- 目标单一；
- 不混入无关资源；
- 不提交临时文件；
- 不提交损坏 Scene；
- 在提交前运行项目；
- 检查 Git Diff；
- 更新必要文档。

---

## 53. Branch Workflow

常见分支：

```text
feature/*
bugfix/*
hotfix/*
release/*
tool/*
upgrade/*
```

引擎升级应使用独立分支。

---

## 54. Engine Upgrade Workflow

```text
ASSESS
→ BACKUP
→ CREATE UPGRADE BRANCH
→ UPDATE ENGINE
→ OPEN PROJECT
→ FIX PARSE ERRORS
→ FIX API CHANGES
→ REIMPORT
→ TEST EDITOR
→ TEST RUNTIME
→ EXPORT ALL TARGETS
→ PERFORMANCE CHECK
→ MERGE
```

## 54.1 升级前

```markdown
- [ ] 阅读迁移说明；
- [ ] 检查插件兼容；
- [ ] 检查 GDExtension；
- [ ] 创建备份；
- [ ] 固定当前 Release；
- [ ] 建立升级分支；
- [ ] 记录基准性能；
```

## 54.2 升级后

```markdown
- [ ] 所有脚本可解析；
- [ ] 所有 Scene 可打开；
- [ ] 资源重新导入；
- [ ] 核心流程通过；
- [ ] 存档兼容；
- [ ] 插件通过；
- [ ] 所有平台导出通过；
- [ ] 性能无明显回归；
- [ ] 文档已更新；
```

---

## 55. Plugin Upgrade Workflow

插件升级应：

- 单独提交；
- 阅读 Changelog；
- 检查 Godot 兼容；
- 测试编辑器；
- 测试 Runtime；
- 测试导出；
- 保留回滚版本；
- 更新依赖记录。

---

## 56. Project Settings 管理

`project.godot` 的变更应谨慎评审。

高风险变更包括：

- Renderer；
- Physics；
- Input Map；
- Autoload；
- Layer Names；
- Display；
- Localization；
- Feature Tags；
- Boot Scene。

避免在一次提交中混入大量无关设置变化。

---

## 57. Main Scene

主场景应承担：

- 应用启动；
- 全局装配；
- 入口流程；
- 服务初始化；
- 路由；
- 崩溃保护；
- 初始加载。

不应承担所有业务逻辑。

---

## 58. Dependency Injection

Godot 虽无固定 DI 框架，但可以通过：

- Export 属性；
- Init 方法；
- Factory；
- Parent 装配；
- Resource 配置；
- 接口对象；

实现依赖注入。

示例：

```gdscript
func setup(data: CharacterData, inventory: InventoryService) -> void:
    _data = data
    _inventory = inventory
```

---

## 59. Service 设计

Service 应：

- 职责单一；
- 接口清楚；
- 无场景层细节；
- 可替换；
- 可测试；
- 生命周期明确。

例如：

- SaveService；
- AudioService；
- LocalizationService；
- PlatformService；
- AnalyticsService。

---

## 60. Data-Driven Design

适合将以下内容配置化：

- 角色参数；
- 道具；
- 技能；
- 关卡；
- 数值；
- UI Theme；
- 音频表；
- 掉落表；
- 状态定义。

优点：

- 降低代码改动；
- 支持工具化；
- 支持内容扩展；
- 支持平衡调整；
- 支持测试。

风险：

- 数据缺少校验；
- 字段过多；
- 版本迁移困难；
- 运行时错误。

应配套：

- Schema；
- 默认值；
- 验证器；
- 编辑工具；
- 版本号。

---

## 61. Content Validation

建议建立工具检查：

- 缺失资源；
- 重复 ID；
- 无效路径；
- 空字段；
- 越界值；
- 循环引用；
- 未本地化文本；
- 不存在的 Input Action；
- 无效碰撞层；
- 错误资源类型。

---

## 62. Build Automation

可自动化：

- Headless 导入；
- Script 检查；
- 测试；
- Export；
- Version Injection；
- 打包；
- Artifact 上传；
- Changelog；
- Smoke Test。

---

## 63. CI Workflow

示例：

```text
Checkout
→ Install Godot
→ Import Project
→ Parse / Lint
→ Run Tests
→ Export Debug
→ Export Release
→ Package
→ Upload Artifact
```

CI 中应固定：

- Godot 版本；
- Export Templates；
- 插件版本；
- 环境变量；
- 平台 SDK。

---

## 64. Definition of Ready for Godot Task

```markdown
- [ ] 目标 Scene 或 System 明确；
- [ ] 依赖明确；
- [ ] Resource 规格明确；
- [ ] Input / Signal / Group 需求明确；
- [ ] 平台要求明确；
- [ ] 存档影响明确；
- [ ] 验收标准明确；
- [ ] Export 验证要求明确；
```

---

## 65. Definition of Done for Godot Feature

```markdown
- [ ] Scene 和 Script 完成；
- [ ] 依赖装配完成；
- [ ] 正常流程通过；
- [ ] 失败流程通过；
- [ ] Resource 和数据正确；
- [ ] 存档与迁移通过；
- [ ] 正式导出通过；
- [ ] 目标平台验证通过；
- [ ] 无阻塞错误；
- [ ] 性能可接受；
- [ ] 插件和资源完整；
- [ ] 文档已更新；
```

---

## 66. Godot Feature Checklist

```markdown
## Structure

- [ ] Scene 职责清楚；
- [ ] Root Node 合理；
- [ ] 子场景拆分合理；
- [ ] Resource 使用合理；
- [ ] 无深层隐藏依赖。

## Script

- [ ] Script 职责单一；
- [ ] 类型清楚；
- [ ] 信号清楚；
- [ ] 错误处理完整；
- [ ] 生命周期安全。

## Dependencies

- [ ] 无不必要 Autoload；
- [ ] 无脆弱 NodePath；
- [ ] 依赖显式；
- [ ] 可测试；
- [ ] 可替换。

## Data

- [ ] 数据模型清楚；
- [ ] Resource 共享行为明确；
- [ ] 存档版本明确；
- [ ] 迁移已验证；
- [ ] 数据校验存在。

## Input / UI

- [ ] Input Map 正确；
- [ ] 支持目标设备；
- [ ] UI Focus 正确；
- [ ] 分辨率适配；
- [ ] 状态完整。

## Performance

- [ ] 无明显每帧搜索；
- [ ] 无明显资源泄漏；
- [ ] 无过度实例化；
- [ ] Profiler 已检查；
- [ ] 正式构建性能通过。

## Export

- [ ] Export Preset 正确；
- [ ] Release Build 成功；
- [ ] 资源完整；
- [ ] 插件完整；
- [ ] 目标平台可运行。
```

---

## 67. 常见失败模式

### 67.1 所有系统都放进 Autoload

结果：

- 隐藏依赖；
- 状态污染；
- 测试困难；
- 生命周期混乱。

---

### 67.2 单个 Scene 管理整个游戏

结果：

- 节点树巨大；
- 冲突频繁；
- 无法复用；
- 难以测试。

---

### 67.3 使用硬编码节点路径

结果：

- 稍微移动节点就报错；
- 子场景无法复用；
- 依赖难发现。

---

### 67.4 Resource 被运行时意外修改

结果：

- 多个实例互相影响；
- 编辑器资源被污染；
- 状态难以追踪。

---

### 67.5 只在编辑器运行

结果：

- Export 缺资源；
- 正式配置不同；
- 平台行为不同；
- 插件失效。

---

### 67.6 输入直接绑定物理键

结果：

- 无法重映射；
- 手柄支持困难；
- 平台切换困难。

---

### 67.7 Scene 继承过深

结果：

- Override 难理解；
- 父场景变化影响巨大；
- Inspector 状态混乱。

---

### 67.8 大量逻辑写在 `_process`

结果：

- 性能问题；
- 帧率相关 Bug；
- 状态难控制。

---

### 67.9 引擎升级直接在主分支进行

结果：

- 项目无法回退；
- 插件损坏；
- 多平台导出失败；
- 大量无关变更混入。

---

### 67.10 插件无版本记录

结果：

- 无法复现；
- 升级后行为变化；
- 许可证不清。

---

### 67.11 不做存档版本化

结果：

- 新版本无法加载旧数据；
- 迁移困难；
- 用户进度丢失。

---

### 67.12 通过节点名称传递架构

例如依赖：

```text
场景树中必须存在名为 GameManager 的节点
```

这种设计难以复用和测试。

---

## 68. Godot 项目维护节奏

### 每日

- 运行核心 Scene；
- 检查错误和警告；
- 提交小范围改动；
- 更新阻塞。

### 每周

- 检查正式导出；
- 检查性能；
- 检查资源引用；
- 清理临时文件；
- 更新插件风险。

### 每个 Milestone

- 全平台 Export；
- 存档迁移验证；
- 依赖审查；
- 项目设置审查；
- 目录和架构审查；
- 文档更新。

### 每个 Release

- 固定 Godot 版本；
- 固定 Export Templates；
- 固定 Commit；
- 正式构建；
- Smoke Test；
- Tag；
- 归档产物。

---

## 69. 项目元数据模板

```markdown
# Godot Project Metadata

> Project：
> Engine Version：
> Renderer：
> Main Scene：
> Target Platforms：
> Last Updated：

## Export Templates

## Plugins

| Name | Version | Source | License |
|---|---|---|---|

## Autoloads

| Name | Responsibility |
|---|---|

## Input Actions

## Collision Layers

## Save Version

## Supported Resolutions

## Build Instructions

## Known Platform Limitations
```

---

## 70. 引擎升级记录模板

```markdown
# Godot Upgrade Record

> From：
> To：
> Branch：
> Owner：
> Date：

## Reason

## Breaking Changes

## Plugin Compatibility

## Script Changes

## Scene Changes

## Resource Reimport

## Save Compatibility

## Export Results

| Platform | Result | Notes |
|---|---|---|

## Performance Comparison

## Known Issues

## Rollback Plan

## Final Decision
```

---

## 71. Export Validation 模板

```markdown
# Export Validation

> Version：
> Build：
> Commit：
> Godot Version：
> Tester：
> Date：

## Platforms

| Platform | Build | Launch | Core Flow | Save | Result |
|---|---|---|---|---|---|

## Resource Validation

## Plugin Validation

## Input Validation

## Performance

## Logs

## Known Issues

## Final Decision

- [ ] Pass
- [ ] Pass with Limitations
- [ ] Fail
```

---

## 72. 最终原则

Godot Workflow 的本质不是：

> 规定每一个 Scene 和 Script 必须使用同一种写法。

它的本质是：

> 建立一套稳定、显式、可维护的项目工作方式，使 Scene、Node、Resource、Script、资产、数据、插件和导出流程能够长期协作。

一个健康的 Godot 项目应做到：

- 引擎版本固定；
- 项目结构清楚；
- Scene 职责明确；
- Resource 数据化；
- 依赖显式；
- Autoload 克制；
- 输入和碰撞统一；
- 存档可迁移；
- 资源导入稳定；
- 插件可追踪；
- 正式导出持续验证；
- 引擎升级可回滚；
- 性能基于测量；
- 架构支持测试和维护。
