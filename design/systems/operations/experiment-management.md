# Experiment Management（实验管理系统）

> Status: V1  
> Category: Operations  
> Path: `design/systems/operations/experiment-management.md`  
> Owner: TBD  
> Reviewers: Product / Design / Engineering / Data / Analytics / Privacy / Legal / Trust and Safety / Accessibility / Live Operations / QA / Finance / Support  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: Critical  
> Dependencies: Analytics and Telemetry, Live Operations, Settings and Preferences, Notification and Reminders, Versioning and Migration, System Design Framework, Integration Rules  
> Affected Systems: Core Loop, Progression, Reward, Content, Social, Matchmaking, Moderation, Monetization, Offers and Pricing, Entitlement, Support

---

## 1. System Summary

Experiment Management 系统负责定义：

```text
什么问题值得做实验；
实验如何提出、评审、分流、运行、监控、分析和结束；
Assignment 与 Exposure 如何区分；
谁进入实验、谁被排除；
样本量、持续时间和统计方法如何确定；
Primary Metric、Guardrail 和 Stop Condition 如何预注册；
多个实验如何避免冲突；
长期 Holdout、网络效应、季节性和版本变化如何处理；
高风险实验如何经过法律、隐私、伦理和安全审查；
实验结果如何转化为产品决策，而不是只追求显著性；
实验如何归档、复现、迁移和清理。
```

该系统通常覆盖：

- Experiment Proposal；
- Hypothesis；
- Experiment Registry；
- Assignment；
- Variant；
- Control；
- Holdout；
- Eligibility；
- Exclusion；
- Exposure；
- Outcome；
- Sample Ratio；
- Sample Size；
- Statistical Power；
- Minimum Detectable Effect；
- Primary Metric；
- Secondary Metric；
- Guardrail；
- Stop Condition；
- Ramp；
- Mutual Exclusion；
- Network Effect；
- Sequential Testing；
- Early Stopping；
- Long-Term Holdout；
- Experiment Analysis；
- Decision；
- Rollout；
- Cleanup；
- Archive；
- Experiment Debt。

健康的实验管理系统应让团队做到：

```text
先定义问题，再决定是否实验；
先定义结果，再看数据；
知道谁真的看到了 Variant；
知道哪些玩家不该进入实验；
不因显著性而忽略伤害；
不把所有产品决策都变成 A/B Test；
不在高风险领域做无边界试验；
实验结束后形成明确结论和行动；
相同问题可以被复现，而不是每次重新发明。
```

---

## 2. Purpose

### 2.1 Player Value

玩家通常不会直接操作实验系统，但会间接受益于：

- 更可靠的产品改进；
- 更少未经验证的全量改动；
- 更快发现伤害性变化；
- 更公平的商业、匹配和奖励规则；
- 更稳定的设置和可访问性；
- 更少无意义的频繁变动；
- 更少以增长为唯一目标的操纵性设计。

### 2.2 Product Value

实验系统帮助团队：

- 验证因果关系；
- 比较多个方案；
- 降低发布风险；
- 测量真实效果；
- 识别副作用；
- 形成可复用知识；
- 避免重复实验；
- 支持灰度与长期 Holdout；
- 支持网络和群体实验；
- 提高决策透明度。

### 2.3 Why This System Exists

缺少统一实验管理时，常见问题包括：

```text
实验被称为 A/B，实际上只是前后对比；
Control 组受到其他 Feature Flag 影响；
Assignment 有记录但玩家未实际 Exposure；
同一玩家跨设备进入不同 Variant；
样本量不足却提前宣布成功；
连续查看结果直到显著；
只看 Revenue，不看 Refund、Support 和 Safety；
实验中途改 Primary Metric；
不显著后切分大量 Cohort 寻找故事；
高风险价格实验缺乏法律和伦理审查；
实验结束后 Flag、事件和 Dashboard 没人清理；
最终结果无法回答“玩家实际看到了什么”。
```

---

## 3. Non-Goals

Experiment Management 不负责：

- 替代用户研究、QA、可访问性评审和安全测试；
- 把所有设计问题都变成实验；
- 用统计显著性替代产品价值；
- 用实验绕过法律、隐私、平台和财务审查；
- 让模型自动决定高风险 Variant 全量上线；
- 在无 Exposure 的情况下分析 Outcome；
- 在实验结束后永久保留临时分支；
- 把灰度发布和因果实验混为一谈；
- 根据儿童、危机、健康或处罚状态进行商业实验；
- 用实验验证本应直接满足的基础正确性和安全性。

---

## 4. Governing Principles

### 4.1 Iteration and Validation

参考：

- `../../philosophy/validation/iteration-and-validation.md`

应用原则：

- 假设先于数据；
- Success 和 Failure 同时定义；
- 定量与定性结合；
- 不显著也是有效结论；
- 结果必须回写设计系统。

### 4.2 Player First Design

参考：

- `../../philosophy/foundation/player-first-design.md`

应用原则：

- 玩家价值是实验核心；
- 不以玩家伤害换取样本；
- 高风险实验先小范围；
- 可逆性和恢复优先；
- 实验失败不应破坏玩家状态。

### 4.3 Challenge and Fairness

参考：

- `../../philosophy/experience/challenge-and-fairness.md`

应用原则：

- 正式竞争规则不随意随机分组；
- 同场玩家使用一致规则；
- Matchmaking、Rating、Reward 和 Probability 实验有更高门槛；
- 网络效应和实验污染必须处理。

### 4.4 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 不测试虚假倒计时、隐藏续费和削弱退款入口；
- 不利用失败、孤独、危机或儿童脆弱性；
- 不用处罚和安全标签进行商业实验分群；
- 负面结果必须报告。

### 4.5 Consistency and Coherence

参考：

- `../../philosophy/long-term/consistency-and-coherence.md`

应用原则：

- Assignment 稳定；
- Variant 有版本；
- Exposure 与 Outcome 使用统一 Experiment ID；
- Experiment、Feature Flag、Live Config 和 Analytics 边界明确；
- 实验结束后清理临时状态。

---

## 5. System Boundary

### 5.1 Inputs

系统接收：

- Experiment Proposal；
- Product Question；
- Hypothesis；
- Variant Definition；
- Eligibility / Exclusion；
- Assignment Key；
- Allocation；
- Sample Plan；
- Primary Metric；
- Guardrail；
- Exposure Definition；
- Feature Flag；
- Live Config；
- Version；
- Platform；
- Region；
- Consent；
- Age / Family；
- Safety Restrictions；
- Legal / Privacy / Ethical Approval；
- Support Readiness；
- Rollback Plan。

### 5.2 Outputs

系统产生：

- Experiment Definition；
- Assignment；
- Variant；
- Exposure Record；
- Experiment State；
- Ramp Plan；
- Conflict Result；
- Sample Ratio Report；
- Guardrail Alert；
- Stop Decision；
- Experiment Dataset；
- Analysis Report；
- Product Decision；
- Rollout Recommendation；
- Cleanup Plan；
- Archive Record。

### 5.3 Owned State

系统拥有：

- Experiment Registry；
- Experiment Definition；
- Hypothesis；
- Variant Definition；
- Allocation；
- Assignment State；
- Eligibility Rule；
- Exclusion Rule；
- Mutual Exclusion Group；
- Exposure Definition；
- Ramp State；
- Stop State；
- Decision State；
- Cleanup State；
- Experiment Version；
- Experiment History。

### 5.4 Out of Scope

系统不直接：

- 修改资产、支付、Rating 或处罚；
- 覆盖家长控制与隐私设置；
- 绕过 Entitlement 和版本兼容；
- 将 Assignment 当作 Exposure；
- 将 Analytics Event 作为业务事务权威。

---

## 6. Core Entities and Concepts

