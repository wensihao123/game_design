# Analytics and Telemetry（分析与遥测系统）

> Status: V1  
> Category: Operations  
> Path: `design/systems/operations/analytics-and-telemetry.md`  
> Owner: TBD  
> Reviewers: Product / Design / Engineering / Data / Analytics / Privacy / Security / Legal / QA / Live Operations / Trust and Safety / Support  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: Critical  
> Dependencies: System Design Framework, Integration Rules, Save and Persistence, Settings and Preferences, Notification and Reminders, Experiment Management, Versioning and Migration  
> Affected Systems: Core Loop, Game State and Flow, Rules and Resolution, Resources and Economy, Progression, Reward, Content, Social, Matchmaking, Moderation, Monetization, Live Operations, Support

---

## 1. System Summary

Analytics and Telemetry 系统负责定义：

```text
产品和系统应该记录什么；
为什么记录；
谁可以访问；
如何保证事件、指标和报表语义一致；
如何区分分析数据、运营遥测、审计日志、安全日志和调试日志；
如何保证数据完整、及时、可验证、可回放和可迁移；
如何尊重同意、隐私、儿童保护、数据最小化和保留策略；
如何让事件失败不阻断核心体验；
如何支持实验、告警、事故分析、经济监控、内容评估和长期决策。
```

该系统通常覆盖：

- Event Taxonomy；
- Event Schema；
- Event Contract；
- Event Version；
- Metric Definition；
- KPI；
- Funnel；
- Cohort；
- Retention；
- Attribution；
- Session Analytics；
- Economy Analytics；
- Progression Analytics；
- Reward Analytics；
- Content Analytics；
- Social Analytics；
- Matchmaking Analytics；
- Moderation Analytics；
- Commercial Analytics；
- Operational Telemetry；
- Performance Metrics；
- Error Telemetry；
- Crash Reporting；
- Audit Log；
- Security Log；
- Experiment Exposure；
- Feature Flag Exposure；
- Sampling；
- Batching；
- Retry；
- Offline Queue；
- Data Quality；
- Late Event；
- Duplicate Event；
- Event Ordering；
- Identity Resolution；
- Privacy Consent；
- Data Retention；
- Access Control；
- Data Catalog；
- Dashboard；
- Alert；
- Incident Support；
- Replay；
- Backfill；
- Migration；
- Deletion；
- Transparency。

健康的分析与遥测系统应让团队获得：

```text
可信而非海量的数据；
可解释而非黑箱的指标；
稳定而非随意变化的事件语义；
及时而非阻断玩家体验的遥测；
保护隐私而非默认过度收集；
能复现问题而非只看到聚合结果；
能支持决策而非替代设计判断；
能识别伤害而非只优化增长。
```

---

## 2. Purpose

### 2.1 Player Value

玩家通常不会直接操作分析系统，但会间接受益于：

- 更稳定的产品；
- 更快的问题修复；
- 更公平的经济和匹配；
- 更少重复错误；
- 更合理的内容节奏；
- 更可靠的存档和交易；
- 更准确的 Support 诊断；
- 更好的可访问性；
- 更健康的通知和商业化；
- 更及时的安全响应；
- 更少基于猜测的产品改动。

### 2.2 Product Value

该系统帮助团队：

- 理解核心体验；
- 发现流失点；
- 评估内容价值；
- 监控经济健康；
- 监控竞争公平；
- 监控商业交易；
- 监控安全和治理；
- 支持实验；
- 支持 Live Operations；
- 支持事故处理；
- 支持版本迁移；
- 支持财务和审计；
- 支持长期产品战略。

### 2.3 Experience Contribution

分析系统本身不会创造体验，但会影响：

- 团队如何定义成功；
- 哪些行为被优化；
- 哪些群体被忽略；
- 哪些风险被及时发现；
- 哪些短期指标压过长期健康；
- 是否会出现数据驱动的 Dark Pattern；
- 是否能够保护非典型玩家；
- 是否能够发现可访问性和公平性问题。

### 2.4 Why This System Exists

缺少统一框架时，常见问题包括：

```text
同一个“开始游戏”被多个系统用不同事件名记录；
同一个 DAU 指标在不同报表定义不同；
事件在客户端和服务端重复上报；
付费成功只靠客户端事件判断；
实验 Exposure 缺失但 Outcome 已进入分析；
时区、平台和版本字段不一致；
日志记录了不必要的个人内容；
同一用户在多设备被重复计算；
数据延迟导致运营错误判断；
事件 Schema 变化未版本化；
指标在功能发布后才临时定义；
Dashboard 没有 Owner；
异常告警没有 Runbook；
数据删除请求只删除主库，未删除分析副本；
事件失败导致核心流程卡住；
团队只看转化，不看投诉、误伤和长期信任。
```

---

## 3. Non-Goals

该系统不负责：

- 代替产品判断；
- 代替用户研究；
- 代替财务账本；
- 代替权威业务状态；
- 代替安全和 Moderation 的正式证据系统；
- 代替完整日志平台；
- 使用分析事件驱动高价值业务交易；
- 让 Analytics Failure 阻断核心流程；
- 默认记录所有可记录数据；
- 收集完整私聊、语音或敏感内容用于普通分析；
- 用数据模型直接决定处罚；
- 用单一指标定义产品成功；
- 根据脆弱状态做商业操纵；
- 使用实验绕过隐私和伦理；
- 把事件数量当作数据质量；
- 把 Dashboard 当作最终事实而不理解定义和限制；
- 将运营遥测与个人画像无限合并。

---

## 4. Governing Principles

### 4.1 Player First Design

参考：

- `../../philosophy/foundation/player-first-design.md`

应用原则：

- 只收集能改善玩家体验或履行合法需求的数据；
- 不因遥测失败阻断玩家；
- 不通过过度记录增加性能和隐私负担；
- 分析结果不能替代真实玩家声音；
- 发现伤害指标时优先处理。

### 4.2 Iteration and Validation

参考：

- `../../philosophy/validation/iteration-and-validation.md`

应用原则：

- 每个设计假设有对应证据；
- 先定义验证问题，再定义事件；
- 指标必须有成功和失败解释；
- 定量与定性结合；
- 结论必须考虑反例和偏差。

### 4.3 Clarity and Feedback

参考：

- `../../philosophy/experience/clarity-and-feedback.md`

应用原则：

- 事件名和属性清楚；
- 指标定义可复述；
- Dashboard 显示版本、时区和数据延迟；
- 告警包含影响和下一步；
- 数据质量异常可见。

### 4.4 Accessibility and Inclusivity

参考：

- `../../philosophy/responsibility/accessibility-and-inclusivity.md`

应用原则：

- 评估不同输入、设备和辅助设置；
- 不把较慢操作简单视为失败；
- 不把辅助技术使用者当异常；
- 数据采集不破坏辅助功能；
- 分析分组避免暴露小群体身份。

### 4.5 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 不优化操纵性行为；
- 不使用危机、健康、安全或处罚信号做商业画像；
- 儿童数据最小化；
- 高风险模型有人类监督；
- 指标中包含长期健康和负面影响；
- 数据访问遵循最小权限。

---

## 5. System Boundary

### 5.1 Inputs

系统接收：

- Domain Event；
- Client Telemetry；
- Server Telemetry；
- Operational Metric；
- Performance Metric；
- Error；
- Crash；
- Audit Event；
- Security Event；
- Experiment Exposure；
- Feature Flag State；
- Account and Device Context；
- Version；
- Platform；
- Region；
- Time；
- Consent；
- Privacy Settings；
- Sampling Rule；
- Data Contract；
- Data Quality Rule；
- Retention Policy。

### 5.2 Outputs

系统产生：

- Event Record；
- Metric；
- Aggregate；
- Dashboard；
- Alert；
- Funnel；
- Cohort；
- Experiment Dataset；
- Operational Report；
- Data Quality Report；
- Audit Trail；
- Backfill；
- Deletion Request；
- Retention Action；
- Support Diagnostic；
- Decision Brief；
- Incident Evidence Reference。

### 5.3 Owned State

系统拥有：

- Event Definition；
- Event Schema；
- Event Version；
- Metric Definition；
- Data Contract；
- Sampling Policy；
- Data Quality Rule；
- Dashboard Definition；
- Alert Definition；
- Data Catalog；
- Lineage Metadata；
- Retention Metadata；
- Access Policy；
- Analytics Job State；
- Backfill State；
- Deletion State；
- Telemetry Version。

### 5.4 Read-Only Dependencies

系统读取：

- Domain State Summary；
- Account Context；
- Settings and Consent；
- Version；
- Platform；
- Region；
- Experiment Assignment；
- Feature Flag；
- Time；
- Privacy Policy；
- Legal Requirements；
- Operational Health。

### 5.5 Write Dependencies

系统通过正式契约请求：

- Event Pipeline 接收事件；
- Metric Store 保存指标；
- Warehouse / Lake 保存分析数据；
- Dashboard 平台展示；
- Alerting 平台发送告警；
- Privacy 系统执行删除；
- Support 提供脱敏诊断；
- Experiment 系统读取 Exposure；
- Live Operations 读取健康指标；
- Versioning 记录 Schema Migration。

### 5.6 Out of Scope

系统不直接：

- 发放奖励；
- 修改玩家状态；
- 修改 Rating；
- 修改 Entitlement；
- 扣款；
- 决定处罚；
- 覆盖玩家设置；
- 作为业务事务唯一来源；
- 在无明确权限时暴露原始个人数据。

---

## 6. Analytics, Telemetry, Logging and Audit

### 6.1 Product Analytics

用于理解玩家行为和体验。

例如：

- Funnel；
- Retention；
- Feature Adoption；
- Content Completion；
- Progression；
- Economy；
- Social；
- Commercial。

### 6.2 Operational Telemetry

用于系统健康和性能。

