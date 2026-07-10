# Conflict Resolution（原则冲突处理）

> Status: V1  
> Scope: `design/philosophy/`  
> Purpose: 为设计原则之间的真实冲突建立统一判断流程，避免依赖职位、个人偏好、临时妥协或局部指标直接决定方案。

---

## 1. 为什么原则会冲突

设计原则通常不是单独生效。

一个方案可能同时希望：

- 更清晰；
- 更有深度；
- 更自由；
- 更快；
- 更公平；
- 更可访问；
- 更有惊喜；
- 更有商业价值。

这些目标经常无法同时最大化。

例如：

```text
展示更多信息
可以提高清晰度，
但也会增加复杂度和信息负担。

不可逆选择
可以强化承诺，
但也会降低实验和恢复空间。

高频限时活动
可以提高短期参与，
但可能伤害玩家时间、自主与信任。
```

原则冲突不代表某个原则错误。

它意味着：

> 当前情境需要明确谁优先、接受什么代价，以及何时重新评估。

---

## 2. 原则冲突不应如何解决

不应仅通过以下方式决定：

- 职位最高的人选择；
- 最快实现的方案；
- 声音最大的人；
- 单一数据指标；
- 竞品如何设计；
- 已经投入很多；
- “玩家会习惯”；
- 没有投诉；
- 临近发布日期。

这些因素可以成为约束，但不能替代设计判断。

---

## 3. 默认优先级

当缺乏更具体项目原则时，使用以下默认优先级：

```text
1. 正确性与安全
2. 信任、透明与伦理边界
3. 可理解性
4. 可控制性
5. 可恢复性
6. 核心体验
7. 长期深度与系统健康
8. 效率与便利
9. 新颖性
10. 装饰性表现
```

说明：

### 3.1 正确性与安全

包括：

- 数据正确；
- 不丢失资产；
- 不错误收费；
- 不危害隐私；
- 不造成身体风险；
- 不破坏社区安全。

### 3.2 信任、透明与伦理边界

包括：

- 不误导；
- 价格与概率透明；
- 不利用脆弱性；
- 不通过虚假紧迫驱动行为；
- 尊重玩家停止与退出。

### 3.3 可理解性

玩家能够理解：

- 当前状态；
- 规则；
- 风险；
- 行动；
- 结果。

### 3.4 可控制性

玩家能够：

- 做出选择；
- 执行意图；
- 调整设置；
- 拒绝；
- 停止。

### 3.5 可恢复性

错误、失败和中断不会轻易造成不可恢复损失。

### 3.6 核心体验

方案应保留项目最重要的体验承诺。

### 3.7 长期深度与系统健康

避免为了短期收益破坏：

- 选择空间；
- 经济；
- 内容；
- 信任；
- 系统扩展能力。

### 3.8 效率与便利

减少时间、操作和流程成本。

### 3.9 新颖性

提供新鲜、差异和惊喜。

### 3.10 装饰性表现

视觉、动画和演出上的提升。

---

## 4. 优先级不是绝对答案

默认优先级用于缺少项目级依据时的安全判断。

以下情况可以调整：

- 项目核心体验明确依赖特殊限制；
- 目标玩家主动寻求高风险或高强度体验；
- 竞技模式需要统一条件；
- 艺术表达需要有意制造陌生或不安；
- 法律、平台和技术约束改变可选方案。

调整优先级时必须记录：

- 为什么；
- 适用范围；
- 接受代价；
- 缓解措施；
- 复审条件。

不能因为某个项目强调挑战，就取消：

- 基础信息公平；
- 输入可靠性；
- 隐私；
- 价格透明；
- 儿童保护。

Responsibility 层通常属于边界，而不是可以被任意交换的普通目标。

---

## 5. 冲突类型

### 5.1 Goal Conflict

两个目标不能同时最大化。

例如：

```text
快速流程 vs 完整解释
```

### 5.2 Resource Conflict

时间、预算、性能或团队资源不足。

### 5.3 Player Segment Conflict

不同玩家需要不同方案。

例如：

```text
新手需要引导；
熟练玩家需要效率。
```

### 5.4 Time Horizon Conflict

短期指标与长期价值冲突。

例如：

```text
短期付费提升
vs
长期信任下降
```

### 5.5 System Conflict

一个系统的局部优化损害其他系统。

### 5.6 Platform Conflict

不同平台惯例与统一体验发生冲突。