| Entity / Concept | Definition | Owner | Lifetime | Notes |
|---|---|---|---|---|
| Experiment Definition | 一次实验的完整定义 | Experiment Ops | 版本级 | 唯一 Experiment ID |
| Hypothesis | 可证伪的预期因果关系 | Product / Design | 实验级 | 包含机制 |
| Variant | 参与比较的体验版本 | Domain / Experiment | 版本级 | Control 也是 Variant |
| Assignment | 将实验单位稳定分配到 Variant | Experiment Service | 实验期 | 不等于 Exposure |
| Eligibility | 是否允许进入实验 | Domain / Experiment | 请求级 | 有 Reason Code |
| Exposure | 玩家实际经历实验差异 | Analytics / Experiment | 实验期 | Outcome 的起点 |
| Outcome | 实验关注的结果 | Analytics | 分析期 | 有时间窗 |
| Primary Metric | 主要决策指标 | Product / Analytics | 实验级 | 预注册 |
| Guardrail | 防止伤害的约束指标 | Domain Owner | 实验级 | 可触发暂停 |
| Ramp | Variant 流量扩大计划 | Live Ops / Experiment | 运行期 | 有 Gate |
| Mutual Exclusion Group | 防止实验互相污染的分组 | Experiment Ops | 长期 | 按 Surface / Domain |
| Holdout | 长期或短期保留的对照组 | Experiment Ops | 周期级 | 需退出策略 |
| Analysis Plan | 统计与解释方案 | Analytics | 实验级 | 运行前冻结 |
| Decision Record | 实验后形成的正式决策 | Product | 长期 | 含适用边界 |
| Cleanup State | 临时 Flag、代码和数据资产清理状态 | Engineering / Experiment | 至完成 | 有 SLA |

---

## 7. Experiment Types

### 7.1 A/B 与 A/B/n

适合比较两个或多个独立 Variant。

### 7.2 Multivariate / Factorial

用于多个因素及交互，但样本和解释成本更高。

### 7.3 Holdout

保留不接受某类变化的对照组，用于长期累积影响。

### 7.4 Canary

极小真实流量验证安全，不一定具备统计检验能力。

### 7.5 Switchback

按时间窗口切换规则，适合市场、队列和共享服务。

### 7.6 Cluster / Network

按 Party、Guild、Region、Server 或社交群体分配，处理相互影响。

### 7.7 Sequential

按照预注册的中途观察和停止规则运行。

### 7.8 Non-Inferiority / Equivalence

验证新方案是否不劣或足够等价。

### 7.9 Quasi-Experiment

无法随机时可使用 Difference-in-Differences、Interrupted Time Series 等方法，但必须明确因果限制。

---

## 8. Experiment Registry

Registry 是所有生产实验的单一目录。

### 8.1 Registry Fields

- Experiment ID；
- Name；
- Status；
- Domain；
- Owner；
- Hypothesis；
- Variants；
- Allocation；
- Start / End；
- Metrics；
- Risk；
- Regions / Platforms / Versions；
- Exclusions；
- Conflicts；
- Approvals；
- Result；
- Decision；
- Cleanup；
- Archive。

### 8.2 Registry Status

- Draft；
- Review；
- Approved；
- Scheduled；
- Running；
- Paused；
- Stopped；
- Analyzing；
- Decided；
- Cleaning；
- Archived；
- Invalidated。

### 8.3 No Unregistered Experiments

生产实验必须存在于 Registry，并能被 Support、Live Operations 和 Analytics 查询。

---

## 9. Experiment Definition Template

```markdown
## Experiment Definition

- Experiment ID:
- Name:
- Domain:
- Owner:
- Product Question:
- Hypothesis:
- Experiment Type:
- Control:
- Variants:
- Assignment Unit:
- Allocation:
- Eligibility:
- Exclusions:
- Exposure:
- Primary Metric:
- Secondary Metrics:
- Guardrails:
- Sample Size:
- Minimum Detectable Effect:
- Start:
- End:
- Ramp:
- Stop Conditions:
- Interactions:
- Versions:
- Platforms:
- Regions:
- Consent:
- Privacy:
- Safety:
- Legal:
- Accessibility:
- Rollback:
- Cleanup:
- Decision Rule:
```

---

## 10. Hypothesis and Product Question

### 10.1 Good Hypothesis

包含：

```text
对于哪类玩家，
在什么场景，
做什么变化，
为什么会改善什么结果，
同时不伤害哪些 Guardrail。
```

### 10.2 Example

```text
对于首次进入角色构筑页面的新玩家，
将默认信息结构从完整详情改为分层展开，
将提高首次成功完成构筑的比例，
同时不提高错误装备率和帮助页面访问率。
```

### 10.3 Falsifiability

必须定义什么结果会使假设不成立。

### 10.4 Not Every Question Is Experimental

以下问题通常更适合其他方法：

- 功能是否正确：QA；
- 是否可用：Usability Test；
- 是否安全：Security / Safety Review；
- 是否可访问：Accessibility Review；
- 是否承载峰值：Load Test；
- 玩家为什么困惑：Interview / Observation。

---

## 11. Variant Definition

每个 Variant 应包含：

- Variant ID；
- Name；
- Description；
- Config；
- Feature Flag；
- Content / Rule / UI Version；
- Expected Mechanism；
- Risk；
- Compatibility；
- Exposure Trigger；
- Rollback。

### 11.1 Variant Stability

运行中不能静默改变 Variant 语义。

若必须改变：

- 创建新 Version；
- 新 Phase；
- 或终止并重新启动实验。

### 11.2 Multiple Changes

一个 Variant 同时改变太多因素时，结果难以解释。

---

## 12. Experiment Lifecycle

```text
Idea
→ Draft
→ Review
→ Approved
→ Scheduled
→ Preflight
→ Ramping
→ Running
→ Monitoring
→ Stopped
→ Analyzing
→ Decided
→ Cleaning
→ Archived
```

异常状态：

```text
Rejected
Paused
Invalidated
Rolled Back
Aborted
Extended
```

### 12.1 State Rules

- `Preflight`：验证 Assignment、Exposure、Metric、Conflict、Support 和 Rollback；
- `Ramping`：逐步扩大流量；
- `Paused`：停止新 Assignment 或 Exposure；
- `Invalidated`：数据或实现问题导致结果不可用；
- `Cleaning`：移除临时 Flag、代码、事件和 Dashboard；
- `Archived`：保留全部定义、结果和局限。

---

## 13. Experiment Invariants

1. 每个实验有唯一 Experiment ID。
2. 每个 Variant 有稳定 ID 和版本。
3. Assignment 与 Exposure 分离。
4. Outcome 分析必须基于有效 Exposure。
5. 同一 Assignment Unit 在实验期保持稳定。
6. Primary Metric 和 Guardrail 在运行前定义。
7. 中途修改 Hypothesis、Primary Metric 或 Variant 必须新版本或重启。
8. 高风险实验必须有 Stop Condition 和 Kill Switch。
9. 实验失败不应破坏玩家状态。
10. Analytics Failure 不得被解释为实验成功。
11. 实验不能绕过 Consent、Age、Family、Safety 和 Legal。
12. Control 组不得被同类实验污染。
13. 实验结束后必须清理临时 Flag 和分支。
14. 不显著结果必须归档。
15. Price、Probability、Matchmaking 和 Moderation 实验有更高门槛。
16. 结果必须记录已知偏差和适用边界。
17. Support 必须能够识别实验状态。
18. 实验不能静默改变已确认 Checkout、Order、Reward 或 Entitlement。


---

## 14. Assignment Unit

### 14.1 Account-Level

适合大多数产品体验，可保证跨设备一致。

### 14.2 Device-Level

适合性能、渲染和设备兼容实验，但同一账号可能体验不同。

### 14.3 Session-Level

适合短期、可逆且不产生持久状态的差异。

### 14.4 Match-Level

适合比赛或服务器规则，但必须保证同场一致和竞争公平。

### 14.5 Party / Guild / Cluster-Level

适合社交、协作和网络效应实验。

### 14.6 Region / Server-Level

适合共享基础设施、市场或区域运营。

### 14.7 Time-Window-Level

适合 Switchback Experiment。

### 14.8 Selection Rule

选择最小但不会造成污染、冲突和体验不一致的 Assignment Unit。

---

## 15. Stable Assignment

### 15.1 Inputs

- Experiment ID；
- Assignment Key；
- Salt Version；
- Experiment Version；
- Eligibility；
- Allocation。

### 15.2 Deterministic Hashing

同一输入稳定分组。

### 15.3 Cross-Device

Account-Level Assignment 必须跨设备一致。

### 15.4 Anonymous to Logged-In

需定义：

- 是否保留匿名 Variant；
- 是否重新分配；
- 是否排除；
- 如何处理重复 Exposure。

### 15.5 Reassignment

仅在明确场景发生：

- 实验重启；
- Assignment Version 变化；
- 资格发生结构性变化；
- 数据修复。

任何 Reassignment 必须保留历史。

---

## 16. Allocation and Sample Ratio

### 16.1 Allocation Types

