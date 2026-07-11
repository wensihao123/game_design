# Difficulty and Challenge（难度与挑战系统）

> Status: V1  
> Category: Progression  
> Path: `design/systems/progression/difficulty-and-challenge.md`  
> Owner: TBD  
> Reviewers: Design / Product / Engineering / UX / QA / Research / Accessibility / Data  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: High  
> Dependencies: Core Loop, Rules and Resolution, Progression System, Characters and Loadouts, Content and Unlocks, Input and Interaction, Settings and Preferences  
> Affected Systems: Objectives and Quests, Reward System, Tutorial and Onboarding, Matchmaking and Competition, Save and Persistence, Analytics and Telemetry, Live Operations

---

## 1. System Summary

Difficulty and Challenge 系统负责定义：

```text
玩家面对什么挑战；
挑战由哪些维度构成；
挑战如何随时间和成长变化；
失败意味着什么；
系统如何保持公平、可理解和可恢复；
辅助、难度模式和动态调整如何生效；
不同玩家如何获得适合自己的挑战；
长期如何避免无聊、挫败、数值膨胀和唯一解。
```

难度不是单一数值。

它可以来自：

- 规则复杂度；
- 信息不完整；
- 资源限制；
- 时间压力；
- 执行要求；
- 判断质量；
- 敌人行为；
- 状态管理；
- 构筑要求；
- 社交协调；
- 风险与惩罚。

健康的挑战系统应让玩家感受到：

```text
失败是可以理解的；
成功来自可学习的判断或执行；
成长改变我面对挑战的方式；
辅助降低障碍，但不否定成就；
难度选择尊重我的目标和状态。
```

---

## 2. Purpose

### 2.1 Player Value

该系统帮助玩家：

- 选择合适挑战；
- 理解难度来源；
- 在失败后识别改进方向；
- 感知掌握和成长；
- 根据能力、时间和身体状态调整体验；
- 在不被羞辱的情况下使用辅助；
- 在长期体验中持续遇到新的判断和变化；
- 相信系统不会隐藏改变规则。

### 2.2 Experience Contribution

挑战系统可以支持：

- 紧张；
- 掌握；
- 策略；
- 试错；
- 风险；
- 成就；
- 社交协作；
- 长期目标。

但不健康的难度会造成：

- 数值墙；
- 失败原因不明；
- 重复劳动；
- 输入障碍；
- 唯一构筑；
- 动态难度欺骗；
- 失败后高压付费；
- 新旧玩家巨大断层；
- 辅助被污名化。

### 2.3 Product Value

该系统为以下能力提供共同基础：

- 关卡与战斗；
- 任务；
- 成长；
- 奖励；
- 难度模式；
- 新手引导；
- 可访问性；
- 匹配；
- 排名；
- 活动；
- 长期留存；
- 数据分群。

### 2.4 Why This System Exists

如果难度由各内容单独定义，常见结果是：

```text
不同关卡使用不同规则；
推荐强度与实际挑战无关；
失败惩罚彼此冲突；
辅助设置只影响表现不影响障碍；
动态难度隐藏修改结果；
难度只能通过提高数值增长；
挑战越来越依赖唯一构筑。
```

统一难度系统用于确保：

- 难度来源可分解；
- 挑战可学习；
- 失败可恢复；
- 辅助和模式规则一致；
- 成长与挑战形成健康闭环；
- 竞技和单人边界清楚。

---

## 3. Non-Goals

该系统不负责：

- 定义全部具体敌人和关卡内容；
- 直接拥有角色能力；
- 替代 Rules and Resolution；
- 用难度替代内容变化；
- 通过高惩罚制造虚假深度；
- 让所有玩家经历同一强度；
- 将辅助功能视为作弊；
- 自动保证所有构筑平衡；
- 通过隐藏缩放抵消玩家成长；
- 让失败成为付费入口；
- 将“更耗时”直接等同于“更困难”。

---

## 4. Governing Principles

### 4.1 Challenge and Fairness

参考：

- `../../philosophy/experience/challenge-and-fairness.md`

应用原则：

- 挑战来源必须可理解；
- 失败应有可学习原因；
- 规则应稳定；
- 玩家应有反制和调整空间；
- 难度不应主要来自系统不可靠。

### 4.2 Choice and Consequence

参考：

- `../../philosophy/experience/choice-and-consequence.md`

应用原则：

- 难度应要求真实选择；
- 风险和后果在承诺前清楚；
- 不同方案有不同适用情境；
- 不应只存在一个正确构筑。

### 4.3 Simplicity and Depth

参考：

- `../../philosophy/experience/simplicity-and-depth.md`

应用原则：

- 基础规则简单；
- 深度来自组合、情境和判断；
- 不通过大量例外增加难度；
- 高级复杂度逐步开放。

### 4.4 Pacing and Rhythm

参考：

- `../../philosophy/experience/pacing-and-rhythm.md`

应用原则：

- 高压与恢复交替；
- 连续失败后有调整空间；
- 难度峰值与会话节奏匹配；
- 长内容提供中断点。

### 4.5 Progression and Motivation

参考：

- `../../philosophy/long-term/progression-and-motivation.md`

应用原则：

- 成长应帮助玩家应对新挑战；
- 挑战不能只追随数值无限膨胀；
- 失败保留进步；
- 回归和追赶有合理路径。

### 4.6 Accessibility and Inclusivity

参考：

- `../../philosophy/responsibility/accessibility-and-inclusivity.md`

应用原则：

- 区分核心挑战与非核心障碍；
- 支持输入、时间、信息和感官辅助；
- 辅助不应被羞辱；
- 重要内容不应只对高执行能力玩家开放。

### 4.7 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 不在失败后操纵性推销；
- 不隐藏动态难度；
- 不用损失恐惧强迫继续；
- 儿童和脆弱用户需要额外保护；
- 竞技公平优先于付费优势。

---

## 5. Player Experience

### 5.1 Player Goal

玩家面对挑战通常为了：

- 测试能力；
- 学习系统；
- 完成目标；
- 获得奖励；
- 验证构筑；
- 体验紧张；
- 与他人竞争或合作；
- 达成长远里程碑。

### 5.2 Entry

