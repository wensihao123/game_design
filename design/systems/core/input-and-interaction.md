# Input and Interaction（输入与交互系统）

> Status: V1  
> Category: Core  
> Path: `design/systems/core/input-and-interaction.md`  
> Owner: TBD  
> Reviewers: Design / UX / Engineering / QA / Accessibility / Research  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: High  
> Dependencies: Game State and Flow, Core Loop, Rules and Resolution, Settings and Preferences, Save and Persistence, Platform Integration  
> Affected Systems: Tutorial and Onboarding, Characters and Loadouts, Difficulty and Challenge, Accessibility, UI, Social and Multiplayer, Analytics and Telemetry

---

## 1. System Summary

Input and Interaction 系统负责将玩家通过：

- 键盘；
- 鼠标；
- 手柄；
- 触摸；
- 手势；
- 辅助设备；
- 平台级输入；

表达的物理操作，转换为稳定、可理解、可配置的玩家意图。

它定义：

```text
玩家想做什么；
当前状态允许什么；
哪个输入上下文正在生效；
不同设备如何映射到相同意图；
输入何时被接受、缓冲、重复、取消或拒绝；
焦点如何移动；
误触如何防止；
输入失败后如何恢复；
辅助设置如何改变输入方式而不破坏核心规则。
```

系统应优先围绕：

```text
Player Intent
```

组织，而不是围绕：

```text
具体按键
```

组织。

---

## 2. Purpose

### 2.1 Player Value

该系统帮助玩家：

- 准确表达自己的意图；
- 在不同设备上获得一致行为；
- 理解输入是否被接受；
- 在高压力情境中减少误触；
- 根据能力和偏好调整操作；
- 在切换设备、页面和模式后保持可控；
- 取消尚未提交的操作；
- 从输入失败、控制器断开或焦点丢失中恢复。

### 2.2 Experience Contribution

Input and Interaction 直接影响：

- 掌控感；
- 响应感；
- 可学习性；
- 公平；
- 可访问性；
- 错误恢复；
- 平台一致性；
- 核心循环节奏。

即使规则正确，只要输入系统存在：

- 延迟；
- 丢输入；
- 错误焦点；
- 不可预测重复；
- 误触；
- 平台语义冲突；

玩家仍会认为整个产品“不好操作”。

### 2.3 Product Value

统一输入系统可以：

- 降低跨平台重复实现；
- 支持输入重绑定；
- 支持手柄、触摸和键鼠共存；
- 支持辅助输入；
- 降低 UI 与玩法耦合；
- 支持自动测试与输入回放；
- 提高 QA 可覆盖性；
- 减少每个功能自行解释按键语义。

### 2.4 Why This System Exists

缺少统一输入层时，项目容易出现：

```text
同一个按键在不同页面代表不同含义；
UI 直接监听物理按键；
游戏逻辑依赖设备类型；
页面切换后旧输入仍然生效；
长按、双击、连点各自实现；
辅助模式与普通模式使用不同规则；
控制器断开后角色继续行动；
重绑定只改变显示，不改变实际行为。
```

---

## 3. Non-Goals

该系统不负责：

- 决定具体玩法数值；
- 计算行动结果；
- 拥有角色状态；
- 替代 Rules and Resolution；
- 决定最终 UI 视觉样式；
- 管理完整设置界面；
- 实现平台底层驱动；
- 将所有操作自动化；
- 为每个功能创建独立输入语义；
- 通过输入复杂度制造非核心难度；
- 让辅助输入绕过权威规则；
- 直接修改业务状态。

---

## 4. Governing Principles

### 4.1 Player First Design

参考：

- `../../philosophy/foundation/player-first-design.md`

应用原则：

- 输入应服务玩家意图；
- 高频操作减少不必要步骤；
- 高影响输入应支持确认、取消或恢复；
- 平台惯例不应破坏玩家控制。

### 4.2 Clarity and Feedback

参考：

- `../../philosophy/experience/clarity-and-feedback.md`

应用原则：

- 每次重要输入都有确认；
- 不可用操作说明原因；
- 焦点、选择和模式清楚可见；
- 输入提示与当前设备一致。

### 4.3 Simplicity and Depth

参考：

- `../../philosophy/experience/simplicity-and-depth.md`

应用原则：

- 基础输入简单；
- 深度来自情境和组合，而不是不必要按键负担；
- 相同意图复用相同输入语义；
- 熟练玩家可使用快捷方式。

### 4.4 Challenge and Fairness

参考：

- `../../philosophy/experience/challenge-and-fairness.md`

应用原则：

- 输入可靠；
- 延迟和设备差异不应产生隐藏不公平；
- 辅助设置不应被污名化；
- 竞技输入规则必须透明。

### 4.5 Consistency and Coherence

参考：

- `../../philosophy/long-term/consistency-and-coherence.md`

应用原则：

- Confirm、Cancel、Back、Pause 等基础意图保持一致；
- 相同组件具有相同焦点和操作方式；
- 不同平台可以映射不同按键，但保持相同语义。

### 4.6 Accessibility and Inclusivity

参考：

- `../../philosophy/responsibility/accessibility-and-inclusivity.md`

应用原则：

- 支持重绑定；
- 支持按住与切换替代；
- 支持输入容错和速度调整；
- 避免只依赖精确、快速或持续输入；
- 支持不同设备和单手操作。

### 4.7 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 不通过隐藏取消或复杂退出限制玩家；
- 不利用误触推动购买；
- 商业输入必须防误操作；
- 不将营销内容映射到高频基础操作。

---

## 5. Player Experience

### 5.1 Player Goal

玩家通过输入系统希望：

- 移动；
- 选择；
- 确认；
- 取消；
- 调整；
- 执行行动；
- 暂停；
- 返回；
- 切换目标；
- 管理配置；
- 使用辅助功能。

### 5.2 Entry

输入上下文可能在以下时机建立：

- 应用启动；
- 页面进入；
- 模式进入；
- Modal 打开；
- 活动开始；
- 暂停；
- 设置变更；
- 设备连接；
- 设备断开；
- 恢复会话。

### 5.3 Main Actions

玩家输入可以被抽象为：

- Navigate；
- Point；
- Select；
- Confirm；
- Cancel；
- Back；
- Pause；
- Move；
- Aim；
- Primary Action；
- Secondary Action；
- Context Action；
- Open Menu；
- Switch Target；
- Adjust Value；
- Scroll；
- Zoom；
- Communicate。