- Equal Allocation；
- Unequal Allocation；
- Ramp Allocation；
- Long-Term Holdout；
- Cluster Allocation。

### 16.2 Allocation Changes

运行中改变比例需记录：

- 时间；
- 原比例；
- 新比例；
- 原因；
- 版本；
- 分析影响。

### 16.3 Sample Ratio Mismatch

当实际样本比例与目标比例显著不一致时：

- 暂停 Ramp；
- 检查 Assignment；
- 检查 Eligibility；
- 检查 Exposure；
- 检查 Version / Platform；
- 必要时 Invalidate。

SRM 不能被当作普通噪声忽略。

---

## 17. Eligibility and Exclusion

### 17.1 Eligibility Dimensions

- Account State；
- Region；
- Platform；
- Version；
- Age；
- Family；
- Consent；
- Entitlement；
- Progression；
- Content；
- Subscription；
- Language；
- Device；
- Prior Exposure；
- Safety / Moderation；
- Conflicting Experiment。

### 17.2 Eligibility Timing

区分：

- Assignment Eligibility；
- Exposure Eligibility；
- Outcome Eligibility。

### 17.3 Common Exclusions

- Internal / QA / Synthetic Accounts；
- Bots；
- Unsupported Version；
- No Consent；
- Prior Exposure；
- Legal Restriction；
- Child Accounts in High-Risk Experiments；
- Active Critical Support Recovery；
- Conflicting Experiments；
- High-Risk Safety Cohorts。

### 17.4 Exclusion Bias

排除过多会降低外部有效性，必须在结果中说明。

---

## 18. Exposure

### 18.1 Assignment vs Exposure

Assignment 表示被分组。

Exposure 表示玩家实际经历了实验差异。

### 18.2 Valid Exposure Trigger

应在真正可感知或生效时触发，例如：

- 页面真实渲染 Variant；
- 新规则实际用于 Match；
- Offer 实际显示；
- Notification 实际呈现；
- 推荐逻辑真实返回结果。

### 18.3 Exposure Fields

- Experiment ID；
- Variant；
- Assignment ID；
- Exposure Time；
- Surface；
- Version；
- Session；
- Account Key；
- Eligibility；
- Experiment Stack。

### 18.4 Exposure Invariants

1. 页面预加载不等于 Exposure。
2. Exposure 需定义 First、Any 或 Count。
3. Outcome Window 从有效 Exposure 开始。
4. 多 Surface Exposure 必须区分。
5. Exposure 丢失会使分析降级或失效。

---

## 19. Outcome

### 19.1 Outcome Window

可以是：

- Immediate；
- Session；
- 24 Hours；
- 7 Days；
- 30 Days；
- Season；
- Subscription Cycle。

### 19.2 Attribution

Outcome 应关联：

- Experiment；
- Variant；
- Exposure；
- Assignment；
- Window；
- Metric Version。

### 19.3 Delayed Outcomes

长期指标可以在实验停止后继续观察，但不能在未定义时无限延长分析。

---

## 20. Metrics

### 20.1 Primary Metric

必须：

- 直接连接 Hypothesis；
- 单义；
- 可稳定计算；
- 不易被刷；
- 有 Owner 和 Version；
- 有 MDE；
- 有明确时间窗。

### 20.2 Secondary Metric

用于解释机制和观察额外影响。

看到结果后新发现的指标必须标记为 Exploratory。

### 20.3 Guardrail Metric

覆盖：

- Reliability；
- Performance；
- Safety；
- Privacy；
- Accessibility；
- Economy；
- Commercial；
- Support；
- Fairness；
- Long-Term Trust。

### 20.4 Hard and Soft Guardrails

- Hard：越界后自动暂停或停止；
- Soft：触发人工评审。

### 20.5 Diagnostic Metrics

用于解释 Exposure、Error、Performance、Version 和 Drop-Off，不作为主要成功结论。

---

## 21. Sample Size and Statistical Power

### 21.1 Inputs

- Baseline；
- Minimum Detectable Effect；
- Variance；
- Alpha；
- Power；
- Allocation；
- Variant Count；
- Expected Exposure；
- Attrition；
- Clustering；
- Network Effect；
- Multiple Testing。

### 21.2 MDE

应代表产品上有意义的最小变化，而不是为了缩短实验而随意设大。

### 21.3 Underpowered Experiments

不显著不能解释为无影响。

### 21.4 Overpowered Experiments

极大样本可能发现无实际价值的微小差异。

### 21.5 Practical Significance

同时报告：

- Effect Size；
- Confidence / Credible Interval；
- 产品意义；
- 玩家意义。

### 21.6 Rare Harm

稀有但严重风险不能只依赖实验样本发现，应使用 Safety Review 和主动监控。

---

## 22. Duration, Novelty and Carryover

### 22.1 Minimum Duration

应覆盖相关周期：

- Weekday / Weekend；
- Content Cycle；
- Notification Cycle；
- Matchmaking Population Cycle；
- Subscription Renewal；
- Seasonal Rhythm。

### 22.2 Maximum Duration

不能为了显著无限运行。

### 22.3 Novelty Effect

初期新鲜感可能暂时提升指标。

### 22.4 Learning Effect

复杂功能需要学习时间。

### 22.5 Carryover Effect

可能来自：

- 玩家学习新 UI；
- 获得永久奖励；
- 形成社交关系；
- 经济余额变化；
- 学会新规则。

持久影响实验通常应使用稳定 Assignment，而不是频繁切换。

---

## 23. Ramp Plan

推荐流程：

```text
Internal
→ 0.1%
→ 1%
→ 5%
→ 20%
→ 50%
```

### 23.1 Ramp Gates

每阶段检查：

- Assignment Ratio；
- Exposure；
- Error / Crash；
- Data Quality；
- Guardrails；
- Support；
- Safety；
- Performance；
- Version Compatibility。

### 23.2 Automatic Halt

达到 Hard Stop Condition 自动停止扩大。

### 23.3 No Automatic Win

Primary Metric 提升不应自动全量发布。

---

## 24. Preflight and A/A Testing

### 24.1 Preflight Checklist

- Experiment Registered；
- Hypothesis Approved；
- Variant Stable；
- Assignment Tested；
- Exposure Tested；
- Outcome Tested；
- Metrics Verified；
- Sample Plan Approved；
- Guardrails Active；
- Stop Conditions Active；
- Conflict Check Complete；
- Legal / Privacy / Safety / Accessibility Complete；
- Support Ready；
- Rollback Tested；
- Cleanup Owner Assigned。

### 24.2 A/A Test

用于验证：

- Assignment；
- Exposure；
- Sample Ratio；
- Metric Balance；
- Cross-Device；
- Pipeline。

A/A 通过不代表 Variant 实现正确。

---

## 25. Mutual Exclusion and Interaction

### 25.1 Mutual Exclusion Groups

可按以下范围建立：

- Onboarding；
- Store；
- Matchmaking；
- Notification；
- Pricing；
- Progression；
- Navigation；
- Recommendation。

### 25.2 Interaction Types

- Same Surface；
- Same Metric；
- Same Population；
- Same Feature Flag；
- Shared Economy；
- Shared Queue；
- Shared Social Graph；
- Shared Commercial Funnel。

### 25.3 Resolution

- Cannot Overlap；
- Can Overlap；
- Factorial；
- Nested；
- Cluster Together；
- Sequence Required。

### 25.4 Unknown Interaction

默认谨慎，不能因为没有证据就假设无影响。

---

## 26. Network Effects

### 26.1 Affected Domains

- Social；
- Matchmaking；
- Marketplace；
- Guild；
- Referral；
- Community Goal；
- UGC；
- Trade。

### 26.2 Spillover

Control 玩家可能受到 Variant 玩家影响。

### 26.3 Mitigation

- Cluster Assignment；
- Region / Server Assignment；
- Match-Level Assignment；
- Switchback；
- Network Modeling。

### 26.4 Interpretation

普通 Individual Randomization 在网络系统中可能无法识别真实效果。

---

## 27. Sequential Testing and Early Stopping

### 27.1 Sequential Requirements

- Statistical Plan；
- Look Schedule；
- Alpha Spending 或 Bayesian Rule；
- Minimum Sample；
- Stop Threshold；
- Guardrails。

### 27.2 No Continuous Peeking

不能每天查看结果直到显著。

### 27.3 Stop for Harm

Guardrail 严重越界可随时停止。

### 27.4 Stop for Futility