挑战入口包括：

- 关卡；
- 任务；
- 战斗；
- 谜题；
- 活动；
- 排名；
- 竞技；
- 限时目标；
- Boss；
- 训练；
- 教学；
- 高难模式。

### 5.3 Main Actions

玩家可以：

- 选择难度；
- 查看要求；
- 准备；
- 尝试；
- 暂停；
- 调整；
- 重试；
- 降低难度；
- 使用辅助；
- 放弃；
- 查看失败原因。

### 5.4 Core Decisions

关键决策包括：

- 是否进入当前难度；
- 选择何种构筑；
- 是否承担更高风险；
- 何时使用有限资源；
- 是否重试；
- 是否调整难度；
- 是否使用辅助；
- 是否继续当前挑战或改变目标。

### 5.5 Success

健康挑战体验意味着：

- 玩家理解成功原因；
- 成功来自判断、执行或准备；
- 难度与奖励关系清楚；
- 使用辅助仍保留成就；
- 失败后有明确下一步；
- 成长和掌握都能发挥作用；
- 不同玩家可以找到合适入口。

### 5.6 Failure

失败包括：

- 条件未满足；
- 规则误解；
- 执行错误；
- 资源管理失败；
- 构筑不适配；
- 信息不足；
- 网络或输入问题；
- 难度超出当前目标；
- 系统不公平。

### 5.7 Exit and Return

挑战退出时应说明：

- 是否保留进度；
- 是否消耗资源；
- 是否可以重试；
- 是否保存阶段；
- 是否影响多人；
- 是否影响排名；
- 是否可以降低难度；
- 下次从哪里恢复。

---

## 6. System Boundary

### 6.1 Inputs

系统接收：

- Player Progression；
- Character and Loadout State；
- Content Definition；
- Challenge Configuration；
- Player-Selected Difficulty；
- Accessibility Preferences；
- Session Performance；
- Matchmaking Context；
- Group Composition；
- Time and Event State；
- Live Operations Rules。

### 6.2 Outputs

系统产生：

- Difficulty Definition；
- Challenge Parameters；
- Recommended Difficulty；
- Eligibility；
- Assist Availability；
- Failure Penalty；
- Retry Rules；
- Scaling Result；
- Challenge Rating；
- Difficulty Changed Event；
- Completion Classification。

### 6.3 Owned State

系统拥有：

- Difficulty Mode；
- Challenge Definition；
- Challenge Tier；
- Difficulty Parameters；
- Recommended Difficulty；
- Assist Profile Reference；
- Dynamic Adjustment State；
- Failure Streak State；
- Retry State；
- Challenge Version；
- Difficulty History。

### 6.4 Read-Only Dependencies

系统读取：

- Progression；
- Characters and Loadouts；
- Settings；
- Accessibility；
- Content；
- Core Loop；
- Reward；
- Matchmaking；
- Save；
- Time。

### 6.5 Write Dependencies

系统通过正式契约请求：

- Rules 应用难度参数；
- Reward 应用难度奖励修正；
- Content 开放高难内容；
- Save 持久化模式和辅助；
- Analytics 记录；
- Matchmaking 使用挑战等级。

### 6.6 Out of Scope

系统不直接：

- 修改角色等级；
- 修改资源余额；
- 发放奖励；
- 处理支付；
- 决定具体敌人 AI；
- 修改玩家输入；
- 自动替玩家选择构筑。

---

## 7. Core Entities and Concepts

| Entity / Concept | Definition | Owner | Lifetime | Notes |
|---|---|---|---|---|
| Difficulty Mode | 玩家或系统选择的整体难度模式 | Difficulty | 长期或会话 | 如 Story、Standard |
| Challenge Definition | 某项挑战的完整定义 | Difficulty / Content | 版本级 | 唯一 ID |
| Challenge Tier | 离散难度层级 | Difficulty | 内容级 | 可比较 |
| Difficulty Parameter | 影响规则的参数 | Difficulty | 配置级 | 数值或规则 |
| Challenge Dimension | 难度来源维度 | Difficulty | 模型级 | 如时间、信息 |
| Assist Option | 降低非核心障碍的设置 | Settings / Difficulty | 长期 | 可组合 |
| Recommended Difficulty | 系统建议 | Difficulty | 会话级 | 非强制 |
| Dynamic Adjustment | 根据表现调整的机制 | Difficulty | 会话级 | 必须透明 |
| Failure Streak | 连续失败状态 | Difficulty | 短期 | 用于支持，不用于操纵 |
| Retry State | 当前挑战重试上下文 | Difficulty | 挑战级 | 可恢复 |
| Completion Classification | 完成难度、辅助和条件记录 | Difficulty | 长期 | 用于成就和排名 |
| Challenge Rating | 挑战强度摘要 | Difficulty | 配置或计算级 | 不等同单一数值 |

---

## 8. Challenge Taxonomy

### 8.1 Execution Challenge

要求：

- 时机；
- 精度；
- 速度；
- 连续输入；
- 手眼协调。

### 8.2 Decision Challenge

要求：

- 选择；
- 优先级；
- 风险判断；
- 资源分配；
- 规划。

### 8.3 Knowledge Challenge

要求：

- 规则理解；
- 模式识别；
- 情报；
- 记忆；
- 推理。

### 8.4 Preparation Challenge

要求：

- 构筑；
- 队伍；
- 装备；
- 资源；
- 目标选择。

### 8.5 Resource Challenge

要求：

- 有限资源管理；
- 消耗时机；
- 保存与投入权衡。

### 8.6 Endurance Challenge

要求：

- 持续时间；
- 多阶段；
- 稳定表现；
- 注意力。

### 8.7 Coordination Challenge

要求：

- 多人沟通；
- 角色分工；
- 同步行动；
- 共同决策。

### 8.8 Exploration Challenge

要求：

- 发现；
- 路线；
- 空间理解；
- 风险选择。

### 8.9 Social Challenge

来自：

- 协商；
- 信任；
- 竞争；
- 团队关系。

### 8.10 Economic Challenge

来自：

- 价格；
- 机会成本；
- 稀缺；
- 长期投资。

---

## 9. Difficulty Dimensions

难度应拆解为可独立调整的维度。