### 5.4 Core Decisions

输入系统需要保护的核心决策包括：

- 当前动作是否已经提交；
- 是否允许取消；
- 是否使用当前目标；
- 是否消耗高价值资源；
- 是否离开当前流程；
- 是否接受设备自动切换；
- 是否启用辅助模式。

### 5.5 Success

成功意味着：

- 物理输入被正确识别；
- 转换成正确玩家意图；
- 在正确上下文中执行；
- 产生及时反馈；
- 不重复执行；
- 不影响被遮挡或失焦区域；
- 下游规则系统只接收有效意图。

### 5.6 Failure

失败可能包括：

- 输入不支持；
- 设备断开；
- 焦点丢失；
- 上下文冲突；
- 重绑定冲突；
- 输入过期；
- 连点重复；
- 网络延迟；
- 当前状态不可执行；
- 辅助设置未应用。

### 5.7 Exit and Return

离开输入上下文时，应：

- 清除或转移焦点；
- 释放按住状态；
- 取消未提交手势；
- 清理缓冲；
- 防止旧上下文继续接收输入；
- 恢复时重新提示当前设备映射。

---

## 6. System Boundary

### 6.1 Inputs

系统接收：

- Raw Device Input；
- Platform Input Events；
- Current Game State；
- Current UI Focus；
- Active Input Context；
- Input Mapping；
- Accessibility Preferences；
- Device Connection State；
- Timing；
- Network Input State；
- Tutorial Restrictions。

### 6.2 Outputs

系统产生：

- Player Intent；
- Navigation Intent；
- Action Intent；
- Cancel Intent；
- Pause Intent；
- Focus Movement；
- Device Change Event；
- Input Rejected Feedback；
- Input Prompt State；
- Buffered Input；
- Input Audit or Replay Data。

### 6.3 Owned State

系统拥有：

- Active Input Context；
- Input Context Stack；
- Active Device Type；
- Input Mapping Profile；
- Focus Ownership；
- Held Input State；
- Input Buffer；
- Repeat State；
- Gesture State；
- Input Lock State；
- Last Accepted Intent；
- Device Switching State。

### 6.4 Read-Only Dependencies

系统读取：

- Game State 当前模式；
- UI 当前可交互元素；
- Settings 输入偏好；
- Accessibility 辅助设置；
- Core Loop 当前阶段；
- Tutorial 当前限制；
- Network 当前连接状态；
- Platform 当前设备能力。

### 6.5 Write Dependencies

系统通过正式意图请求：

- Game State 执行模式切换；
- Rules 执行动作；
- UI 更新焦点；
- Settings 保存重绑定；
- Core Loop 暂停或继续；
- Analytics 记录输入失败或设备变化。

### 6.6 Out of Scope

输入系统不直接：

- 修改角色位置；
- 扣除资源；
- 完成任务；
- 发放奖励；
- 修改存档业务状态；
- 决定支付结果；
- 决定规则是否成功。

---

## 7. Input Architecture

推荐使用分层结构：

```text
Physical Input
→ Device Adapter
→ Input Mapping
→ Input Context
→ Player Intent
→ Domain Validation
→ Resolution
→ Feedback
```

### 7.1 Physical Input

具体设备产生的原始信号。

例如：

- Key Down；
- Button Press；
- Axis；
- Pointer Move；
- Touch；
- Gesture；
- Voice；
- Switch Input。

### 7.2 Device Adapter

统一不同平台和设备的数据格式。

### 7.3 Input Mapping

将物理输入映射为抽象 Action。

### 7.4 Input Context

决定当前哪些 Action 有效。

### 7.5 Player Intent

描述玩家想做什么。

### 7.6 Domain Validation

由领域系统判断当前是否允许。

### 7.7 Resolution

由 Rules and Resolution 产生权威结果。

### 7.8 Feedback

向玩家确认输入与结果。

---

## 8. Core Entities and Concepts

| Entity / Concept | Definition | Owner | Lifetime | Notes |
|---|---|---|---|---|
| Physical Input | 设备原始信号 | Platform / Adapter | 瞬时 | 不直接进入业务 |
| Device Profile | 设备能力和默认映射 | Input | 设备级 | 键鼠、手柄、触摸等 |
| Input Action | 抽象输入动作 | Input | 项目级 | 如 Confirm、Move |
| Player Intent | 面向业务的玩家意图 | Input | 单次请求 | 下游验证 |
| Input Mapping | Physical Input 到 Action 的映射 | Input / Settings | 持久化 | 可重绑定 |
| Input Context | 当前允许的 Action 集合 | Input | 模式级 | 可堆叠 |
| Focus Target | 当前导航焦点 | Input / UI Contract | 页面级 | 唯一主焦点 |
| Input Buffer | 暂存未到执行窗口的意图 | Input | 短期 | 有过期时间 |
| Held State | 持续按住状态 | Input | 输入持续期 | 切换上下文时清理 |
| Repeat Policy | 按住后重复行为 | Input | Context 级 | 导航和数值调整 |
| Gesture State | 手势识别过程 | Input | 手势期 | 可取消 |
| Input Prompt | 当前设备的提示表达 | UI | 页面级 | 来源于映射 |

---

## 9. Input Action Taxonomy

### 9.1 Navigation Actions

- Navigate Up；
- Navigate Down；
- Navigate Left；
- Navigate Right；
- Next Tab；
- Previous Tab；
- Focus Next；
- Focus Previous。

### 9.2 Confirmation Actions

- Confirm；
- Submit；
- Primary Action；
- Context Action。

### 9.3 Cancellation Actions

- Cancel；
- Back；
- Close；
- Abort；
- Undo。

这些动作语义不能随意混用。

### 9.4 System Actions

- Pause；
- Open Menu；
- Open Settings；
- Screenshot；
- Accessibility Shortcut。

### 9.5 Continuous Actions

- Move；
- Aim；
- Pan；
- Scroll；
- Zoom；
- Adjust Value。

### 9.6 Communication Actions

- Ping；
- Emote；
- Push to Talk；
- Open Chat；
- Quick Message。

### 9.7 Debug and Development Actions

必须与玩家输入隔离，不能进入正式映射。

---

## 10. Action vs Intent

Input Action 是抽象操作。

Player Intent 是结合当前上下文后的业务意图。

例如：

```text
Input Action:
Confirm

Context:
Inventory Item Selected

Player Intent:
Equip Selected Item
```

另一个情境：