根据预定义规则判断继续实验已无价值。

### 27.5 Stop for Success

至少满足：

- Minimum Sample；
- Minimum Duration；
- Practical Effect；
- Guardrails Healthy；
- Data Quality Healthy；
- 无明显 Novelty 风险。

---

## 28. Stop Conditions

### 28.1 Technical

- Crash；
- Error；
- Latency；
- Save Failure；
- Data Loss；
- Version Incompatibility。

### 28.2 Safety and Privacy

- Harassment；
- Child Safety；
- Privacy Leak；
- Moderation Failure；
- Consent Bypass。

### 28.3 Commercial

- Refund；
- Chargeback；
- Complaint；
- Price Mismatch；
- High-Spend Concentration。

### 28.4 Fairness and Accessibility

- Match Quality；
- Rating Bias；
- Reward Disparity；
- Accessibility Failure；
- Region Disparity。

### 28.5 Data Quality

- Exposure Missing；
- Sample Ratio Mismatch；
- Identity Error；
- Variant Contamination；
- Metric Pipeline Failure。

---

## 29. Pause, Abort and Invalidation

### 29.1 Pause Types

- Stop New Assignment；
- Stop New Exposure；
- Disable Variant；
- Freeze Analysis；
- Full Pause。

### 29.2 Existing Participants

需定义：

- 保持当前 Variant；
- 安全回到 Control；
- 完成当前 Session；
- 保留持久状态；
- 是否重新记录 Exposure。

### 29.3 Invalidation Reasons

- Assignment 错误；
- Exposure 丢失；
- SRM；
- Control 污染；
- Variant 中途变化；
- Metric 定义错误；
- 重大数据丢失；
- Version 不兼容；
- 实现 Bug 改变机制。

`Invalidated` 是正式结果，必须归档，而不是隐藏。

---

## 30. External Events and Seasonality

可能影响实验的因素：

- Holiday；
- Marketing；
- Major Release；
- Outage；
- Platform Promotion；
- Competitor Event；
- School Calendar；
- Regional Incident。

### 30.1 Controls

- Operations Calendar Review；
- Stratification；
- Covariates；
- Holdout；
- Longer Window；
- Repeat Test。

### 30.2 Applicability

结果中应说明外部事件和适用时间范围。

---

## 31. Domain-Specific Experiment Rules

### 31.1 Matchmaking and Competition

必须：

- 同一 Match 使用统一规则；
- 不根据消费安排输赢；
- 正式 Ranked 的不可逆实验更严格；
- Rule Version 可追踪；
- Match Quality、Fairness、Latency 和 Integrity 是 Guardrail；
- 支持 Historical Reconstruction。

### 31.2 Economy

必须：

- 独立 Economy Version；
- Transaction Audit；
- 监控 Inflation、Wealth Gap 和 Progression；
- 有 Max Exposure；
- 使用 Forward Fix 处理持久资产；
- 不通过回滚删除已合法获得资产。

### 31.3 Reward

必须：

- Reward Version；
- Assignment 先于 Eligibility；
- Grant 幂等；
- 评估永久价值差异；
- 定义 Compensation 和 Post-Experiment Policy。

### 31.4 Pricing

属于 Critical Risk：

- Price Snapshot；
- Stable Assignment；
- No Mid-Checkout Change；
- No Sensitive Targeting；
- Finance / Legal / Ethics Approval；
- Refund、Complaint 和 Long-Term Trust Guardrails；
- 默认禁止秘密个体价格差异。

### 31.5 Offers

可以测试展示、组合、频率和文案，但禁止：

- 虚假倒计时；
- 虚假原价；
- 隐藏取消；
- 危机或失败后的高压触达；
- 儿童高压营销。

### 31.6 Randomized Purchases

属于 Critical Risk：

- Pool 和 Probability Version；
- Pity / Duplicate / Purchase Limit；
- No Hidden Personal Probability；
- 法律和儿童审查；
- 完整 Audit；
- Spending Health Guardrails。

### 31.7 Notification

可测试：

- Timing；
- Copy；
- Channel；
- Frequency；
- Relevance。

必要交易、安全和处罚通知不得作为普通营销实验。

### 31.8 Accessibility

- Control 必须已经可访问；
- Variant 不能降低基本可用性；
- 必要辅助不能只开放给实验组；
- 默认设置变化属于高风险；
- 结合定性研究。

### 31.9 Moderation and Safety

- 不得通过减少保护测试留存；
- Child Safety、Report、Block 和 Enforcement 需要 Safety Approval；
- 保留 Human Oversight 和 Appeal；
- 必须有即时 Kill Switch。

### 31.10 Social

- 使用 Network-Aware 或 Cluster Assignment；
- 监控 Block、Mute、Report、Invite Spam、Repeat Harm 和 Child Contact。

### 31.11 Onboarding

关注：

- Learning Outcome；
- Error；
- Time to First Success；
- Help Use；
- Long-Term Retention；
- Tutorial Replay；
- Accessibility。

### 31.12 Content and Recommendation

- 关注 Discovery、Completion、Diversity 和 Long-Tail；
- 避免只强化热门内容；
- 对 Creator 和内容多样性设置 Guardrail。


---

## 32. Long-Term Holdout

### 32.1 Purpose

测量累积变化的长期影响，例如：

- Personalization；
- Notification；
- Store；
- Recommendation；
- Social；
- Economy；
- Progression；
- Live Operations。

### 32.2 Risks

- 长期体验差异；
- Support 复杂度；
- 版本兼容；
- Fairness；
- Legacy Debt；
- 安全修复无法下发。

### 32.3 Requirements

- 明确期限；
- 最小必要范围；
- 定期复核；
- 安全、法律和关键修复不受 Holdout 限制；
- 明确退出和迁移策略；
- 不让 Holdout 成为永久旧版本。

---

## 33. Analysis Plan

### 33.1 Pre-Registered Elements

- Population；
- Assignment Unit；
- Exposure；
- Primary Metric；
- Guardrails；
- Outcome Window；
- Sample Plan；
- Statistical Method；
- Exclusions；
- Missing Data；
- Outliers；
- Multiple Testing；
- Confirmatory Segments；
- Stop Rule；
- Decision Rule。

### 33.2 Exploratory Analysis

可以进行，但必须与 Confirmatory 结果分开标记。

### 33.3 Reproducibility

记录：

- Query；
- Code；
- Data Version；
- Metric Version；
- Experiment Version；
- Analysis Time；
- Analyst；
- Environment。

---

## 34. Frequentist Analysis

正式报告至少包含：

- Sample Size；
- Baseline；
- Effect Size；
- Confidence Interval；
- P-Value；
- Power；
- Practical Significance；
- Assumptions；
- Missing Data；
- Multiple Testing Correction。

### 34.1 Assumptions

需要检查：

- Independence；
- Distribution；
- Stable Assignment；
- Cluster Correlation；
- Missingness；
- Interference。

### 34.2 Multiple Comparisons

可以使用：

- Bonferroni；
- Holm；
- False Discovery Rate；
- Predefined Metric Hierarchy。

### 34.3 Interpretation

显著不等于重要，不显著不等于等价。

---

## 35. Bayesian Analysis

报告可以包含：

- Posterior Distribution；
- Credible Interval；
- Probability of Improvement；
- Probability of Harm；
- Expected Loss；
- Decision Threshold。

### 35.1 Priors

Priors 必须透明并经过评审。

### 35.2 Limits

Bayesian 方法不能修复错误 Assignment、Exposure 缺失或 Control 污染。

---

## 36. Heterogeneous Effects

### 36.1 Valid Segments

- Platform；
- Region；
- Device Class；
- New / Return；
- Progression；
- Version；
- Explicit Preference；
- Accessibility Setting（聚合且隐私保护）。

### 36.2 Risks

- Small Sample；
- Multiple Testing；
- Reidentification；
- Post-Hoc Storytelling。

### 36.3 Confirmatory Follow-Up

重要 Segment 发现应通过新实验验证。

---

## 37. Missing Data and Outliers

### 37.1 Missing Data Sources

- Telemetry Failure；
- Consent；
- Offline；
- Platform；
- Unsupported Version；
- Dropout；
- Provider Delay。

### 37.2 Handling

预定义：

- Exclude；
- Impute；
- Weight；
- Sensitivity Analysis；
- Invalidate。

### 37.3 No Silent Drop

报告缺失比例和 Variant 分布。