### 9.1 Rule Complexity

需要同时理解多少规则。

### 9.2 Information Load

需要同时处理多少信息。

### 9.3 Uncertainty

未知和随机程度。

### 9.4 Time Pressure

可决策时间。

### 9.5 Precision Demand

输入精度要求。

### 9.6 Reaction Demand

反应速度要求。

### 9.7 Planning Horizon

需要规划多远。

### 9.8 Resource Scarcity

可用资源限制。

### 9.9 Error Tolerance

错误后可恢复程度。

### 9.10 Punishment

失败损失。

### 9.11 Duration

挑战持续时间。

### 9.12 Coordination Load

多人同步要求。

### 9.13 Adaptation Load

环境或规则变化频率。

### 9.14 Build Requirement

对角色、装备和构筑的依赖程度。

---

## 10. Challenge Definition Template

```markdown
## Challenge Definition

- Challenge ID:
- Display Name:
- Category:
- Core Experience:
- Entry Condition:
- Difficulty Mode:
- Challenge Dimensions:
- Required Knowledge:
- Required Execution:
- Recommended Progression:
- Allowed Assists:
- Failure Condition:
- Failure Penalty:
- Retry:
- Checkpoint:
- Reward Modifier:
- Multiplayer:
- Competitive:
- Owner:
- Risk Level:
```

### 10.1 必须回答

- 挑战测试什么；
- 哪些是核心难度；
- 哪些是可移除障碍；
- 成功需要理解什么；
- 失败会损失什么；
- 如何重试；
- 是否依赖特定构筑；
- 辅助是否改变资格或记录。

---

## 11. Difficulty Modes

### 11.1 Story / Relaxed

目标：

- 降低执行和惩罚；
- 保留核心内容；
- 支持探索与叙事。

### 11.2 Standard

目标：

- 默认核心体验；
- 平衡学习、挑战和恢复。

### 11.3 Hard

目标：

- 提高判断、执行或资源要求；
- 减少容错；
- 增加规则组合。

### 11.4 Expert / Mastery

目标：

- 面向已经掌握系统的玩家；
- 引入高级组合和变化；
- 不仅提高数值。

### 11.5 Custom

允许玩家独立调整维度。

### 11.6 Assist Mode

不一定是单独难度，而是辅助组合。

### 11.7 Competitive Standard

竞技模式使用统一规则和可审计设置。

---

## 12. Mode Design Principles

1. 不同模式应说明改变了什么。
2. 不应只写“敌人更强”。
3. 高难应优先增加判断和组合，而不是纯数值。
4. 低难应减少障碍，但保留核心体验。
5. Custom 不应要求玩家理解全部内部参数。
6. 模式切换规则必须清楚。
7. 奖励差异不应迫使不适合的玩家进入高难。
8. 竞技规则不受个人单人辅助直接影响。

---

## 13. Difficulty Parameter Model

推荐结构：

```markdown
| Parameter | Dimension | Default | Min | Max | Affected Rule | Visible |
|---|---|---:|---:|---:|---|---|
```

可能参数包括：

- Enemy Health；
- Enemy Damage；
- Decision Window；
- Input Window；
- Resource Availability；
- Failure Penalty；
- Checkpoint Frequency；
- Hint Strength；
- Aim Assist；
- AI Aggression；
- Spawn Complexity；
- Randomness；
- Retry Cost。

### 13.1 Parameter Ownership

Difficulty 负责选择参数值。

Rules and Resolution 负责权威应用。

### 13.2 Avoid Hidden Coupling

一个参数不应同时无说明地改变多个维度。

---

## 14. Difficulty Curves

### 14.1 Introduction

建立基础规则理解。

### 14.2 Reinforcement

重复并巩固已学规则。

### 14.3 Combination

组合多个已学机制。

### 14.4 Variation

改变情境和约束。

### 14.5 Mastery Test

要求稳定应用和高级判断。

### 14.6 Recovery

提供低压内容、整理和学习。

### 14.7 Curve Pattern

通用节奏：

```text
Teach
→ Practice
→ Test
→ Combine
→ Peak
→ Recover
→ Recontextualize
```

### 14.8 Avoid Monotonic Increase

难度不应永远单调上升。

长期需要：

- 波峰；
- 波谷；
- 新规则；
- 熟悉内容；
- 自由选择。

---

## 15. Content Difficulty

内容难度应来自明确组合。

### 15.1 Enemy or Obstacle Design

改变：

- 行为；
- 组合；
- 信息；
- 位置；
- 节奏；
- 反制。

### 15.2 Environment

改变：

- 空间；
- 路线；
- 视野；
- 资源；
- 风险。

### 15.3 Objective

改变：

- 成功条件；
- 时间；
- 保护对象；
- 顺序；
- 评分。

### 15.4 Rule Modifier

改变局部规则。

必须清楚表达，不能隐藏。

### 15.5 Content Density

更多对象不必然等于更好挑战。

过高密度可能只是信息和性能负担。

---

## 16. Stat Difficulty

数值可以支持挑战，但不应成为唯一来源。

### 16.1 Health Scaling

增加持续时间和容错要求。

风险：

- 重复；
- 消耗战；
- 构筑单一。

### 16.2 Damage Scaling

提高错误成本。

风险：

- 一击失败；
- 学习机会减少；
- 防御路线强制。

### 16.3 Speed Scaling

提高反应和输入要求。

风险：

- 可访问性；
- 平台差异；
- 网络问题。

### 16.4 Resource Scaling

减少可用资源或增加成本。

风险：

- 囤积；
- 付费压力；
- 失败后无法恢复。

### 16.5 Stat Budget

多个数值参数应共享整体难度预算，避免全部同时提升。

---

## 17. Mechanical Difficulty

机械难度来自：

- 新行为；
- 新规则；
- 新组合；
- 新空间；
- 新目标；
- 新反制。

### 17.1 Teach Before Test

重要机制应在高压测试前：

- 展示；
- 练习；
- 反馈；
- 允许安全失败。

### 17.2 Combination Budget

一次引入过多机制会变成信息负担。

### 17.3 Readability

复杂机械仍需清楚：

- 预警；
- 状态；
- 目标；
- 结果；
- 反制。