```text
Input Action:
Confirm

Context:
Delete Confirmation Modal

Player Intent:
Confirm Permanent Deletion
```

因此：

```text
同一 Action
可以在不同 Context 中产生不同 Intent，
但语义必须清楚且符合预期。
```

---

## 11. Input Context

### 11.1 Context Types

- Global；
- Application；
- Menu；
- Modal；
- Gameplay；
- Targeting；
- Text Entry；
- Spectator；
- Tutorial；
- Accessibility；
- Debug。

### 11.2 Context Stack

推荐：

```text
Global
→ Current Mode
→ Current Flow
→ Current Modal
→ Temporary Capture
```

越上层的临时上下文可以阻止下层接收输入。

### 11.3 Context Priority

例如：

```text
System Dialog
> Blocking Modal
> Text Entry
> Gameplay
> Global Shortcut
```

但某些安全操作可以保留：

- Pause；
- Emergency Exit；
- Accessibility Shortcut。

### 11.4 Context Enter

进入时：

- 激活允许 Action；
- 设置焦点；
- 清理不兼容 Held State；
- 更新提示；
- 记录来源。

### 11.5 Context Exit

退出时：

- 释放捕获；
- 清理缓冲；
- 恢复下层焦点；
- 更新提示；
- 阻止旧事件穿透。

---

## 12. Context Invariants

1. 同一设备事件只能由一个权威上下文消费。
2. Blocking Modal 打开时，背景业务输入不得生效。
3. Text Entry 时，普通快捷键不得误触业务操作。
4. 上下文切换后，旧 Held State 不得继续生效。
5. Context Stack 必须可追踪。
6. UI 页面销毁时必须释放 Focus 和 Capture。
7. 输入提示必须来自当前映射和当前设备。
8. Debug Context 不得进入正式玩家会话。
9. 支付和永久删除流程不得被背景输入确认。
10. 高风险 Intent 必须由当前可见上下文发起。

---

## 13. Device Profiles

### 13.1 Keyboard and Mouse

特点：

- 多键；
- 精确指针；
- 快捷键；
- 滚轮；
- 多显示器；
- 键盘布局差异。

需要考虑：

- AZERTY / QWERTZ；
- 左右键语义；
- DPI；
- 鼠标加速度；
- 指针锁定；
- Window Focus。

### 13.2 Gamepad

特点：

- 方向导航；
- 模拟轴；
- 按键图标；
- 控制器断开；
- 多控制器；
- 平台确认键差异。

需要考虑：

- Dead Zone；
- Stick Drift；
- Trigger Range；
- Vibration；
- Controller Assignment。

### 13.3 Touch

特点：

- 直接触摸；
- 无 Hover；
- 手指遮挡；
- 多点；
- 手势；
- 设备尺寸差异。

需要考虑：

- Hit Area；
- Edge Gesture；
- Scroll Conflict；
- Accidental Touch；
- Orientation；
- Safe Area。

### 13.4 Assistive Devices

包括：

- Switch；
- Adaptive Controller；
- Eye Tracking；
- Voice；
- One-Handed Device；
- External Keyboard。

输入系统应通过抽象 Action 接入，而不是要求领域系统单独支持。

---

## 14. Active Device Detection

### 14.1 设备切换

当检测到有效输入时，可以切换 Active Device。

### 14.2 防止抖动

鼠标轻微移动、Stick Drift 或触摸噪声不应频繁切换提示。

### 14.3 Prompt Switching

提示应在设备切换后更新：

- 按键图标；
- 操作说明；
- 焦点表现；
- 导航模式。

### 14.4 混合输入

部分玩家会同时使用：

- 手柄移动；
- 鼠标瞄准；
- 键盘快捷键。

系统应允许项目定义：

- 单一 Active Device；
- 混合模式；
- 分输入域设备。

### 14.5 多控制器

必须定义：

- 哪个控制器属于哪个玩家；
- 谁可以控制全局菜单；
- 断开后如何处理；
- 重新连接如何恢复。

---

## 15. Input Mapping

### 15.1 Default Mapping

每个平台提供符合惯例的默认映射。

### 15.2 Remapping

应支持：

- 单个 Action 重绑定；
- 恢复默认；
- 多套 Profile；
- 冲突检测；
- 主键和备用键；
- 设备独立映射。

### 15.3 Reserved Inputs

平台保留输入不应被覆盖。

### 15.4 Essential Actions

以下通常必须始终可达：

- Confirm；
- Cancel / Back；
- Pause；
- Navigation；
- Accessibility Access。

### 15.5 Unbound Action

若 Action 未绑定，应：

- 明确提示；
- 提供快速修复；
- 阻止保存不可用配置；
- 保留安全返回方式。

### 15.6 Binding Conflict

冲突可能：

- 允许；
- 警告；
- 阻止；
- 按 Context 区分。

必须说明冲突范围。

---

## 16. Mapping Data Model

推荐：

```markdown
| Action | Device | Primary Binding | Secondary Binding | Context | Rebindable |
|---|---|---|---|---|---|
```

映射记录应包含：

- Action ID；
- Device Type；
- Physical Control；
- Modifier；
- Context；
- Hold / Toggle；
- Sensitivity；
- Dead Zone；
- Repeat Policy；
- Version。

---

## 17. Focus System

### 17.1 Focus Types

- Navigation Focus；
- Text Focus；
- Pointer Hover；
- Selection；
- Input Capture；
- Accessibility Focus。

这些状态可能相关，但不应完全混为一体。

### 17.2 Single Primary Focus

每个导航层应有唯一主焦点。

### 17.3 Initial Focus

页面进入时应有合理默认焦点。

优先：

- 上次位置；
- 主要任务；
- 安全默认；
- 第一个可用元素。

### 17.4 Focus Restore

关闭 Modal 后恢复原焦点。

### 17.5 Focus Lost

若目标被删除、禁用或隐藏，应移动到：

- 最近可用元素；
- 父级；
- 安全默认。

### 17.6 Spatial Navigation

方向导航应考虑：

- 几何位置；
- 逻辑分组；
- 视觉顺序；
- 循环；
- 跳过禁用；
- 大列表。

### 17.7 Focus Trap

Modal 可以限制焦点，但必须保留退出方式。

---

## 18. Pointer Interaction

### 18.1 Hit Target

目标大小应考虑：

- 触摸；
- 鼠标；
- 高分辨率；
- 运动能力差异；
- 屏幕尺寸。

### 18.2 Pointer Down vs Up