### 37.4 Outliers

不能因为结果不符合预期就删除异常值。

---

## 38. Bots, Fraud and Internal Accounts

### 38.1 Exclude from Primary Analysis

- Internal；
- QA；
- Synthetic；
- Known Bots；
- Confirmed Fraud；
- Automation。

### 38.2 Separate Analysis

滥用和 Bot 影响可以单独分析，但不能混入普通玩家因果结果。

### 38.3 Exclusion Version

排除规则必须版本化。

---

## 39. Experiment Decision Framework

### 39.1 Possible Decisions

- Ship Variant；
- Keep Control；
- Iterate；
- Run Follow-Up；
- Roll Out to Segment；
- Roll Back；
- Stop Feature；
- Gather Qualitative Evidence；
- Invalid；
- No Decision。

### 39.2 Decision Inputs

- Primary Metric；
- Guardrails；
- Effect Size；
- Confidence；
- Long-Term Effect；
- Cost；
- Complexity；
- Accessibility；
- Safety；
- Ethics；
- Support Burden；
- Maintenance；
- Strategic Fit。

### 39.3 No Automatic Ship

统计成功不等于应该上线。

### 39.4 Example Rule

```text
当 Primary 达到最小有意义提升，
Hard Guardrail 不恶化，
Soft Guardrail 在可接受范围，
数据质量健康，
长期风险可接受，
才进入上线评审。
```

---

## 40. Qualitative Follow-Up

实验结束后可结合：

- Interview；
- Survey；
- Usability Test；
- Support Cases；
- Community Feedback；
- Session Review；
- Accessibility Review；
- Moderator Feedback。

数据告诉团队发生了什么，不一定解释为什么。

---

## 41. Rollout After Experiment

### 41.1 Rollout Plan

- Scope；
- Sequence；
- Product / Config Version；
- Feature Flag；
- Migration；
- Support；
- Notification；
- Monitoring；
- Rollback；
- Cleanup。

### 41.2 Revalidate at Scale

实验流量和全量环境不同，需要继续监控：

- Performance；
- Capacity；
- Economy；
- Safety；
- Support；
- Commercial；
- Accessibility。

### 41.3 Temporary Holdout

可保留短期 Holdout 验证规模效应。

---

## 42. Cleanup

### 42.1 Cleanup Scope

- Feature Flag；
- Variant Code；
- Live Config；
- Event；
- Metric；
- Dashboard；
- Alert；
- Assignment；
- Eligibility；
- Mutual Exclusion；
- Override；
- Support Script；
- Tests；
- Documentation。

### 42.2 Cleanup Owner and SLA

必须在实验启动前指定 Owner 和完成期限。

### 42.3 Permanentization

获胜方案进入正式系统设计、代码和版本管理，不能永远依赖实验 Flag。

---

## 43. Archive

归档内容包括：

- Proposal；
- Hypothesis；
- Definition；
- Variants；
- Assignment；
- Exposure；
- Metrics；
- Sample Plan；
- Approvals；
- Data Quality；
- Analysis；
- Result；
- Decision；
- Rollout；
- Cleanup；
- Limitations；
- Follow-Up。

Negative、Invalid 和 No-Decision 结果同样必须归档。

---

## 44. Experiment Result Template

```markdown
# Experiment Result

## Summary

- Experiment ID:
- Hypothesis:
- Decision:
- Confidence:
- Applicability:

## Execution

- Start:
- End:
- Variants:
- Assignment:
- Exposure:
- Sample:
- Data Quality:
- Incidents:

## Metrics

| Metric | Control | Variant | Effect | Interval | Decision |
|---|---:|---:|---:|---|---|

## Guardrails

| Guardrail | Result | Threshold | Status |
|---|---:|---:|---|

## Interpretation

- Mechanism:
- What Changed:
- What Did Not:
- Unknowns:
- Bias:
- Long-Term Risks:

## Decision

- Ship / Keep / Iterate:
- Rollout:
- Rollback:
- Cleanup:
- Follow-Up:
- Owner:
```

---

## 45. Experiment Review Board

### 45.1 Purpose

集中评审高风险实验。

### 45.2 Members

按需要包括：

- Product；
- Design；
- Engineering；
- Data；
- Privacy；
- Legal；
- Trust and Safety；
- Accessibility；
- Finance；
- Live Operations。

### 45.3 Review Levels

| Risk | Review |
|---|---|
| Low | 团队内部审批 |
| Medium | 跨职能评审 |
| High | Experiment Review Board |
| Critical | 高级管理、法律、安全及领域负责人共同批准 |

---

## 46. Risk Classification

### 46.1 Low

- 非关键文案；
- 低影响布局；
- 可逆导航；
- 不影响状态的展示。

### 46.2 Medium

- Onboarding；
- Notification；
- Content Ordering；
- Recommendation；
- Progression Presentation。

### 46.3 High

- Economy；
- Reward；
- Subscription；
- Social；
- Matchmaking；
- Accessibility Defaults；
- UGC；
- Children。

### 46.4 Critical

- Price；
- Random Probability；
- Moderation；
- Child Safety；
- Payment；
- Entitlement；
- Ranking；
- Permanent Assets；
- Privacy Consent；
- Security。

---

## 47. Ethical Review

### 47.1 Required Questions

- Control 是否仍然安全和可用；
- Variant 是否利用脆弱状态；
- 是否可能造成不可逆损失；
- 是否影响儿童；
- 是否改变安全保护；
- 是否制造虚假紧迫；
- 是否影响付费公平；
- 是否需要明确同意；
- 是否可能损害长期信任。

### 47.2 Prohibited Experiments

- 减少儿童安全保护以观察留存；
- 根据危机信号提高商业压力；
- 秘密个体价格歧视；
- 隐藏随机概率；
- 弱化退款或取消入口；
- 给 Control 不可访问体验；
- 通过 Matchmaking 操纵输赢；
- 以付费状态影响 Moderation；
- 未经批准收集敏感数据。

---

## 48. Privacy Review

### 48.1 Data Categories

- Assignment；
- Exposure；
- Outcome；
- Segment；
- Consent；
- Region；
- Platform；
- Age Policy；
- Device；
- Commercial；
- Safety。

### 48.2 Data Minimization

只记录实验所需字段。

### 48.3 Consent

实验不能扩大原有数据用途。

### 48.4 Deletion

账户删除应覆盖：

- Assignment；
- Exposure；
- Outcome；
- Feature Store；
- Analysis Dataset；
- Export。

### 48.5 Small Cohort

使用最小 Cohort、抑制和聚合防止重识别。

---

## 49. Accessibility Review

### 49.1 Requirements

- Control 体验本身可访问；
- Variant 不降低最低可用性；
- 辅助设置不被实验覆盖；
- 必要辅助不能只对实验组开放；
- Accessibility Defaults 属于高风险变更；
- 结果按聚合 Accessibility Cohort 检查；
- 结合定性验证。

---

## 50. Child and Family Safety

- 高风险商业、随机、社交和通知实验默认排除儿童账户；
- 实验不能绕过家长设置；
- Consent 遵守地区和年龄政策；
- 儿童相关结果使用聚合和更高访问保护；
- Family Sharing 不能造成同一家庭成员意外进入不同危险规则。

---

## 51. Support Readiness and Overrides

### 51.1 Support View

Support 应能查看：

- Experiment ID；
- Variant；
- Assignment；
- Exposure；
- Start / End；
- Known Issues；
- Eligibility；
- Player Action；
- Rollback State。

### 51.2 Override Use Cases

- Support Recovery；
- Safety；
- Legal；
- Accessibility；
- Incident；
- Internal Test。

### 51.3 Override Requirements

- Reason；
- Scope；
- Expiry；
- Audit；
- Exposure Update；
- Analysis Exclusion or Marking。

不能静默 Override。

---

## 52. Technical Architecture

### 52.1 Core Components

- Experiment Registry；
- Assignment Service；
- Eligibility Engine；
- Feature Flag Service；
- Exposure Service；
- Conflict Resolver；
- Ramp Controller；
- Guardrail Monitor；
- Analysis Workspace；
- Audit Store；
- Cleanup Tracker。

### 52.2 Assignment Service

负责：

- Stable Hash；
- Allocation；
- Eligibility；
- Exclusion；
- Mutual Exclusion；
- Version；
- Override。

### 52.3 Exposure Service

负责：

- Valid Exposure；
- Event Integrity；
- Deduplication；
- Context；
- Experiment Stack。