---

## 18. Recommended Difficulty

系统可以根据：

- Progression；
- Loadout；
- 完成历史；
- 熟练度；
- 辅助设置；
- 队伍；

提供建议。

### 18.1 Recommendation Is Not Gate

推荐不应自动成为硬门槛。

### 18.2 Explain Recommendation

说明：

- 主要差距；
- 推荐原因；
- 可能风险；
- 可调整项。

### 18.3 Avoid False Precision

不要使用看似精确但无法解释的单一“战力数字”。

### 18.4 Skilled Player

允许技巧型玩家尝试更高难。

### 18.5 Underprepared Player

低于建议时应提示，而不是直接禁止，除非存在数据或团队风险。

---

## 19. Difficulty Eligibility

硬门槛适用于：

- 教学前置；
- 多人资格；
- 竞技规则；
- 必需能力；
- 平台限制。

软门槛适用于：

- 推荐能力；
- 预计时间；
- 构筑建议；
- 风险提示。

### 19.1 Hard Gate 必须有理由

不能只因为：

```text
数字不够
```

而阻止所有尝试。

---

## 20. Dynamic Difficulty Adjustment

### 20.1 Definition

根据玩家当前表现调整挑战。

### 20.2 Possible Adjustments

- 提示；
- Checkpoint；
- 敌人行为；
- 资源；
- 时间窗口；
- 失败惩罚；
- 推荐；
- 匹配；
- 内容顺序。

### 20.3 Transparency

动态调整应说明：

- 是否启用；
- 调整什么；
- 是否可关闭；
- 是否影响奖励和记录。

### 20.4 Safe Uses

适合：

- 新手引导；
- 单人辅助；
- 防卡关；
- 推荐；
- 提示强度。

### 20.5 High-Risk Uses

谨慎：

- 隐藏降低敌人；
- 根据付费调整难度；
- 根据失败推动购买；
- 竞技中个性化；
- 改变已承诺奖励。

### 20.6 Do Not Punish Success Invisibly

表现好后隐藏增强敌人，会削弱成长感和信任。

---

## 21. Assist Options

辅助可以作用于：

### 21.1 Input

- Aim Assist；
- Snap；
- Toggle；
- Input Window；
- Auto Repeat；
- One-Handed Mode。

### 21.2 Timing

- Slow Motion；
- Pause；
- Extended Timer；
- Turn-Based Alternative。

### 21.3 Information

- Stronger Telegraph；
- Objective Highlight；
- Navigation Help；
- Hint；
- Log；
- Preview。

### 21.4 Rule Support

- Reduced Penalty；
- Additional Checkpoint；
- Resource Recovery；
- Damage Adjustment；
- Invulnerability Window。

### 21.5 Automation

- Auto Resolve Low-Risk Actions；
- Auto Target；
- Auto Manage Non-Core Tasks。

### 21.6 Assist Ownership

Settings 保存偏好。

Difficulty 定义影响。

Rules 应用权威效果。

---

## 22. Core Challenge vs Non-Core Barrier

### 22.1 Core Challenge

项目希望玩家掌握的主要判断或行动。

### 22.2 Non-Core Barrier

不属于体验目标但阻止参与的障碍。

例如：

- 精确点击；
- 持续按住；
- 极小文字；
- 无字幕；
- 颜色识别；
- 重复输入。

### 22.3 Design Goal

辅助应优先移除 Non-Core Barrier，同时尽量保留 Core Challenge。

---

## 23. Failure Model

失败应分类。

### 23.1 Knowledge Failure

不了解规则。

### 23.2 Decision Failure

选择错误。

### 23.3 Execution Failure

操作失败。

### 23.4 Preparation Failure

构筑或资源不适配。

### 23.5 Coordination Failure

多人协作失败。

### 23.6 System Failure

网络、输入、性能或 Bug。

### 23.7 Unfair Failure

玩家没有合理信息或反制。

不同失败需要不同反馈和恢复。

---

## 24. Failure Feedback

失败后应提供：

- 主要原因；
- 次要原因；
- 关键时刻；
- 可调整项；
- 推荐练习；
- 是否适合降低难度；
- 是否存在系统问题。

### 24.1 Avoid Generic Failure

只显示：

```text
失败
```

通常不足以支持学习。

### 24.2 Avoid Over-Explaining

反馈应优先突出最有价值的 1–3 个原因。

---

## 25. Failure Penalty

### 25.1 Penalty Types

- 时间；
- 资源；
- 进度；
- 排名；
- 机会；
- 社交；
- 内容重置。

### 25.2 Penalty Purpose

惩罚可以支持：

- 风险；
- 承诺；
- 紧张；
- 资源判断。

### 25.3 Healthy Penalty

应：

- 提前说明；
- 与风险匹配；
- 可恢复；
- 不阻止学习；
- 不让系统错误转化为玩家损失。

### 25.4 Unhealthy Penalty

包括：

- 过长重复；
- 不可恢复核心资产损失；
- 因网络错误受罚；
- 失败后强制付费；
- 隐藏扣除；
- 连续失败越来越难恢复。

---

## 26. Retry

### 26.1 Retry Types

- Immediate Retry；
- Checkpoint Retry；
- Full Restart；
- Practice Retry；
- Modified Retry；
- Assisted Retry。

### 26.2 Retry Preparation

允许：

- 调整构筑；
- 查看失败原因；
- 修改辅助；
- 降低难度；
- 更换目标。

### 26.3 Fast Retry

高频短挑战应支持快速重试。

### 26.4 Long Challenge

长挑战需要：

- Checkpoint；
- 中断保存；
- 阶段恢复；
- 资源处理。

### 26.5 Retry Cost

不能让正常学习因成本过高而停止。

---

## 27. Checkpoints

### 27.1 Checkpoint Purpose

减少低价值重复。

### 27.2 Checkpoint Types

- Time-Based；
- Phase-Based；
- Objective-Based；
- Manual；
- Safe Room；
- Dynamic。

### 27.3 Checkpoint State

记录：

- 玩家状态；
- 目标状态；
- 资源；
- 规则版本；
- 难度；
- 辅助；
- 时间。

### 27.4 Checkpoint Abuse