### 5.7 Ethical Conflict

商业、参与或内容目标与玩家自主、安全和隐私冲突。

### 5.8 Evidence Conflict

不同证据来源给出不同判断。

---

## 6. 冲突严重度

### Level 0：无真实冲突

可以同时满足，只是团队尚未找到方案。

### Level 1：局部取舍

影响：

- 少量流程；
- 低风险；
- 可快速回退。

### Level 2：系统取舍

影响：

- 核心系统；
- 多类玩家；
- 长期维护；
- 需要正式记录。

### Level 3：高风险取舍

涉及：

- 付费；
- 隐私；
- 儿童；
- 数据；
- 永久损失；
- 多人公平；
- 核心体验方向。

必须进行跨职能评审和严格验证。

### Level 4：不可接受边界

包括：

- 欺骗性收费；
- 隐瞒核心概率；
- 无法合理取消；
- 严重隐私风险；
- 利用儿童和脆弱用户；
- 安全风险；
- 明知会造成资产损失仍发布。

这类问题不是普通取舍，应直接阻止发布。

---

## 7. 冲突处理流程

### Step 1：描述冲突

使用中立语言描述双方价值。

不写：

```text
产品想要赚钱，但玩家不喜欢付费。
```

更好：

```text
商业目标希望提高活动期收入；
玩家价值要求价格透明、参与自愿且不依赖永久错过压力。
```

### Step 2：定义情境

明确：

- 目标玩家；
- 使用阶段；
- 平台；
- 风险；
- 时间范围；
- 是否首次；
- 是否可逆；
- 是否涉及儿童或付费。

### Step 3：确认是否是真冲突

询问：

```text
是否可以通过分层、设置、渐进披露、可选模式、默认值或不同阶段同时满足？
```

很多“冲突”只是方案空间过窄。

### Step 4：列出不可突破边界

优先检查：

- 正确性；
- 安全；
- 隐私；
- 伦理；
- 法规；
- 玩家资产；
- 核心体验身份。

### Step 5：列出备选方案

至少记录：

- 方案 A；
- 方案 B；
- 组合方案；
- 不做；
- 延后；
- 小范围试验。

### Step 6：比较代价

从以下维度比较：

- 玩家价值；
- 核心体验；
- 理解；
- 控制；
- 恢复；
- 长期系统；
- 商业；
- 制作成本；
- 技术风险；
- 伦理风险；
- 可回退性。

### Step 7：选择与缓解

明确：

- 当前优先保护什么；
- 放弃什么；
- 如何降低负面影响；
- 为什么不选择其他方案。

### Step 8：定义验证与复审

记录：

- 成功标准；
- 失败标准；
- 监控指标；
- 定性反馈；
- 复审时间；
- 自动触发复审的条件。

---

## 8. 冲突评估矩阵

```markdown
| Dimension | Option A | Option B | Combined Option |
|---|---:|---:|---:|
| Correctness and Safety |  |  |  |
| Trust and Ethics |  |  |  |
| Clarity |  |  |  |
| Control |  |  |  |
| Recoverability |  |  |  |
| Core Experience |  |  |  |
| Long-Term Health |  |  |  |
| Efficiency |  |  |  |
| Novelty |  |  |  |
| Production Cost |  |  |  |
| Reversibility |  |  |  |
```

可以使用：

```text
+2 强支持
+1 略支持
 0 中性
-1 有风险
-2 严重冲突
```

评分只用于结构化讨论，不应替代判断。

---

## 9. 证据优先级

冲突决策应尽量基于证据。

建议参考顺序：

```text
1. 正确性、安全、法律与伦理边界
2. 明确的项目价值与核心体验
3. 目标玩家真实行为与研究
4. 相关可用性和体验测试
5. 长期数据与分群数据
6. 制作与运营经验
7. 行业启发式方法
8. 团队个人偏好
```

### 9.1 证据冲突

当数据与访谈冲突时：

- 检查样本；
- 检查指标是否只是代理；
- 检查时间范围；
- 检查不同玩家群；
- 检查短期与长期；
- 设计新的验证。

### 9.2 没有证据时

没有证据不代表不能决策。

应：

- 选择更安全、可恢复和可回退的方案；
- 缩小影响范围；
- 记录假设；
- 尽快验证。

---

## 10. 常见冲突处理框架

### 10.1 Clarity vs Discovery

**冲突**

公开信息提高理解，但可能降低探索和惊喜。