### 52.4 Guardrail Monitor

负责：

- Near Real-Time Metrics；
- Alert；
- Automatic Halt；
- Incident Integration。

---

## 53. Assignment Contract Template

```markdown
# Experiment Assignment Contract

## Request

- Experiment ID:
- Assignment Unit:
- Assignment Key:
- Account:
- Device:
- Session:
- Region:
- Platform:
- Version:

## Eligibility

- Eligible:
- Exclusion:
- Consent:
- Age:
- Conflict:
- Override:

## Allocation

- Variant:
- Allocation Version:
- Salt Version:
- Assigned At:
- Stable Until:

## Output

- Assignment ID:
- Reason:
- Experiment Version:
- Expiry:
- Audit:
```

---

## 54. Exposure Contract Template

```markdown
# Experiment Exposure Contract

## Experiment

- Experiment ID:
- Variant:
- Assignment ID:
- Experiment Version:

## Exposure

- Trigger:
- Surface:
- Exposed At:
- Session:
- Context:
- Feature Version:
- Config Version:

## Validation

- Eligible:
- Visible / Effective:
- Deduplication:
- Exposure Count:
- Experiment Stack:

## Privacy

- Consent:
- Sensitive Fields:
- Retention:
```

---

## 55. Conflict Contract Template

```markdown
# Experiment Conflict Contract

## Experiment

- Experiment ID:
- Domain:
- Surface:
- Metrics:
- Population:

## Existing Experiments

| Experiment | Overlap Type | Risk | Decision |
|---|---|---|---|

## Resolution

- Mutual Exclusion:
- Nested:
- Factorial:
- Sequence:
- Cluster:
- Allowed Overlap:

## Governance

- Owner:
- Review:
- Analysis Requirement:
```


---

## 56. Experiment Monitoring Dashboard

### 56.1 Required Sections

- Assignment；
- Exposure；
- Sample Ratio；
- Eligibility；
- Version；
- Platform；
- Region；
- Primary Metric；
- Guardrails；
- Data Quality；
- Incidents；
- Support；
- Ramp；
- Stop Conditions。

### 56.2 Quality Banner

Dashboard 必须显示：

- Healthy；
- Warning；
- Degraded；
- Invalid。

### 56.3 No Outcome-Only View

不能只展示转化或收入，必须同时展示实验健康和 Guardrail。

---

## 57. Alerting

### 57.1 Alerts

- Sample Ratio Mismatch；
- Exposure Drop；
- Event Loss；
- Crash Spike；
- Error Spike；
- Guardrail Breach；
- Refund Spike；
- Safety Spike；
- Accessibility Failure；
- Assignment Drift；
- Variant Contamination；
- Version Mismatch。

### 57.2 Alert Contract

每个告警包含：

- Severity；
- Owner；
- Trigger；
- Window；
- First Action；
- Pause / Stop / Rollback；
- Runbook；
- Recovery；
- Close Condition。

---

## 58. Failure and Recovery

| Failure | Cause | Player Impact | Recovery | Data Guarantee |
|---|---|---|---|---|
| Assignment Drift | Key、Salt 或 Version 变化 | 体验来回切换 | 恢复稳定 Assignment | History 保留 |
| Exposure Missing | Instrumentation 错误 | 分析无效 | Pause、修复、重新运行 | 不伪造 Exposure |
| Sample Ratio Mismatch | 分流或资格错误 | 结果偏差 | Pause、调查、Invalidate | Assignment Audit |
| Variant Contamination | Flag 或 Config 冲突 | Control 不纯 | Exclusion、Restart | Experiment Stack 保留 |
| Metric Definition Error | Formula 错误 | 结论错误 | Recompute、Invalidate | Metric Version 保留 |
| Guardrail Alert Missing | Monitoring 故障 | 伤害持续 | Manual Watch、Pause | Incident Audit |
| Cross-Device Split | Device Assignment 错误 | 体验不一致 | Account Assignment / Reset | Source 保留 |
| Override Lost | Support / Safety 状态丢失 | 问题复现 | Restore Override | Audit 保留 |
| Cleanup Incomplete | Flag 债务 | 长期分支 | Cleanup Sprint | Decision 保留 |
| Result Dataset Corrupt | Join / Backfill 错误 | 无法分析 | Rebuild、Reconcile | Raw Data 保留 |
| Consent Enforcement Failed | Cache / SDK 错误 | 未授权实验 | Stop、Delete、Incident | Consent Context 保留 |
| High-Risk Harm | Variant 设计问题 | 玩家损失 | Kill Switch、Rollback、Compensate | Exposure Cohort 保留 |

---

## 59. Edge Cases

### 59.1 Assignment

- Anonymous 转 Login；
- 多设备；
- 多平台；
- Account Merge；
- Account Deletion；
- Region Change；
- Version Upgrade；
- Family Sharing；
- Session Restore。

### 59.2 Exposure

- 页面部分渲染；
- App Background；
- Feature Flag 开启但未使用；
- Push 已送达但未呈现；
- Match Cancelled；
- Offer 在折叠区域；
- Offline；
- Cached Config。

### 59.3 Analysis

- Late Outcome；
- Missing Outcome；
- Multiple Exposure；
- Reassignment；
- Cluster；
- Network Spillover；
- Season Change；
- External Incident。

### 59.4 Cleanup

- Flag 已结束但代码仍读 Variant；
- Dashboard 仍被使用；
- Event 被其他团队复用；
- Override 残留；
- Long-Term Holdout 未退出；
- Config Migration 未完成。

---

## 60. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| Analytics and Telemetry | Critical | 双向 | Exposure / Outcome / Quality | 实验无效 |
| Live Operations | Critical | 双向 | Ramp / Pause / Rollout | 风险扩散 |
| Versioning and Migration | Critical | 双向 | Variant / Schema / Compatibility | 历史不可比 |
| Settings and Preferences | Critical | Settings → Experiment | Consent / Accessibility / Privacy | 合规风险 |
| Notification and Reminders | Hard | 双向 | Notification Experiment | 触达伤害 |
| Core Loop | Hard / Soft | Experiment → Core | Loop Variant | 核心体验波动 |
| Progression System | Critical | 双向 | Pace / Unlock / State | 持久影响 |
| Reward System | Critical | 双向 | Reward Variant / Grant | 资产风险 |
| Resources and Economy | Critical | 双向 | Economy Version / Transaction | 长期失衡 |
| Content Systems | Hard | 双向 | Discovery / Difficulty / Rotation | 内容偏差 |
| Social and Multiplayer | Critical | 双向 | Network / Cluster | Spillover |
| Matchmaking and Competition | Critical | 双向 | Queue / Rule / Result | 公平风险 |
| Moderation and Safety | Critical Boundary | 双向 | Safety Guardrails / Exclusion | 伤害风险 |
| Monetization System | Critical | 双向 | Checkout / Subscription / Order | 财务风险 |
| Offers and Pricing | Critical | 双向 | Price / Offer / Snapshot | 合规风险 |
| Entitlement and Ownership | Critical | 双向 | Access / Trial / Ownership | 已购风险 |
| Support | Hard | 双向 | Variant / Override / Recovery | 无法诊断 |

---

## 61. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Experiment Definition | 是 | Experiment Registry | 发布变化 | 长期 | Version History |
| Variant Definition | 是 | Experiment Registry | 版本变化 | 长期 | Version Restore |
| Assignment | 是或可重算 | Assignment Service | 分流 | 实验及审计期 | Deterministic Rebuild |
| Exposure | 是 | Exposure Service / Analytics | 真实曝光 | 实验及审计期 | Raw Event |
| Eligibility Result | 是或短期 | Experiment Service | 分流 | 实验期 | Re-evaluate |
| Conflict State | 是 | Experiment Registry | 审批变化 | 实验期 | Registry |
| Ramp State | 是 | Live Operations | 比例变化 | 实验期 | Rollback |
| Stop State | 是 | Experiment Service | 停止变化 | 长期审计 | Audit |
| Analysis Dataset | 是或可重算 | Analytics | 分析运行 | 分析期 | Rebuild |
| Decision Record | 是 | Product / Experiment | 决策 | 长期 | Archive |
| Cleanup State | 是 | Experiment Registry | 清理变化 | 至完成 | Task Recovery |
| Experiment Audit | 是 | Audit System | 高风险动作 | 长期 | Append-Only |

---

## 62. Analytics and Validation

