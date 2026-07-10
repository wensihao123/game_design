# Decision Records（决策记录）

> 状态：草案  
> 适用范围：通用的软件、游戏、媒体与数字产品项目  
> 目的：定义如何记录重要产品、设计、技术、流程、工具和运营决策，使决策背景、选项、原因、后果和复审条件能够长期被追踪。

---

## 1. 文档目的

Decision Records 用于保存项目中的重要决策，确保团队能够回答：

- 为什么选择了这个方案；
- 当时有哪些替代方案；
- 决策基于哪些约束和假设；
- 谁做出了决策；
- 决策带来了哪些后果；
- 哪些风险被接受；
- 什么情况下需要重新评估；
- 旧方案为什么被放弃；
- 当前实现是否仍然符合原始决策。

决策记录不是会议纪要，也不是聊天记录归档。

它的核心目标是：

> 将重要决策从个人记忆和临时讨论中抽离出来，形成可追踪、可复审、可替代的长期项目知识。

---

## 2. 什么是 Decision Record

Decision Record 是对一个重要决策的结构化记录。

它通常包含：

```text
Context
Problem
Constraints
Options
Decision
Rationale
Consequences
Risks
Follow-up
Review Trigger
Status
```

---

## 3. 什么时候需要记录决策

以下情况通常值得创建 Decision Record：

- 技术栈选择；
- 引擎选择；
- 架构模式；
- 数据格式；
- 存档版本策略；
- 第三方依赖；
- 数据库选择；
- API 设计；
- 构建与发布策略；
- 分支策略；
- 平台支持；
- 性能目标；
- 安全策略；
- 资产生产方式；
- AI 工具使用；
- 外包与授权；
- 产品方向变化；
- 重要 Scope 决定；
- 放弃某个方案；
- 高风险例外；
- 流程变更；
- 长期维护策略；
- 不可逆或高成本决策。

---

## 4. 哪些内容不需要独立 Decision Record

以下内容通常不需要单独记录：

- 局部实现细节；
- 可轻易回滚的小修改；
- 已由编码规范明确规定的选择；
- 低风险命名调整；
- 一次性临时操作；
- 不影响后续工作的个人偏好；
- 已经由其他正式文档完整覆盖的稳定规则。

判断标准：

```text
未来有人可能会问“为什么当时这样做”吗？
```

如果答案是“很可能”，则值得记录。

---

## 5. 决策记录与其他文档的关系

### 5.1 与 Design Document

Design Document 说明：

- 要做什么；
- 如何工作；
- 用户体验是什么。

Decision Record 说明：

- 为什么采用这个方案；
- 为什么没有采用其他方案。

---

### 5.2 与 Technical Design

Technical Design 说明：

- 系统如何设计；
- 模块如何协作；
- 数据如何流动。

Decision Record 说明：

- 为什么选择这种架构；
- 选择时有哪些约束；
- 替代方案是什么。

---

### 5.3 与 Meeting Notes

Meeting Notes 记录：

- 讨论过程；
- Action；
- 参与人；
- 临时结论。

Decision Record 只保留：

- 最终决策；
- 关键依据；
- 后果；
- 复审条件。

---

### 5.4 与 Issue / Ticket

Issue 用于跟踪执行。

Decision Record 用于保存长期决策知识。

Issue 可以链接到 Decision Record，但不应替代它。

---

## 6. 决策生命周期

```text
PROPOSED
  ↓
UNDER REVIEW
  ↓
ACCEPTED
  ↓
ACTIVE
  ↓
SUPERSEDED / DEPRECATED / REJECTED
```

补充状态：

```text
DRAFT
CANCELLED
NEEDS REVIEW
```

---

## 7. 状态定义

### 7.1 Draft

内容尚未完整。

---

### 7.2 Proposed

方案已提出，等待正式评审。

---

### 7.3 Under Review

正在评估选项、风险和影响。

---

### 7.4 Accepted

决策已批准。

---

### 7.5 Active

决策当前有效，并正在指导项目。

---

### 7.6 Rejected

该提案未被采用。

Rejected 记录仍应保留，以避免重复讨论。

---

### 7.7 Superseded

