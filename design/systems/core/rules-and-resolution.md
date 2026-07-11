# Rules and Resolution（规则与结算系统）

> Status: V1  
> Category: Core  
> Path: `design/systems/core/rules-and-resolution.md`  
> Owner: TBD  
> Reviewers: Design / Engineering / QA / UX / Data / Research  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: High  
> Dependencies: Core Loop, Game State and Flow, Input and Interaction, Configuration, Save and Persistence  
> Affected Systems: Combat or Primary Activity, Objectives and Quests, Resources and Economy, Reward System, Progression System, Difficulty and Challenge, Characters and Loadouts, Analytics and Telemetry

---

## 1. System Summary

Rules and Resolution 系统负责将：

```text
玩家意图
+
当前状态
+
系统规则
+
配置与随机性
```

转化为一个：

- 正确；
- 稳定；
- 可重现；
- 可解释；
- 可保存；
- 可验证；

的权威结果。

它定义：

```text
哪些规则生效；
规则以什么顺序生效；
多个效果如何叠加；
冲突如何解决；
随机结果如何生成；
数值如何舍入；
状态何时创建、刷新、消耗和结束；
同时事件如何排序；
递归与无限触发如何阻止；
最终结果如何记录和展示。
```

该系统是所有规则驱动玩法和业务流程的共同结算基础。

---

## 2. Purpose

### 2.1 Player Value

规则与结算系统帮助玩家：

- 理解自己的行动为什么产生当前结果；
- 根据稳定规则进行预测和规划；
- 相信相同条件下系统不会任意改变；
- 在失败后识别可改进的部分；
- 在面对随机性时拥有合理的风险管理空间；
- 在复杂效果中获得清楚的结果摘要；
- 避免因隐藏顺序、异常叠加或数值误差遭受不合理损失。

### 2.2 Experience Contribution

规则与结算直接决定：

- 掌控感；
- 公平感；
- 可预测性；
- 策略深度；
- 挑战；
- 反馈质量；
- 长期系统稳定性。

即使 UI、动画和内容表现良好，只要结算规则不稳定，玩家仍会认为系统：

- 不公平；
- 随机；
- 有 Bug；
- 不值得学习；
- 无法信任。

### 2.3 Product Value

该系统为以下能力提供共同基础：

- 战斗与技能；
- 任务条件；
- 资源交易；
- 奖励发放；
- 成长计算；
- 难度调整；
- 排名与评分；
- 模拟；
- 回放；
- 自动测试；
- 平衡分析；
- 争议审计。

### 2.4 Why This System Exists

如果规则与结算由每个功能自行实现，通常会出现：

```text
同类效果使用不同顺序；
UI 预览与实际结果不同；
客户端与服务端结果不同；
活动规则覆盖基础规则；
浮点误差逐步积累；
状态在错误时机结束；
同一事件被重复触发；
随机结果无法复现；
团队无法解释玩家争议。
```

统一结算框架用于减少这些系统性风险。

---

## 3. Non-Goals

该系统不负责：

- 定义所有具体技能、装备和任务内容；
- 决定最终动画和视觉表现；
- 拥有所有领域状态；
- 替代 Input and Interaction；
- 替代 Economy 或 Progression 的状态所有权；
- 决定完整商业模式；
- 自动保证所有数值平衡；
- 使用复杂规则制造不必要深度；
- 通过隐藏结算顺序增加难度；
- 让所有系统共享同一套具体数值公式；
- 代替技术层的物理引擎或数据库事务实现。

---

## 4. Governing Principles

### 4.1 Clarity and Feedback

参考：

- `../../philosophy/experience/clarity-and-feedback.md`

应用原则：

- 关键规则和结果必须可解释；
- 高影响结算应展示主要来源；
- 不可生效的效果应说明原因；
- 预览与实际结算使用同一规则来源。

### 4.2 Choice and Consequence

参考：

- `../../philosophy/experience/choice-and-consequence.md`

应用原则：

- 玩家选择必须真实影响结算；
- 风险和后果应在决策前可理解；
- 不能让隐藏优先级否定玩家判断。

### 4.3 Challenge and Fairness

参考：

- `../../philosophy/experience/challenge-and-fairness.md`

应用原则：

- 规则稳定；
- 输入可靠；
- 随机性可管理；
- 同类情况使用同类结算；
- 玩家有合理反制空间。

### 4.4 Simplicity and Depth

参考：

- `../../philosophy/experience/simplicity-and-depth.md`

应用原则：

- 规则数量应受控；
- 优先通过规则组合产生深度；
- 避免大量特例和隐藏例外；
- 结算顺序应统一而不是由内容单独决定。

### 4.5 Consistency and Coherence

参考：

- `../../philosophy/long-term/consistency-and-coherence.md`

应用原则：

- 相同术语具有相同语义；
- 相同效果遵循相同生命周期；
- 跨模式使用统一规则分类；
- 例外必须显式记录。

### 4.6 Accessibility and Inclusivity

参考：

- `../../philosophy/responsibility/accessibility-and-inclusivity.md`

应用原则：

- 结果不能只通过快速动画表达；
- 关键因果应支持日志或复盘；
- 实时压力不是理解规则的唯一方式；
- 辅助设置不应产生未说明的结算差异。

### 4.7 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 不使用隐藏概率或隐藏优先级误导玩家；
- 付费相关随机性必须透明；
- 不通过故意复杂化结算掩盖真实价值；
- 玩家资产相关结算必须可审计。

---

## 5. Player Experience

### 5.1 Player Goal

玩家希望：

- 执行一个意图；
- 预期可能结果；
- 理解实际结果；
- 根据结果调整；
- 相信规则稳定。

### 5.2 Entry

结算可能由以下行为触发：

- 输入操作；
- 技能或行动；
- 时间到达；
- 状态变化；
- 任务完成；
- 资源交易；
- 奖励领取；
- 成长升级；
- 系统事件；
- 外部平台确认。