### 62.1 Key Assumptions

1. Assignment 稳定且可复现。
2. Exposure 只在真实体验发生时记录。
3. Sample Ratio 和数据质量异常能及时发现。
4. Primary Metric、Guardrail 和 Stop Condition 在实验前定义。
5. 多实验冲突和网络效应可以被识别。
6. 高风险实验经过法律、隐私、伦理、安全和可访问性审查。
7. 结果能区分统计显著与实际价值。
8. 不显著和失败结果也会归档。
9. 实验结束后 Flag、代码、事件和 Dashboard 会被清理。
10. 实验结果不会被用来绕过产品和伦理责任。

### 62.2 Validation Plan

| Hypothesis | Evidence | Success | Failure | Method |
|---|---|---|---|---|
| Assignment 稳定 | 多设备测试 | 同一 Unit 同 Variant | Variant 漂移 | Integration Test |
| Exposure 正确 | 行为回放 | 真实可见才触发 | 预加载即曝光 | QA |
| SRM 可发现 | A/A Test | 比例健康 | 异常未告警 | A/A |
| Guardrail 有效 | 故障模拟 | 自动暂停 | 伤害持续 | Game Day |
| Conflict 可治理 | Registry Review | 同 Surface 不污染 | 多实验交叉 | Conflict Test |
| 高风险审查有效 | Approval Audit | 审批完整 | 绕过审查 | Governance Review |
| 结果可解释 | Result Review | Effect、Bias、Limit 清楚 | 只报显著性 | Analysis Audit |
| Cleanup 可完成 | Post-Decision | Flag 和代码移除 | 长期债务 | Cleanup Audit |
| 负结果被保留 | Registry Search | 可查到 | 重复实验 | Knowledge Review |
| Ethics 受控 | Complaint / Guardrail | 无操纵性伤害 | 退款和投诉上升 | Ethics Review |

### 62.3 Behavioral Metrics

- Experiment Proposed；
- Experiment Approved；
- Assignment Created；
- Exposure Recorded；
- Variant Activated；
- Ramp Increased；
- Ramp Paused；
- Experiment Stopped；
- Experiment Invalidated；
- Analysis Completed；
- Decision Recorded；
- Cleanup Completed；
- Override Applied。

### 62.4 Outcome Metrics

- Assignment Stability；
- Exposure Completeness；
- Sample Ratio Health；
- Experiment Cycle Time；
- Guardrail Detection Time；
- Invalid Experiment Rate；
- Repeated Experiment Rate；
- Cleanup SLA；
- Decision Time；
- Result Reproducibility；
- High-Risk Approval Compliance；
- Support Incident Rate；
- Ethical Incident Rate。

### 62.5 Negative Metrics

- Assignment Drift；
- Exposure Missing；
- SRM；
- Control Contamination；
- Variant Mutation；
- Guardrail Failure；
- Selective Reporting；
- Child Inclusion in High-Risk Test；
- Sensitive Commercial Targeting；
- Flag Debt；
- Cleanup Delay；
- Repeat Experiments；
- Refund / Complaint / Safety Spikes。

---

## 63. Test Strategy

### 63.1 Assignment Tests

- Deterministic Hash；
- Multi-Device；
- Anonymous to Login；
- Region；
- Platform；
- Version；
- Allocation；
- Reassignment；
- Override；
- Mutual Exclusion。

### 63.2 Exposure Tests

- Visible；
- Partially Visible；
- Not Visible；
- Cached；
- Offline；
- Repeat；
- Session；
- Match；
- Push；
- Store。

### 63.3 Metric Tests

- Primary；
- Guardrail；
- Time Window；
- Deduplication；
- Missing；
- Late；
- Sampling；
- Version；
- Time Zone。

### 63.4 Ramp Tests

- Internal；
- Small Cohort；
- Automatic Halt；
- Pause；
- Resume；
- Rollback；
- Version Change；
- Data Failure。

### 63.5 Conflict Tests

- Same Surface；
- Same Metric；
- Same Population；
- Network；
- Nested；
- Factorial；
- Global Holdout。

### 63.6 High-Risk Domain Tests

- Price；
- Subscription；
- Reward；
- Economy；
- Matchmaking；
- Moderation；
- Accessibility；
- Child；
- Entitlement。

### 63.7 Security and Privacy Tests

- Assignment Spoof；
- Variant Override；
- Consent Bypass；
- Small Cohort；
- Export；
- Deletion；
- Admin Abuse；
- Audit Tamper。

### 63.8 Cleanup Tests

- Flag Removal；
- Code Removal；
- Event Deprecation；
- Dashboard Archive；
- Override Clear；
- Holdout End；
- Config Migration。

---

## 64. Experiment Debt

包括：

- 无 Registry 的实验；
- Assignment 和 Feature Flag 混用；
- Exposure 缺失；
- 同一 Surface 多个实验；
- Primary Metric 中途修改；
- 无 Guardrail；
- 无 Sample Plan；
- Continuous Peeking；
- Negative Result 不归档；
- 高风险实验无审批；
- Variant Code 永久保留；
- Flag 无 Expiry；
- Dashboard 无 Owner；
- Holdout 无退出；
- Small Cohort 可识别；
- Support 不知道玩家 Variant；
- 同一问题重复实验；
- 结果无法复现。

### 64.1 Signals

- 生产中存在未知 Flag；
- 同一玩家多设备体验不同；
- 团队无法解释 Exposure；
- A/B 结果频繁反转；
- 实验结束后仍有多套代码；
- 同一问题被不同团队重复测试；
- 高风险实验事故频繁；
- Result 文档只有显著性；
- Control 组持续污染；
- Cleanup 长期延期。

### 64.2 Reduction

- Experiment Registry；
- Assignment Service；
- Exposure Contract；
- Mutual Exclusion；
- Pre-Registration；
- Guardrail Monitor；
- A/A Testing；
- Review Board；
- Cleanup SLA；
- Flag Expiry；
- Result Archive；
- Quarterly Experiment Health Review。

---

## 65. Rollout and Migration

### 65.1 Rollout

Experiment Management 系统自身变更应按：

```text
Design Review
→ Registry Sandbox
→ Assignment Shadow
→ Exposure Shadow
→ Internal Accounts
→ Low-Risk Experiments
→ Selected Domains
→ Broad Adoption
→ Full Governance
```

### 65.2 High-Risk Changes

- Assignment Algorithm；
- Salt；
- Exposure Definition；
- Identity；
- Mutual Exclusion；
- Sample Ratio；
- Guardrail Monitor；
- Long-Term Holdout；
- Override；
- Cleanup Automation；
- Consent Handling。

### 65.3 Migration Scope

- Experiment ID；
- Variant；
- Assignment；
- Exposure；
- Eligibility；
- Allocation；
- Conflict；
- Metrics；
- Ramp；
- Stop；
- Result；
- Cleanup；
- Archive；
- Audit。

### 65.4 Salt Migration

改变 Hash Salt 会导致重分配，必须：

- Shadow Compare；
- 评估重新分组比例；
- 停止在运行实验中直接切换；
- 保留 Assignment History；
- 明确迁移边界。

### 65.5 Rollback

回滚时：

- 保留 Assignment History；
- 不伪造 Exposure；
- 恢复旧 Flag；
- 暂停新 Assignment；
- 保留 Decision 和 Result；
- 清理错误 Override；
- 标记 Data Quality；
- 必要时 Invalidate。

### 65.6 Stop Conditions

出现以下情况应停止发布：

- Assignment 大面积漂移；
- Exposure 严重缺失；
- SRM；
- Consent 绕过；
- Control 污染；
- Guardrail Monitor 不可用；
- High-Risk Variant 无 Kill Switch；
- Support 无法识别 Variant；
- Registry 与实际 Flag 分叉；
- Cleanup 自动化误删正式配置；
- Small Cohort 隐私风险；
- 结果数据无法复现。

---