已被新的决策替代。

必须链接到替代记录。

---

### 7.8 Deprecated

不再推荐使用，但仍可能有旧系统依赖。

---

### 7.9 Cancelled

问题不再需要决策，或决策流程终止。

---

## 8. 决策记录编号

推荐使用稳定编号：

```text
ADR-0001
ADR-0002
ADR-0003
```

ADR 表示 Architecture Decision Record，但也可扩展为通用 Decision Record。

如果不仅记录架构，可以使用：

```text
DR-0001
```

建议项目统一一种编号方式。

---

## 9. 文件命名

推荐：

```text
adr-0001-use-godot.md
adr-0002-version-save-data.md
adr-0003-adopt-short-lived-feature-branches.md
```

或：

```text
0001-use-godot.md
0002-version-save-data.md
```

原则：

- 编号唯一；
- 标题清楚；
- 使用 kebab-case；
- 文件名稳定；
- 不因状态变化重命名。

---

## 10. 决策标题

标题应表达实际决定。

推荐：

```text
Use Godot as the Primary Game Engine
Version Save Data Explicitly
Adopt Short-lived Feature Branches
Use Resources for Static Game Configuration
```

不推荐：

```text
Engine Decision
Save System
Branching
Architecture
```

---

## 11. 核心结构

推荐结构：

```text
Title
Status
Date
Owner
Decision Makers
Context
Problem
Constraints
Options
Decision
Rationale
Consequences
Risks
Follow-up
Review Trigger
Related Records
```

---

## 12. Context：背景

Context 应说明：

- 当前系统状态；
- 为什么需要决策；
- 已知问题；
- 业务背景；
- 技术背景；
- 时间背景；
- 相关历史；
- 触发事件。

Context 应足够让未来读者理解当时情境。

---

## 13. Problem：问题

Problem 应描述：

> 需要解决的具体决策问题是什么。

不合格：

```text
我们需要一个更好的架构。
```

合格：

```text
当前角色行为同时依赖场景树路径和多个 Autoload，导致测试困难、场景复用受限，并在场景重构时频繁失效。
```

---

## 14. Constraints：约束

约束可能包括：

- 时间；
- 人力；
- 平台；
- 成本；
- 性能；
- 兼容性；
- 数据；
- 法律；
- 安全；
- 团队经验；
- 工具；
- 发布窗口；
- 第三方依赖；
- 维护成本。

约束应区分：

```text
Hard Constraint
Soft Constraint
Assumption
Preference
```

---

## 15. Assumptions：假设

决策经常依赖假设。

例如：

```text
未来 12 个月内不需要多人实时同步。
目标平台主要是 Windows 和 macOS。
团队规模保持在 1–3 人。
```

假设一旦失效，可能触发复审。

---

## 16. Options：选项

每个重要选项应包含：

```text
Description
Advantages
Disadvantages
Cost
Risk
Reversibility
Fit
```

不应只记录被选中的方案。

记录替代方案的价值在于：

- 避免重复讨论；
- 理解取舍；
- 识别未来替换条件；
- 解释为什么不是另一种方式。

---

## 17. Option 结构示例

```markdown
### Option A：使用 Resource 驱动配置

优点：
- 与 Godot 原生工作流一致；
- Inspector 可编辑；
- 易于复用；
- 支持版本控制。

缺点：
- 运行时修改需要注意共享引用；
- 大规模数据编辑能力有限；
- 跨工具使用不方便。

风险：
- 资源被意外修改；
- 数据校验不足。

可逆性：
- 中等。
```

---

## 18. Decision：决策

Decision 应明确说明：

```text
选择什么
适用范围
何时生效
谁负责
有哪些例外
```

不推荐：

```text
暂时先这样。
```

推荐：

```text
静态玩法配置统一使用自定义 Resource 表示。
运行时状态不得直接写回原始 Resource，需使用实例状态对象或 duplicate 后的副本。
该规则适用于角色、技能、道具和敌人配置。
```

---

## 19. Rationale：理由

Rationale 应解释：

- 为什么选择该方案；
- 哪些因素权重最高；
- 为什么替代方案不适合；
- 哪些取舍被接受；
- 哪些目标被优先。