例如：

- Latency；
- Error Rate；
- Queue Depth；
- CPU；
- Memory；
- Database；
- Provider；
- Delivery；
- Throughput。

### 6.3 Debug Logging

用于技术诊断。

通常：

- 短期；
- 高细节；
- 环境受限；
- 不适合产品分析。

### 6.4 Audit Logging

用于高风险状态变化和责任追踪。

例如：

- Grant；
- Refund；
- Enforcement；
- Admin Action；
- Migration；
- Price Change。

### 6.5 Security Logging

用于：

- Authentication；
- Authorization；
- Abuse；
- Fraud；
- Intrusion；
- Data Access。

### 6.6 Separation Principle

这些数据：

```text
目的不同；
保留期不同；
访问权限不同；
完整性要求不同；
隐私风险不同。
```

不能全部混为一个“Analytics Event”。

---

## 7. Core Entities and Concepts

| Entity / Concept | Definition | Owner | Lifetime | Notes |
|---|---|---|---|---|
| Event Definition | 某类事件的稳定语义 | Analytics | 版本级 | 唯一 ID |
| Event Instance | 一次具体发生记录 | Analytics | 数据期 | 不等于业务事实 |
| Event Schema | 事件字段结构 | Analytics | 版本级 | 兼容性要求 |
| Event Version | 事件语义或结构版本 | Analytics | 长期 | 必须记录 |
| Metric Definition | 指标计算和语义 | Analytics | 版本级 | 有 Owner |
| KPI | 高层关键指标 | Product / Analytics | 周期级 | 不等于全部成功 |
| Dimension | 分析切分维度 | Analytics | 版本级 | 需隐私审查 |
| Measure | 可聚合数值 | Analytics | 数据级 | 有单位 |
| Exposure | 实验或功能曝光事实 | Experiment / Analytics | 实验期 | 必须先于 Outcome |
| Session | 分析会话定义 | Analytics | 短期 | 不等于游戏 Session |
| Identity Key | 用于聚合的去标识身份 | Identity / Analytics | 长期或轮换 | 不直接公开 |
| Correlation ID | 跨系统关联某次流程 | Domain / Analytics | 流程期 | 便于诊断 |
| Sampling Rule | 采样策略 | Analytics | 版本级 | 影响解释 |
| Data Contract | 生产者与消费者协议 | Analytics | 版本级 | 有 SLA |
| Data Lineage | 数据从源到指标的路径 | Data Platform | 长期 | 审计 |
| Data Quality Rule | 对完整性和正确性的要求 | Analytics | 版本级 | 可告警 |
| Retention Rule | 数据保留和删除规则 | Privacy / Analytics | 版本级 | 按目的 |
| Dashboard | 指标可视化 | Analytics / Product | 长期 | 有 Owner |
| Alert | 指标异常触发 | Operations | 版本级 | 有 Runbook |
| Backfill | 对缺失或错误数据的补算 | Analytics | 事件级 | 版本化 |
| Deletion Request | 删除或匿名化任务 | Privacy | 至完成 | 可审计 |

---

## 8. Event Taxonomy

### 8.1 Domain Fact Event

表示业务事实。

例如：

- Reward Granted；
- Match Result Finalized；
- Purchase Completed；
- Entitlement Revoked；
- Quest Completed。

### 8.2 Interaction Event

表示玩家操作。

例如：

- Button Activated；
- Screen Opened；
- Offer Dismissed；
- Loadout Edited。

### 8.3 State Transition Event

表示状态变化。

例如：

- Session Started；
- Queue Cancelled；
- Character Unlocked；
- Subscription Expired。

### 8.4 Exposure Event

表示玩家看见或进入某功能、实验或内容。

### 8.5 Outcome Event

表示结果。

### 8.6 Error Event

表示失败。

### 8.7 Performance Event

表示客户端或服务性能。

### 8.8 Operational Event

表示系统容量、队列或依赖状态。

### 8.9 Audit Event

表示高风险管理或交易行为。

### 8.10 Safety Event

表示安全相关状态，但应严格限制内容和用途。

### 8.11 Support Event

表示诊断、恢复或 Case 操作。

---

## 9. Event Naming

### 9.1 Naming Pattern

推荐：

```text
<domain>.<entity>.<action>
```

例如：

```text
progression.level.completed
reward.instance.granted
matchmaking.queue.entered
commercial.order.completed
social.invite.accepted
content.item.unlocked
```

### 9.2 Naming Rules

- 使用稳定领域术语；
- 使用过去式或明确动作；
- 不包含 UI 文案；
- 不包含版本号；
- 不包含平台名；
- 不为同一语义创建多个别名；
- 不使用模糊词；
- 不使用 `click_event_1`；
- 不使用临时项目代号作为长期事件名。

### 9.3 Event Name Stability

字段可以演进，事件语义不能随意改变。

若语义改变，应：

- 新版本；
- 或新事件；
- 记录迁移。

---

## 10. Event Definition Template

```markdown
## Event Definition

- Event ID:
- Event Name:
- Domain:
- Purpose:
- Business Meaning:
- Trigger:
- Producer:
- Authority:
- Event Time:
- Ingestion Time:
- Identity Scope:
- Required Properties:
- Optional Properties:
- Sensitive Properties:
- Sampling:
- Ordering:
- Deduplication:
- Retention:
- Consumers:
- Version:
- Owner:
- Risk Level:
```

### 10.1 必须回答

- 为什么需要；
- 它代表行为还是事实；
- 谁负责发送；
- 哪个系统最权威；
- 在什么业务节点触发；
- 必需字段；
- 是否包含敏感信息；
- 是否采样；
- 如何去重；
- 谁使用；
- 保留多久。

---

## 11. Event Envelope

所有事件使用统一外层结构。

推荐字段：

| Field | Meaning |
|---|---|
| event_id | 事件实例唯一 ID |
| event_name | 稳定事件名 |
| event_version | Schema 和语义版本 |
| event_time | 业务发生时间 |
| ingestion_time | 接收时间 |
| producer | 生产者 |
| environment | Production / Staging 等 |
| account_key | 去标识账户键 |
| device_key | 去标识设备键 |
| session_id | 分析或游戏 Session |
| correlation_id | 流程关联 |
| platform | 平台 |
| region | 地区 |
| app_version | 客户端版本 |
| server_version | 服务版本 |
| experiment_context | 曝光上下文引用 |
| consent_context | 同意版本引用 |
| properties | 领域字段 |
| integrity | 完整性与来源信息 |

### 11.1 Event ID

用于：

- 去重；
- 重试；
- 追踪；
- 回放。

### 11.2 Event Time vs Ingestion Time

必须区分。

### 11.3 Authority

Envelope 应说明：

- Client；
- Server；
- Provider；
- Admin；
- Derived。

---

## 12. Event Invariants

1. `event_id` 唯一。
2. 重试使用同一 Event ID。
3. 事件失败不能阻断核心业务。
4. 高价值业务结果不能只依赖客户端事件。
5. 事件名和版本必须存在于 Catalog。
6. Event Time 与 Ingestion Time 分离。
7. Schema 不兼容变化必须升级版本。
8. Sensitive Property 必须显式标记。
9. Experiment Outcome 必须能关联 Exposure。
10. 删除和匿名化必须覆盖派生数据。
11. Analytics Event 不应成为业务交易权威。
12. Client Event 默认视为可伪造。
13. Audit Event 不能使用普通低可靠采样管道。
14. Consent 变化后新事件必须立即遵守。
15. Analytics Failure 不影响 Grant、Purchase、Save、Match Result 等正式流程。
16. 小群体数据不能在 Dashboard 中被直接识别。
17. Debug Payload 不能静默进入长期 Warehouse。
18. Derived Event 必须记录来源和算法版本。

---

## 13. Event Authority

### 13.1 Server-Authoritative Events

适合：

- Purchase；
- Reward；
- Entitlement；
- Match Result；
- Rating；
- Economy Transaction；
- Enforcement；
- Save Commit。

### 13.2 Client-Observed Events

适合：

- UI Exposure；
- Interaction；
- Rendering；
- Input；
- Local Performance；
- Tutorial View。

### 13.3 Provider Events

适合：

- Payment；
- Email；
- Push；
- Ad；
- Platform Subscription。

### 13.4 Derived Events

由数据处理生成。

### 13.5 Authority Hierarchy

当多个来源冲突时：

```text
Business Authority
→ Trusted Provider
→ Server Observation
→ Client Observation
→ Derived Inference
```

具体按领域定义。

---

## 14. Event Trigger

事件应在明确业务节点触发。

例如：

```text
Purchase Completed
```

应在：

- Order 完成；
- Payment 已确认；
- Fulfillment 达到定义状态；

后发送，而不是玩家点击“购买”时。

### 14.1 Intent vs Outcome

区分：

- purchase.intent.created；
- checkout.opened；
- payment.authorized；
- order.completed；
- fulfillment.completed。

### 14.2 No Ambiguous Success

事件名称中 `completed` 必须与业务状态定义一致。

---

## 15. Event Properties

### 15.1 Required

没有则事件无意义。

### 15.2 Optional

仅在适用时存在。

### 15.3 Derived

由下游计算，不应由多个生产者各自填写。

### 15.4 Sensitive

需要额外权限、保留或禁止。

### 15.5 Enumerations

使用稳定枚举，不使用自由文本。

### 15.6 Units

所有数值明确：

- Currency；
- Duration；
- Distance；
- Percentage；
- Count；
- Bytes；
- Milliseconds。

### 15.7 Null Semantics

区分：

- Missing；
- Not Applicable；
- Unknown；
- Redacted；
- Zero。

---

## 16. Identity Model

### 16.1 Account Key

用于账户级分析的去标识键。

### 16.2 Device Key

用于设备级技术分析。

### 16.3 Session Key