### 5.3 Main Actions

玩家可能：

- 选择目标；
- 选择行动；
- 确认成本；
- 承担随机风险；
- 查看预览；
- 执行；
- 查看结果；
- 复盘原因。

### 5.4 Core Decisions

核心决策包括：

- 是否满足前置条件；
- 是否值得消耗资源；
- 是否接受概率风险；
- 效果组合的顺序；
- 是否在当前状态执行；
- 是否等待更有利时机。

### 5.5 Success

结算成功不仅意味着系统写入结果，还意味着：

- 结果符合规则；
- 结果只发生一次；
- 玩家能理解主要原因；
- 下游系统接收到一致结果；
- 数据可保存和审计；
- 必要反馈已经生成。

### 5.6 Failure

结算失败包括：

- 输入无效；
- 条件不满足；
- 目标失效；
- 状态冲突；
- 配置错误；
- 随机上下文缺失；
- 持久化失败；
- 重复事务；
- 版本不兼容。

### 5.7 Exit and Return

若结算进入 Pending，玩家应知道：

- 当前是否已扣除成本；
- 结果是否已确定；
- 是否可以离开；
- 系统如何恢复；
- 是否会重复执行。

---

## 6. System Boundary

### 6.1 Inputs

系统接收：

- Action Intent；
- Actor State；
- Target State；
- Environment State；
- Rule Set；
- Configuration Version；
- Difficulty Context；
- Modifier Set；
- Random Seed；
- Transaction ID；
- Timing Context；
- Platform or Mode Context。

### 6.2 Outputs

系统产生：

- Resolution Result；
- State Deltas；
- Applied Effects；
- Rejected Effects；
- Resource Deltas；
- Triggered Events；
- Random Rolls；
- Explanation Summary；
- Audit Record；
- Feedback Payload；
- Persistence Request。

### 6.3 Owned State

该系统拥有：

- Resolution Context；
- Resolution Transaction；
- Resolution Stage；
- Rule Evaluation Trace；
- Random Context；
- Applied Rule Version；
- Resolution Result Record；
- Trigger Queue；
- Recursion Guard State。

### 6.4 Read-Only Dependencies

系统读取：

- 角色状态；
- 资源余额；
- 任务状态；
- 内容配置；
- 难度参数；
- 权益状态；
- 环境状态；
- 时间状态。

### 6.5 Write Dependencies

系统不直接拥有所有领域状态。

它通过正式结果或命令请求：

- Character 应用属性变化；
- Economy 应用资源变化；
- Objectives 应用进度变化；
- Reward 创建奖励；
- Progression 更新成长；
- Save 持久化结果。

### 6.6 Out of Scope

该系统不直接：

- 处理支付；
- 拥有角色身份；
- 拥有资源余额；
- 拥有任务生命周期；
- 决定 UI 排版；
- 决定内容文案；
- 修改权限和权益。

---

## 7. Core Entities and Concepts

| Entity / Concept | Definition | Owner | Lifetime | Notes |
|---|---|---|---|---|
| Rule Definition | 稳定规则定义 | Rules and Resolution | 版本级 | 具有唯一 ID |
| Rule Set | 当前上下文启用的规则集合 | Rules and Resolution | 模式或版本级 | 可组合 |
| Resolution Context | 一次结算所需全部权威输入 | Rules and Resolution | 单次事务 | 不应在中途隐式变化 |
| Resolution Transaction | 一次权威结算事务 | Rules and Resolution | 单次事务 | 具有唯一 ID |
| Actor | 发起行为的实体 | Domain System | 领域生命周期 | 只读快照 |
| Target | 行为作用对象 | Domain System | 领域生命周期 | 可为空或多个 |
| Effect | 对状态产生变化的规则单元 | Rules and Resolution | 单次或持续 | 有类型和来源 |
| Modifier | 调整基础值或规则的因素 | Rules and Resolution | 上下文内 | 有优先级 |
| Condition | 判断规则是否适用 | Rules and Resolution | 评估期 | 不直接修改状态 |
| Trigger | 在特定事实发生后产生后续规则评估 | Rules and Resolution | 事务内或跨事务 | 必须有递归保护 |
| State Delta | 结算产生的状态变化描述 | Rules and Resolution | 事务内 | 由 Owner 应用 |
| Random Context | 随机种子、流和结果记录 | Rules and Resolution | 事务内 | 可重放 |
| Resolution Trace | 规则执行轨迹 | Rules and Resolution | 审计期 | 可裁剪展示 |
| Result Summary | 面向玩家和下游系统的结果摘要 | Rules and Resolution | 事务后 | 不等同完整 Trace |

---

## 8. Rule Taxonomy

规则应按职责分类。

### 8.1 Eligibility Rule

判断行动是否允许。

例如：

- 目标是否合法；
- 资源是否足够；
- 冷却是否结束；
- 权限是否满足；
- 当前状态是否允许。

### 8.2 Targeting Rule

决定：

- 可以作用于谁；
- 目标数量；
- 目标优先级；
- 无目标时行为；
- 目标失效时行为。

### 8.3 Cost Rule

定义：

- 消耗什么；
- 何时扣除；
- 失败是否返还；
- 部分成功如何处理。

### 8.4 Calculation Rule

计算基础结果。

### 8.5 Modifier Rule

修改基础结果。

### 8.6 Constraint Rule

应用：

- 上限；
- 下限；
- 免疫；
- 禁止；
- 唯一性；
- 容量。

### 8.7 Trigger Rule

根据已发生事实产生后续效果。

### 8.8 Lifecycle Rule

决定状态：

- 创建；
- 刷新；
- 叠加；
- 消耗；
- 过期；
- 移除。

### 8.9 Resolution Rule

定义最终结算顺序。