**默认判断**

```text
隐藏答案，不隐藏必要线索。
```

**保护内容**

- 当前目标；
- 风险类型；
- 基础规则；
- 高影响不可逆后果。

**可以隐藏**

- 完整结果；
- 最优路线；
- 具体奖励；
- 叙事真相。

**缓解方式**

- 分层信息；
- 可选侦察；
- 失败后建立规律；
- 图鉴和日志。

**关联文档**

- [Clarity and Feedback](./experience/clarity-and-feedback.md)
- [Choice and Consequence](./experience/choice-and-consequence.md)

---

### 10.2 Simplicity vs Depth

**冲突**

减少规则和选项可能降低长期深度。

**默认判断**

```text
优先删除重复、装饰性和低交互复杂度，
保留能产生情境差异和组合关系的复杂度。
```

**缓解方式**

- 规则复用；
- 正交设计；
- 渐进式披露；
- 高级层按需展开；
- 删除冗余选项。

**关联文档**

- [Simplicity and Depth](./experience/simplicity-and-depth.md)

---

### 10.3 Freedom vs Core Experience

**冲突**

更多自由可能让玩家绕过项目核心体验。

**默认判断**

```text
允许多种实现路径，
但保留定义项目身份的核心约束。
```

**缓解方式**

- 软约束；
- 可选路线；
- 不同风格完成同一目标；
- 明确不支持的玩法。

**关联文档**

- [Core Experience and Fantasy](./foundation/core-experience-and-fantasy.md)
- [Choice and Consequence](./experience/choice-and-consequence.md)

---

### 10.4 Challenge vs Accessibility

**冲突**

辅助选项可能改变操作和难度要求。

**默认判断**

```text
保留核心判断与情绪，
降低与核心体验无关的感官、输入和设备障碍。
```

**缓解方式**

- 多维辅助；
- 输入容错；
- 时间调整；
- 信息增强；
- 竞技条件分组；
- 不羞辱辅助玩家。

**关联文档**

- [Challenge and Fairness](./experience/challenge-and-fairness.md)
- [Accessibility and Inclusivity](./responsibility/accessibility-and-inclusivity.md)

---

### 10.5 Consistency vs Novelty

**冲突**

复用已有规则提高理解，但可能减少新鲜感。

**默认判断**

```text
保持术语、操作和基础语义稳定，
在目标、组合、情境、节奏和表现上创新。
```

**只有当以下条件成立时创建新规则**

- 现有规则无法表达；
- 体验价值足够高；
- 可以复用；
- 能够明确教学；
- 例外成本可控。

**关联文档**

- [Consistency and Coherence](./long-term/consistency-and-coherence.md)
- [Simplicity and Depth](./experience/simplicity-and-depth.md)

---

### 10.6 Retention vs Player Time

**冲突**

频繁目标、通知和限时结构可能提高活跃，但占用玩家现实时间。

**默认判断**

```text
优先价值驱动留存，
不通过不可补回损失和高压频率惩罚离开。
```

**缓解方式**

- 累计目标；
- 宽限期；
- 自然停止点；
- 回归支持；
- 通知可控；
- 核心内容长期可达。

**关联文档**

- [Progression and Motivation](./long-term/progression-and-motivation.md)
- [Pacing and Rhythm](./experience/pacing-and-rhythm.md)
- [Ethical Design](./responsibility/ethical-design.md)

---

### 10.7 Monetization vs Trust

**冲突**

提高付费转化可能增加压力、摩擦或信息不对称。

**默认判断**

```text
商业价值必须来自清楚、真实、自愿的价值交换。
```

**不可接受**

- 隐藏价格；
- 虚假折扣；
- 虚假倒计时；
- 难以取消；
- 儿童高压随机付费；
- 利用失败和脆弱状态推销。

**缓解方式**

- 真实价格；
- 概率透明；
- 非随机路径；
- 消费限额；
- 冷静期；
- 易取消。

**关联文档**

- [Ethical Design](./responsibility/ethical-design.md)

---

### 10.8 Automation vs Agency

**冲突**

自动化减少重复，但可能替玩家做出核心选择。

**默认判断**

```text
自动化已掌握的低价值执行，
保留高影响目标、策略和资源判断。
```

**缓解方式**

- 首次完整体验；
- 掌握后解锁；
- 自动规则可配置；
- 结果可解释；
- 高影响决定需要确认。