## 66. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| Assignment 与 Exposure 混淆 | Analysis Risk | 严重 | 高 | Separate Contracts | Analytics |
| 多实验相互污染 | Validity Risk | 高 | 高 | Mutual Exclusion | Product |
| Continuous Peeking 导致假阳性 | Statistical Risk | 高 | 高 | Sequential Plan | Data |
| 高风险价格实验伤害信任 | Ethical / Legal Risk | 严重 | 中 | Review Board | Legal |
| 儿童进入不当实验 | Safety Risk | 严重 | 低 | Default Exclusion | Safety |
| Flag 和 Variant 债务增长 | Architecture Risk | 高 | 高 | Cleanup SLA | Engineering |
| 网络效应未处理 | Validity Risk | 高 | 中 | Cluster Design | Data |
| 长期效果与短期相反 | Strategic Risk | 高 | 中 | Long-Term Holdout | Product |
| Small Cohort 重识别 | Privacy Risk | 严重 | 低 | Minimum Cohort | Privacy |
| Negative Result 不归档 | Knowledge Risk | 高 | 高 | Registry Requirement | Experiment Ops |
| Experiment Debt 持续增长 | Governance Risk | 高 | 高 | Quarterly Review | Leadership |

---

## 67. Review Checklist

### Proposal and Hypothesis

- [ ] Product Question 明确；
- [ ] Hypothesis 可证伪；
- [ ] Expected Mechanism 已定义；
- [ ] 已确认实验是合适方法；
- [ ] Control 和 Variant 清楚；
- [ ] Risk Level 已评估。

### Assignment and Eligibility

- [ ] Assignment Unit 合理；
- [ ] Stable Assignment 可验证；
- [ ] Allocation 和 Salt 版本明确；
- [ ] Eligibility 和 Exclusion 可解释；
- [ ] Anonymous、Multi-Device 和 Cross-Platform 已处理；
- [ ] Mutual Exclusion 完成。

### Exposure and Outcome

- [ ] Exposure 在真实体验发生时触发；
- [ ] Assignment 与 Exposure 分离；
- [ ] Outcome Window 明确；
- [ ] 多次 Exposure 处理清楚；
- [ ] Experiment Stack 可追踪；
- [ ] Exposure 缺失有告警。

### Metrics and Statistics

- [ ] Primary Metric 在运行前定义；
- [ ] Secondary 与 Exploratory 分开；
- [ ] Guardrail 完整；
- [ ] Sample Size、Power 和 MDE 明确；
- [ ] Multiple Testing 处理清楚；
- [ ] Practical Significance 已定义；
- [ ] Sequential Testing 规则明确。

### Ramp and Monitoring

- [ ] Preflight 完成；
- [ ] A/A 或等价验证完成；
- [ ] Ramp Gates 和 Stop Conditions 明确；
- [ ] SRM、Data Quality 和 Guardrail Alert 可用；
- [ ] Pause、Abort 和 Rollback 可执行；
- [ ] Support Ready。

### High-Risk Domains

- [ ] Price、Probability、Reward、Economy、Matchmaking、Moderation 和 Entitlement 有专项审查；
- [ ] 高风险实验有 Kill Switch；
- [ ] 持久状态有 Forward Fix；
- [ ] 正式竞争公平已评估；
- [ ] 儿童和脆弱群体得到保护。

### Privacy, Ethics and Accessibility

- [ ] Consent 不被扩大；
- [ ] Sensitive Signal 不用于商业实验；
- [ ] Control 仍然安全和可访问；
- [ ] Small Cohort 有保护；
- [ ] Data Deletion 可传播；
- [ ] 不存在 Dark Pattern 实验；
- [ ] Accessibility Default 经过专项评审。

### Analysis and Decision

- [ ] Analysis Plan 预注册；
- [ ] 数据质量健康；
- [ ] Missing Data 和 Bias 已报告；
- [ ] Effect Size 和 Interval 已报告；
- [ ] Negative Result 也归档；
- [ ] Decision 不只依赖显著性；
- [ ] Long-Term 和 Strategic Fit 已评估。

### Cleanup and Governance

- [ ] Rollout Plan 明确；
- [ ] Cleanup Owner 和 SLA 明确；
- [ ] Flag、Code、Event、Dashboard 和 Override 有清理计划；
- [ ] Registry 已更新；
- [ ] Archive 完整；
- [ ] Experiment Debt 可监控。

---

## 68. V1 Completion Criteria

Experiment Management 可以被视为 V1，当：

- A/B、A/B/n、Multivariate、Factorial、Holdout、Canary、Switchback、Cluster、Network、Sequential、Non-Inferiority 和 Quasi-Experiment 类型完整；
- Experiment Registry、Definition、Hypothesis、Variant、Assignment、Eligibility、Exposure、Outcome、Metric、Guardrail、Ramp、Decision 和 Cleanup 实体明确；
- Idea、Draft、Review、Approved、Scheduled、Preflight、Ramping、Running、Paused、Stopped、Invalidated、Analyzing、Decided、Cleaning 和 Archived 生命周期完整；
- 每个实验有稳定 ID、Owner、Risk Level、Version 和 Archive；
- Product Question、Hypothesis、Expected Mechanism 和 Falsifiability 明确；
- Assignment Unit、Stable Hash、Allocation、Salt、Reassignment、Anonymous、Multi-Device 和 Cross-Platform 规则完整；
- Assignment、Exposure 和 Outcome 的语义、时序和数据契约分离；
- Eligibility、Exclusion、Mutual Exclusion、Nested、Factorial 和 Conflict Matrix 可执行；
- Primary、Secondary、Guardrail、Diagnostic、Sample Size、MDE、Power、Duration 和 Decision Rule 完整；
- A/A Test、SRM、Data Quality、Missing Data 和 Invalidation 有专项治理；
- Sequential Testing、Early Stopping、Novelty、Learning、Carryover 和 Seasonality 规则明确；
- Network、Social、Matchmaking、Marketplace 和 Cluster Experiment 有正确设计边界；
- Economy、Reward、Pricing、Randomized Purchase、Notification、Accessibility、Safety、Onboarding 和 Content Experiment 有专项要求；
- 高风险实验有 Review Board、Kill Switch、Stop Conditions、Rollback、Forward Fix 和 Compensation；
- Frequentist、Bayesian、Heterogeneous Effect、Multiple Testing 和 Practical Significance 有统一解释规则；
- Experiment Result、Decision、Rollout、Long-Term Holdout、Cleanup 和 Archive 形成完整闭环；
- Privacy、Consent、Data Minimization、Deletion、Small Cohort、Child、Family 和 Sensitive Signal 禁止用途通过评审；
- Support 能识别 Variant、Exposure、Known Issue 和 Override；
- Assignment Service、Exposure Service、Conflict Resolver、Ramp Controller 和 Guardrail Monitor 的职责明确；
- Assignment、Exposure、Metric、Ramp、Guardrail、Decision 和 Cleanup 有验证计划；
- Experiment Debt 有识别、清理和季度治理方式；
- Experiment Management 自身高风险变更支持 Shadow、灰度、迁移、回滚和停止条件；
- 所有下游系统可以直接引用本文件定义、执行和解释实验。

---

## 69. Related Documents

### Philosophy

- [Player First Design](../../philosophy/foundation/player-first-design.md)
- [Challenge and Fairness](../../philosophy/experience/challenge-and-fairness.md)
- [Consistency and Coherence](../../philosophy/long-term/consistency-and-coherence.md)
- [Accessibility and Inclusivity](../../philosophy/responsibility/accessibility-and-inclusivity.md)
- [Ethical Design](../../philosophy/responsibility/ethical-design.md)
- [Iteration and Validation](../../philosophy/validation/iteration-and-validation.md)

### Systems

- [Systems README](../README.md)
- [System Design Framework](../system-design-framework.md)
- [System Map](../system-map.md)
- [Integration Rules](../integration-rules.md)
- [Resources and Economy](../progression/resources-and-economy.md)
- [Progression System](../progression/progression-system.md)
- [Reward System](../progression/reward-system.md)
- [Content and Unlocks](../content/content-and-unlocks.md)
- [Content Lifecycle](../content/content-lifecycle.md)
- [Save and Persistence](../player/save-and-persistence.md)
- [Settings and Preferences](../player/settings-and-preferences.md)
- [Notification and Reminders](../player/notification-and-reminders.md)
- [Social and Multiplayer](../social/social-and-multiplayer.md)
- [Matchmaking and Competition](../social/matchmaking-and-competition.md)
- [Moderation and Safety](../social/moderation-and-safety.md)
- [Monetization System](../commercial/monetization-system.md)
- [Offers and Pricing](../commercial/offers-and-pricing.md)
- [Entitlement and Ownership](../commercial/entitlement-and-ownership.md)
- [Analytics and Telemetry](./analytics-and-telemetry.md)
- [Live Operations](./live-operations.md)
- `versioning-and-migration.md`