高风险操作通常应在：

```text
Pointer Up
```

且指针仍在目标范围内时提交。

### 18.3 Drag Threshold

避免轻微移动被识别为拖拽。

### 18.4 Hover

Hover 不能作为唯一信息来源。

### 18.5 Pointer Capture

拖拽或滑动时，应明确：

- 谁拥有 Capture；
- 离开窗口怎么办；
- 中断如何取消；
- 页面切换如何释放。

---

## 19. Touch and Gesture

### 19.1 Tap

定义：

- 最大时间；
- 最大移动；
- 多次点击间隔；
- 误触保护。

### 19.2 Double Tap

不应作为唯一基础操作。

### 19.3 Long Press

应提供：

- 明确反馈；
- 可取消；
- 可调整持续时间；
- 替代方式。

### 19.4 Swipe

需要区分：

- 页面滚动；
- 系统边缘手势；
- 玩法手势；
- 拖拽。

### 19.5 Pinch and Zoom

提供按钮或其他替代方式。

### 19.6 Multi-Touch

若非核心体验，不应强制多指同时操作。

---

## 20. Button and Key Lifecycle

典型生命周期：

```text
Up
→ Pressed
→ Held
→ Released
```

可能额外包括：

- Repeat；
- Cancelled；
- Consumed；
- Blocked。

### 20.1 Pressed

适合：

- 即时低风险行动；
- 开始蓄力；
- 反馈。

### 20.2 Released

适合：

- 确认点击；
- 完成蓄力；
- 防误触提交。

### 20.3 Held

适合：

- 持续移动；
- 持续瞄准；
- 连续调整。

### 20.4 Context Change During Hold

上下文变化时应明确：

- Cancel；
- Release；
- Transfer；
- Continue。

默认应优先 Cancel，避免旧输入穿透。

---

## 21. Hold, Toggle, and Sticky Inputs

### 21.1 Hold

玩家必须持续按住。

### 21.2 Toggle

按一次开启，再按一次关闭。

### 21.3 Sticky

短暂输入在一定窗口内保持。

### 21.4 Accessibility

对于需要持续按住的操作，应考虑提供 Toggle。

例如：

- 瞄准；
- 奔跑；
- 防御；
- 拖拽；
- Push to Talk。

### 21.5 State Feedback

Toggle 状态必须可见。

---

## 22. Repeat Policy

按住输入可能产生重复。

### 22.1 Repeat Parameters

- Initial Delay；
- Repeat Interval；
- Acceleration；
- Maximum Rate。

### 22.2 Context Differences

导航重复与数值调整重复可以不同。

### 22.3 防止重复提交

Confirm、Purchase、Delete 等高风险 Action 默认不应自动 Repeat。

### 22.4 Repeat Reset

以下情况应重置：

- 释放；
- Context 变化；
- 焦点变化；
- Modal 打开；
- 设备切换。

---

## 23. Input Buffer

### 23.1 Purpose

在当前行动即将结束时暂存下一意图，提高响应感。

### 23.2 适用

- 连续动作；
- 战斗；
- 节奏；
- 动画转换；
- 页面切换后的单次导航。

### 23.3 不适用

- 购买；
- 永久删除；
- 高价值消耗；
- 账户切换；
- 隐私确认。

### 23.4 Buffer Fields

- Intent；
- Timestamp；
- Context；
- Expiry；
- Priority；
- Source Device；
- Consumed State。

### 23.5 Expiry

过期输入必须丢弃，不得在意外后续状态中突然执行。

### 23.6 Context Validation

消费 Buffer 前重新验证当前 Context。

---

## 24. Input Queue and Priority

### 24.1 Priority Examples

```text
Emergency System Action
> Cancel / Pause
> Current Core Action
> Navigation
> Cosmetic Shortcut
```

### 24.2 不应无限排队

输入队列必须有：

- 容量；
- 去重；
- 过期；
- 合并；
- 丢弃策略。

### 24.3 Continuous Input

轴输入通常读取当前值，不应当作大量离散事件排队。

### 24.4 Latest Wins

适用于：

- Aim；
- Pointer Position；
- Scroll Direction；
- Analog Movement。

### 24.5 First Wins

适用于：

- 单次提交；
- 事务开始；
- 模式切换。

---

## 25. Cancellation and Undo

### 25.1 Cancel

停止尚未提交的当前流程。

### 25.2 Back

返回上一级 Flow 或关闭覆盖层。

### 25.3 Undo

撤销已经提交但可逆的结果。

三者不应混淆。

### 25.4 Cancel Window

高风险交互可提供：

- 按住确认；
- 二次确认；
- 提交前预览；
- 短时间撤销。

### 25.5 Cancel Feedback

取消后应说明：

- 是否扣除成本；
- 是否保留进度；
- 是否可以重新开始。

---

## 26. Accidental Input Protection

### 26.1 High-Risk Actions

包括：

- 购买；
- 永久删除；
- 分解高价值物品；
- 放弃不可恢复进度；
- 退出多人；
- 发布公开内容。

### 26.2 Protection Methods

- Confirmation；
- Hold to Confirm；
- Type to Confirm；
- Protected State；
- Lock；
- Undo；
- Cooldown；
- Pointer Up Commit；
- Safe Default Focus。

### 26.3 不使用危险默认焦点

高风险按钮不应成为 Modal 默认焦点。

### 26.4 不使用重复输入确认高风险操作

例如：

```text
快速连续按 Confirm
```

不应意外穿过两层确认。

---

## 27. Input Lock

### 27.1 Lock Types

- Full Lock；
- Movement Lock；
- Action Lock；
- Navigation Lock；
- Transaction Lock；
- Tutorial Lock。

### 27.2 Lock Reason

必须可追踪：

- Animation；
- Loading；
- Network；
- Modal；
- Cutscene；
- Transition；
- System Failure。

### 27.3 Input Lock 不应替代状态设计

如果大量流程依赖“临时锁全部输入”，可能说明：

- 状态边界不清；
- 过渡逻辑不完整；
- UI 与业务耦合。

### 27.4 Lock Feedback

较长 Lock 应显示：

- 正在处理；
- 原因；
- 是否可取消；
- 超时行为。

---

## 28. Input Feedback

### 28.1 Immediate Feedback

物理输入被识别时的即时响应。

例如：

- 按压；
- 高亮；
- 音效；
- 震动；
- 光标变化。

### 28.2 Acceptance Feedback

系统接受了 Intent。