### 8.10 Presentation Rule

只影响：

- 显示；
- 文案；
- 汇总；
- 反馈优先级。

Presentation Rule 不应改变业务事实。

---

## 9. Rule Hierarchy

推荐规则层级：

```text
1. Invariant
2. Permission and Safety
3. Ownership and Entitlement
4. Mode Rule
5. Core Rule
6. Content Rule
7. Actor and Target Modifier
8. Temporary Effect
9. Presentation Rule
```

### 9.1 Invariant

任何情况下不可被覆盖。

例如：

- 同一奖励不可重复领取；
- 资源余额不得出现未允许负值；
- 无权限内容不可被使用；
- 已确认购买不得重复授予。

### 9.2 Permission and Safety

包括：

- 隐私；
- 儿童保护；
- 账户限制；
- 平台限制；
- 安全规则。

### 9.3 Ownership and Entitlement

决定实体和内容是否可用。

### 9.4 Mode Rule

例如：

- 竞技；
- 单人；
- 教学；
- 活动；
- 辅助模式。

### 9.5 Core Rule

系统普遍适用的基础规则。

### 9.6 Content Rule

具体技能、任务、装备和事件规则。

### 9.7 Modifier

来自实体、环境、状态和难度的调整。

### 9.8 Temporary Effect

短期状态、增益、减益和活动效果。

### 9.9 Presentation Rule

不参与权威结算。

---

## 10. Resolution Context

一次结算开始前，应创建不可隐式变化的 Resolution Context。

推荐字段：

```markdown
| Field | Meaning |
|---|---|
| Resolution ID | 事务唯一标识 |
| Rule Version | 规则版本 |
| Configuration Version | 配置版本 |
| Actor Snapshot | 发起者权威快照 |
| Target Snapshot | 目标权威快照 |
| Environment Snapshot | 环境状态 |
| Mode Context | 当前模式 |
| Difficulty Context | 当前难度 |
| Time Context | 权威时间 |
| Random Seed | 随机种子 |
| Correlation ID | 跨系统追踪 |
| Idempotency Key | 防重复业务键 |
```

### 10.1 Snapshot 原则

结算过程中，不应从多个系统反复读取可能变化的状态。

应优先：

```text
验证并冻结必要输入
→ 创建 Context
→ 完成结算
```

### 10.2 Context 失效

若权威状态在执行前已变化，应：

- 拒绝；
- 重新创建 Context；
- 或进入明确冲突处理。

不能静默混合不同时间点状态。

---

## 11. Resolution Pipeline

推荐统一结算管线：

```text
1. Receive Intent
2. Normalize Input
3. Validate Context
4. Resolve Targets
5. Validate Eligibility
6. Reserve Costs
7. Build Base Effects
8. Apply Modifiers
9. Resolve Randomness
10. Apply Constraints
11. Resolve Triggers
12. Produce State Deltas
13. Validate Final Invariants
14. Commit Authoritative Result
15. Publish Events
16. Generate Feedback
17. Persist Trace
```

---

## 12. Stage 1: Receive and Normalize Intent

### 12.1 Normalize

统一：

- 输入别名；
- 平台差异；
- 默认目标；
- 默认参数；
- 单位；
- 顺序。

### 12.2 Intent Validation

检查：

- 类型；
- 版本；
- 权限；
- 来源；
- 重复；
- 过期；
- 签名或可信来源。

### 12.3 不应由 UI 提供权威结果

UI 可以提交：

```text
玩家想做什么。
```

但不能提交：

```text
最终应造成多少结果。
```

---

## 13. Stage 2: Resolve Targets

### 13.1 Target Types

- Self；
- Single；
- Multiple；
- Area；
- Random；
- All；
- Lowest / Highest；
- Contextual；
- No Target。

### 13.2 Target Validation

检查：

- 是否存在；
- 是否可见；
- 是否可达；
- 是否有效；
- 是否免疫；
- 是否已离开；
- 是否满足阵营或类型要求。

### 13.3 Target Lost

目标失效时可采用：

- Cancel；
- Retarget；
- Resolve Without Target；
- Partial Resolve；
- Convert to Self；
- Enter Pending。

具体行为必须由规则明确定义。

### 13.4 自动选目标

自动目标选择应：

- 使用稳定优先级；
- 可被解释；
- 不依赖隐藏信息；
- 在竞技中保持公平。

---

## 14. Stage 3: Eligibility and Cost

### 14.1 Eligibility

检查：

- 当前状态；
- 冷却；
- 资源；
- 权益；
- 目标；
- 时间；
- 次数；
- 模式；
- 容量；
- 内容版本。

### 14.2 Cost Timing

成本可以在：

- Commit 时扣除；
- Resolution 成功时扣除；
- 分阶段扣除；
- 失败后返还。

必须明确：

```text
何时视为已消费。
```

### 14.3 Reservation

高风险跨系统成本应先：

```text
Reserve
→ Resolve
→ Commit or Release
```

### 14.4 Failure Refund

应区分：

- 玩家主动取消；
- 输入无效；
- 系统故障；
- 行动失败；
- 部分成功。

不同原因不一定使用相同返还规则。

---

## 15. Stage 4: Build Base Effects

Base Effect 描述未经过外部修正的基础结果。

例如：

- 基础伤害；
- 基础治疗；
- 基础资源变化；
- 基础进度；
- 基础持续时间；
- 基础概率；
- 基础奖励。

### 15.1 Base Value 来源

必须来自明确来源：

- 内容定义；
- 规则公式；
- Actor 属性；
- Target 属性；
- 环境；
- 难度。

### 15.2 不在多个系统重复计算

例如：

```text
UI 不应单独实现一套伤害公式；
Analytics 不应重新推导权威奖励；
客户端预览应引用同一规则模型。
```

---

## 16. Modifier Model

Modifier 应具有统一结构：