用于一次使用会话。

### 16.4 Anonymous Key

登录前分析。

### 16.5 Household / Family

默认不应建立长期家庭画像，除非明确用途和合法基础。

### 16.6 Identity Resolution

匿名转登录时：

- 是否合并；
- 合并范围；
- 同意；
- 多设备；
- 跨平台；
- 删除；

需明确。

### 16.7 Avoid Direct Identifiers

普通分析事件不记录：

- Email；
- 电话；
- 真实姓名；
- 完整 IP；
- 支付凭据；
- 精确位置；
- 私聊正文。

---

## 17. Sessionization

### 17.1 Analytics Session

表示一段连续产品使用。

### 17.2 Game Session

表示具体游戏或活动会话。

### 17.3 Match Session

表示一场多人比赛。

### 17.4 Store Session

表示商业浏览和结账上下文。

这些不能混用。

### 17.5 Session Start

可由：

- App Foreground；
- Login；
- Main Menu；
- Gameplay Enter；

定义。

### 17.6 Session End

可由：

- Logout；
- App Background Timeout；
- Crash；
- Explicit Exit；
- Idle Timeout；

定义。

---

## 18. Time

### 18.1 Time Sources

- Server Time；
- Trusted Provider；
- Client Time；
- Monotonic Time。

### 18.2 Event Time

优先业务权威时间。

### 18.3 Client Clock

只用于本地表现，不用于高价值事务判断。

### 18.4 Time Zone

存储 UTC，展示时转换。

### 18.5 Day Boundary

DAU、Retention 和 Revenue 必须明确使用：

- UTC；
- Account Local；
- Region Local；
- Business Time Zone。

### 18.6 Late Events

延迟到达事件需按 Window 处理。

---

## 19. Event Ordering

### 19.1 Sequence

同一流程可以记录：

- Sequence Number；
- Revision；
- Tick；
- Transaction Version。

### 19.2 Out-of-Order

下游必须能处理：

- 延迟；
- 离线；
- Retry；
- Provider Callback；
- 多设备。

### 19.3 No Global Ordering Assumption

分布式系统通常没有全局严格顺序。

### 19.4 Business Reconciliation

对关键流程使用业务状态而非单纯事件顺序。

---

## 20. Deduplication

### 20.1 Dedupe Key

优先使用 `event_id`。

### 20.2 Domain Dedupe

关键业务事件可以额外使用：

- transaction_id；
- order_id；
- result_id；
- grant_id；
- session_id + revision。

### 20.3 Dedupe Window

按事件类型定义。

### 20.4 Duplicate Visibility

数据质量报表应显示重复率。

### 20.5 No Silent Double Counting

指标层必须显式去重策略。

---

## 21. Batching and Retry

### 21.1 Client Batching

减少网络和电量消耗。

### 21.2 Flush Triggers

- Batch Size；
- Time；
- App Background；
- Session End；
- Critical Event；
- Network Available。

### 21.3 Retry

使用：

- Exponential Backoff；
- Jitter；
- Max Retry；
- Expiry；
- Idempotency。

### 21.4 Priority

- Audit / Security；
- Critical Operational；
- Product Analytics；
- Low-Priority Interaction。

### 21.5 Retry Failure

丢弃策略必须可监控。

---

## 22. Offline Queue

### 22.1 Purpose

网络不可用时暂存低风险事件。

### 22.2 Limits

限制：

- Size；
- Time；
- Event Types；
- Sensitive Data；
- Encryption；
- Device Storage。

### 22.3 Expiry

过旧事件不应无限上传。

### 22.4 Ordering

上传后仍使用原 Event Time。

### 22.5 Consent Change

离线期间玩家撤回同意后，尚未上传的可选事件应删除。

---

## 23. Sampling

### 23.1 Why Sample

减少：

- 成本；
- 性能；
- 网络；
- 存储；
- 隐私风险。

### 23.2 Sampling Types

- Random；
- Deterministic；
- Account-Based；
- Session-Based；
- Event-Based；
- Adaptive；
- Error-Biased；
- Tail Sampling。

### 23.3 Never Sample Critical Audit

财务、权益、处罚和安全审计不能使用普通随机采样。

### 23.4 Sampling Metadata

事件记录：

- Sampling Rate；
- Sampling Rule；
- Sampling Version；
- Weight。

### 23.5 Deterministic Sampling

同一账号稳定进入或退出样本。

---

## 24. Client Performance Telemetry

### 24.1 Metrics

- FPS；
- Frame Time；
- Hitch；
- Load Time；
- Memory；
- CPU；
- GPU；
- Battery；
- Thermal；
- Network；
- Disk；
- Shader；
- Crash；
- ANR。

### 24.2 Context

包括：

- Device Class；
- Platform；
- Graphics Settings；
- Resolution；
- Scene；
- Content；
- Version。

### 24.3 Privacy

不记录不必要硬件序列号。

### 24.4 Sampling

高频性能数据必须采样和聚合。

### 24.5 Accessibility Impact

监控辅助设置和性能的交互，但不得暴露个人身份。

---

## 25. Server Operational Telemetry

### 25.1 Metrics

- Request Rate；
- Error Rate；
- Latency；
- Saturation；
- Queue Depth；
- Cache Hit；
- Database；
- Provider；
- Retry；
- Timeout；
- Capacity；
- Region；
- Deployment；
- Version。

### 25.2 Golden Signals

- Latency；
- Traffic；
- Errors；
- Saturation。

### 25.3 Domain Health

每个系统定义额外健康指标。

例如：

- Queue Join Success；
- Reward Grant Lag；
- Purchase Fulfillment Lag；
- Save Commit Failure；
- Notification Delivery；
- Entitlement Access Failure。

---

## 26. Error Telemetry

### 26.1 Error Definition

每类错误应有：

- Error Code；
- Domain；
- Severity；
- Recoverability；
- User Impact；
- Retry；
- Version；
- Correlation；
- Context。

### 26.2 Error Message

不使用自由文本作为主要聚合字段。

### 26.3 Stack Trace

只进入受控调试管道。

### 26.4 Player-Facing Error

通过 Error Code 关联，不记录完整玩家输入。

### 26.5 Unknown Errors

需要归类机制，不能长期全部进入 `unknown_error`。

---

## 27. Crash Reporting

### 27.1 Crash Record

可包含：

- Build；
- Platform；
- Device Class；
- Stack；
- Scene；
- Last Safe Events；
- Memory；
- Thread；
- Exception；
- Consent。

### 27.2 Minidump

高敏感，需严格访问和保留。

### 27.3 Breadcrumbs

只记录必要上下文。

### 27.4 Sensitive Redaction

移除：

- Chat；
- Tokens；
- Email；
- Payment；
- Personal Files；
- Clipboard；
- Precise Location。

### 27.5 Crash-Free Metrics

定义：

- Crash-Free Sessions；
- Crash-Free Users；
- Fatal；
- Non-Fatal；
- ANR。

---

## 28. Audit Logging

### 28.1 Audit Use Cases

- Order；
- Refund；
- Grant；
- Revocation；
- Price Change；
- Moderator Action；
- Admin Action；
- Migration；
- Feature Flag；
- Bulk Operation；
- Data Access；
- Support Recovery。

### 28.2 Audit Fields

- Actor；
- Action；
- Target；
- Before；
- After；
- Reason；
- Approval；
- Time；
- Environment；
- Correlation；
- Version；
- Source IP Reference；
- Result。

### 28.3 Integrity

Audit Log 需要：

- Append-Only；
- Tamper Evidence；
- Access Control；
- Retention；
- Export Control。

### 28.4 Audit vs Analytics

Audit 不采样，不用于普通增长报表。

---

## 29. Security Telemetry

### 29.1 Signals

- Login Failure；
- MFA；
- Token Abuse；
- Permission Denied；
- Suspicious Device；
- Fraud；
- Rate Limit；
- Bot；
- Injection；
- Admin Access；
- Data Export；
- Entitlement Forgery；
- Receipt Forgery。

### 29.2 Access

Security Team 和最小必要人员。

### 29.3 Separation

不将安全标签用于商业个性化。

### 29.4 Retention

依据安全、法律和调查需要。

---

## 30. Event Schema Evolution

### 30.1 Compatible Changes

- 新增 Optional Field；
- 新增 Enum Value（需消费者兼容）；
- 放宽非关键限制；
- 添加 Derived Metadata。

### 30.2 Breaking Changes

- 删除字段；
- 改字段类型；
- 改语义；
- 改单位；
- 改身份范围；
- 改触发点；
- 改 Required；
- 改隐私分类。

### 30.3 Version Strategy

- Semantic Version；
- Integer Version；
- Event Name + Version。

必须统一。

### 30.4 Dual Write

迁移期可以同时发送旧、新版本。

### 30.5 Consumer Readiness

发布前确认所有关键消费者兼容。

---

## 31. Data Contract

Data Contract 包含：

- Producer；
- Consumer；
- Event；
- Schema；
- Version；
- SLA；
- Volume；
- Latency；
- Completeness；
- Ordering；
- Deduplication；
- Privacy；
- Retention；
- Backfill；
- Owner；
- Deprecation。

### 31.1 Producer Responsibilities

- 正确触发；
- 正确字段；
- 正确版本；
- 不泄露敏感信息；
- 监控发送失败；
- 发布前测试。

### 31.2 Consumer Responsibilities

- 兼容版本；
- 不假设未承诺顺序；
- 处理重复和 Late Event；
- 不扩大数据用途；
- 监控数据质量。

### 31.3 Contract Breach

触发告警和 Runbook。

---

## 32. Data Quality Dimensions

### 32.1 Completeness

事件是否缺失。

### 32.2 Validity

字段是否符合 Schema。

### 32.3 Uniqueness

是否重复。

### 32.4 Timeliness

是否及时。

### 32.5 Consistency