### 28.3 Rejection Feedback

系统拒绝时说明：

- 原因；
- 条件；
- 可恢复方式。

### 28.4 Result Feedback

下游规则产生结果。

### 28.5 不要混淆

输入动画不等于业务成功。

例如：

```text
按钮按下
≠
购买成功
```

---

## 29. Latency and Responsiveness

### 29.1 Latency Types

- Device Latency；
- Input Processing；
- Rendering；
- Network；
- Server Resolution；
- Feedback Delay。

### 29.2 Local Feedback

在线动作可以先提供低风险本地反馈，但最终结果必须由权威系统确认。

### 29.3 Prediction

适用于：

- 移动；
- 瞄准；
- 低风险视觉反馈。

不适用于：

- 购买；
- 资源扣除；
- 权益；
-永久资产。

### 29.4 Latency Compensation

竞技场景需要明确：

- 输入时间戳；
- 权威时间；
- 回滚；
- 插值；
- 预测；
- 公平边界。

### 29.5 Responsiveness 不等于立即提交

高风险操作优先正确性与防误触。

---

## 30. Input Timing

### 30.1 Input Window

定义动作可被接受的时间范围。

### 30.2 Grace Window

允许轻微提前或延迟输入。

### 30.3 Coyote Time

适用于平台跳跃等情境，提供离开边缘后的短暂容错。

### 30.4 Queue Window

允许下一个动作提前进入 Buffer。

### 30.5 Accessibility Scaling

时间窗口可根据辅助设置调整，但应说明：

- 是否影响竞技资格；
- 是否改变规则；
- 是否只影响本地单人体验。

---

## 31. Analog Input

### 31.1 Dead Zone

包括：

- Inner Dead Zone；
- Outer Dead Zone；
- Axial；
- Radial。

### 31.2 Sensitivity

支持：

- 水平；
- 垂直；
- 分设备；
- 分模式；
- 反转轴。

### 31.3 Curve

可能包括：

- Linear；
- Exponential；
- Custom Curve。

### 31.4 Drift

检测到持续微小输入时：

- 不切换 Active Device；
- 不移动焦点；
- 提供校准；
- 应用 Dead Zone。

### 31.5 Digital Conversion

模拟轴转数字导航时，应定义：

- 阈值；
- 回中；
- Repeat；
- 对角线策略。

---

## 32. Text Input

### 32.1 Context Isolation

文本输入时应避免：

- 快捷键触发；
- 角色移动；
- 菜单关闭；
- 购买确认。

### 32.2 IME

支持：

- 中文；
- 日文；
- 韩文；
- 组合输入；
- 候选窗口。

### 32.3 Validation

文本内容由对应领域系统验证。

### 32.4 Privacy

密码、支付、个人信息输入不得进入普通输入日志或分析事件。

### 32.5 Cancel and Submit

必须明确：

- Enter；
- Escape；
- Platform Confirm；
- Done；
- Back。

---

## 33. Input Prompts

### 33.1 Dynamic Prompts

提示根据：

- Active Device；
- Current Context；
- Current Mapping；
- Availability；

动态变化。

### 33.2 Prompt Source

UI 不应硬编码：

```text
Press E
```

应引用 Action：

```text
Context Action
```

再由输入系统提供实际绑定。

### 33.3 Multiple Bindings

可以显示：

- 当前设备主绑定；
- 备用绑定；
- 长按或组合提示。

### 33.4 Unavailable Action

不可用时应：

- 隐藏；
- 禁用；
- 或说明原因。

根据情境选择，但语义一致。

---

## 34. Tutorial Integration

### 34.1 Tutorial Teaches Intent

优先教学：

```text
做什么
```

而不是只教学：

```text
按哪个键
```

### 34.2 Device Change

教程过程中设备切换时，提示应自动更新。

### 34.3 Rebinding

教学应读取玩家实际映射。

### 34.4 Tutorial Lock

限制输入时必须：

- 保留安全退出；
- 不阻止辅助；
- 避免长时间锁定；
- 解释下一步。

### 34.5 Skilled Player

允许：

- 跳过；
- 快速完成；
- 使用已知快捷方式。

---

## 35. Networked Input

### 35.1 Client Intent

客户端发送：

- Intent；
- Timestamp；
- Sequence；
- Context；
- Relevant Parameters。

### 35.2 Server Authority

服务端验证：

- 状态；
- 权限；
- 目标；
- 资源；
- 时间窗口；
- 速率；
- 重复。

### 35.3 Sequence

需要处理：

- 丢包；
- 重复；
- 乱序；
- 延迟。

### 35.4 Rate Limit

避免：

- 输入洪泛；
- 恶意操作；
- 重复事务。

### 35.5 Reconciliation

客户端预测与服务端结果不同，应：

- 修正；
- 平滑；
- 解释高影响差异；
- 不重复触发反馈。

---

## 36. Multiplayer and Local Co-op

### 36.1 Player Assignment

每个设备或输入源映射到明确玩家。

### 36.2 Shared UI

需要定义：

- 谁拥有主焦点；
- 谁可以暂停；
- 谁可以确认全局操作；
- 多玩家同时输入如何处理。

### 36.3 Conflict

多人同时操作菜单时可以：

- 单一主控；
- 分区焦点；
- 投票；
- 轮流控制。

### 36.4 Disconnect

控制器断开时：

- 对应玩家暂停或 AI 接管；
- 不影响其他玩家输入；
- 提示重新连接；
- 保留玩家身份。

---

## 37. Haptics and Vibration

### 37.1 Purpose

震动用于：

- 输入确认；
- 结果反馈；
- 风险提示；
- 环境表达。

### 37.2 Accessibility

必须支持：

- 关闭；
- 强度调整；
- 类型调整。

### 37.3 Haptics 不应是唯一反馈

关键结果需有视觉或音频替代。

### 37.4 Device Capability

不同设备能力不同，应有安全降级。

---

## 38. Platform Conventions

### 38.1 Semantic Consistency

不同平台可使用不同物理输入，但保持：

- Confirm；
- Cancel；
- Back；
- Pause；
- Menu；

语义一致。

### 38.2 System Reserved Actions

遵守平台保留操作。

### 38.3 Certification

输入设计应考虑：

- 控制器断开；
- 用户切换；
- 系统菜单；
- Suspend / Resume；
- Safe Area；
- Text Entry；
- Accessibility Requirements。

### 38.4 Platform Override

平台 Override 不能绕过：