竞技和排名内容需要防止：

- 重刷随机结果；
- 复制资源；
- 利用保存回退。

---

## 28. Difficulty Switching

### 28.1 Between Challenges

通常允许自由切换。

### 28.2 During Challenge

可以：

- 立即生效；
- 下个 Checkpoint 生效；
- 重启生效；
- 不允许。

必须提前说明。

### 28.3 Lowering Difficulty

不应羞辱玩家。

### 28.4 Raising Difficulty

可允许，但应说明：

- 当前进度；
- 奖励；
- 记录；
- 资格。

### 28.5 Achievement and Record

使用辅助或切换难度是否影响：

- 成就；
- 排名；
- 奖励；
- 记录；

必须透明。

---

## 29. Reward Integration

### 29.1 Reward Modifier

更高难可以提供：

- 更多资源；
- 更高概率；
- 特定表达奖励；
- 额外挑战目标；
- 认可。

### 29.2 Avoid Coercion

核心内容和关键成长不应只在不适合大多数玩家的高难中获得。

### 29.3 Recognition Reward

高难更适合提供：

- 称号；
- 外观；
- 记录；
- 排名；
- 展示；

而不是不可替代的基础能力。

### 29.4 Assist and Rewards

辅助对奖励的影响必须：

- 有明确理由；
- 不自动降低全部价值；
- 区分竞技和单人；
- 不羞辱玩家。

---

## 30. Progression Integration

成长应改变：

- 可用策略；
- 容错；
- 资源；
- 选择；
- 情境应对。

### 30.1 Avoid Nullifying Growth

如果动态缩放完全抵消成长，玩家会失去成长感。

### 30.2 Avoid Pure Overpower

如果成长完全消除挑战，核心体验会失效。

### 30.3 Healthy Relation

推荐：

```text
成长降低旧挑战成本，
同时开放新的挑战维度。
```

---

## 31. Build Diversity

挑战应支持多种可行构筑。

### 31.1 Diversity Methods

- 不同目标；
- 不同环境；
- 多种反制；
- 角色互补；
- 风险差异；
- 路线选择。

### 31.2 Build Check

内容可以测试特定能力，但不应长期强制唯一构筑。

### 31.3 Hard Counter

使用 Hard Counter 时应：

- 提前提示；
- 提供替代；
- 不完全否定玩家投资；
- 控制频率。

---

## 32. Competitive Difficulty

竞技挑战强调：

- 统一规则；
- 可审计输入；
- 匹配公平；
- 延迟处理；
- 反作弊；
- 公开评分。

### 32.1 Competitive Assists

区分：

- 基础可访问性；
- 影响竞技结果的辅助；
- 平台标准辅助。

### 32.2 Paid Power

付费成长不应直接决定竞技结果，除非模式明确隔离。

### 32.3 Ranking

排名应考虑：

- 对手质量；
- 赛制；
- 样本量；
- 断线；
- 弃权；
- 队伍差异。

---

## 33. Cooperative Difficulty

合作挑战需要考虑：

- 队伍规模；
- 角色分工；
- 贡献；
- 缺席；
- 断线；
- 新手与老手；
- 辅助差异。

### 33.1 Scaling by Party Size

需要明确：

- 数值；
- 目标；
- 机制；
- 资源；
- 复活。

### 33.2 Mixed Skill Group

提供：

- 灵活角色；
- 教学；
- 共享目标；
- 个人辅助；
- 不羞辱表现差异。

### 33.3 Carrying

高水平玩家带领低水平玩家时，应避免：

- 低水平玩家完全无参与；
- 奖励滥用；
- 社交压力；
- 贡献误判。

---

## 34. Time-Limited Challenge

### 34.1 Purpose

时间限制可以：

- 增加优先级；
- 提高紧张；
- 限制完美规划；
- 组织节奏。

### 34.2 Risks

- 可访问性；
- 网络；
- 输入设备；
- 阅读速度；
- 焦虑；
- 误触。

### 34.3 Requirements

- 清楚计时；
- 提前预警；
- 暂停规则；
- 中断处理；
- 辅助选项；
- 失败反馈。

### 34.4 Hidden Timer

禁止隐藏影响核心结果的计时器。

---

## 35. Endurance Challenge

长时挑战需要：

- Checkpoint；
- 保存；
- 暂停；
- 阶段反馈；
- 补给；
- 中断恢复。

### 35.1 Fatigue

难度可能来自玩家疲劳，而非系统掌握。

应避免用过长持续时间作为主要难度。

### 35.2 Session Fit

挑战时长应与目标平台和会话模式匹配。

---

## 36. Tutorial Difficulty

教学阶段的挑战用于：

- 验证理解；
- 提供安全练习；
- 建立信心；
- 识别需要帮助的玩家。

### 36.1 Tutorial Failure

不应造成高惩罚。

### 36.2 Adaptive Guidance

可以根据失败提供：

- 更清楚提示；
- 分步；
- 演示；
- 练习；
- 跳过。

### 36.3 Skilled Player

允许快速通过或跳过已掌握内容。

---

## 37. Return and Catch-Up Difficulty

回归玩家可能面临：

- 规则遗忘；
- 构筑过期；
- 内容跳跃；
- 数值落后；
- 输入不熟悉。

### 37.1 Return Support

提供：

- 安全练习；
- 构筑检查；
- 规则变化摘要；
- 推荐难度；
- 可重置；
- 追赶内容。

### 37.2 Avoid Immediate High Pressure

不要回归后立即进入：

- 限时；
- 排名；
- 高惩罚；
- 高付费压力。

---

## 38. Dynamic Support After Failure

连续失败后可以提供：

- 提示；
- 构筑建议；
- 难度调整；
- Checkpoint；
- 练习；
- 辅助；
- 替代路线。

### 38.1 Respectful Offer

提示应：

- 可忽略；
- 不羞辱；
- 不自动改变；
- 不影响已确认记录，除非说明。

### 38.2 Do Not Monetize Failure

失败支持不应直接首先指向付费。

---

## 39. Difficulty Debt

Difficulty Debt 包括：

- 纯数值膨胀；
- 过多模式；
- 大量特殊规则；
- 隐藏缩放；
- 旧内容失效；
- 唯一构筑；
- 难度标签失真；
- 辅助与成就冲突。