跨源是否一致。

### 32.6 Accuracy

是否反映真实业务状态。

### 32.7 Coverage

平台、版本、地区是否完整。

### 32.8 Stability

事件量和分布是否异常。

---

## 33. Data Quality Rules

示例：

```text
purchase.order.completed
必须满足：
- order_id 非空；
- total_amount >= 0；
- currency 合法；
- 同一 order_id 只出现一次；
- 与 Order 表一致；
- 事件延迟小于目标；
- 不能来自 Client Authority。
```

### 33.1 Quality Severity

- Critical；
- High；
- Medium；
- Low。

### 33.2 Quality Action

- Alert；
- Quarantine；
- Block Dashboard Refresh；
- Mark Incomplete；
- Backfill；
- Rollback；
- Disable Consumer。

### 33.3 Do Not Hide Bad Data

Dashboard 显示质量状态。

---

## 34. Reconciliation

### 34.1 Purpose

将 Analytics 与业务权威状态对比。

### 34.2 Common Reconciliation

- Orders vs Purchase Events；
- Ledger vs Economy Events；
- Rewards vs Grant Events；
- Match Results vs Rating Events；
- Entitlements vs Access Events；
- Notifications vs Delivery Events；
- Save Commits vs Save Events。

### 34.3 Difference Handling

- Missing；
- Duplicate；
- Late；
- Wrong Amount；
- Wrong Version；
- Wrong Identity。

### 34.4 Authority

业务数据库或正式账本优先。

---

## 35. Metric Definition

每个 Metric 必须定义：

- Metric ID；
- Name；
- Purpose；
- Formula；
- Numerator；
- Denominator；
- Unit；
- Population；
- Time Window；
- Time Zone；
- Inclusion；
- Exclusion；
- Deduplication；
- Sampling；
- Data Sources；
- Freshness；
- Known Bias；
- Owner；
- Version。

### 35.1 Metric Example

```markdown
- Metric: Match Join Success Rate
- Numerator: 成功进入 Session 的 Match Assignment
- Denominator: 所有有效 Match Assignment
- Exclude: 玩家主动取消、Admin Test
- Window: Daily UTC
- Deduplication: Match ID
- Source: Match Assignment + Session Membership
```

### 35.2 No Unowned Metrics

没有 Owner 的关键指标不应成为正式 KPI。

---

## 36. KPI Framework

### 36.1 Outcome KPI

表示真正的产品结果。

### 36.2 Driver Metric

影响 Outcome 的因素。

### 36.3 Guardrail Metric

防止优化伤害。

### 36.4 Diagnostic Metric

用于解释。

### 36.5 Health Metric

系统和数据健康。

### 36.6 Balanced Set

例如：

```text
Primary:
- Core Activity Completion

Drivers:
- Time to First Success
- Feature Adoption

Guardrails:
- Error Rate
- Complaint Rate
- Accessibility Failure
- Refund Rate
- Notification Opt-Out

Health:
- Event Completeness
- Pipeline Delay
```

---

## 37. North Star Metric

### 37.1 Requirements

North Star 应：

- 连接真实玩家价值；
- 不易被刷；
- 不鼓励伤害；
- 可持续；
- 可分解；
- 不完全由付费定义。

### 37.2 Risks

单一 North Star 可能：

- 忽略小群体；
- 忽略安全；
- 忽略长期信任；
- 促进过度时长；
- 促进 Dark Pattern。

### 37.3 Guardrails Required

任何 North Star 都必须配套 Guardrail。

---

## 38. Funnel

### 38.1 Funnel Definition

- Entry；
- Step；
- Success；
- Failure；
- Time Window；
- Identity；
- Reentry；
- Branch；
- Exclusion。

### 38.2 Step Ordering

不要假设事件严格顺序。

### 38.3 Repeated Steps

定义：

- First；
- Last；
- Any；
- Count；
- Unique。

### 38.4 Drop-Off

需要结合：

- Error；
- Intentional Exit；
- Eligibility；
- Offline；
- Accessibility；
- Performance；
- Confusion。

### 38.5 No Automatic Blame

Drop-Off 不一定表示设计失败。

---

## 39. Cohort

### 39.1 Cohort Types

- Acquisition；
- Version；
- Platform；
- Region；
- Feature Exposure；
- Content；
- Skill；
- Subscription；
- Accessibility Setting；
- Return Player；
- New Player。

### 39.2 Sensitive Cohorts

涉及：

- Age；
- Child；
- Safety；
- Health；
- Identity；

需严格审查，通常避免普通分析。

### 39.3 Minimum Size

小样本抑制，避免重识别。

### 39.4 Cohort Stability

动态 Cohort 与固定 Cohort 区分。

---

## 40. Retention

### 40.1 Types

- Classic；
- Rolling；
- Return；
- Activity-Based；
- Feature Retention；
- Subscription Retention。

### 40.2 Anchor

定义 Day 0：

- Install；
- Account Creation；
- First Core Activity；
- Purchase；
- Subscription；
- Return。

### 40.3 Time Zone

必须明确。

### 40.4 Healthy Interpretation

Retention 不等于玩家价值的全部。

需要结合：

- 满意度；
- 投诉；
- 时长健康；
- 支出；
-安全；
-自愿回归。

---

## 41. Engagement

### 41.1 Metrics

- Active Days；
- Sessions；
- Core Actions；
- Content Depth；
- Social Sessions；
- Creation；
- Completion；
- Return Interval。

### 41.2 Avoid Time Maximization

更多时长不一定更好。

### 41.3 Healthy Engagement

关注：

- 自愿；
- 满足；
- 目标完成；
- 无过度压力；
- 无异常疲劳。

---

## 42. Economy Analytics

### 42.1 Sources

- Currency Earned；
- Currency Spent；
- Balance；
- Faucet；
- Sink；
- Item Creation；
- Item Destruction；
- Trade；
- Inflation；
- Price；
- Wealth Distribution。

### 42.2 Required Dimensions

- Currency；
- Source；
- Sink；
- Account Age；
- Progression；
- Content；
- Version；
- Region；
- Platform。

### 42.3 Critical Metrics

- Net Flow；
- Velocity；
- Median Balance；
- Distribution；
- Sink Coverage；
- Inflation；
- Hoarding；
- Negative Balance；
- Transaction Failure；
- Duplicate Grant。

### 42.4 Authority

Ledger 是权威，不使用客户端余额事件。

---

## 43. Progression Analytics

### 43.1 Metrics

- Time to Unlock；
- Level Distribution；
- Mastery；
- Upgrade Choice；
- Respec；
- Drop-Off；
- Gate Failure；
- Progression Pace；
- Catch-Up；
- Power Gap。

### 43.2 Risks

不能只看：

- 速度；
- 完成率；

还要看：

- 理解；
- 选择多样性；
- 资源压力；
- 付费差异；
- 回归体验；
- Accessibility。

---

## 44. Reward Analytics

### 44.1 Metrics

- Reward Earned；
- Granted；
- Claimed；
- Expired；
- Duplicate；
- Pending；
- Recovery；
- Value Distribution；
- Surprise；
- Satisfaction。

### 44.2 Failure Metrics

- Grant Lag；
- Claim Failure；
- Duplicate Grant；
- Missing Reward；
- Inventory Overflow；
- Expiry Regret。

### 44.3 Authority

Reward Instance 是权威。

---

## 45. Content Analytics

### 45.1 Metrics

- Discoverability；
- View；
- Unlock；
- Start；
- Completion；
- Replay；
- Abandon；
- Time；
- Difficulty；
- Reward；
- Return；
- Retirement Impact。

### 45.2 Content Lifecycle

比较：

- Announced；
- Available；
- Featured；
- Mature；
- Declining；
- Retired；
- Returned。

### 45.3 Avoid Popularity-Only Decisions

小众内容可能：

- 对特定群体高价值；
- 支持多样性；
- 支持长期身份；
- 是关键学习内容。

---

## 46. Social Analytics

### 46.1 Metrics

- Friend Request；
- Party Formation；
- Invite；
- Join；
- Session Together；
- Repeat Group；
- Voice；
- Ping；
- Block；
- Mute；
- Report；
- Social Exit。

### 46.2 Safety Guardrails

社交增长必须同时看：

- Harassment；
- Block；
- Complaint；
- Child Safety；
- Repeat Harm；
- Voice Dependency。

### 46.3 Privacy

不分析完整社交图谱以满足无关商业目的。

---

## 47. Matchmaking Analytics

### 47.1 Metrics

- Queue Wait；
- Match Quality；
- Skill Gap；
- Latency；
- Party Symmetry；
- Accept；
- Join；
- Completion；
- Disconnect；
- Remake；
- Requeue；
- Block Enforcement；
- Smurf；
- Win Trading。

### 47.2 Calibration

比较预测胜率与实际结果。

### 47.3 Fairness

按：

- Party；
- Input；
- Platform；
- Region；
- Rank；
- Role；
- Accessibility；

评估。

### 47.4 No Hidden Manipulation

分析系统不得反馈“为了留存安排输赢”的策略。

---

## 48. Moderation Analytics

### 48.1 Metrics

- Report；
- Triage；
- SLA；
- Enforcement；
- Appeal；
- Overturn；
- Repeat Harm；
- Block Enforcement；
- Child Safety；
- Moderator Agreement；
- Backlog。

### 48.2 Interpretation

低报告率可能意味着：

- 更安全；
- 举报难找；
- 失去信任；
- 玩家退出。

### 48.3 Privacy

普通分析不访问完整案件内容。

---

## 49. Commercial Analytics

### 49.1 Metrics

- Product View；
- Offer Exposure；
- Checkout；
- Purchase；
- Fulfillment；
- Refund；
- Chargeback；
- Subscription；
- Cancellation；
- Currency；
- Spending Limit；
- High-Spend Concentration；
- Child Control；
- Complaint。