不要只写：

```text
因为这是最好的方案。
```

---

## 20. Consequences：后果

每个决策都会带来后果。

应记录：

### Positive Consequences

- 获得什么；
- 简化什么；
- 解锁什么。

### Negative Consequences

- 增加什么成本；
- 接受什么限制；
- 放弃什么能力。

### Neutral Consequences

- 需要配套哪些规则；
- 需要更新哪些文档；
- 需要改变哪些流程。

---

## 21. Risk：风险

风险应包含：

```text
Risk
Probability
Impact
Mitigation
Owner
Trigger
```

决策不是因为没有风险才被接受，而是因为风险可被理解和管理。

---

## 22. Reversibility：可逆性

建议标记：

```text
Easy to Reverse
Moderately Reversible
Difficult to Reverse
Effectively Irreversible
```

高不可逆决策需要更严格评审。

---

## 23. Cost of Change：变更成本

应考虑：

- 代码迁移；
- 数据迁移；
- 资产迁移；
- 培训；
- 工具；
- 兼容；
- 发布；
- 用户影响；
- 外部合同；
- 长期维护。

---

## 24. Review Trigger：复审触发条件

决策不一定永久有效。

应定义复审条件，例如：

```text
当项目需要支持移动端时复审。
当存档数量超过 10,000 条时复审。
当团队规模超过 5 人时复审。
当当前插件停止维护时复审。
当性能低于目标时复审。
```

---

## 25. Review Date：复审日期

对于快速变化领域，可以设定日期：

```text
Review Date: 2027-01-01
```

但触发条件通常比固定日期更重要。

---

## 26. Decision Owner

Owner 负责：

- 推动评审；
- 维护记录；
- 跟踪后果；
- 触发复审；
- 更新状态；
- 关联替代记录。

Owner 不一定是唯一决策人。

---

## 27. Decision Makers

可以记录：

- Product Owner；
- Tech Lead；
- Design Lead；
- Security；
- Operations；
- Project Owner。

个人项目也可以只有一人，但仍应明确决策角色。

---

## 28. Stakeholders

Stakeholder 是被决策影响但不一定做最终决定的人。

例如：

- 开发；
- 设计；
- QA；
- 运营；
- 用户；
- 外包；
- 平台合作方。

---

## 29. 评审等级

建议根据影响分级：

```text
Lightweight
Standard
High Impact
Critical
```

---

## 30. Lightweight Decision

适用于：

- 影响局部；
- 易回滚；
- 低成本；
- 无外部契约；
- 无数据风险。

可以使用简化模板。

---

## 31. Standard Decision

适用于：

- 跨模块；
- 中等成本；
- 影响多人协作；
- 有长期维护影响。

---

## 32. High Impact Decision

适用于：

- 架构；
- 数据格式；
- 平台；
- 第三方依赖；
- 安全；
- 发布；
- 高迁移成本；
- 多团队影响。

需要完整选项和风险分析。

---

## 33. Critical Decision

适用于：

- 不可逆；
- 法律与合规；
- 大规模数据；
- 重大财务影响；
- 核心平台锁定；
- 高安全风险。

需要更高审批和证据标准。

---

# 34. 决策流程

```text
Identify
→ Frame Problem
→ Gather Constraints
→ Generate Options
→ Evaluate
→ Decide
→ Record
→ Communicate
→ Implement
→ Monitor
→ Review
```

---

## 35. Identify：识别决策

识别信号：

- 团队反复争论同一问题；
- 多个实现方向并存；
- 技术债务持续扩大；
- 新需求与现有架构冲突；
- 依赖需要锁定；
- 需要长期规范；
- 方案不可逆；
- 需要明确例外。

---

## 36. Frame Problem：定义问题

一个好的决策问题应：

- 中立；
- 不预设答案；
- 范围明确；
- 可比较选项；
- 关联实际目标。

不推荐：

```text
为什么我们应该使用微服务？
```

推荐：

```text
当前系统是否需要拆分为独立部署服务，以解决扩展、所有权和发布耦合问题？
```

---

## 37. Gather Evidence：收集证据