- 高风险确认；
- 权限；
- 存档；
- 退出保护。

---

## 39. Failure and Recovery

| Failure | Cause | Player Impact | Recovery | Data Guarantee |
|---|---|---|---|---|
| Device Disconnected | 控制器断开 | 无法继续操作 | 自动暂停并重连 | 不提交新意图 |
| Focus Lost | 元素删除或页面变化 | 无法导航 | 移动到安全焦点 | 不修改业务 |
| Mapping Invalid | 配置缺失或冲突 | Action 不可用 | 恢复默认或引导修复 | 保留旧配置备份 |
| Context Conflict | 多层上下文争夺 | 输入穿透或无响应 | 使用优先级并记录 | 不重复执行 |
| Buffer Expired | 输入窗口结束 | 动作未执行 | 丢弃并反馈 | 不延迟执行 |
| Duplicate Submit | 连点或重试 | 高风险重复 | 幂等拒绝 | 只执行一次 |
| Network Rejected | 服务端状态不同 | 动作回滚 | Reconcile 和说明 | 权威状态优先 |
| Accessibility Profile Failed | 设置未加载 | 操作困难 | 使用 Last Known Good | 不覆盖玩家偏好 |
| Text Capture Leak | 文本输入未隔离 | 快捷键误触 | 立即锁定背景输入 | 取消未提交业务动作 |

---

## 40. Edge Cases

### Device

- 启动时无控制器；
- 多控制器同时连接；
- 控制器中途断开；
- Stick Drift；
- 键盘布局变化；
- 触摸与鼠标同时使用；
- 辅助设备重新连接。

### Context

- Modal 打开瞬间输入；
- 页面切换瞬间连点；
- 文本输入时按快捷键；
- Loading 结束前输入；
- Tutorial 与设置同时打开；
- Background 时 Held State 未释放。

### Mapping

- Essential Action 被解绑；
- 两个 Action 共用按键；
- 旧版本 Action 已删除；
- 新设备没有默认映射；
- Profile 损坏；
- 云端与本地映射冲突。

### Timing

- 极低帧率；
- 高延迟；
- 输入在 Pause 前一帧发生；
- Buffer 在模式切换后过期；
- 长按过程中应用后台。

### UI

- 焦点元素被禁用；
- 列表刷新后焦点消失；
- 大字体导致布局变化；
- Pointer 与 Gamepad 焦点冲突；
- Modal Stack 多层关闭。

### High Risk

- 连点穿透购买确认；
- Back 触发购买；
- 控制器断开后默认确认；
- 支付返回时旧输入生效；
- 永久删除默认焦点错误。

---

## 41. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| Game State and Flow | Hard | State → Input | Active Context | 错误输入上下文 |
| Rules and Resolution | Hard | Input → Rules | Player Intent | 无法执行行动 |
| Core Loop | Hard | 双向 | Loop Stage / Intent | 循环无法推进 |
| Settings and Preferences | Hard / Soft | Settings → Input | Mapping Profile | 使用默认配置 |
| Save and Persistence | Soft | Input → Save | Mapping State | 偏好丢失 |
| Tutorial and Onboarding | Soft | Tutorial → Input | Guidance Context | 教学不一致 |
| Accessibility | Hard / Soft | Settings → Input | Assist Profile | 操作障碍 |
| UI | Hard | 双向 Contract | Focus / Prompt | 无法导航 |
| Platform Integration | Hard | Platform → Input | Raw Events | 无法识别设备 |
| Social and Multiplayer | Soft / Hard | Input → Social | Communication Intent | 社交操作失败 |
| Analytics | Soft | Input → Analytics | Input Failures | 不阻断 |

---

## 42. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Input Mapping Profile | 是 | Input / Settings | 修改确认 | 长期 | Last Known Good |
| Device Preferences | 是 | Input / Settings | 修改后 | 长期 | 默认值 |
| Sensitivity | 是 | Settings | 修改后 | 长期 | 默认值 |
| Dead Zone | 是 | Settings | 修改后 | 长期 | 平台默认 |
| Hold / Toggle Preference | 是 | Settings | 修改后 | 长期 | 默认值 |
| Active Device | 否或会话级 | Input | 设备切换 | 会话 | 自动检测 |
| Input Context Stack | 否 | Input | 不适用 | 瞬时 | 由 Game State 重建 |
| Focus History | 可选 | Input / UI | 页面退出 | 短期 | 安全默认 |
| Buffered Input | 否 | Input | 不适用 | 极短期 | 不恢复 |
| Accessibility Input Profile | 是 | Settings | 修改确认 | 长期 | Last Known Good |

### 42.1 Versioning

Mapping Profile 需要版本，以处理：

- Action 新增；
- Action 删除；
- 默认映射变化；
- 平台变化；
- 设备变化。

### 42.2 Cloud Conflict

输入偏好冲突时可：

- 最近修改优先；
- 分设备合并；
- 保留备份；
- 让玩家选择。

---

## 43. Accessibility

### 43.1 Remapping

- 所有核心 Action 可重绑定；
- 支持备用绑定；
- Essential Action 不可全部解绑；
- 映射冲突可解释。

### 43.2 Hold Alternatives

提供：

- Toggle；
- Sticky；
- 自动保持；
- 时间调整。

### 43.3 Repeated Input

提供：

- 自动连发；
- 降低频率要求；
- 批量操作；
- 长按替代连点。

### 43.4 Timing

- 扩大输入窗口；
- 延长确认时间；
- 调整双击和长按阈值；
- 支持暂停和减速。

### 43.5 Precision

- Aim Assist；
- Snap；
- Larger Hit Targets；
- Cursor Speed；
- Dead Zone；
- Drag Alternative。

### 43.6 One-Handed Use

考虑：

- 单手 Profile；
- 输入集中；
- Toggle；
- 自动化低价值动作；
- UI 重新布局。

### 43.7 Cognitive Support

- 统一术语；
- 动态提示；
- 减少同时输入；
- 预览；
- 可取消；
- 清楚焦点。

### 43.8 Competitive Context

竞技模式中的辅助规则应：

- 透明；
- 根据公平需求分层；
- 不污名化；
- 不剥夺基础可访问性。

---

## 44. Ethical and Safety Review

### 44.1 Purchase Protection

- 购买不使用高频基础 Action 直接提交；
- 防止连点穿透；
- 默认焦点不放在确认购买；
- 价格和结果在输入前清楚；
- 支持取消和恢复。