```markdown
| Field | Meaning |
|---|---|
| Modifier ID | 唯一标识 |
| Source | 来源 |
| Target Property | 修改对象 |
| Operation | Add / Multiply / Override / Clamp |
| Value | 数值 |
| Priority | 优先级 |
| Condition | 生效条件 |
| Duration | 生命周期 |
| Stack Group | 叠加组 |
| Tags | 分类标签 |
```

### 16.1 Operation Types

#### Add

加法修正。

#### Multiply

乘法修正。

#### Override

覆盖最终或中间值。

#### Clamp

应用上下限。

#### Transform

改变规则语义，应谨慎使用。

### 16.2 推荐顺序

通用数值计算可采用：

```text
Base
→ Flat Additions
→ Additive Percent
→ Multiplicative Modifiers
→ Override
→ Clamp
→ Rounding
```

具体项目可以调整，但必须统一记录。

### 16.3 不要依赖添加顺序

除非明确设计，否则 Modifier 不应因为内容加载顺序不同而改变结果。

---

## 17. Stacking Rules

### 17.1 Stack Types

- Independent；
- Additive；
- Multiplicative；
- Highest Only；
- Lowest Only；
- Replace；
- Refresh Duration；
- Extend Duration；
- Limited Stack；
- Unique by Source；
- Unique by Category。

### 17.2 Stack Group

同类效果应使用明确 Stack Group。

### 17.3 Stack Cap

需要定义：

- 最大层数；
- 最大总值；
- 超出后行为；
- 是否刷新；
- 是否忽略；
- 是否转换。

### 17.4 同名不等于同组

应根据规则语义，而不是显示名称判断叠加。

### 17.5 叠加反馈

玩家应能理解：

- 当前层数；
- 剩余时间；
- 来源；
- 是否达到上限；
- 新效果是否生效。

---

## 18. Priority and Overrides

### 18.1 Priority 的用途

用于处理：

- 同时覆盖；
- 免疫；
- 强制规则；
- 模式规则；
- 状态冲突。

### 18.2 Priority 不应被滥用

大量内容自定义 Priority 会导致：

- 隐藏顺序；
- 调试困难；
- 内容耦合；
- 不可预测。

### 18.3 Override 规则

Override 必须说明：

- 覆盖什么；
- 覆盖范围；
- 是否可被更高层覆盖；
- 生命周期；
- 原因。

### 18.4 不变量不可被普通 Override 改写

---

## 19. Simultaneous Events

“同时发生”必须转化为可执行顺序。

### 19.1 常见情境

- 双方同时击败；
- 多个状态同时过期；
- 同时完成多个任务；
- 同时触发多个被动；
- 资源达到上限同时获得奖励；
- 活动结束同时提交结果。

### 19.2 推荐处理模型

```text
Collect
→ Group by Resolution Window
→ Sort by Priority
→ Resolve
→ Validate
→ Commit
```

### 19.3 Resolution Window

定义哪些事件被视为同一结算窗口。

### 19.4 Tie Breaker

稳定 Tie Breaker 可以是：

- 明确规则优先级；
- 行动速度；
- 创建顺序；
- 实体稳定 ID；
- 权威随机种子。

不应依赖：

- 未定义容器顺序；
- 网络到达顺序；
- 客户端帧率；
- 数据库返回顺序。

### 19.5 真正同时

如果设计要求真正同时，应：

- 先计算所有结果；
- 再统一提交；
- 避免前一个结果改变后一个的输入。

---

## 20. Trigger System

### 20.1 Trigger Structure

```text
When [Fact]
If [Condition]
Then [Effect]
Within [Scope]
With [Priority]
```

### 20.2 Trigger Facts

例如：

- ActionStarted；
- DamageApplied；
- ResourceSpent；
- QuestCompleted；
- StateExpired；
- TurnEnded。

### 20.3 Trigger 不应监听模糊 UI 行为

例如：

```text
ButtonClicked
```

通常不是领域事实。

### 20.4 Trigger Queue

触发器进入统一队列，并记录：

- Source；
- Cause；
- Depth；
- Priority；
- Already Processed。

### 20.5 Trigger Timing

必须明确：

- Before；
- During；
- After；
- On Commit；
- On Presentation。

---

## 21. Recursion and Infinite Loop Protection

触发系统容易形成循环。

例如：

```text
获得治疗
→ 触发奖励
→ 奖励再次治疗
→ 再次触发奖励
```

### 21.1 保护方式

- Maximum Trigger Depth；
- Same Trigger Once per Resolution；
- Once per Entity；
- Once per Source；
- Cooldown；
- Trigger Budget；
- Cycle Detection；
- Explicit Non-Reentrant Tag。

### 21.2 达到上限

系统应：

- 停止后续触发；
- 保留已确认结果；
- 记录异常；
- 产生调试信息；
- 不让玩家遭受重复扣除。

### 21.3 保护规则不是正常平衡手段

若正常玩法频繁触发递归上限，说明规则设计需要调整。

---

## 22. Randomness

### 22.1 Random Types

#### Input Randomness

先生成情境，再让玩家应对。

#### Output Randomness

行动后随机决定结果。

#### Content Randomness

生成内容、地图、敌人或奖励。

#### Cosmetic Randomness

只影响表现。

### 22.2 Random Context

每次权威随机结算应记录：

- Seed；
- Random Stream；
- Roll Index；
- Probability；
- Result；
- Rule Version。

### 22.3 独立随机流

不同系统应考虑独立随机流，避免：

```text
一个无关视觉随机
改变后续关键结果。
```

### 22.4 概率表达

玩家可见概率应与实际结算一致。

若经过：

- 保底；
- 权重；
- 动态概率；
- 去重；
- 条件调整；

必须说明实际规则。

### 22.5 Random Protection

可能包括：