证据可以包括：

- 原型；
- Benchmark；
- 用户反馈；
- 故障数据；
- 成本估算；
- 依赖文档；
- 性能测试；
- 兼容性测试；
- 法律意见；
- 团队经验；
- 维护数据。

应区分：

```text
Evidence
Assumption
Opinion
Preference
```

---

## 38. Evaluate：评估

可以使用评估矩阵。

示例：

| Criteria | Weight | Option A | Option B | Option C |
|---|---:|---:|---:|---:|
| 实现成本 | 3 | 3 | 2 | 1 |
| 可维护性 | 5 | 4 | 5 | 3 |
| 性能 | 4 | 4 | 3 | 5 |
| 可逆性 | 2 | 5 | 3 | 2 |

分数只用于辅助，不应代替判断。

---

## 39. Trade-off：权衡

应明确：

```text
我们优先什么？
我们接受什么损失？
哪些目标不能同时满足？
```

例如：

```text
优先降低维护成本，接受少量运行时开销。
```

---

## 40. Decide：做出决策

决策应有明确结论。

避免：

```text
大家都觉得差不多。
以后再看。
先做着。
```

如果暂时无法决定，应明确记录：

```text
Decision Deferred
Missing Evidence
Next Experiment
Decision Date
Owner
```

---

## 41. Communicate：沟通

决策被接受后，应：

- 更新状态；
- 通知受影响人员；
- 链接到相关文档；
- 更新执行计划；
- 更新规范；
- 更新代码或架构；
- 标记旧方案。

---

## 42. Implement：实施

Decision Record 不应停留在文档层面。

应创建：

- Feature；
- Task；
- Migration；
- Refactor；
- Documentation Update；
- Tooling；
- Training；
- Cleanup。

---

## 43. Monitor：观察后果

应检查：

- 实际收益是否出现；
- 风险是否发生；
- 成本是否符合预期；
- 约束是否变化；
- 假设是否失效；
- 是否出现意外后果。

---

## 44. Review：复审

复审可能产生：

```text
Keep
Amend
Supersede
Deprecate
Reverse
```

---

# 45. Amend：修订

小型澄清可以直接更新原记录，但必须保留变更历史。

适合：

- 补充例外；
- 澄清范围；
- 更新 Owner；
- 补充后果；
- 修正非核心错误。

---

# 46. Supersede：替代

如果核心决策发生变化，应创建新的 Decision Record。

旧记录标记：

```text
Superseded by ADR-00XX
```

新记录应链接旧记录。

不要直接重写历史，导致原决策消失。

---

# 47. Reverse：反转决策

反转也应作为新决策记录。

应说明：

- 原决策为何不再适用；
- 哪些假设失效；
- 迁移成本；
- 回滚计划；
- 新方案。

---

# 48. Rejected Decision

被拒绝的方案也值得保留。

需要说明：

- 为什么被拒绝；
- 哪些条件下可能重新考虑；
- 是否有局部可用部分。

---

# 49. Exception Record

当项目需要违反既有规则时，可以创建例外记录。

应包含：

```text
Rule
Reason
Scope
Risk
Owner
Expiry
Removal Plan
```

例外必须：

- 有明确范围；
- 有截止时间；
- 有清理计划；
- 不自动变成新标准。

---

# 50. Temporary Decision

临时决策应标记：

```text
Temporary
Expires On
Replacement Plan
```

否则临时方案很容易永久化。

---

# 51. Decision Debt

Decision Debt 包括：

- 重要决定未记录；
- 多个方案并存；
- 旧决定未替代；
- 例外无截止时间；
- 假设未复审；
- 决策与实现不一致；
- 决策状态不清。

Decision Debt 会导致：

- 重复争论；
- 架构漂移；
- 维护成本上升；
- 决策依赖个人记忆。

---

# 52. 决策目录

推荐：

```text
docs/
└── decisions/
    ├── README.md
    ├── adr-0001-use-godot.md
    ├── adr-0002-version-save-data.md
    └── archive/
```

---

# 53. 决策索引

README 应包含：

| ID | Title | Status | Date | Owner |
|---|---|---|---|---|