### 44.2 Exit and Cancellation

- Back 和 Cancel 可见；
- 不故意增加退出步骤；
- 不用输入延迟阻止离开；
- 不隐藏关闭和取消。

### 44.3 Notification and Marketing

营销入口不应：

- 劫持基础导航；
- 覆盖 Pause；
- 伪装成系统警告；
- 自动取得焦点并响应旧输入。

### 44.4 Children and Vulnerable Users

- 高风险消费采用额外保护；
- 不通过快速反应推动购买；
- 社交输入可屏蔽和拒绝；
- 辅助设置不会被重置以制造摩擦。

### 44.5 Privacy

- 不记录密码、聊天全文和敏感文本输入；
- 输入回放数据应最小化；
- 语音和眼动数据需要明确目的和控制。

---

## 45. Analytics and Validation

### 45.1 Key Assumptions

1. 玩家能够理解基础输入语义。
2. 相同 Action 在不同 Context 中保持可预测。
3. 当前设备提示能够正确更新。
4. 重绑定不会产生不可用配置。
5. 焦点导航在不同布局下仍然稳定。
6. 输入缓冲提高响应感而不制造误操作。
7. 辅助输入能够降低非核心障碍。
8. 高风险操作不会因连点、穿透或旧输入被误提交。

### 45.2 Validation Plan

| Hypothesis | Evidence | Success | Failure | Method |
|---|---|---|---|---|
| 基础语义可理解 | 任务完成与复述 | Confirm、Cancel、Back 使用正确 | 频繁误操作 | 可用性测试 |
| Context 可预测 | 场景任务 | 相同情境行为稳定 | 输入穿透 | 集成测试 |
| Device Prompt 正确 | 设备切换测试 | 提示快速稳定更新 | 提示抖动或错误 | QA |
| Remapping 可用 | 重绑定任务 | 无死锁、可恢复 | Essential Action 丢失 | 可用性与 QA |
| Focus 稳定 | 键盘手柄导航 | 可到达全部元素 | 焦点丢失或陷阱 | Accessibility Test |
| Buffer 合理 | 动作测试 | 响应提升且误触低 | 过期动作突然执行 | Gameplay Test |
| 辅助有效 | 目标用户测试 | 操作负担降低 | 辅助无效或影响核心 | Research |
| 高风险安全 | 连点与切换测试 | 只提交一次 | 重复购买或删除 | Fault Injection |

### 45.3 Behavioral Metrics

- Input Accepted；
- Input Rejected；
- Action Unbound；
- Mapping Changed；
- Device Switched；
- Focus Lost；
- Buffer Consumed；
- Buffer Expired；
- Duplicate Prevented；
- Controller Disconnected；
- Assist Option Used；
- Cancel Used。

### 45.4 Outcome Metrics

- 操作成功率；
- 误触率；
- 重绑定完成率；
- 输入恢复率；
- 焦点可达率；
- 设备切换成功率；
- 高风险重复提交率；
- 辅助设置任务完成率；
- 输入延迟分布。

### 45.5 Negative Metrics

- 输入穿透；
- 焦点卡死；
- Essential Action 未绑定；
- 提示与实际映射不一致；
- 控制器断开后继续行动；
- Buffer 过期后执行；
- 连点重复提交；
- 文本输入触发业务快捷键；
- 辅助设置失效；
- 平台确认和取消语义相反；
- 输入配置丢失。

### 45.6 Event Intents

| Event Intent | Trigger | Key Properties | Privacy Notes |
|---|---|---|---|
| Input Context Changed | Context 切换 | From, To, Reason | 不记录敏感内容 |
| Device Changed | 有效设备切换 | Device Type | 不收集唯一硬件标识 |
| Input Rejected | Intent 被拒绝 | Action, Reason | 参数最小化 |
| Mapping Changed | 重绑定保存 | Action, Device | 不记录个人文本 |
| Focus Recovery | 焦点丢失恢复 | Source, Target Type | 不记录内容文本 |
| Duplicate Prevented | 高风险重复 | Action Category | 审计用途 |
| Assist Profile Applied | 辅助启用 | Category | 不推断健康身份 |
| Controller Disconnected | 设备断开 | Context, Result | 不记录序列号 |

---

## 46. Test Strategy

### 46.1 Mapping Tests

- 默认映射完整；
- 重绑定；
- 冲突；
- 恢复默认；
- Profile 迁移。

### 46.2 Context Tests

- Modal；
- Text Entry；
- Gameplay；
- Pause；
- Loading；
- Background；
- Tutorial。

### 46.3 Focus Tests

- 动态列表；
- 禁用元素；
- 大字体；
- Modal；
- Pointer 与 Gamepad 切换；
- Focus Restore。

### 46.4 Device Tests

- 键鼠；
- 多种手柄；
- Touch；
- Adaptive Controller；
- 设备断开与重连；
- 混合输入。

### 46.5 Timing Tests

- 低帧率；
- 高延迟；
- 连点；
- 长按；
- Repeat；
- Buffer；
- Pause 边界。

### 46.6 High-Risk Tests

- 购买；
- 删除；
- 资源消耗；
- 退出多人；
- 账户切换；
- 输入穿透。

### 46.7 Accessibility Tests

- 单手；
- Toggle；
- 大 Hit Area；
- 降低重复；
- 延长窗口；
- 语音或 Switch 输入。

---

## 47. Rollout and Migration

### 47.1 Rollout

可以按：

- 设备；
- 平台；
- 页面；
- 模式；
- 玩家分群；

逐步发布。

### 47.2 Feature Flags

可用于：

- 新输入框架；
- 新 Focus 算法；
- 新设备支持；
- 新 Buffer；
- 新辅助功能。

必须避免不同 Flag 导致：

- 映射 Profile 无法读取；
- Essential Action 丢失；
- 新旧 Context 同时生效。

### 47.3 Mapping Migration

旧映射迁移需要：

- Action ID 映射；
- 删除 Action 处理；
- 新 Action 默认值；
- 冲突解决；
- Profile 备份；
- 回退。

### 47.4 Rollback

回滚时：

- 保留旧 Profile；
- 恢复 Last Known Good；
- 清理新 Context；
- 释放 Held State；
- 恢复默认提示；
- 不影响业务状态。

### 47.5 Stop Conditions

出现以下情况应停止发布：