- Pity；
- Bad Luck Protection；
- Guaranteed Range；
- Reroll；
- Choice after Roll；
- Duplicate Protection；
- Non-Random Alternative。

---

## 23. Determinism and Replay

### 23.1 Deterministic Resolution

相同：

- 规则版本；
- 输入；
- 状态快照；
- 随机种子；

应尽量产生相同结果。

### 23.2 用途

- 回放；
- 调试；
- QA；
- 服务端验证；
- 争议审计；
- 自动测试；
- 模拟。

### 23.3 非确定因素

需要隔离：

- 本地时间；
- 帧率；
- 线程调度；
- 容器顺序；
- 网络顺序；
- 浮点平台差异。

### 23.4 Replay Record

至少保存：

- Intent；
- Context Version；
- Seed；
- Rule Version；
- Key State；
- Result Hash。

---

## 24. Numeric Precision

### 24.1 Number Types

根据用途选择：

- Integer；
- Fixed Point；
- Decimal；
- Floating Point。

### 24.2 高风险数值

以下通常不应依赖不稳定浮点累积：

- 货币；
- 价格；
- 权益；
- 高价值资源；
- 排名分数；
- 长期成长。

### 24.3 Unit

每个数值应明确单位。

例如：

- milliseconds；
- seconds；
- percentage points；
- ratio；
- cents；
- resource units。

### 24.4 Percentage Ambiguity

必须区分：

```text
增加 10 个百分点
vs
在原值基础上增加 10%
```

---

## 25. Rounding

### 25.1 Rounding Stage

必须明确在哪一步舍入。

例如：

```text
Base
→ Modifiers
→ Clamp
→ Round
→ Commit
```

### 25.2 Rounding Modes

- Floor；
- Ceil；
- Half Up；
- Half Even；
- Toward Zero；
- Away from Zero。

### 25.3 Display vs Authority

显示值和权威值可能精度不同。

必须避免：

```text
界面显示可以支付，
实际结算却因隐藏小数拒绝。
```

### 25.4 Accumulation

长期累计时，应避免每一步重复舍入导致系统偏差。

---

## 26. Caps, Floors, and Bounds

### 26.1 Hard Cap

不可突破。

### 26.2 Soft Cap

超过后收益递减。

### 26.3 Display Cap

只影响显示，不影响权威状态。

### 26.4 Capacity Cap

资源或容器容量。

### 26.5 Overflow

超出上限时必须定义：

- 丢弃；
- 邮件；
- 转换；
- 延迟；
- 拒绝；
- 自动扩展。

不应静默丢失高价值资产。

### 26.6 Underflow

低于下限时应：

- Clamp；
- 拒绝；
- 允许负值；
- 进入债务状态。

---

## 27. Status and Effect Lifecycle

状态生命周期建议统一：

```text
Created
→ Active
→ Refreshed or Stacked
→ Consumed or Expired
→ Removed
```

### 27.1 Created

记录：

- 来源；
- 目标；
- 开始时间；
- 持续时间；
- 层数；
-规则版本。

### 27.2 Active

参与规则计算。

### 27.3 Refresh

可能：

- 重置持续时间；
- 增加持续时间；
- 不改变持续时间；
- 重算数值；
- 保留原始快照。

### 27.4 Consume

由特定事件消耗。

### 27.5 Expire

时间或阶段结束。

### 27.6 Remove

被解除、覆盖或目标销毁。

### 27.7 Snapshot vs Dynamic

状态数值应明确：

- 创建时锁定；
- 每次结算动态读取；
- 部分锁定。

---

## 28. Cancellation and Interruption

### 28.1 Cancel Before Commit

通常可以：

- 不扣成本；
- 不产生结果；
- 返回原状态。

### 28.2 Interrupt During Action

必须定义：

- 是否保留成本；
- 是否产生部分结果；
- 是否进入冷却；
- 是否触发被动；
- 是否可重试。

### 28.3 Cancel After Commit

通常不能简单撤销。

应使用：

- 正式回滚；
- 补偿；
- 新事务。

### 28.4 Interrupt Priority

例如：

```text
Safety / Death
→ Forced State
→ Manual Cancel
→ Normal Completion
```

具体项目应显式定义。

---

## 29. Validation Failure

规则验证失败应使用稳定分类。

### 29.1 Invalid Intent

请求格式或类型错误。

### 29.2 Unauthorized

无权限。

### 29.3 Invalid State

当前状态不允许。

### 29.4 Invalid Target

目标无效。

### 29.5 Insufficient Resource

资源不足。

### 29.6 Expired

内容或请求过期。

### 29.7 Conflict

状态版本冲突。

### 29.8 Duplicate

事务已处理。

### 29.9 Configuration Error

规则或配置无效。

### 29.10 System Failure

内部系统无法完成。

面向玩家的反馈不应直接展示技术错误码。

---

## 30. Final Invariant Validation

提交前重新检查：

- 状态所有权；
- 资源上下限；
- 唯一性；
- 权益；
- 权限；
- 容量；
- 事务重复；
- 数据版本；
- 结果完整性。

若最终不变量失败：

- 不提交部分结果；
- 或进入明确补偿路径；
- 记录完整 Trace；
- 停止后续下游事件。

---

## 31. Commit Model

### 31.1 Atomic Where Possible

单领域状态尽量原子提交。

### 31.2 Cross-System Commit

跨系统使用：

- Transaction ID；
- Reservation；
- Idempotency；
- Outbox or Equivalent Intent；
- Compensation；
- Audit。

### 31.3 Commit Before Event

权威状态成功后再发布：

```text
StateChanged
```

避免发布事实后写入失败。

### 31.4 UI Feedback

高风险结果应在权威提交成功后显示最终成功。

---

## 32. Resolution Result

标准结果结构建议包括：