还可以按领域分类：

- Architecture；
- Product；
- Process；
- Data；
- Security；
- Tooling；
- Release；
- Asset。

---

# 54. Decision Record 标准模板

```markdown
# ADR-XXXX：决策标题

> 状态：Proposed  
> 日期：  
> Owner：  
> 决策人：  
> 影响范围：  
> 复审触发条件：  
> 替代记录：

---

## 1. Context

## 2. Problem

## 3. Goals

## 4. Non-goals

## 5. Constraints

## 6. Assumptions

## 7. Options

### Option A

#### Description

#### Advantages

#### Disadvantages

#### Risks

#### Cost

#### Reversibility

### Option B

#### Description

#### Advantages

#### Disadvantages

#### Risks

#### Cost

#### Reversibility

## 8. Decision

## 9. Rationale

## 10. Consequences

### Positive

### Negative

### Neutral

## 11. Risks and Mitigations

| Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|

## 12. Implementation Actions

| Action | Owner | Status |
|---|---|---|

## 13. Review Trigger

## 14. Related Records

## 15. Change Log
```

---

# 55. Lightweight Decision 模板

```markdown
# DR-XXXX：决策标题

> 状态：
> 日期：
> Owner：

## Context

## Decision

## Why

## Consequences

## Review Trigger
```

---

# 56. Exception Decision 模板

```markdown
# Exception Record

> ID：
> 状态：
> Owner：
> 开始日期：
> 到期日期：

## Rule Being Bypassed

## Reason

## Scope

## Risks

## Mitigation

## Removal Plan

## Approval

## Final Outcome
```

---

# 57. Decision Review 模板

```markdown
# Decision Review

> Decision：
> Reviewer：
> Date：

## Problem Framing

- [ ] 问题中立；
- [ ] 范围明确；
- [ ] 不预设答案。

## Evidence

- [ ] 关键证据充分；
- [ ] 假设已标记；
- [ ] 数据来源可信；
- [ ] 不确定性透明。

## Options

- [ ] 有真实替代方案；
- [ ] 优缺点完整；
- [ ] 成本已评估；
- [ ] 风险已评估；
- [ ] 可逆性已评估。

## Decision

- [ ] 结论明确；
- [ ] 适用范围明确；
- [ ] 例外明确；
- [ ] Owner 明确；
- [ ] 生效条件明确。

## Consequences

- [ ] 正面后果明确；
- [ ] 负面后果明确；
- [ ] Follow-up 明确；
- [ ] 复审条件明确。

## Result

- [ ] Accepted
- [ ] Accepted with Conditions
- [ ] Needs Revision
- [ ] Deferred
- [ ] Rejected
```

---

# 58. 决策检查清单

```markdown
## 必要性

- [ ] 决策足够重要；
- [ ] 不只是局部实现细节；
- [ ] 未来需要解释原因；
- [ ] 不与已有记录重复。

## 问题

- [ ] Context 清楚；
- [ ] Problem 清楚；
- [ ] Goals 清楚；
- [ ] Non-goals 清楚；
- [ ] 约束清楚。

## 选项

- [ ] 至少有一个真实替代方案；
- [ ] 优点已记录；
- [ ] 缺点已记录；
- [ ] 成本已记录；
- [ ] 风险已记录；
- [ ] 可逆性已记录。

## 决策

- [ ] 结论明确；
- [ ] 适用范围明确；
- [ ] 例外明确；
- [ ] 生效时间明确；
- [ ] Owner 明确。

## 后果

- [ ] 正面后果明确；
- [ ] 负面后果明确；
- [ ] Follow-up 已创建；
- [ ] 迁移影响已记录；
- [ ] 文档影响已记录。

## 复审

- [ ] 假设已记录；
- [ ] 复审触发条件明确；
- [ ] 替代关系明确；
- [ ] 状态明确；
- [ ] 索引已更新。
```

---

# 59. 常见失败模式

## 59.1 只记录结论

表现：

```text
决定使用 Godot。
```

问题：

- 不知道为什么；
- 不知道替代方案；
- 不知道约束；
- 无法判断未来是否仍适用。

---

## 59.2 记录成会议纪要