- 核心 Action 无法使用；
- 大量 Focus 卡死；
- 连点导致重复高风险提交；
- Mapping 丢失；
- 控制器断开造成进度损失；
- 输入穿透；
- 辅助设置失效；
- 平台确认与取消错误；
- 输入延迟显著上升。

---

## 48. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| Context Stack 复杂化 | Architecture Risk | 高 | 中 | 限制层级、统一 Owner | Engineering |
| Focus 与页面布局耦合 | UX Risk | 高 | 中 | 语义分组和自动测试 | UX |
| 重绑定产生死锁 | Accessibility Risk | 高 | 低 | Essential Action 保护 | UX |
| 设备提示频繁抖动 | Experience Risk | 中 | 中 | 有效输入阈值 | Engineering |
| Buffer 导致误操作 | Gameplay Risk | 高 | 中 | 过期和 Context 验证 | Design |
| 触摸与系统手势冲突 | Platform Risk | 中 | 中 | Safe Area 和边缘规则 | UX |
| 辅助模式被视为作弊 | Ethical Risk | 高 | 中 | 透明分层与公平策略 | Product |
| 输入回放泄露隐私 | Privacy Risk | 高 | 低 | 数据最小化 | Data |
| 新旧 Profile 迁移失败 | Migration Risk | 高 | 中 | 备份和 Last Known Good | Engineering |

---

## 49. Review Checklist

### Purpose and Boundary

- [ ] 物理输入、Action、Intent 和规则结算分层明确；
- [ ] 输入系统不直接修改业务状态；
- [ ] Non-Goals 已定义；
- [ ] Player Intent 优先于具体按键。

### Context

- [ ] Input Context 类型明确；
- [ ] Context Stack 有优先级；
- [ ] Blocking Modal 阻止背景输入；
- [ ] Text Entry 隔离业务快捷键；
- [ ] Context Exit 清理 Held、Buffer 和 Capture。

### Mapping

- [ ] 默认映射完整；
- [ ] 核心 Action 可重绑定；
- [ ] Essential Action 不会全部解绑；
- [ ] 冲突检测明确；
- [ ] Profile 支持版本和恢复。

### Device

- [ ] 键鼠、手柄、触摸和辅助设备有统一抽象；
- [ ] Active Device 防抖；
- [ ] 混合输入规则明确；
- [ ] 控制器断开有恢复；
- [ ] Prompt 与实际绑定一致。

### Focus and Pointer

- [ ] 页面有合理 Initial Focus；
- [ ] Modal 关闭恢复焦点；
- [ ] 动态元素删除后可恢复；
- [ ] Hover 不是唯一信息来源；
- [ ] 高风险按钮不是默认焦点。

### Timing

- [ ] Hold、Toggle、Repeat 规则明确；
- [ ] Buffer 有容量、过期和 Context 验证；
- [ ] 高风险 Action 不自动 Repeat；
- [ ] 延迟补偿不影响资产正确性；
- [ ] 时间窗口支持辅助调整。

### Safety

- [ ] 高风险操作防连点穿透；
- [ ] Cancel、Back、Undo 语义分离；
- [ ] Input Lock 有原因和反馈；
- [ ] 购买、删除和退出有保护；
- [ ] UI 动画不等于业务成功。

### Accessibility

- [ ] 支持重绑定；
- [ ] 支持 Hold / Toggle 替代；
- [ ] 支持降低重复输入；
- [ ] 支持单手和不同设备；
- [ ] 辅助设置不会被模式切换丢失。

### Validation

- [ ] Mapping、Context、Focus、Device 和 High-Risk 测试完整；
- [ ] 误触、穿透、重复提交有负面指标；
- [ ] 辅助输入有目标用户验证；
- [ ] 新旧 Profile 有迁移和回滚。

---

## 50. V1 Completion Criteria

Input and Interaction 可以被视为 V1，当：

- Physical Input、Device Adapter、Input Mapping、Input Context、Player Intent 和 Resolution 分层明确；
- Input Action 与 Player Intent 的区别已经定义；
- Navigation、Confirmation、Cancellation、System、Continuous 和 Communication Action 有统一分类；
- Input Context Stack 和优先级规则明确；
- Game State、Modal、Text Entry、Gameplay 和 Tutorial 上下文不会互相穿透；
- 键鼠、手柄、触摸和辅助设备具有统一 Device Profile；
- Active Device 检测、混合输入和多控制器规则明确；
- 默认映射、重绑定、冲突、Essential Action 和 Profile 版本得到定义；
- Focus、Pointer Capture、Touch Gesture 和文本输入规则完整；
- Press、Hold、Release、Toggle、Repeat 和 Buffer 生命周期明确；
- Input Buffer 具有过期、容量和 Context 重新验证；
- Cancel、Back、Undo 和高风险确认语义明确；
- 连点、穿透、误触和重复提交具有保护；
- 延迟、预测和网络输入只在安全范围内使用；
- 输入提示与当前设备和实际映射一致；
- Tutorial 教学意图而不是硬编码按键；
- 重绑定、单手、Toggle、重复输入和时间窗口等可访问性能力完整；
- 支付、删除、退出和账户切换通过输入安全评审；
- Mapping、Context、Focus、Device、Timing 和 High-Risk 测试策略完整；
- 输入 Profile 支持迁移、Last Known Good 和回滚；
- 下游 UI、玩法、教程、设置和网络系统可以直接引用本文件。

---

## 51. Related Documents

### Philosophy

- [Player First Design](../../philosophy/foundation/player-first-design.md)
- [Clarity and Feedback](../../philosophy/experience/clarity-and-feedback.md)
- [Simplicity and Depth](../../philosophy/experience/simplicity-and-depth.md)
- [Challenge and Fairness](../../philosophy/experience/challenge-and-fairness.md)
- [Consistency and Coherence](../../philosophy/long-term/consistency-and-coherence.md)
- [Accessibility and Inclusivity](../../philosophy/responsibility/accessibility-and-inclusivity.md)
- [Ethical Design](../../philosophy/responsibility/ethical-design.md)

### Systems

- [Systems README](../README.md)
- [System Design Framework](../system-design-framework.md)
- [System Map](../system-map.md)
- [Integration Rules](../integration-rules.md)
- [Core Loop](./core-loop.md)
- [Game State and Flow](./game-state-and-flow.md)
- [Rules and Resolution](./rules-and-resolution.md)
- `../player/settings-and-preferences.md`
- `../player/tutorial-and-onboarding.md`
- `../player/save-and-persistence.md`
- `../progression/difficulty-and-challenge.md`