### 49.2 Ethics Guardrails

必须看：

- Refund；
- Confusion；
- Dark Pattern；
- Offer Fatigue；
- Spending Health；
- Long-Term Trust；
- Child Impact。

### 49.3 Revenue Source

财务账本是收入权威，Analytics 仅作分析。

---

## 50. Feature Adoption

### 50.1 Adoption Definition

不只是打开页面。

可以定义：

- Exposed；
- Tried；
- Activated；
- Repeated；
- Retained；
- Value Achieved。

### 50.2 Exposure Required

未 Exposure 的玩家不应进入 Adoption 分母。

### 50.3 Eligibility

不符合资格者不进入合理分母。

### 50.4 Discovery vs Value

区分：

- 看见；
- 理解；
- 使用；
- 获得价值；
- 长期保留。

---

## 51. Experiment Exposure

### 51.1 Exposure Event

记录：

- Experiment ID；
- Variant；
- Assignment；
- Exposure Time；
- Surface；
- Eligibility；
- Version；
- Account；
- Session。

### 51.2 Assignment vs Exposure

被分组不等于实际看到。

### 51.3 Outcome Attribution

Outcome 必须发生在 Exposure 后，并在定义窗口内。

### 51.4 Multiple Experiments

记录重叠实验上下文。

### 51.5 Holdout

长期 Holdout 需要隐私和伦理评审。

---

## 52. Experiment Analysis

### 52.1 Pre-Registration

定义：

- Hypothesis；
- Primary Metric；
- Guardrails；
- Sample；
- Duration；
- Segments；
- Stop Conditions；
- Exclusions；
- Analysis Plan。

### 52.2 Avoid P-Hacking

- 不随意切片；
- 不只报告显著结果；
- 记录全部主要指标；
- 区分探索与确认。

### 52.3 Long-Term Effects

重要实验需要观察：

- Retention；
- Trust；
- Refund；
- Safety；
- Economy；
- Content Diversity；
- Accessibility。

### 52.4 Interference

社交、匹配和市场系统存在玩家间干扰，普通独立样本假设可能失效。

---

## 53. Attribution

### 53.1 Attribution Types

- Acquisition；
- Campaign；
- Referral；
- Notification；
- Offer；
- Content；
- Social Invite；
- Creator；
- Platform。

### 53.2 Attribution Window

明确：

- First Touch；
- Last Touch；
- Multi-Touch；
- View-Through；
- Click-Through；
- Expiry。

### 53.3 Privacy

遵守平台、同意和地区要求。

### 53.4 No False Precision

多渠道归因存在不确定性，应标记。

---

## 54. Dashboard

### 54.1 Dashboard Definition

每个 Dashboard 包含：

- Purpose；
- Audience；
- Owner；
- Metrics；
- Data Sources；
- Refresh；
- Time Zone；
- Filters；
- Quality Status；
- Known Limitations；
- Access；
- Version；
- Runbook。

### 54.2 Dashboard Types

- Executive；
- Product；
- Feature；
- Economy；
- Content；
- Live Ops；
- Commercial；
- Safety；
- Operational；
- Data Quality。

### 54.3 No Zombie Dashboards

长期无人使用和无人维护的 Dashboard 应归档。

### 54.4 Quality Banner

显示：

- Freshness；
- Incomplete；
- Backfill；
- Incident；
- Version Change。

---

## 55. Alerting

### 55.1 Alert Types

- Threshold；
- Ratio；
- Anomaly；
- Missing Data；
- Data Delay；
- Business Integrity；
- Safety；
- Financial；
- Operational；
- Capacity。

### 55.2 Alert Definition

- Alert ID；
- Metric；
- Threshold；
- Window；
- Severity；
- Owner；
- Channel；
- Runbook；
- Suppression；
- Escalation；
- Recovery；
- Version。

### 55.3 Alert Fatigue

控制：

- 去重；
- 聚合；
- 冷却；
- Maintenance Window；
- Dependency Awareness；
- Severity。

### 55.4 Actionable

没有 Owner 和 Runbook 的高频告警应被修复或移除。

---

## 56. SLO and SLA

### 56.1 Analytics SLO

例如：

- Event Ingestion Availability；
- Data Freshness；
- Dashboard Availability；
- Quality；
- Backfill Time；
- Deletion Completion。

### 56.2 Domain Telemetry SLO

例如：

- Purchase Event Delay；
- Match Result Event Completeness；
- Reward Grant Reconciliation；
- Save Failure Visibility。

### 56.3 Error Budget

用于平衡：

- 新功能；
- 数据稳定；
- Pipeline 变更；
- Backfill。

---

## 57. Data Freshness

### 57.1 Freshness Classes

- Real-Time；
- Near Real-Time；
- Hourly；
- Daily；
- Weekly；
- Batch。

### 57.2 Use Case Fit

- 事故和交易：近实时；
- 产品趋势：小时或日；
- 长期战略：周或月。

### 57.3 Freshness Display

Dashboard 明确最近更新时间。

### 57.4 Stale Data

过期数据不得被无提示用于运营决策。

---

## 58. Real-Time Analytics

### 58.1 Suitable Uses

- Incident；
- Capacity；
- Fraud；
- Live Event；
- Purchase Failure；
- Queue Health；
- Safety Spike；
- Deployment Regression。

### 58.2 Risks

- 成本；
- 噪声；
- 误判；
- 过度反应；
- 不完整；
- Late Event。

### 58.3 No Real-Time Business Mutation by Default

实时指标不直接修改高价值业务状态，除非经过专门控制系统和安全审查。

---

## 59. Replay and Backfill

### 59.1 Replay

从原始事件重新处理。

### 59.2 Backfill

补算缺失或修正历史。

### 59.3 Requirements

- Source；
- Scope；
- Time Range；
- Code Version；
- Schema；
- Output Version；
- Idempotency；
- Validation；
- Approval；
- Rollback。

### 59.4 Backfill Isolation

不要让 Backfill 触发：

- 玩家通知；
- 奖励；
- 交易；
- 处罚；
- Live Operation；

除非明确设计。

### 59.5 Dashboard Marking

Backfill 期间标记数据状态。

---

## 60. Derived Data

### 60.1 Examples

- Session；
- Cohort；
- Skill Segment；
- Churn Risk；
- Content Preference；
- Economy Health；
- Safety Risk。

### 60.2 Requirements

- 输入；
- 算法；
- 版本；
- 用途；
- 误差；
- 访问；
- 保留；
- 解释；
- 禁止用途。

### 60.3 High-Risk Derived Data

例如：

- 脆弱性；
- 付费冲动；
- 安全风险；
- 儿童推断；
- 身份推断。

默认禁止用于普通商业优化。

---

## 61. Predictive Models

### 61.1 Use Cases

- Capacity；
- Churn；
- Match Quality；
- Fraud；
- Content Recommendation；
- Support Prioritization；
- Error Prediction。

### 61.2 Model Governance

- Purpose；
- Training Data；
- Features；
- Bias；
- Version；
- Evaluation；
- Monitoring；
- Human Oversight；
- Rollback；
- Appeal；
- Prohibited Uses。

### 61.3 Prediction Is Not Fact

模型输出必须标记为推断。

### 61.4 High-Risk Decisions

不能仅凭模型：

- 永久处罚；
- 拒绝退款；
- 提高个人价格；
- 剥夺权益；
- 识别儿童；
- 判定危机；
- 安排输赢。

---

## 62. Privacy Consent

### 62.1 Consent Categories

- Essential；
- Product Analytics；
- Personalization；
- Advertising；
- Crash Reporting；
- Voice Safety；
- Research；
- Marketing Attribution。

### 62.2 Consent Context

事件记录：

- Consent Version；
- Category；
- Effective Time；
- Region；
- Age Policy。

### 62.3 Withdrawal

撤回后：

- 停止新采集；
- 删除本地未上传队列；
- 按政策处理历史数据；
- 更新个性化；
- 不降低核心功能，除非数据确实必要。

### 62.4 Essential Telemetry

必须有明确合法和产品必要性。

---

## 63. Data Minimization

### 63.1 Questions

每个字段都应回答：

- 为什么需要；
- 谁使用；
- 保留多久；
- 是否能聚合；
- 是否能去标识；
- 是否能在客户端计算；
- 是否能使用枚举代替文本；
- 删除后如何处理。

### 63.2 Avoid Payload Dumping

禁止把整个对象、存档、请求或聊天直接作为普通事件属性。

### 63.3 Context Limits

只记录诊断所需的少量上下文。

---

## 64. Sensitive Data

包括：

- 直接身份；
- 精确位置；
- 支付；
- 儿童；
- 健康；
- 安全 Case；
- 私聊；
- 语音；
- 设备指纹；
- 法律；
- 账户恢复；
- 真实姓名。

### 64.1 Handling

- 分类；
- 加密；
- 最小访问；
- 独立保留；
- 审计；
- 脱敏；
- 限制导出；
- 禁止普通 Dashboard。

### 64.2 No Sensitive Dimensions in Small Cohorts

防止重识别。

---

## 65. Data Retention

### 65.1 Retention by Purpose

不同数据：

- Product Analytics；
- Crash；
- Audit；
- Security；
- Finance；
- Experiment；
- Support；
- Raw Event；
- Aggregate；

使用不同期限。

### 65.2 Retention Fields

- Data Category；
- Purpose；
- Duration；
- Start；
- Legal Hold；
- Delete；
- Aggregate；
- Owner；
- Region。

### 65.3 Raw vs Aggregate

原始数据通常比聚合数据保留更短。

### 65.4 Retention Enforcement

自动化，并有完成验证。

---

## 66. Data Deletion

### 66.1 Deletion Scope