问题：

- 讨论冗长；
- 结论不清；
- 重要信息难查；
- 长期价值低。

---

## 59.3 只写被选中的方案

结果：

- 替代方案被反复提出；
- 无法理解取舍；
- 后续复审缺少依据。

---

## 59.4 决策被修改但历史被覆盖

结果：

- 不知道何时改变；
- 不知道为什么改变；
- 无法追踪迁移过程。

应创建新的 Superseding Record。

---

## 59.5 每个小问题都创建 ADR

结果：

- 数量过多；
- 维护成本高；
- 真正重要决策被淹没。

---

## 59.6 决策没有 Owner

结果：

- 无人实施；
- 无人复审；
- 无人更新状态。

---

## 59.7 假设未记录

结果：

- 未来条件变化后仍沿用旧方案；
- 不知道何时应复审。

---

## 59.8 负面后果被隐藏

表现：

- 记录只用于证明方案正确；
- 不承认成本和限制。

高质量决策应真实记录取舍。

---

## 59.9 临时方案没有到期时间

结果：

- 临时方案永久存在；
- 技术债务不可见；
- 没有清理行动。

---

## 59.10 决策与实现不一致

原因：

- 实施未完成；
- 后续改动未更新记录；
- 实际行为漂移。

应在 Review 中检查一致性。

---

## 59.11 被拒绝方案完全删除

结果：

- 同样讨论重复发生；
- 历史原因丢失。

---

## 59.12 使用决策记录代替实际验证

文档不能代替：

- Prototype；
- Benchmark；
- Test；
- User Research；
- Security Review。

重要决策应基于证据。

---

# 60. 决策质量评估

可以从以下维度评估：

- 问题是否正确；
- 证据是否充分；
- 选项是否真实；
- 取舍是否透明；
- 结论是否清楚；
- 可逆性是否理解；
- 后果是否完整；
- 实施是否完成；
- 复审条件是否有效。

---

# 61. Decision Metrics

可参考：

- Active Decision 数量；
- 无 Owner 决策数量；
- 无复审条件决策数量；
- Superseded 数量；
- 临时决策过期数量；
- 决策到实施的 Lead Time；
- 重复讨论次数；
- 决策反转率；
- 决策相关 Incident 数；
- 未记录重大决策数。

这些指标用于改善：

- 决策透明度；
- 执行一致性；
- 知识保留；
- 复审质量。

不应追求：

- 决策数量越多越好；
- 决策永不改变；
- 所有决策一次正确。

---

# 62. 决策维护节奏

## 每次重大设计

- 检查是否需要 ADR；
- 关联 Feature；
- 关联 Technical Design。

## 每个 Milestone

- 检查是否有未记录决策；
- 检查临时例外；
- 检查实施状态。

## 每个 Release

- 检查重大行为变化；
- 检查数据和兼容性决策；
- 更新替代状态。

## 每季度或每半年

- 检查无 Owner；
- 检查过期假设；
- 检查复审触发条件；
- 检查 Deprecated 和 Superseded 记录；
- 更新索引。

---

# 63. 决策完成定义

一个 Decision Record 可以被视为完成，当：

- Context 清楚；
- Problem 清楚；
- 约束和假设已记录；
- 真实选项已比较；
- Decision 明确；
- Rationale 明确；
- 正面与负面后果已记录；
- 风险和缓解方案已记录；
- Owner 明确；
- 实施行动已创建；
- 复审条件明确；
- 状态已更新；
- 索引已更新；
- 相关文档已链接。

---

# 64. 最终原则

Decision Records 的本质不是：

> 给已经做出的选择补一份形式化文档。

它的本质是：

> 保存重要决策的背景、证据、选项、取舍和后果，让未来的人能够理解为什么这样做，并知道什么时候应该重新评估。

一个健康的决策记录体系应做到：

- 只记录重要决策；
- 问题定义中立；
- 选项真实；
- 证据透明；
- 取舍明确；
- 负面后果不隐藏；
- 状态清楚；
- Owner 清楚；
- 可逆性明确；
- 实施可追踪；
- 复审条件明确；
- 替代历史完整。