### 39.1 Signals

- 玩家只看战力；
- 高难只是更厚；
- 大量内容必须跳过；
- 同一模式不同关卡差异巨大；
- 推荐强度失真；
- 失败原因无法解释；
- 动态难度投诉。

### 39.2 Reduction

- 维度化审计；
- 参数预算；
- 模式合并；
- 规则统一；
- 内容重构；
- 可访问性评审；
- 奖励去强迫化。

---

## 40. Failure and Recovery

| Failure | Cause | Player Impact | Recovery | Data Guarantee |
|---|---|---|---|---|
| Invalid Difficulty Config | 配置错误 | 挑战异常 | 使用 Last Known Good | 保留版本 |
| Challenge Not Loadable | 内容缺失 | 无法进入 | 返回安全状态 | 不扣成本 |
| Dynamic Adjustment Error | 状态错误 | 难度异常 | 恢复默认模式 | 记录变更 |
| Checkpoint Corruption | 存档损坏 | 进度丢失 | 前一 Checkpoint 或补偿 | 保留备份 |
| Retry Transaction Failure | 状态未重置 | 无法重试 | 幂等恢复 | 不重复扣除 |
| Assist Not Applied | 设置同步失败 | 操作障碍 | Last Known Good | 保留偏好 |
| Reward Modifier Mismatch | 难度与奖励不一致 | 信任受损 | 纠正与补偿 | 记录挑战版本 |
| Network Failure | 在线挑战中断 | 失败或断线 | 重连、暂停或无惩罚退出 | 权威状态保留 |
| Ranking Dispute | 评分异常 | 竞技争议 | 审计、回滚、申诉 | 保留记录 |

---

## 41. Edge Cases

### Difficulty Selection

- 模式在挑战开始后被删除；
- Custom 参数组合无效；
- 辅助与竞技冲突；
- 推荐难度计算缺失；
- 多人队伍成员设置不同。

### Scaling

- 玩家在挑战中升级；
- 队伍人数变化；
- 角色更换；
- 内容版本变化；
- 动态难度和活动倍率叠加。

### Failure

- 失败与断线同时发生；
- 失败后资源不足；
- Checkpoint 正好创建；
- 连续失败状态跨会话；
- 失败支持提示重复。

### Time

- 限时挑战进入后台；
- 跨日；
- 活动结束；
- 设备时间错误；
- 暂停计时差异。

### Accessibility

- 辅助设置中途切换；
- 控制器断开；
- 读屏焦点丢失；
- 大字体遮挡预警；
- 色觉模式未应用。

---

## 42. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| Rules and Resolution | Hard | Difficulty → Rules | Parameters | 难度不生效 |
| Progression System | Hard / Soft | 双向 | Power / Requirement | 难度失配 |
| Characters and Loadouts | Hard / Soft | Characters → Difficulty | Build State | 推荐错误 |
| Content and Unlocks | Hard | Content → Difficulty | Challenge Definition | 内容无法进入 |
| Core Loop | Hard | 双向 | Challenge Stage | 循环中断 |
| Input and Interaction | Soft / Hard | Input → Difficulty | Assist / Execution | 操作障碍 |
| Settings and Preferences | Hard / Soft | Settings → Difficulty | Assist Profile | 使用默认配置 |
| Reward System | Soft / Hard | Difficulty → Reward | Reward Modifier | 奖励不一致 |
| Save and Persistence | Hard | Difficulty → Save | Mode / Checkpoint | 无法恢复 |
| Matchmaking | Hard for Competitive | Matchmaking → Difficulty | Rating / Rules | 公平受损 |
| Analytics | Soft | Difficulty → Analytics | Outcome Events | 不阻断 |
| Live Operations | Soft | Live → Difficulty | Event Rules | 使用 Last Known Good |

---

## 43. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Difficulty Mode | 是 | Difficulty | 修改确认 | 长期 | 默认或 Last Known Good |
| Custom Parameters | 是 | Difficulty / Settings | 修改确认 | 长期 | Profile 恢复 |
| Assist Profile Reference | 是 | Settings | 修改确认 | 长期 | Last Known Good |
| Challenge Version | 是 | Difficulty | 挑战开始 | 审计期 | 重放 |
| Retry State | 是 | Difficulty | 失败与重试 | 挑战期 | 幂等恢复 |
| Checkpoint | 是 | Save / Difficulty | 节点创建 | 挑战期 | 前一版本 |
| Dynamic Adjustment State | 按需 | Difficulty | 调整时 | 会话或挑战期 | 恢复默认 |
| Completion Classification | 是 | Difficulty | 完成 | 长期 | 审计 |
| Difficulty History | 是 | Difficulty | 关键变化 | 长期 | 回顾 |

---

## 44. Accessibility

### 44.1 Perception

- 强化预警；
- 颜色替代；
- 字幕；
- 高对比；
- 可调特效；
- 重要状态可回看。

### 44.2 Input

- Aim Assist；
- Snap；
- 输入窗口；
- Toggle；
- 单手；
- 降低重复；
- 速度调整。

### 44.3 Cognition

- 更清楚目标；
- 提示；
- 日志；
- 分步；
- 预览；
- 规则解释；
- 减少同时信息。

### 44.4 Timing

- 暂停；
- 慢速；
- 延长计时；
- Checkpoint；
- 取消时间限制；
- 回合制替代。

### 44.5 Failure

- 降低惩罚；
- 保留进度；
- 练习；
- 提示；
- 自动恢复。

### 44.6 Assist Presets

可以提供：

- Motor；
- Vision；
- Hearing；
- Cognitive；
- Low Stress；
- Custom。

但不应推断用户身份或强迫标签。

---

## 45. Ethical and Safety Review

### 45.1 Hidden Difficulty

禁止：

- 无说明提高敌人；
- 表现好后偷偷削弱玩家；
- 根据付费状态改变成功率；
- 根据失败推动高价购买。

### 45.2 Failure Monetization

失败后不应优先展示：

- 付费复活；
- 付费强化；
- 限时礼包；
- 高压随机抽取。

如存在付费恢复，必须：