**关联文档**

- [Simplicity and Depth](./experience/simplicity-and-depth.md)
- [Progression and Motivation](./long-term/progression-and-motivation.md)
- [Accessibility and Inclusivity](./responsibility/accessibility-and-inclusivity.md)

---

### 10.9 Fairness vs Randomness

**冲突**

随机性增加变化，但可能削弱控制和归因。

**默认判断**

```text
优先使用可管理的输入随机性，
严格控制高影响输出随机性。
```

**缓解方式**

- 概率透明；
- 风险管理；
- 保底；
- 重抽；
- 结果上下限；
- 失败补偿；
- 非随机替代路径。

**关联文档**

- [Challenge and Fairness](./experience/challenge-and-fairness.md)
- [Choice and Consequence](./experience/choice-and-consequence.md)

---

### 10.10 Expression vs Balance

**冲突**

玩家偏好的角色、外观或构筑可能明显弱于最优方案。

**默认判断**

```text
允许局部效率差异，
避免长期让表达型选择完全失去可行性。
```

**缓解方式**

- 情境价值；
- 多种可行目标；
- 非竞技分离；
- 表达与性能解绑；
- 降低切换成本。

**关联文档**

- [Choice and Consequence](./experience/choice-and-consequence.md)
- [Progression and Motivation](./long-term/progression-and-motivation.md)

---

### 10.11 Fast Feedback vs Interruption

**冲突**

及时反馈提高理解，但频繁弹窗和演出会破坏操作流。

**默认判断**

```text
即时确认轻量化，
普通结果汇总，
重大结果完整表达。
```

**关联文档**

- [Clarity and Feedback](./experience/clarity-and-feedback.md)
- [Pacing and Rhythm](./experience/pacing-and-rhythm.md)

---

### 10.12 Platform Consistency vs Platform Convention

**冲突**

跨平台完全一致可能违反输入和导航惯例。

**默认判断**

```text
保持意图、规则和结果一致，
允许输入、布局和快捷方式适配平台。
```

**关联文档**

- [Consistency and Coherence](./long-term/consistency-and-coherence.md)
- [Accessibility and Inclusivity](./responsibility/accessibility-and-inclusivity.md)

---

## 11. 设计冲突记录模板

```markdown
# Principle Conflict Record

## 1. Summary

- Conflict ID:
- Title:
- Date:
- Owner:
- Severity:
- Status:

## 2. Context

- Target Players:
- Scenario:
- Project Stage:
- Platforms:
- Time Horizon:
- Reversibility:
- Affected Systems:

## 3. Conflicting Principles

### Principle A

- Principle:
- Value Protected:
- Supporting Evidence:

### Principle B

- Principle:
- Value Protected:
- Supporting Evidence:

## 4. Non-Negotiable Boundaries

- Correctness:
- Safety:
- Trust:
- Ethics:
- Legal:
- Player Assets:
- Core Experience:

## 5. Options

### Option A

- Description:
- Benefits:
- Costs:
- Risks:
- Reversibility:

### Option B

- Description:
- Benefits:
- Costs:
- Risks:
- Reversibility:

### Combined Option

- Description:
- Benefits:
- Costs:
- Risks:
- Reversibility:

### Do Nothing

- Consequence:

## 6. Decision

- Selected Option:
- Priority Principle:
- Reason:
- Accepted Consequences:
- Mitigation:
- Rejected Options:

## 7. Validation

- Assumption:
- Success Criteria:
- Failure Criteria:
- Qualitative Evidence:
- Quantitative Evidence:
- Negative Metrics:
- Test Scope:

## 8. Review

- Review Date:
- Trigger Conditions:
- Rollback Plan:
- Final Outcome:
- Lessons:
```

---

## 12. 简化记录模板

适合 Level 1 局部冲突：

```markdown
## Conflict

- Conflict:
- Context:
- Selected Principle:
- Reason:
- Accepted Cost:
- Mitigation:
- Review Condition:
```

---

## 13. 示例一：新手信息 vs 界面简洁

### Conflict

```text
新手需要持续看见资源说明，
但熟练玩家认为界面拥挤。
```

### Principles

- Clarity；
- Simplicity；
- Player First；
- Accessibility。

### Decision

采用：

- 新手默认显示解释；
- 完成学习后缩短；
- 熟练玩家可展开；
- 关键状态始终保留。

### Reason

该冲突可以通过熟练度分层同时满足，不需要二选一。