- Raw Events；
- Warehouse；
- Feature Store；
- Dashboard Cache；
- Model Training Data；
- Exports；
- Backups；
- Support；
- Data Lake；
- Attribution；
- Offline Queue。

### 66.2 Deletion Types

- Delete；
- Anonymize；
- Aggregate；
- Suppress；
- Legal Hold Exemption。

### 66.3 Deletion Workflow

```text
Request
→ Identity Resolve
→ Scope Determine
→ Execute
→ Verify
→ Propagate
→ Complete
```

### 66.4 Audit

记录完成和例外，不保留不必要原始数据。

---

## 67. Access Control

### 67.1 Roles

- Product Analyst；
- Data Scientist；
- Engineer；
- Finance；
- Trust and Safety；
- Support；
- Executive；
- Privacy；
- Security；
- Vendor。

### 67.2 Least Privilege

默认访问聚合数据。

### 67.3 Row / Column Security

按：

- Region；
- Data Class；
- Team；
- Purpose；
- Environment；

控制。

### 67.4 Temporary Access

有期限、审批和审计。

### 67.5 Export

高风险导出需要额外审批。

---

## 68. Data Catalog

### 68.1 Catalog Contents

- Event；
- Metric；
- Table；
- Dashboard；
- Owner；
- Description；
- Version；
- Privacy Class；
- Lineage；
- Retention；
- Consumers；
- Quality；
- Deprecation。

### 68.2 Search

团队应能找到：

- 是否已有事件；
- 是否已有指标；
- 谁拥有；
- 数据是否可信；
- 最近变化。

### 68.3 Certification

关键数据可标记：

- Draft；
- Verified；
- Certified；
- Deprecated；
- Broken。

---

## 69. Data Lineage

### 69.1 Purpose

知道：

```text
某 Dashboard 数字来自哪些事件、表、Job 和算法。
```

### 69.2 Lineage Levels

- Event → Raw；
- Raw → Clean；
- Clean → Aggregate；
- Aggregate → Metric；
- Metric → Dashboard；
- Dashboard → Decision。

### 69.3 Change Impact

Schema 变更前识别消费者。

### 69.4 Incident

快速定位受影响报表。

---

## 70. Data Ownership

### 70.1 Event Owner

负责语义和生产。

### 70.2 Metric Owner

负责公式和解释。

### 70.3 Pipeline Owner

负责处理和 SLA。

### 70.4 Dashboard Owner

负责使用和维护。

### 70.5 Privacy Owner

负责合法用途和保留。

### 70.6 No Shared Ambiguity

“Data Team 负责全部”不是有效 Owner 模型。

---

## 71. Instrumentation Workflow

```text
Define Question
→ Define Hypothesis
→ Define Metric
→ Check Existing Data
→ Define Event Contract
→ Privacy Review
→ Implement
→ Automated Test
→ Staging Validation
→ Production Shadow
→ Release
→ Quality Monitor
→ Analysis
→ Deprecate
```

### 71.1 Instrument Before Release

关键功能在发布前完成最小必要遥测。

### 71.2 Avoid Instrument Everything

只记录能回答明确问题的数据。

### 71.3 Definition of Done

功能完成应包含：

- Event Contract；
- Test；
- Dashboard；
- Quality Rule；
- Owner；
- Privacy；
- Runbook。

---

## 72. Instrumentation Testing

### 72.1 Unit Test

- Trigger；
- Required Fields；
- Version；
- Enum；
- Consent；
- Redaction。

### 72.2 Integration Test

- Pipeline；
- Retry；
- Dedupe；
- Late Event；
- Offline；
- Multi-Device；
- Provider。

### 72.3 Contract Test

Producer 和 Consumer Schema。

### 72.4 End-to-End Test

从业务动作到 Dashboard。

### 72.5 Production Validation

使用测试账户或标记流量。

---

## 73. Telemetry Performance Budget

### 73.1 Client Budget

限制：

- CPU；
- Memory；
- Disk；
- Network；
- Battery；
- Startup；
- Frame Time。

### 73.2 Event Volume Budget

每个功能有预计量。

### 73.3 High-Frequency Events

优先：

- 聚合；
- 采样；
- 直方图；
- 时间窗；
- 本地计算。

### 73.4 Kill Switch

遥测异常时可以关闭特定事件，不影响产品。

---

## 74. Cost Governance

### 74.1 Cost Sources

- Event Volume；
- Storage；
- Compute；
- Query；
- Dashboard；
- Real-Time；
- Export；
- Third-Party；
- Retention。

### 74.2 Cost per Value

低价值高成本事件应删除或降采样。

### 74.3 Budget

按：

- Domain；
- Environment；
- Event；
- Team；
- Product；

监控。

### 74.4 No Cost-Driven Privacy Tradeoff

不能因存储便宜就无限保留。

---

## 75. Third-Party Analytics

### 75.1 Risks

- 数据出境；
- SDK 性能；
- 隐私；
- 供应商锁定；
- 黑箱处理；
- 删除困难；
- 儿童；
- 广告共享。

### 75.2 Requirements

- Vendor Review；
- Data Processing Agreement；
- Data Map；
- Consent；
- SDK Audit；
- Access；
- Retention；
- Deletion；
- Incident；
- Exit Plan。

### 75.3 Minimize SDKs

避免多个 SDK 重复采集。

### 75.4 No Hidden Data Sharing

向第三方共享必须透明并符合用途。

---

## 76. Support Diagnostics

### 76.1 Support View

可查看：

- Account Health Summary；
- Version；
- Platform；
- Recent Error Codes；
- Save Status；
- Purchase Status；
- Entitlement Status；
- Notification Status；
- Session IDs；
- Correlation IDs；
- Data Freshness。

### 76.2 Redaction

不展示：

- 私聊；
- 完整 IP；
- 支付凭据；
- 安全证据；
- 不必要设备标识。

### 76.3 Diagnostic Token

玩家可主动生成短期诊断包。

### 76.4 Purpose Limitation

Support 数据不进入普通商业画像。

---

## 77. Live Operations Integration

### 77.1 Live Health

监控：

- Event Entry；
- Participation；
- Completion；
- Reward；
- Error；
- Capacity；
- Economy；
- Safety；
- Commercial；
- Notification。

### 77.2 Pre-Launch Baseline

活动前建立基准。

### 77.3 Real-Time Guardrails

可触发：

- Alert；
- Pause；
- Kill Switch；
- Capacity Scale；
- Communication。

### 77.4 No Automatic Player Punishment

运营指标异常不能直接处罚玩家。

---

## 78. Versioning Integration

### 78.1 Version Fields

所有关键事件记录：

- Client；
- Server；
- Content；
- Rule；
- Schema；
- Experiment；
- Economy；
- Rating；
- Offer；
- Entitlement。

### 78.2 Migration Windows

分析必须能区分迁移前后。

### 78.3 Mixed Versions

多版本并存时避免直接合并不兼容指标。

### 78.4 Historical Recompute

指标公式变化时保留旧版本或重算并明确。

---

## 79. Analytics Incident Management

### 79.1 Incident Types

- Pipeline Down；
- Event Loss；
- Duplicate Spike；
- Schema Break；
- Wrong Metric；
- Privacy Leak；
- Dashboard Stale；
- Experiment Exposure Missing；
- Audit Log Missing；
- Deletion Failure；
- Cost Spike；
- Third-Party Incident；
- Identity Merge Error。

### 79.2 Severity

#### SEV-1

隐私泄露、财务审计缺失、大规模错误决策风险。

#### SEV-2

关键事件缺失或严重错误。

#### SEV-3

局部 Dashboard 或延迟。

#### SEV-4

低影响文档或非关键事件问题。

### 79.3 Actions

- Stop Ingestion；
- Quarantine；
- Disable Consumer；
- Mark Dashboard；
- Rollback Schema；
- Backfill；
- Notify；
- Preserve Audit；
- Privacy Incident；
- Root Cause；
- Repair；
- Review Decisions Made on Bad Data。

---

## 80. Failure and Recovery

| Failure | Cause | Player Impact | Recovery | Data Guarantee |
|---|---|---|---|---|
| Event Send Failed | 网络或 SDK | 分析缺失 | Retry / Offline Queue | 不阻断业务 |
| Duplicate Event | Retry / 多端 | 指标放大 | Event ID 去重 | 原事件保留 |
| Schema Invalid | Producer 变更 | Pipeline Reject | Quarantine、修复、Backfill | 不污染 Certified Data |
| Event Loss | Pipeline 故障 | 分析不完整 | Source Replay、Backfill | 标记缺口 |
| Late Event | 离线或 Provider | 指标延迟 | Window Recompute | Event Time 保留 |
| Wrong Identity Merge | 账号映射 | 串号分析 | Split、Recompute、Privacy Review | 原映射审计 |
| Consent Enforcement Failed | SDK / Cache | 未授权采集 | Stop、Delete、Incident | 同意版本保留 |
| Dashboard Wrong Formula | Metric 变更 | 错误决策 | Rollback、Recompute、Notice | Metric Version 保留 |
| Experiment Exposure Missing | Instrumentation | 结果不可解释 | Exclude、Repair、Rerun | 不伪造 Exposure |
| Audit Log Missing | Pipeline / 权限 | 无法审计 | Failover、Source Log | 高风险操作升级 |
| Deletion Incomplete | 派生或备份遗漏 | 隐私风险 | Propagate、Verify、Incident | 删除任务审计 |
| Cost Explosion | 高频事件 | 运营成本 | Kill Switch、Sampling | 核心业务不受影响 |

---

## 81. Edge Cases

### Identity

- 匿名转登录；
- 多设备；
- 多平台；
- 账户合并；
- 账户删除；
- 家庭共享；
- Guest；
- 平台解绑。

### Events

- 重试；
- 离线；
- Late；
- Duplicate；
- Out-of-Order；
- Version Change；
- Provider Delay；
- App Crash；
- Batch Partial Failure。