- 有免费替代；
- 价格透明；
- 不破坏公平；
- 不利用疲劳。

### 45.3 FOMO

高难和活动不应让核心能力永久错过。

### 45.4 Children and Vulnerable Users

- 不用羞辱推动继续；
- 支持时间和消费限制；
- 不将失败与角色失望绑定付费；
- 提供低压力模式。

### 45.5 Competitive Fairness

- 规则公开；
- 付费优势隔离；
- 辅助规则透明；
- 作弊和网络异常有处理；
- 排名可申诉。

---

## 46. Analytics and Validation

### 46.1 Key Assumptions

1. 玩家能理解主要难度来源。
2. 失败后能够识别改进方向。
3. 默认模式符合核心体验。
4. 高难增加判断和组合，而不只是数值。
5. 低难和辅助保留核心体验。
6. 推荐难度有帮助但不成为错误硬门槛。
7. 动态调整透明且不会破坏信任。
8. 失败惩罚不会阻止学习。
9. 不同构筑和玩家类型都有可行路径。
10. 长期挑战不会陷入纯数值膨胀。

### 46.2 Validation Plan

| Hypothesis | Evidence | Success | Failure | Method |
|---|---|---|---|---|
| 难度来源清楚 | 失败复述 | 能说明主要原因 | 只认为数值不够 | 访谈 |
| 失败可学习 | 重试行为 | 调整后成功率提高 | 重复同样失败 | 观察 |
| 默认模式合适 | 完成和压力反馈 | 大多数玩家可进入节奏 | 大量过难或过易 | 数据与研究 |
| 高难有深度 | 行为多样性 | 更多判断和组合 | 只是耗时更长 | 玩法测试 |
| 辅助保留核心 | 目标用户测试 | 障碍降低且体验成立 | 辅助破坏主要目标 | Accessibility Test |
| 推荐有效 | 进入与完成 | 建议与结果相关 | 战力建议失真 | 数据分析 |
| 动态调整可信 | 认知测试 | 玩家理解并接受 | 感到系统作弊 | 研究 |
| 构筑多样 | 构筑分布 | 多条可行路线 | 唯一构筑压倒性 | 平衡分析 |
| 长期健康 | 难度趋势 | 机制变化持续 | 纯数值增长 | 长期审计 |

### 46.3 Behavioral Metrics

- Difficulty Selected；
- Difficulty Changed；
- Assist Enabled；
- Challenge Started；
- Challenge Completed；
- Challenge Failed；
- Retry；
- Checkpoint Resume；
- Hint Used；
- Dynamic Adjustment Applied；
- Challenge Abandoned；
- Difficulty Recommendation Accepted。

### 46.4 Outcome Metrics

- Completion Rate；
- Failure Reason Distribution；
- Retry Success；
- Time to Mastery；
- Assist Success；
- Build Diversity；
- Difficulty Switch Rate；
- Checkpoint Recovery；
- Competitive Fairness；
- Recommended vs Actual Outcome；
- Return Recovery Time。

### 46.5 Negative Metrics

- 无法解释失败；
- 一击失败；
- 过长重复；
- 动态难度投诉；
- 辅助使用后羞辱或奖励大幅削减；
- 唯一构筑；
- 付费恢复依赖；
- Checkpoint 损坏；
- 难度模式标签失真；
- 网络问题造成惩罚；
- 高难纯数值膨胀；
- 低难失去核心体验。

### 46.6 Event Intents

| Event Intent | Trigger | Key Properties | Privacy Notes |
|---|---|---|---|
| Difficulty Selected | 模式确认 | Mode, Source | 不推断能力身份 |
| Challenge Failed | 失败确认 | Failure Category, Stage | 数据最小化 |
| Retry Started | 重试 | Retry Type, Changes | 不记录敏感文本 |
| Assist Changed | 辅助变化 | Category, Context | 不推断健康状况 |
| Dynamic Adjustment Applied | 调整生效 | Type, Reason | 透明审计 |
| Checkpoint Restored | 恢复 | Checkpoint Type | 不记录不必要状态 |
| Difficulty Recommendation Shown | 推荐展示 | Basis Category | 不用于歧视性定价 |
| Challenge Completed | 完成 | Mode, Assists, Duration | 匿名化 |

---

## 47. Difficulty Model Template

```markdown
# Difficulty Model

## Challenge

- Name:
- Core Skill:
- Entry Condition:
- Recommended Progression:

## Dimensions

| Dimension | Baseline | Easy | Hard | Expert |
|---|---:|---:|---:|---:|

## Rules

- Failure:
- Penalty:
- Retry:
- Checkpoint:
- Assist:
- Reward Modifier:

## Scaling

- Static:
- Dynamic:
- Party Size:
- Competitive:

## Fairness

- Information:
- Counterplay:
- Build Diversity:
- Paid Influence:
- Network:

## Validation

- Success:
- Failure:
- Metrics:
```

---

## 48. Rollout and Migration

### 48.1 Rollout

难度变更应按：

- 内部；
- 可用性测试；
- 目标用户测试；
- 小范围；
- 分模式；
- 全量；

逐步发布。

### 48.2 High-Risk Changes

包括：

- 失败惩罚；
- Checkpoint；
- 动态难度；
- 竞技规则；
- 付费恢复；
- 辅助与奖励关系；
- 大规模数值缩放；
- 难度标签重构。

### 48.3 Migration

必须定义：

- 旧难度模式；
- Custom 参数；
- 辅助 Profile；
- 进行中挑战；
- Checkpoint；
- 排名；
- 成就；
- 奖励资格；
- 动态调整状态。

### 48.4 Rollback

回滚时：

- 进行中挑战使用原版本或安全映射；
- 不撤销已确认完成；
- 保留辅助设置；
- 恢复旧参数；
- 竞技记录按规则处理；
- 必要时补偿。

### 48.5 Stop Conditions

出现以下情况应停止发布：

- 大量挑战无法完成；
- 失败原因异常；
- Checkpoint 丢失；
- 辅助失效；
- 动态调整错误；
- 竞技公平异常；
- 奖励与难度不一致；
- 付费恢复使用激增；
- 网络错误造成大量惩罚；
- 构筑多样性骤降。