```markdown
## Resolution Result

- Resolution ID:
- Status:
- Actor:
- Targets:
- Rule Version:
- Configuration Version:
- Costs:
- Applied Effects:
- Rejected Effects:
- State Deltas:
- Triggered Events:
- Random Rolls:
- Primary Cause:
- Warnings:
- Pending Operations:
- Result Hash:
```

### 32.1 Status

- Succeeded；
- Failed；
- Partially Succeeded；
- Pending；
- Cancelled；
- Duplicate。

### 32.2 Applied Effects

记录实际生效效果。

### 32.3 Rejected Effects

记录未生效及原因。

### 32.4 Pending Operations

记录尚待其他系统确认的操作。

---

## 33. Explainability

### 33.1 Explanation Levels

#### Player Summary

只显示关键原因。

#### Advanced Details

显示：

- 来源；
- 修正；
- 随机；
- 抵抗；
- 上限。

#### Support Trace

用于客服与争议处理。

#### Developer Trace

完整规则执行轨迹。

### 33.2 Primary Cause

系统应尝试识别：

- 结果最主要来源；
- 失败关键因素；
- 被阻止效果；
- 决定性随机结果。

### 33.3 不暴露不必要内部细节

解释不等于展示所有内部实现。

应优先展示能支持：

- 理解；
- 学习；
- 信任；
- 复盘；

的信息。

---

## 34. Preview and Simulation

### 34.1 Preview

在执行前显示预计结果。

### 34.2 Shared Rule Source

Preview 应与权威结算共享：

- 规则；
- 公式；
- 配置；
- 上下限。

### 34.3 Uncertain Results

随机或隐藏信息存在时，可显示：

- 范围；
- 概率；
- 最差与最好；
- 未知因素；
- 主要风险。

### 34.4 Preview 不是承诺

若结果可能变化，应明确原因。

但不能频繁出现：

```text
预览与实际完全不同
```

否则会破坏信任。

---

## 35. Domain Examples

### 35.1 Damage or Impact

通用结构：

```text
Base Impact
→ Source Modifiers
→ Target Modifiers
→ Environment
→ Critical or Random
→ Mitigation
→ Clamp
→ Rounding
→ Apply
```

### 35.2 Healing or Restoration

需定义：

- 是否允许超量；
- 是否转化护盾；
- 是否受减疗；
- 是否触发事件；
- 已满时行为。

### 35.3 Resource Transaction

```text
Validate Balance
→ Reserve
→ Apply Modifiers
→ Clamp
→ Commit
→ Publish
```

### 35.4 Quest Progress

```text
Receive Domain Fact
→ Validate Objective
→ Calculate Progress
→ Apply Cap
→ Check Completion
→ Publish Completion
```

### 35.5 Reward Roll

```text
Validate Reward Table
→ Select Random Stream
→ Apply Eligibility
→ Roll
→ Apply Protection
→ Resolve Duplicates
→ Create Reward Instance
```

---

## 36. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| Input and Interaction | Hard | Input → Rules | Action Intent | 无法开始结算 |
| Game State and Flow | Hard | State → Rules | Mode Context | 使用错误规则集 |
| Core Loop | Hard | 双向 | Activity Result | 循环无法推进 |
| Configuration | Hard | Config → Rules | Rule Version | 无法计算 |
| Characters and Loadouts | Read / Write | 双向契约 | Actor State / Delta | 实体状态错误 |
| Resources and Economy | Write | Rules → Economy | Resource Delta | 资产风险 |
| Objectives and Quests | Event / Write | Rules → Objectives | Progress Fact | 任务不一致 |
| Reward System | Event | Rules → Reward | Reward Eligibility | 奖励错误 |
| Progression | Read / Write | 双向契约 | Growth State | 成长错误 |
| Difficulty | Read | Difficulty → Rules | Challenge Modifier | 难度异常 |
| Save and Persistence | Hard | Rules → Save | Result Record | 无法恢复 |
| Analytics | Soft | Rules → Analytics | Resolution Summary | 不阻断 |
| UI and Feedback | Soft | Rules → UI | Explanation Payload | 结果不清楚 |

---

## 37. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Resolution ID | 是 | Rules | 事务创建 | 审计期 | 幂等查询 |
| Rule Version | 是 | Rules | 事务创建 | 长期 | 重放 |
| Configuration Version | 是 | Configuration | 事务创建 | 长期 | 重放 |
| Random Seed | 按风险 | Rules | 随机开始 | 审计期 | 重放 |
| Result Record | 是 | Rules | Commit | 长期或业务期 | 查询 |
| State Deltas | 是 | Domain Owners | Commit | 领域生命周期 | 对账 |
| Resolution Trace | 按风险 | Rules | 完成 | 可裁剪 | 调试 |
| Result Hash | 是 | Rules | 完成 | 审计期 | 一致性检查 |

### 37.1 高风险长期保留

建议长期保留：

- 购买；
- 权益；
- 高价值奖励；
- 排名；
- 迁移；
- 补偿；
- 争议结算。

### 37.2 Trace 裁剪

完整 Trace 可能很大。

应区分：

- 常规摘要；
- 错误 Trace；
- 高风险审计；
- 开发调试。

---

## 38. Accessibility

### 38.1 Visual

- 关键结果有文本或图标说明；
- 不只依赖瞬间数字飘字；
- 状态免疫、抵抗和无效原因可见；
- 重要随机结果可回看。

### 38.2 Audio

- 结算结果不只依赖声音；
- 声音差异有视觉替代；
- 音效不改变权威时机。

### 38.3 Input

- 输入辅助不会产生未说明的规则优势或劣势；
- 自动化行为使用同一权威规则；
- 重复输入不会重复结算。

### 38.4 Cognitive

- 提供简要结果；
- 高级详情按需展开；
- 术语一致；
- 复杂结算支持日志；
- 失败原因突出。

### 38.5 Timing