### Consent

- 会话中撤回；
- 地区变化；
- 儿童转成年；
- 设备设置不同；
- Offline Queue；
- Third-Party SDK；
- Legal Hold。

### Metrics

- 分母变化；
- Event Definition 变化；
- Time Zone；
- Sample Rate；
- Backfill；
- Data Gap；
- Bot；
- Test Account；
- Internal Account。

### Experiments

- Exposure 重复；
- 多实验冲突；
- Assignment 漂移；
- Feature Flag 与 Variant 不一致；
- 早停；
- Long-Term Holdout；
- Network Interference；
- Social Spillover。

---

## 82. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| Core Loop | Soft / Hard | Core → Analytics | Core Activity | 无法评估体验 |
| Game State and Flow | Hard / Soft | Flow → Analytics | State / Session | Funnel 错误 |
| Rules and Resolution | Hard | Rules → Analytics | Outcome / Version | 结果误解 |
| Resources and Economy | Critical | Economy → Analytics | Ledger / Flow | 经济监控错误 |
| Progression System | Hard | Progression → Analytics | Pace / Gate | 成长误判 |
| Reward System | Critical | Reward → Analytics | Grant / Claim | 奖励质量错误 |
| Content Systems | Hard | Content → Analytics | Discovery / Completion | 内容决策错误 |
| Save and Persistence | Hard / Soft | Save → Analytics | Commit / Failure | 稳定性不可见 |
| Settings and Preferences | Critical | Settings → Analytics | Consent / Privacy | 合规风险 |
| Notification and Reminders | Hard / Soft | Notification → Analytics | Delivery / Open | 触达误判 |
| Social and Multiplayer | Hard | Social → Analytics | Party / Session | 社交健康错误 |
| Matchmaking and Competition | Critical | Matchmaking → Analytics | Quality / Result | 公平风险 |
| Moderation and Safety | Critical Boundary | Safety → Analytics | Aggregate Safety | 隐私和误判风险 |
| Monetization System | Critical | Monetization → Analytics | Order / Fulfillment | 收入误判 |
| Offers and Pricing | Hard | Pricing → Analytics | Exposure / Quote | 价格实验错误 |
| Entitlement and Ownership | Critical | Entitlement → Analytics | Grant / Access | 拥有状态错误 |
| Live Operations | Critical | 双向 | Health / Event | 运营事故 |
| Experiment Management | Critical | 双向 | Exposure / Outcome | 实验无效 |
| Versioning and Migration | Critical | 双向 | Schema / Version | 历史不可比 |

---

## 83. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Event Definition | 是 | Analytics | Catalog 发布 | 长期版本 | Version History |
| Event Schema | 是 | Analytics | Schema 发布 | 长期 | Registry |
| Raw Event | 是 | Event Pipeline | Ingestion | 短期至中期 | Replay |
| Clean Event | 是 | Data Platform | Validation | 中期 | Rebuild |
| Metric Definition | 是 | Analytics | Metric 发布 | 长期版本 | Catalog |
| Aggregate | 是或可重算 | Analytics | Job 完成 | 业务期 | Backfill |
| Dashboard Definition | 是 | Analytics | 发布变化 | 长期 | Version |
| Alert Definition | 是 | Operations | 发布变化 | 长期 | LKG |
| Data Quality Result | 是 | Analytics | 检查运行 | 质量期 | Recompute |
| Experiment Exposure | 是 | Experiment / Analytics | Exposure | 实验及审计期 | Source Replay |
| Audit Event | 是 | Audit System | 高风险动作 | 长期 | Append-Only |
| Deletion State | 是 | Privacy | 删除变化 | 审计期 | Retry |
| Backfill Record | 是 | Analytics | Backfill | 长期审计 | Rollback |
| Access Audit | 是 | Security / Analytics | 数据访问 | 审计期 | Security Log |

---

## 84. Accessibility and Inclusive Analysis

### 84.1 Accessibility Settings

可以分析功能使用和成功，但：

- 使用聚合；
- 避免小群体重识别；
- 不用于价格和商业压力；
- 不把辅助设置当负面信号；
- 不公开个人状态。

### 84.2 Inclusive Segmentation

评估：

- Input Method；
- Screen Reader；
- Reduced Motion；
- Color Adjustment；
- Caption；
- Difficulty Assist；
- Device Performance。

### 84.3 Interpretation

较长操作时间可能来自：

- 读屏；
- 辅助输入；
- 理解；
- 网络；
- 设备；
- 刻意探索。

不能简单标记为摩擦。

### 84.4 Research Partnership

重要可访问性结论应结合定性研究。

---

## 85. Ethical Review

### 85.1 Metric Ethics

每个 KPI 问：

- 优化它会伤害谁；
- 能否被刷；
- 是否促进过度时长；
- 是否促进高压消费；
- 是否忽略非主流玩家；
- 是否压低报告和投诉；
- 是否牺牲安全和隐私。

### 85.2 Prohibited Uses

禁止将以下数据用于普通商业优化：

- 自伤；
- 危机；
- 儿童脆弱性；
- Moderation Case；
- 安全处罚；
- 健康；
- 精确财务困境；
- 私聊和语音内容；
- 账户恢复风险。

### 85.3 Long-Term Guardrails

至少包括：

- Trust；
- Refund；
- Complaint；
- Safety；
- Accessibility；
- Churn after Pressure；
- High-Spend Concentration；
- Notification Opt-Out；
- Support Burden。

### 85.4 Data Does Not Remove Responsibility

“数据显著”不是伦理免责。

---

## 86. Security

### 86.1 Threats

- Event Injection；
- Identity Spoof；
- Metric Manipulation；
- Dashboard Unauthorized Access；
- Data Export；
- Schema Poisoning；
- Audit Tampering；
- Consent Bypass；
- Deletion Bypass；
- Third-Party Leak；
- Insider Abuse；
- Query Exfiltration；
- Reidentification；
- Model Poisoning。

### 86.2 Controls

- Authentication；
- Authorization；
- Signed Server Events；
- Schema Registry；
- Rate Limit；
- Encryption；
- RBAC；
- Row / Column Security；
- Audit；
- Anomaly Detection；
- Data Loss Prevention；
- Tokenization；
- Environment Separation；
- Approval；
- Retention Enforcement。

### 86.3 Client Events

默认不可信，需要：

- Validation；
- Rate Limit；
- Fraud Detection；
- Server Cross-Check。

---

## 87. Analytics Contract Template

```markdown
# Analytics Event Contract

## Event

- Event ID:
- Event Name:
- Domain:
- Purpose:
- Authority:
- Trigger:
- Version:

## Identity

- Account Scope:
- Device Scope:
- Session:
- Anonymous:
- Consent:

## Properties

| Property | Type | Required | Unit | Sensitive | Description |
|---|---|---:|---|---:|---|

## Delivery

- Producer:
- Event Time:
- Retry:
- Batch:
- Offline:
- Sampling:
- Dedupe:
- Ordering:

## Governance

- Retention:
- Access:
- Consumers:
- Quality Rules:
- Deprecation:
- Owner:
```

---

## 88. Metric Contract Template

```markdown
# Metric Contract

## Definition

- Metric ID:
- Name:
- Purpose:
- Owner:
- Version:

## Formula

- Numerator:
- Denominator:
- Unit:
- Aggregation:
- Time Window:
- Time Zone:

## Population

- Included:
- Excluded:
- Identity:
- Deduplication:
- Sampling:

## Sources

| Source | Version | Authority | Freshness |
|---|---|---|---|

## Interpretation

- Success Meaning:
- Failure Meaning:
- Known Bias:
- Guardrails:
- Misuse Risks:

## Operations

- Dashboard:
- Alert:
- SLA:
- Backfill:
- Deprecation:
```

---

## 89. Dashboard Contract Template

```markdown
# Dashboard Contract

## Purpose

- Dashboard ID:
- Audience:
- Owner:
- Decision Supported:

## Metrics

| Metric | Version | Freshness | Quality |
|---|---|---|---|

## Filters

- Platform:
- Region:
- Version:
- Cohort:
- Time Zone:

## Governance

- Access:
- Sensitive Data:
- Refresh:
- Known Limitations:
- Data Quality Banner:
- Runbook:
- Review Cadence:
- Archive Rule:
```

---

## 90. Alert Contract Template

```markdown
# Alert Contract

## Alert

- Alert ID:
- Metric:
- Severity:
- Owner:
- Purpose:

## Trigger

- Condition:
- Window:
- Baseline:
- Suppression:
- Maintenance:
- Dependency:

## Response

- Channel:
- Runbook:
- First Action:
- Escalation:
- Recovery:
- Close Condition:

## Validation

- False Positive:
- False Negative:
- Test:
- Review Cadence:
```

---

## 91. Analytics and Telemetry Debt

包括：

- 多套事件命名；
- 同一指标多个定义；
- 无 Event Catalog；
- 无 Metric Owner；
- Client Event 被当成业务事实；
- 高价值事件被采样；
- Schema 变更无版本；
- Dashboard 无质量状态；
- 实验无 Exposure；
- Consent 只在客户端；
- Raw Payload Dump；
- 小群体可被识别；
- Data Retention 不执行；
- 删除请求无法覆盖派生数据；
- Audit 和 Analytics 混用；
- 高成本事件无人使用；
- Support 使用过度原始数据；
- 第三方 SDK 重复采集。

### 91.1 Signals

- 团队争论同一 KPI 数字；
- 发布后才发现事件缺失；
- Dashboard 经常手工修正；
- 数据量增长但决策质量不提高；
- 实验结果无法复现；
- 事故时找不到 Correlation ID；
- 账户删除后数据仍存在；
- 安全和商业团队使用同一原始数据集；
- Analytics SDK 影响性能；
- 同一事件不同平台字段不同。