---

## 49. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| 高难只增加数值 | Experience Risk | 高 | 高 | 维度化和机械变化 | Design |
| 推荐强度失真 | Trust Risk | 中 | 中 | 可解释建议 | Data |
| 动态难度破坏信任 | Ethical Risk | 高 | 中 | 透明与可关闭 | Product |
| 辅助被污名化 | Accessibility Risk | 高 | 中 | 中性表达与记录分离 | UX |
| 唯一构筑 | Balance Risk | 高 | 中 | 内容多样和分布监控 | Design |
| Checkpoint 滥用 | Integrity Risk | 中 | 低 | 排名和随机保护 | Engineering |
| 失败后付费压力 | Ethical Risk | 严重 | 中 | 免费恢复与限制 | Product |
| 多人难度缩放失衡 | Social Risk | 高 | 中 | 队伍规模测试 | Design |
| 旧模式迁移失败 | Migration Risk | 高 | 中 | 映射和备份 | Engineering |

---

## 50. Review Checklist

### Purpose and Taxonomy

- [ ] 挑战类型和难度维度明确；
- [ ] 核心挑战与非核心障碍区分；
- [ ] 每项挑战有统一定义；
- [ ] 难度不等同单一数值；
- [ ] Non-Goals 已定义。

### Modes and Curves

- [ ] Story、Standard、Hard、Expert、Custom 等模式差异清楚；
- [ ] 每种模式说明改变了什么；
- [ ] 难度曲线包含教学、练习、测试、峰值和恢复；
- [ ] 不单调无限上升；
- [ ] 高难增加判断而不只是血量。

### Failure and Recovery

- [ ] Failure Type 可分类；
- [ ] 失败反馈突出主要原因；
- [ ] Penalty 提前说明；
- [ ] Retry 和 Checkpoint 合理；
- [ ] 网络和系统错误不转化为玩家惩罚。

### Recommendation and Scaling

- [ ] 推荐难度可解释；
- [ ] 推荐不是无理由硬门槛；
- [ ] 动态难度透明；
- [ ] 动态调整可关闭或有边界；
- [ ] 成长不会被完全抵消。

### Accessibility

- [ ] 输入、时间、信息、规则和自动化辅助完整；
- [ ] 辅助保留核心体验；
- [ ] 使用辅助不被羞辱；
- [ ] 单人和竞技辅助边界清楚；
- [ ] Assist Profile 可持久化。

### Fairness

- [ ] 规则和反制清楚；
- [ ] 多种构筑可行；
- [ ] Hard Counter 有提示和替代；
- [ ] 竞技规则统一；
- [ ] 付费优势不破坏公平。

### Reward and Ethics

- [ ] 高难奖励不强迫所有玩家；
- [ ] 核心能力不永久锁定高难；
- [ ] 失败不直接首先推付费；
- [ ] 不隐藏缩放；
- [ ] 儿童和脆弱用户保护完整。

### Validation and Maintenance

- [ ] 完成、失败、重试、辅助、构筑和公平指标完整；
- [ ] 动态难度和推荐可验证；
- [ ] Difficulty Debt 有治理方式；
- [ ] 迁移、回滚和停止条件明确；
- [ ] 进行中挑战有版本策略。

---

## 51. V1 Completion Criteria

Difficulty and Challenge 可以被视为 V1，当：

- Challenge Taxonomy 和 Difficulty Dimensions 已定义；
- 每项挑战有统一 Challenge Definition；
- Story、Standard、Hard、Expert、Custom 和 Assist 模式职责明确；
- Difficulty Parameter Model 和参数所有权清楚；
- 难度曲线包含教学、巩固、组合、峰值和恢复；
- 数值难度与机械难度得到区分；
- Recommended Difficulty 可解释且不是错误硬门槛；
- Static Scaling、Dynamic Adjustment 和 Party Scaling 规则明确；
- 动态难度透明、可审计且不根据付费操纵结果；
- Core Challenge 与 Non-Core Barrier 区分；
- Assist Option 覆盖输入、时间、信息、规则和自动化；
- Failure Type、Failure Feedback、Penalty、Retry 和 Checkpoint 规则完整；
- 难度切换、辅助、奖励、成就和记录关系透明；
- Progression、Reward、Content、Character 和 Rules 的状态边界明确；
- Build Diversity、Competitive Fairness 和 Cooperative Scaling 通过评审；
- Tutorial、Return 和 Catch-Up Difficulty 有专门策略；
- Difficulty Debt 有识别和治理方式；
- 可访问性、伦理、儿童保护和失败商业化通过专项评审；
- Completion、Failure、Retry、Assist、Build Diversity 和 Fairness 有验证计划；
- 高风险难度变更具有灰度、迁移、回滚和停止条件；
- 下游 Content、Objectives、Matchmaking、Tutorial、Reward 和 Live Operations 可以直接引用本文件。

---

## 52. Related Documents

### Philosophy

- [Simplicity and Depth](../../philosophy/experience/simplicity-and-depth.md)
- [Choice and Consequence](../../philosophy/experience/choice-and-consequence.md)
- [Challenge and Fairness](../../philosophy/experience/challenge-and-fairness.md)
- [Pacing and Rhythm](../../philosophy/experience/pacing-and-rhythm.md)
- [Progression and Motivation](../../philosophy/long-term/progression-and-motivation.md)
- [Accessibility and Inclusivity](../../philosophy/responsibility/accessibility-and-inclusivity.md)
- [Ethical Design](../../philosophy/responsibility/ethical-design.md)

### Systems

- [Systems README](../README.md)
- [System Design Framework](../system-design-framework.md)
- [System Map](../system-map.md)
- [Integration Rules](../integration-rules.md)
- [Core Loop](../core/core-loop.md)
- [Rules and Resolution](../core/rules-and-resolution.md)
- [Input and Interaction](../core/input-and-interaction.md)
- [Resources and Economy](./resources-and-economy.md)
- [Progression System](./progression-system.md)
- [Reward System](./reward-system.md)
- `../content/content-and-unlocks.md`
- `../content/characters-and-loadouts.md`
- `../player/tutorial-and-onboarding.md`
- `../social/matchmaking-and-competition.md`