- 快速结算可暂停回看；
- 实时模式中的重要结果可在事后复盘；
- 辅助减速不改变权威规则，除非明确属于模式规则。

---

## 39. Ethical and Safety Review

### 39.1 Randomness Transparency

付费或高价值随机必须说明：

- 实际概率；
- 保底；
- 权重；
- 去重；
- 条件变化；
- 非随机替代路径。

### 39.2 Asset Safety

资源、奖励和权益结算必须：

- 幂等；
- 可审计；
- 可恢复；
- 不静默丢失；
- 不依赖客户端自报结果。

### 39.3 Hidden Rules

不能通过隐藏：

- 优先级；
- 概率；
- 上限；
- 失败成本；

制造误导。

### 39.4 Dynamic Difficulty

如果难度影响结算，应：

- 明确适用范围；
- 不偷偷削弱玩家已承诺的结果；
- 不根据付费状态制造不公平；
- 支持验证和关闭。

### 39.5 Children and Vulnerable Users

- 不使用难以理解的随机付费结算；
- 不在失败后修改概率推动消费；
- 不用虚假预览；
- 不隐藏自动续费和权益规则。

---

## 40. Analytics and Validation

### 40.1 Key Assumptions

1. 规则顺序能够被团队和玩家理解。
2. 预览与实际结果足够一致。
3. 同类效果使用统一叠加逻辑。
4. 随机结果可以重放和审计。
5. 高影响事务不会重复结算。
6. 复杂触发不会形成无限循环。
7. 玩家能识别主要成功和失败原因。
8. 跨平台和不同性能环境产生一致业务结果。

### 40.2 Validation Plan

| Hypothesis | Evidence | Success | Failure | Method |
|---|---|---|---|---|
| 规则可理解 | 复述与预测 | 玩家能预测主要结果 | 频繁认为系统任意 | 可用性测试 |
| Preview 一致 | 预览误差 | 差异在明确范围 | 经常出现反向结果 | 自动对比 |
| 叠加统一 | 规则测试 | 同类效果稳定 | 内容顺序改变结果 | 单元与组合测试 |
| 随机可重放 | Replay | 同 Seed 同结果 | 无法复现 | 自动测试 |
| 幂等成立 | 重复请求 | 只产生一次结果 | 重复扣除或发放 | 故障注入 |
| Trigger 安全 | 压力测试 | 无无限循环 | 达到异常深度 | 模拟 |
| 因果清楚 | 回顾测试 | 能说明关键原因 | 只归因 Bug | 访谈 |
| 跨平台一致 | 结果 Hash | 权威结果一致 | 平台差异 | 跨平台测试 |

### 40.3 Behavioral Metrics

- Action Validation Failure；
- Resolution Success；
- Pending；
- Duplicate；
- Retry；
- Cancel；
- Preview Usage；
- Detail Expansion；
- Replay Request；
- Trigger Depth；
- Random Protection Activation。

### 40.4 Outcome Metrics

- 预览准确率；
- 结果解释成功率；
- 结算一致性；
- 重复事务率；
- Pending 恢复率；
- 跨平台结果一致率；
- 争议率；
- 客服修复率。

### 40.5 Negative Metrics

- 重复扣除；
- 重复发放；
- 负余额；
- 结算卡死；
- 无限触发；
- 预览误导；
- 随机概率不一致；
- 状态层数错误；
- 事件顺序依赖；
- 浮点平台差异；
- 玩家无法理解失败；
- UI 与权威结果不一致。

### 40.6 Event Intents

| Event Intent | Trigger | Key Properties | Privacy Notes |
|---|---|---|---|
| Resolution Started | Context 创建 | Rule Version, Action Type | 不记录不必要身份 |
| Validation Failed | 前置失败 | Failure Category | 错误字段脱敏 |
| Resolution Completed | Commit 完成 | Result Status, Duration | 使用实体匿名 ID |
| Random Resolved | 高风险随机 | Probability, Protection | 不记录支付敏感数据 |
| Trigger Limit Reached | 递归保护 | Trigger Type, Depth | 调试用途 |
| Preview Compared | 实际完成 | Difference Category | 不记录自由文本 |
| Duplicate Prevented | 幂等命中 | Transaction Type | 审计 |
| Resolution Recovered | Pending 恢复 | Recovery Type | 追踪事务 |

---

## 41. Test Strategy

### 41.1 Unit Tests

覆盖：

- 单规则；
- 单 Modifier；
- 边界；
- 舍入；
- 上限；
- 条件；
- 失败分类。

### 41.2 Combination Tests

覆盖：

- 多 Modifier；
- 多状态；
- 多 Trigger；
- 同时事件；
- 覆盖规则；
- 随机保护。

### 41.3 Property-Based Tests

验证不变量：

- 余额不非法；
- 同一事务不重复；
- 上限不突破；
- 同 Seed 可重放；
- 无无限递归。

### 41.4 Golden Tests

固定输入和版本对应固定结果。

### 41.5 Simulation

用于：

- 平衡分布；
- 长期经济；
- 极端组合；
- 随机概率；
- Trigger 压力。

### 41.6 Fault Injection

模拟：

- Save 失败；
- Economy 超时；
- 重复请求；
- 事件乱序；
- 配置缺失；
- 版本冲突。

---

## 42. Rollout and Migration

### 42.1 Rollout

可按：

- 内部；
- 测试环境；
- 玩法；
- 平台；
- 玩家分群；
- 全量；

逐步发布。

### 42.2 Rule Version

新规则必须具有新版本。

已有进行中实例应明确：

- 继续旧规则；
- 迁移；
- 重新开始；
- 补偿。

### 42.3 Configuration Migration

检查：

- 字段；
- 单位；
- 默认值；
- 上下限；
- 引用；
- 随机表；
- 状态映射。

### 42.4 Save Migration

旧状态需要映射：