### 91.2 Reduction

- Event Registry；
- Metric Catalog；
- Contract Testing；
- Authority Mapping；
- Quality Monitoring；
- Consent Gateway；
- Retention Automation；
- Deletion Propagation；
- Audit Separation；
- Sampling Governance；
- Dashboard Ownership；
- Alert Runbook；
- Cost Review；
- Third-Party SDK Review；
- Quarterly Data Health Review。

---

## 92. Rollout and Migration

### 92.1 Rollout

分析系统变更应按：

```text
Contract Draft
→ Privacy Review
→ Schema Review
→ Local Test
→ Staging
→ Shadow Production
→ Small Cohort
→ Broad Production
→ Certified
```

### 92.2 Shadow Production

事件发送到隔离管道，不影响正式指标。

### 92.3 High-Risk Changes

包括：

- Identity；
- Consent；
- Sensitive Data；
- Event Authority；
- Metric Formula；
- Experiment Exposure；
- Audit；
- Deletion；
- Retention；
- Third-Party SDK；
- Revenue；
- Safety；
- Child Data。

### 92.4 Migration

必须定义：

- Event Name；
- Schema；
- Version；
- Producer；
- Consumer；
- Metric；
- Dashboard；
- Alert；
- Sampling；
- Identity；
- Consent；
- Retention；
- Lineage；
- Backfill；
- Deprecation。

### 92.5 Dual Write

迁移期间：

- 比较旧、新事件；
- 验证数量和分布；
- 验证 Metric；
- 不长期双写。

### 92.6 Rollback

回滚时：

- 恢复旧 Schema；
- 停止新事件；
- 保留原始数据；
- 标记 Dashboard；
- 不伪造缺失；
- 回滚 Metric；
- 记录 Migration；
- 必要时 Backfill。

### 92.7 Stop Conditions

出现以下情况应停止发布：

- 未授权数据采集；
- Sensitive Data 泄露；
- 高价值事件大规模缺失；
- Duplicate 激增；
- Schema 破坏关键消费者；
- Experiment Exposure 丢失；
- 财务指标与账本严重不一致；
- Audit Log 丢失；
- Deletion 失败；
- 客户端性能显著下降；
- 第三方 SDK 异常；
- Dashboard 产生高风险错误决策。

---

## 93. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| 事件量增长但价值不足 | Cost Risk | 高 | 高 | Event Budget | Analytics |
| Client Event 被伪造 | Integrity Risk | 高 | 高 | Server Reconciliation | Engineering |
| Metric 定义分裂 | Decision Risk | 高 | 高 | Metric Catalog | Product |
| Consent 执行失效 | Privacy Risk | 严重 | 中 | Consent Gateway | Privacy |
| Experiment Exposure 缺失 | Experiment Risk | 高 | 中 | Exposure Contract | Data |
| 删除未传播到派生数据 | Privacy Risk | 严重 | 中 | Deletion Lineage | Data Platform |
| 小群体被重识别 | Privacy Risk | 严重 | 低 | Minimum Cohort | Privacy |
| Dashboard 过期仍被使用 | Decision Risk | 高 | 高 | Quality Banner | Analytics |
| 第三方 SDK 泄露 | Vendor Risk | 严重 | 中 | Vendor Review | Security |
| 数据驱动 Dark Pattern | Ethical Risk | 严重 | 中 | Guardrail Governance | Product |
| Analytics Debt 持续增长 | Architecture Risk | 高 | 高 | Contract Governance | Architecture |

---

## 94. Review Checklist

### Event Definition

- [ ] Event 有明确业务问题；
- [ ] Event Name 稳定；
- [ ] Trigger 唯一；
- [ ] Authority 明确；
- [ ] Required / Optional / Sensitive 字段已定义；
- [ ] Version、Owner 和 Retention 完整。

### Envelope and Delivery

- [ ] Event ID 唯一；
- [ ] Event Time / Ingestion Time 分离；
- [ ] Correlation ID 可用；
- [ ] Retry 幂等；
- [ ] Offline Queue 有限制；
- [ ] Analytics Failure 不阻断核心流程。

### Authority and Quality

- [ ] 高价值事实使用 Server 或 Provider；
- [ ] Client Event 不被直接当作交易事实；
- [ ] Dedupe 和 Ordering 明确；
- [ ] Quality Rule 可告警；
- [ ] Reconciliation 已定义；
- [ ] Dashboard 显示质量状态。

### Metrics

- [ ] Formula、Population、Window 和 Time Zone 清楚；
- [ ] Owner 和 Version 完整；
- [ ] Sampling 和 Bias 明确；
- [ ] Guardrail 完整；
- [ ] 不同团队使用同一 Certified Definition。

### Experiments

- [ ] Assignment 与 Exposure 区分；
- [ ] Outcome 能关联 Exposure；
- [ ] Primary、Guardrail 和 Stop Conditions 预先定义；
- [ ] 重叠实验可追踪；
- [ ] 不使用高风险敏感信号。

### Privacy

- [ ] Consent Category 明确；
- [ ] 撤回后停止采集；
- [ ] Offline Queue 能删除；
- [ ] Data Minimization 通过评审；
- [ ] Sensitive Data 独立管理；
- [ ] Retention 和 Deletion 可执行。

### Security and Access

- [ ] RBAC、Row / Column Security 完整；
- [ ] Export 有审批；
- [ ] Third-Party SDK 已审查；
- [ ] Audit 与 Analytics 分离；
- [ ] Client Injection 和 Identity Spoof 有防护。

### Operations

- [ ] Dashboard 有 Owner；
- [ ] Alert 有 Runbook；
- [ ] SLO 和 Freshness 可见；
- [ ] Replay、Backfill 和 Rollback 可执行；
- [ ] Cost 和 Volume 有预算；
- [ ] Incident 分级和停止条件明确。

### Ethics and Accessibility

- [ ] KPI 不只优化时长和收入；
- [ ] Long-Term Guardrail 完整；
- [ ] 不使用危机、儿童和处罚数据商业化；
- [ ] Accessibility Cohort 有隐私保护；
- [ ] 定量结论与定性研究结合。

---

## 95. V1 Completion Criteria

Analytics and Telemetry 可以被视为 V1，当：

- Product Analytics、Operational Telemetry、Debug Logging、Audit Logging 和 Security Logging 的边界明确；
- Event Definition、Schema、Version、Metric、Dashboard、Alert、Contract、Lineage 和 Retention 实体完整；
- Domain Fact、Interaction、State Transition、Exposure、Outcome、Error、Performance、Operational 和 Audit Event 分类完整；
- Event Naming、Envelope、Authority、Trigger、Required Property、Sensitive Property 和 Unit 规则明确；
- Event ID、Retry、Dedupe、Ordering、Late Event、Offline Queue、Batching 和 Sampling 可执行；
- Server、Client、Provider 和 Derived Event 的信任边界明确；
- Session、Identity、Anonymous Merge、Time Zone 和 Day Boundary 规则完整；
- Client Performance、Server Health、Error、Crash、Audit 和 Security Telemetry 有专项规则；
- Event Schema Evolution、Dual Write、Consumer Compatibility 和 Deprecation 可执行；
- Data Contract、Producer / Consumer Responsibility 和 Contract Breach 规则完整；
- Completeness、Validity、Uniqueness、Timeliness、Consistency、Accuracy、Coverage 和 Stability 可监控；
- Reconciliation 能将 Analytics 与 Order、Ledger、Reward、Match Result 和 Entitlement 等权威源对账；
- Metric Definition、KPI、North Star、Driver、Guardrail、Diagnostic 和 Health Metric 体系完整；
- Funnel、Cohort、Retention、Engagement、Attribution 和 Feature Adoption 语义清楚；
- Economy、Progression、Reward、Content、Social、Matchmaking、Moderation 和 Commercial Analytics 有明确指标和权威来源；
- Experiment Assignment、Exposure、Outcome、Pre-Registration、Long-Term Effect 和 Interference 规则完整；
- Dashboard、Alert、SLO、Freshness、Real-Time、Replay 和 Backfill 可执行；
- Derived Data 和 Predictive Model 有用途、版本、偏差、禁止用途和人工监督；
- Consent、Data Minimization、Sensitive Data、Retention、Deletion、Access Control 和 Catalog 通过隐私评审；
- Accessibility 和 Inclusive Analysis 能识别不同输入和辅助设置，但不用于歧视或商业压力；
- Analytics Performance Budget、Cost Governance 和 Third-Party SDK Review 完整；
- Support、Live Operations 和 Versioning Integration 有明确边界；
- Event、Metric、Dashboard、Alert、Privacy、Experiment 和 Quality 有验证计划；
- Analytics and Telemetry Debt 有识别和治理方式；
- 高风险变更支持 Shadow、Dual Write、灰度、迁移、回滚和停止条件；
- 所有下游系统可以直接引用本文件定义事件、指标和数据契约。

---

## 96. Related Documents

### Philosophy

- [Player First Design](../../philosophy/foundation/player-first-design.md)
- [Clarity and Feedback](../../philosophy/experience/clarity-and-feedback.md)
- [Consistency and Coherence](../../philosophy/long-term/consistency-and-coherence.md)
- [Accessibility and Inclusivity](../../philosophy/responsibility/accessibility-and-inclusivity.md)
- [Ethical Design](../../philosophy/responsibility/ethical-design.md)
- [Iteration and Validation](../../philosophy/validation/iteration-and-validation.md)

### Systems

- [Systems README](../README.md)
- [System Design Framework](../system-design-framework.md)
- [System Map](../system-map.md)
- [Integration Rules](../integration-rules.md)
- [Game State and Flow](../core/game-state-and-flow.md)
- [Rules and Resolution](../core/rules-and-resolution.md)
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
- `live-operations.md`
- `experiment-management.md`
- `versioning-and-migration.md`