### Validation

- 新手能否解释资源；
- 熟练玩家操作时间；
- 提示关闭率；
- 错误率。

---

## 14. 示例二：永久职业选择 vs 实验空间

### Conflict

```text
永久职业选择强化身份和承诺，
但玩家可能在不了解后果时长期后悔。
```

### Principles

- Choice and Consequence；
- Player First；
- Progression；
- Ethical Design。

### Decision

采用：

```text
训练阶段免费体验
→ 第一章节阶段性锁定
→ 中期提供有成本重置
→ 重大叙事身份不可完全抹除
```

### Reason

保留承诺和身份，同时降低信息不足导致的永久损失。

### Validation

- 玩家是否理解职业差异；
- 重置率；
- 攻略依赖；
- 后悔反馈；
- 构筑分布。

---

## 15. 示例三：限时活动收入 vs 玩家时间

### Conflict

```text
短期活动和每日任务可能提高收入与活跃，
但会制造错过恐惧和生活压力。
```

### Principles

- Progression and Motivation；
- Pacing and Rhythm；
- Ethical Design；
- Player First。

### Non-Negotiable Boundaries

- 不使用虚假倒计时；
- 核心能力不可永久错过；
- 儿童账户不使用高压随机付费。

### Decision

采用：

- 活动期累计任务；
- 关键内容长期保留；
- 表达奖励限时但可返场；
- 活动窗口覆盖多个周末；
- 支持补做；
- 通知可关闭。

### Validation

同时监控：

- 活动参与；
- 付费；
- 任务完成；
- 压力反馈；
- 通知关闭；
- 退款；
- 回归率。

---

## 16. 何时升级评审

以下情况应升级为跨职能评审：

- 涉及真实付费；
- 涉及概率；
- 涉及儿童；
- 涉及隐私与数据；
- 涉及永久资产损失；
- 影响多人公平；
- 改变核心体验；
- 影响多个系统；
- 难以回退；
- 团队证据明显冲突。

建议参与者：

- Design；
- Product；
- Engineering；
- UX Research；
- Data；
- Legal / Compliance；
- Accessibility；
- Community / Support；
- Security / Privacy。

按风险选择参与范围，不要求所有冲突都召开大型会议。

---

## 17. 何时重新打开决策

出现以下情况时自动复审：

- 失败标准达到；
- 玩家群体发生变化；
- 商业模式改变；
- 新平台上线；
- 法规变化；
- 投诉、退款或误购上升；
- 新证据推翻原假设；
- 临时例外开始扩散；
- 核心体验方向改变；
- 实施成本显著变化。

---

## 18. 冲突决策质量检查

- [ ] 双方原则均被准确描述；
- [ ] 没有把一方描述成“阻碍项目”；
- [ ] 目标玩家和情境明确；
- [ ] 已确认是否可以同时满足；
- [ ] 不可突破边界明确；
- [ ] 至少比较两个方案和不做方案；
- [ ] 玩家成本和长期影响已评估；
- [ ] 选择理由不是职位或个人偏好；
- [ ] 接受的代价明确；
- [ ] 缓解措施明确；
- [ ] 成功、失败和复审标准明确；
- [ ] 高风险冲突经过适当跨职能评审；
- [ ] 决策记录可供未来团队理解。

---

## 19. 原则冲突索引

| Conflict | Primary Documents |
|---|---|
| Clarity vs Discovery | Clarity, Choice |
| Simplicity vs Depth | Simplicity and Depth |
| Freedom vs Core Experience | Core Experience, Choice |
| Challenge vs Accessibility | Challenge, Accessibility |
| Consistency vs Novelty | Consistency, Simplicity |
| Retention vs Player Time | Progression, Pacing, Ethical |
| Monetization vs Trust | Ethical Design |
| Automation vs Agency | Simplicity, Progression, Accessibility |
| Fairness vs Randomness | Challenge, Choice |
| Expression vs Balance | Choice, Progression |
| Feedback vs Interruption | Clarity, Pacing |
| Platform Unity vs Convention | Consistency, Accessibility |

---

## 20. Related Documents

- [Philosophy README](./README.md)
- [Glossary](./glossary.md)
- [Principle Map](./principle-map.md)
- [Design Principles](./foundation/design-principles.md)
- [Ethical Design](./responsibility/ethical-design.md)
- [Iteration and Validation](./validation/iteration-and-validation.md)