- Stack；
- Duration；
- Source；
- Rule Version；
- Pending Transaction。

### 42.5 Rollback

回滚时：

- 已提交结果不自动撤销；
- Pending 使用原 Rule Version 恢复；
- 新状态映射回旧状态；
- 保留审计；
- 必要时发放补偿。

### 42.6 Stop Conditions

出现以下情况应停止发布：

- 重复发放或扣除；
- 负余额；
- 大量 Resolution Pending；
- Preview 与实际严重不一致；
- 跨平台结果差异；
- 无限触发；
- 状态无法迁移；
- 高价值随机概率错误；
- 权益或权限被绕过。

---

## 43. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| 规则系统过度通用化 | Architecture Risk | 高 | 中 | 保持领域规则边界 | Engineering |
| Priority 数量膨胀 | Complexity Risk | 高 | 中 | 固定层级、限制自定义 | Design |
| 内容特例过多 | Maintenance Risk | 高 | 高 | 例外审计与淘汰 | Design |
| Preview 与实际分叉 | Trust Risk | 高 | 中 | 共享规则源 | Engineering |
| 浮点跨平台差异 | Technical Risk | 高 | 低 | Fixed Point / Hash Test | Engineering |
| Trigger 无限循环 | System Risk | 高 | 中 | 深度与循环保护 | Engineering |
| Trace 数据过大 | Operations Risk | 中 | 中 | 分级保留 | Data |
| 动态难度隐藏影响 | Ethical Risk | 高 | 低 | 透明范围与审计 | Product |
| 旧实例规则迁移 | Migration Risk | 高 | 中 | 规则版本锁定 | Engineering |

---

## 44. Review Checklist

### Purpose and Boundary

- [ ] 规则系统目的明确；
- [ ] 不直接拥有领域状态；
- [ ] Non-Goals 已定义；
- [ ] 规则与表现职责分离。

### Rule Model

- [ ] Rule Taxonomy 完整；
- [ ] Invariant 与普通规则分离；
- [ ] Modifier 操作和顺序明确；
- [ ] Stack Group 和 Cap 明确；
- [ ] Override 不可绕过不变量。

### Resolution Pipeline

- [ ] Context 在结算前冻结；
- [ ] Target、Eligibility、Cost 顺序明确；
- [ ] 随机性有独立上下文；
- [ ] Final Invariant 在 Commit 前检查；
- [ ] Commit 后再发布事实事件。

### Simultaneous and Trigger

- [ ] Resolution Window 明确；
- [ ] Tie Breaker 稳定；
- [ ] 不依赖容器或网络顺序；
- [ ] Trigger Timing 明确；
- [ ] 递归和循环保护完整。

### Numeric

- [ ] 单位明确；
- [ ] 百分比语义明确；
- [ ] 舍入阶段和模式明确；
- [ ] 上下限和溢出处理明确；
- [ ] 高风险资产不依赖不稳定浮点。

### Randomness

- [ ] Seed、Stream 和 Roll 可记录；
- [ ] 同 Seed 可以重放；
- [ ] 概率与实际一致；
- [ ] 保底和去重规则明确；
- [ ] 付费随机透明。

### Result and Explainability

- [ ] Applied 与 Rejected Effects 可记录；
- [ ] Primary Cause 可提取；
- [ ] Player Summary 与 Developer Trace 分层；
- [ ] Preview 使用相同规则源；
- [ ] Pending 和 Duplicate 有明确状态。

### Integration

- [ ] State Delta 由领域 Owner 应用；
- [ ] 跨系统事务具有幂等和补偿；
- [ ] Save、Economy、Reward、Progression 边界明确；
- [ ] Analytics 不影响业务结果。

### Validation

- [ ] Unit、Combination、Property、Golden Tests 已规划；
- [ ] Fault Injection 已规划；
- [ ] 跨平台一致性可检查；
- [ ] 负面指标和停止条件明确。

---

## 45. V1 Completion Criteria

Rules and Resolution 可以被视为 V1，当：

- 规则分类和优先级层级已经定义；
- Invariant、Permission、Mode、Core、Content、Modifier 和 Presentation Rule 得到区分；
- Resolution Context 字段完整；
- 统一结算管线已经确定；
- Target、Eligibility、Cost、Base Effect、Modifier、Random、Constraint、Trigger 和 Commit 顺序清楚；
- Modifier 操作、叠加、覆盖和优先级规则统一；
- 同时事件具有 Resolution Window 和稳定 Tie Breaker；
- Trigger 具有 Timing、Queue、Depth 和循环保护；
- 随机性具有 Seed、Stream、Roll 和重放能力；
- 数值精度、单位、舍入、上限与溢出规则明确；
- 状态生命周期、刷新、叠加、消耗和过期规则明确；
- Cancel、Interrupt、Pending、Duplicate 和 Partial Success 有标准语义；
- Final Invariant 和 Commit Model 已定义；
- Resolution Result、Trace 和玩家解释分层明确；
- Preview 与权威结算共享规则来源；
- 跨系统状态变化由领域 Owner 应用；
- 高价值事务支持幂等、审计和恢复；
- 可访问性、伦理、随机透明和资产安全通过评审；
- 测试、灰度、版本、迁移和回滚方案完整；
- 下游战斗、任务、经济、奖励和成长系统可以直接引用本文件。

---

## 46. Related Documents

### Philosophy

- [Clarity and Feedback](../../philosophy/experience/clarity-and-feedback.md)
- [Simplicity and Depth](../../philosophy/experience/simplicity-and-depth.md)
- [Choice and Consequence](../../philosophy/experience/choice-and-consequence.md)
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
- `input-and-interaction.md`
- `../progression/resources-and-economy.md`
- `../progression/reward-system.md`
- `../progression/progression-system.md`
- `../progression/difficulty-and-challenge.md`
- `../content/objectives-and-quests.md`
