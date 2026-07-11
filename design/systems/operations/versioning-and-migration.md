# Versioning and Migration（版本与迁移系统）

> Status: V1  
> Category: Operations  
> Path: `design/systems/operations/versioning-and-migration.md`  
> Owner: TBD  
> Reviewers: Product / Design / Engineering / Architecture / Data / QA / SRE / Security / Privacy / Legal / Live Operations / Support / Finance  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: Critical  
> Dependencies: System Design Framework, Integration Rules, Save and Persistence, Analytics and Telemetry, Live Operations, Experiment Management  
> Affected Systems: Core Loop, Game State and Flow, Rules and Resolution, Economy, Progression, Reward, Content, Player, Social, Commercial, Operations

---

## 1. System Summary

Versioning and Migration 系统负责定义：

```text
什么需要版本化；
版本如何命名、比较、兼容、发布、弃用和归档；
旧客户端、旧存档、旧配置、旧内容、旧规则和旧事件如何继续工作；
数据、状态和玩家资产如何从旧结构迁移到新结构；
迁移失败时如何暂停、回滚、前向修复和恢复；
多版本并存时哪个系统是权威；
如何保证历史结果、交易、奖励、权益和审计仍可解释；
如何减少长期版本分裂和迁移债务。
```

该系统通常覆盖：

- Client Version；
- Server Version；
- API Version；
- Protocol Version；
- Schema Version；
- Save Version；
- Content Version；
- Rule Version；
- Economy Version；
- Progression Version；
- Reward Version；
- Matchmaking Version；
- Rating Version；
- Offer Version；
- Price Version；
- Entitlement Version；
- Event Schema Version；
- Experiment Version；
- Live Config Version；
- Platform Version；
- Migration Plan；
- Compatibility Matrix；
- Upgrade；
- Downgrade；
- Dual Read；
- Dual Write；
- Shadow Migration；
- Backfill；
- Rebuild；
- Reconciliation；
- Rollback；
- Forward Fix；
- Legacy Support；
- Deprecation；
- Sunset；
- Archive；
- Audit。

健康的版本与迁移系统应让玩家感受到：

```text
升级不会轻易破坏存档；
旧版本不会无提示地失效；
跨设备和跨平台状态保持一致；
已购买、已获得和已完成的价值可被正确继承；
必要的兼容限制有清楚说明；
迁移事故能够被快速发现和恢复；
版本变化不会秘密改变规则、价格或结果。
```

---

## 2. Purpose

### 2.1 Player Value

该系统帮助玩家：

- 保留存档；
- 保留进度；
- 保留资产；
- 保留已购权益；
- 跨设备继续；
- 跨平台继续；
- 安全升级；
- 在版本不兼容时获得清楚说明；
- 在迁移事故后恢复；
- 继续访问 Legacy Content；
- 避免重复购买和重复发奖；
- 避免排行榜、经济和多人状态被错误重置。

### 2.2 Product Value

统一版本和迁移系统可以：

- 管理多版本并存；
- 降低发布风险；
- 支持客户端和服务端独立演进；
- 支持 Live Operations；
- 支持 Schema 演进；
- 支持长期运营；
- 支持内容退役；
- 支持数据库迁移；
- 支持跨平台；
- 支持兼容层；
- 支持数据修复；
- 支持回滚和前向修复；
- 支持历史审计；
- 支持事故恢复；
- 降低 Legacy Debt。

### 2.3 Experience Contribution

版本和迁移会直接影响：

- 稳定性；
- 连续性；
- 信任；
- 兼容性；
- 多人可用性；
- 经济完整性；
- 商业完整性；
- 内容可访问性；
- 运营速度；
- 支持成本。

### 2.4 Why This System Exists

如果每个系统自行处理版本，常见问题包括：

```text
客户端版本号和存档版本号被混为一谈；
服务端升级后旧客户端发送的字段无法解析；
同一 Save Schema 在不同平台行为不同；
规则变化后历史比赛无法复现；
价格更新覆盖旧订单；
实验版本没有被记录；
奖励定义变化后补发结果不同；
经济表更新后旧交易无法对账；
Entitlement 迁移丢失来源；
Dual Write 长期存在且两边数据分叉；
回滚代码成功但新数据无法降级；
迁移脚本重复执行造成重复资产；
运营配置与客户端版本不兼容；
Support 无法判断玩家当时使用的版本。
```

---

## 3. Non-Goals

Versioning and Migration 不负责：

- 代替代码发布系统；
- 代替数据库平台；
- 代替 Save and Persistence；
- 代替 Analytics；
- 代替 Live Operations；
- 自动保证所有变化都向后兼容；
- 允许无限长期支持所有旧版本；
- 用版本号掩盖语义变化；
- 让客户端自行决定迁移结果；
- 在无审计时直接修复生产数据；
- 用回滚代替完整恢复设计；
- 将所有配置变化都视为新客户端版本；
- 在无兼容评估时进行跨平台迁移；
- 通过删除历史数据解决 Schema 冲突；
- 让迁移脚本成为长期业务逻辑；
- 让 Dual Read / Dual Write 永久存在。

---

## 4. Governing Principles

### 4.1 Consistency and Coherence

参考：

- `../../philosophy/long-term/consistency-and-coherence.md`

应用原则：

- 每类版本有明确语义；
- 相同版本号在各系统中含义稳定；
- 兼容矩阵和迁移路径是单一来源；
- 历史对象保留当时版本；
- 版本变化不会静默改变旧事实。

### 4.2 Player First Design

参考：

- `../../philosophy/foundation/player-first-design.md`

应用原则：

- 优先保护玩家存档、资产和购买；
- 迁移失败优先进入安全模式；
- 不要求玩家重复完成或重复购买；
- 不兼容时给出可执行的下一步；
- 旧版本 Sunset 有合理通知和过渡。

### 4.3 Clarity and Feedback

参考：

- `../../philosophy/experience/clarity-and-feedback.md`

应用原则：

- 版本状态可见；
- 迁移进度可见；
- 失败原因清楚；
- 兼容性要求清楚；
- Support 能看到版本和迁移历史。

### 4.4 Iteration and Validation

参考：

- `../../philosophy/validation/iteration-and-validation.md`

应用原则：

- 迁移先 Dry Run；
- 先 Shadow，再写入；
- 有可验证不变量；
- 有数据对账；
- 有回滚和停止条件；
- 迁移结果进入复盘。

### 4.5 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 版本变化不能用于静默剥夺已购价值；
- 不以迁移为由删除玩家数据；
- 不对旧版本玩家施加不合理商业压力；
- 不将无法升级的玩家排除在基本支持之外而无说明；
- 高风险修复不通过实验绕过正式治理。

---

## 5. System Boundary

### 5.1 Inputs

系统接收：

- Client Build；
- Server Build；
- API Contract；
- Protocol；
- Schema；
- Save State；
- Content Definition；
- Rule Definition；
- Economy Definition；
- Reward Definition；
- Offer and Price；
- Entitlement；
- Analytics Event；
- Experiment；
- Live Config；
- Platform Requirement；
- Migration Script；
- Compatibility Policy；
- Deprecation Policy；
- Rollback Plan；
- Support Plan；
- Legal and Retention Requirement。

### 5.2 Outputs

系统产生：

- Version Definition；
- Compatibility Matrix；
- Migration Plan；
- Migration Job；
- Migration State；
- Upgrade Result；
- Downgrade Result；
- Compatibility Decision；
- Read Strategy；
- Write Strategy；
- Reconciliation Report；
- Backfill；
- Rollback Decision；
- Forward Fix；
- Legacy Mapping；
- Deprecation Notice；
- Sunset Plan；
- Version Audit。

### 5.3 Owned State

系统拥有：

- Version Registry；
- Version Type；
- Compatibility Policy；
- Compatibility Matrix；
- Migration Definition；
- Migration Instance；
- Migration Checkpoint；
- Migration Result；
- Backfill State；
- Reconciliation State；
- Deprecation State；
- Sunset State；
- Legacy Mapping；
- Version History；
- Migration Audit。

### 5.4 Read-Only Dependencies

系统读取：

- Business State；
- Save；
- Database；
- Content；
- Orders；
- Rewards；
- Entitlements；
- Analytics；
- Feature Flags；
- Live Config；
- Platform；
- Account；
- Region；
- Time；
- Security；
- Privacy；
- Legal。

### 5.5 Write Dependencies

系统通过正式契约请求：

- Save 更新 Schema；
- Database 执行 Migration；
- Content 更新映射；
- Reward 执行补发或转换；
- Economy 执行 Ledger 修复；
- Entitlement 执行 Legacy Mapping；
- Live Operations 控制灰度和暂停；
- Analytics 更新 Schema 和指标版本；
- Support 暴露迁移诊断；
- Notification 发送必要升级或 Sunset 信息；
- Audit 保存高风险变化。

### 5.6 Out of Scope

系统不直接：

- 计算战斗结果；
- 修改商业价格；
- 直接授予资产；
- 决定处罚；
- 决定退款；
- 修改玩家偏好；
- 作为所有领域状态的最终权威；
- 在无领域验证下自动转换高价值数据。

---

## 6. Version Taxonomy

### 6.1 Client Version

客户端应用版本。

### 6.2 Server Version

服务端部署版本。

### 6.3 API Version

接口契约版本。

### 6.4 Protocol Version

网络协议版本。

### 6.5 Schema Version

数据结构版本。

### 6.6 Save Version

存档格式版本。

### 6.7 Content Version

内容定义版本。

### 6.8 Rule Version

规则和结算逻辑版本。

### 6.9 Economy Version

经济规则、价格、Source 和 Sink 版本。

### 6.10 Progression Version

成长路径和门槛版本。

### 6.11 Reward Version

奖励定义和发放语义版本。

### 6.12 Matchmaking Version

匹配规则版本。

### 6.13 Rating Version

Rating 算法版本。

### 6.14 Commercial Version

商品、Offer、Price 和 Entitlement 版本。

### 6.15 Analytics Version

Event Schema 和 Metric 版本。

### 6.16 Experiment Version

Variant、Assignment 和 Exposure 版本。

### 6.17 Live Config Version

远程配置版本。

### 6.18 Platform Version

平台 SDK、Store、认证和系统能力版本。

---

## 7. Version Registry

Version Registry 是所有正式版本的单一目录。

### 7.1 Registry Fields

- Version ID；
- Version Type；
- Domain；
- Semantic Version；
- Build；
- Status；
- Effective From；
- Effective Until；
- Compatible With；
- Migrates From；
- Migrates To；
- Owner；
- Risk Level；
- Release；
- Rollback；
- Deprecation；
- Archive；
- Audit。

### 7.2 Registry Status

- Draft；
- Review；
- Approved；
- Scheduled；
- Active；
- Supported；
- Deprecated；
- Unsupported；
- Retired；
- Archived；
- Invalidated。

### 7.3 No Unregistered Production Version

生产中可影响持久状态和业务结果的版本必须注册。

---

## 8. Version Definition Template

```markdown
## Version Definition

- Version ID:
- Version Type:
- Domain:
- Name:
- Semantic Version:
- Build:
- Effective From:
- Effective Until:
- Compatibility:
- Migration Required:
- Migration From:
- Migration To:
- Rollback:
- Deprecation:
- Sunset:
- Owner:
- Risk Level:
```

---

## 9. Versioning Strategy

### 9.1 Semantic Versioning

常见形式：

```text
MAJOR.MINOR.PATCH
```

- MAJOR：破坏兼容；
- MINOR：向后兼容功能；
- PATCH：向后兼容修复。

### 9.2 Integer Schema Version

适合 Save、Database 和 Event Schema。

### 9.3 Date-Based Version

适合 Content Pack、Live Season 和运营批次。

### 9.4 Content Hash

适合不可变内容包。

### 9.5 Build Number

适合发布和诊断。

### 9.6 Rule

不同版本类型可以使用不同策略，但必须在 Registry 中统一解释。

---

## 10. Version Identity

### 10.1 Stable Version ID

Version ID 永不复用。

### 10.2 Display Version vs Internal Version

玩家看到：

```text
1.5.0
```

内部可能使用：

```text
client-1.5.0+build.2847
```

### 10.3 Immutable Meaning

同一个 Version ID 不能重新指向不同内容。

### 10.4 Hotfix

Hotfix 应有独立 Build 或 Patch Version。

---

## 11. Compatibility Taxonomy

### 11.1 Backward Compatible

新版本能读取或服务旧版本。

### 11.2 Forward Compatible

旧版本能容忍新数据中的未知部分。

### 11.3 Bidirectional Compatible

新旧版本可以安全互通。

### 11.4 Read Compatible

可读取，但不一定可写。

### 11.5 Write Compatible

写出的数据可被目标版本读取。

### 11.6 Session Compatible

可以在同一 Session、Match 或 Party 中共存。

### 11.7 Save Compatible

可以打开同一存档。

### 11.8 Network Compatible

协议可互通。

### 11.9 Content Compatible

内容定义可被当前版本解析。

### 11.10 Commercial Compatible

商品、价格和权益语义兼容。

---

## 12. Compatibility Matrix

| Producer | Consumer | Read | Write | Session | Migration | Notes |
|---|---|---:|---:|---:|---:|---|
| Client N | Server N | Yes | Yes | Yes | No | 当前版本 |
| Client N-1 | Server N | Yes | Limited | Yes | Maybe | 旧客户端 |
| Save N-1 | Client N | Yes | Upgrade Only | Yes | Yes | 自动迁移 |
| Save N | Client N-1 | No | No | No | Unsupported | 防止降级损坏 |

### 12.1 Matrix Dimensions

- Client；
- Server；
- Save；
- API；
- Protocol；
- Content；
- Platform；
- Region；
- Feature；
- Schema；
- Rule。

### 12.2 Single Source of Truth

发布、Support、Store 和运营使用同一 Matrix。

---

## 13. Compatibility Decision

Compatibility Decision 应返回：

- Compatible；
- Compatible with Degradation；
- Read-Only；
- Upgrade Required；
- Migration Required；
- Retry Later；
- Unsupported；
- Blocked for Safety；
- Blocked for Legal。

### 13.1 Decision Fields

- Source Version；
- Target Version；
- Result；
- Reason Code；
- Required Action；
- Fallback；
- Expiry；
- Policy Version。

### 13.2 No Boolean-Only API

只返回 true / false 不足以支持恢复。

---

## 14. Version Invariants

1. Version ID 永不复用。
2. 同一 Version ID 的语义不可改变。
3. 持久对象必须记录创建或结算时的关键版本。
4. Breaking Change 必须有迁移或明确不兼容政策。
5. Migration 必须幂等。
6. Analytics Failure 不影响 Migration 事务。
7. Rollback 代码不等于数据可降级。
8. 已发生的合法交易和奖励不能因版本回退被静默删除。
9. Compatibility Matrix 必须在发布前更新。
10. 旧版本 Sunset 必须有通知和支持计划。
11. Dual Write 必须有结束条件。
12. Shadow Migration 不得影响玩家状态。
13. Backfill 不得触发业务副作用。
14. 迁移失败必须可暂停和恢复。
15. 高价值数据迁移必须保留来源和前后状态。
16. Client 不能决定 Schema Migration 成功。
17. 版本变更必须可审计。
18. Support 能识别玩家当前和历史关键版本。

---

## 15. Versioned Object

任何关键对象可包含：

- Object ID；
- Object Type；
- Created Version；
- Current Version；
- Schema Version；
- Rule Version；
- Content Version；
- Migrated From；
- Migration History；
- Last Updated；
- Correlation ID。

### 15.1 Examples

- Save；
- Order；
- Reward Instance；
- Entitlement；
- Match Result；
- Quest；
- Character；
- Loadout；
- Content Instance；
- Experiment Assignment。

---

## 16. Client and Server Compatibility

### 16.1 Supported Client Window

例如：

- Current；
- Previous Minor；
- Critical Hotfix Only。

### 16.2 Minimum Supported Version

由服务端和平台共同决定。

### 16.3 Soft Update

建议升级，但仍可进入。

### 16.4 Hard Update

必须升级。

### 16.5 Emergency Block

因安全或数据损坏紧急阻止。

### 16.6 Player Communication

说明：

- 为什么需要升级；
- 是否影响存档；
- 下载大小；
- 维护时间；
- 平台限制；
- Support。

---

## 17. API Versioning

### 17.1 Strategies

- URL；
- Header；
- Media Type；
- Capability Negotiation；
- Schema Evolution。

### 17.2 Additive Change

优先新增 Optional Field。

### 17.3 Breaking Change

- Field Removal；
- Type Change；
- Semantic Change；
- Required Change；
- Error Contract Change；
- Auth Change。

### 17.4 Deprecation

提供：

- Notice；
- Migration Guide；
- Usage Tracking；
- Sunset Date；
- Owner。

### 17.5 Consumer-Driven Contract

关键消费者参与验证。

---

## 18. Protocol Versioning

### 18.1 Handshake

客户端和服务端交换：

- Protocol Version；
- Capabilities；
- Compression；
- Encryption；
- Content Version；
- Feature Set。

### 18.2 Capability Negotiation

优先根据能力而非只看版本号。

### 18.3 Unknown Message

定义：

- Ignore；
- Reject；
- Downgrade；
- Disconnect；
- Safe Fallback。

### 18.4 Match Compatibility

同一 Match 中协议和规则必须可兼容。

---

## 19. Save Versioning

### 19.1 Save Header

建议包含：

- Save ID；
- Save Version；
- Schema Version；
- Client Version；
- Content Version；
- Platform；
- Created At；
- Updated At；
- Integrity；
- Migration History。

### 19.2 Save Upgrade

旧存档首次在新版本打开时迁移。

### 19.3 Save Downgrade

默认高风险，通常不支持。

### 19.4 Read-Only Legacy

可以允许旧存档只读预览。

### 19.5 Backup

迁移前创建备份或快照。

---

## 20. Save Migration

### 20.1 Migration Types

- Field Rename；
- Field Add；
- Field Remove；
- Type Conversion；
- Split；
- Merge；
- Reference Remap；
- Default Fill；
- Content Replacement；
- Progress Conversion；
- Inventory Conversion。

### 20.2 Migration Chain

例如：

```text
v1 → v2 → v3 → v4
```

### 20.3 Direct Migration

大量旧版本时可使用：

```text
v1 → latest
```

但需更高测试成本。

### 20.4 Chain Integrity

每一步：

- 幂等；
- 可验证；
- 有输入和输出版本；
- 有失败处理。

---

## 21. Save Migration Lifecycle

```text
Detected
→ Backup
→ Validating
→ Migrating
→ Verifying
→ Committed
→ Completed
```

异常：

```text
Failed
Paused
Rolled Back
Recovered
Manual Review
```

### 21.1 Atomicity

迁移不能留下半完成存档。

### 21.2 Commit

验证成功后切换新版本。

### 21.3 Failure

保留旧存档和错误详情。

---

## 22. Database Schema Migration

### 22.1 Migration Types

- Additive；
- Transformative；
- Destructive；
- Index；
- Constraint；
- Partition；
- Table Split；
- Table Merge；
- Storage Move。

### 22.2 Expand and Contract

推荐：

```text
Expand
→ Dual Support
→ Backfill
→ Switch Read
→ Switch Write
→ Validate
→ Contract
```

### 22.3 Destructive Change

需要：

- Backup；
- Retention Review；
- Rollback；
- Data Export；
- Approval。

### 22.4 Online Migration

避免长时间停机。

---

## 23. Expand and Contract Pattern

### 23.1 Expand

新增字段、表或接口，不删除旧结构。

### 23.2 Dual Support

新旧版本都可工作。

### 23.3 Backfill

填充新结构。

### 23.4 Read Switch

先切换读。

### 23.5 Write Switch

再切换写。

### 23.6 Contract

确认无消费者后删除旧结构。

### 23.7 Invariant

Contract 不能早于验证和使用追踪完成。

---

## 24. Dual Read

### 24.1 Purpose

比较旧、新数据来源。

### 24.2 Modes

- Read Old, Shadow New；
- Read New, Fallback Old；
- Compare Both；
- Segment Read。

### 24.3 Risks

- 延迟；
- 成本；
- 不一致；
- 逻辑复杂；
- 长期债务。

### 24.4 Exit Criteria

- 一致率；
- 性能；
- 错误；
- 覆盖；
- 消费者迁移。

---

## 25. Dual Write

### 25.1 Purpose

迁移期间同时写入旧、新结构。

### 25.2 Risks

- Partial Failure；
- Ordering；
- Duplicate；
- Divergence；
- Retry；
- Side Effects。

### 25.3 Authority

必须明确哪一边是权威。

### 25.4 Transaction

高价值状态需要：

- Outbox；
- Idempotency；
- Reconciliation；
- Retry；
- Dead Letter；
- Audit。

### 25.5 Exit

Dual Write 有明确结束日期。

---

## 26. Shadow Migration

### 26.1 Purpose

生成新结果但不影响正式状态。

### 26.2 Compare

比较：

- Count；
- Values；
- Distribution；
- References；
- Invariants；
- Performance；
- Missing；
- Duplicate。

### 26.3 Suitable Uses

- Save；
- Entitlement；
- Economy；
- Rating；
- Analytics；
- Content Mapping；
- Experiment Assignment。

### 26.4 No Side Effects

Shadow 不发奖励、不改余额、不发通知。

---

## 27. Backfill

### 27.1 Purpose

为历史数据填充新结构或修复错误。

### 27.2 Backfill Contract

- Backfill ID；
- Source；
- Target；
- Scope；
- Time Range；
- Query；
- Code Version；
- Idempotency；
- Checkpoint；
- Rate Limit；
- Verification；
- Rollback；
- Owner。

### 27.3 Side Effect Isolation

Backfill 不触发：

- Reward；
- Notification；
- Purchase；
- Enforcement；
- Achievement；

除非明确设计。

### 27.4 Resume

支持 Checkpoint。

---

## 28. Data Rebuild

### 28.1 Rebuild from Source

例如：

- Balance from Ledger；
- Entitlement from Orders；
- Metrics from Raw Events；
- Leaderboard from Match Results；
- Cache from Authority。

### 28.2 Requirements

- Source Authority；
- Determinism；
- Version；
- Time Range；
- Exclusions；
- Verification；
- Cutover。

### 28.3 Rebuild vs Migration

Rebuild 从权威源重新生成。

Migration 转换现有状态。

---

## 29. Reconciliation

### 29.1 Purpose

比较新旧状态和权威来源。

### 29.2 Dimensions

- Count；
- Missing；
- Duplicate；
- Amount；
- Status；
- Source；
- Version；
- Timestamp；
- Ownership；
- Progress；
- Balance。

### 29.3 Severity

- Critical；
- High；
- Medium；
- Low。

### 29.4 Action

- Auto Repair；
- Manual Review；
- Pause；
- Rollback；
- Forward Fix；
- Compensate。

---

## 30. Migration Definition

```markdown
## Migration Definition

- Migration ID:
- Domain:
- Source Version:
- Target Version:
- Object Type:
- Scope:
- Preconditions:
- Transform:
- Invariants:
- Idempotency:
- Checkpoint:
- Verification:
- Rollback:
- Forward Fix:
- Player Impact:
- Support:
- Owner:
- Risk Level:
```

---

## 31. Migration Instance

应包含：

- Instance ID；
- Migration Definition；
- Environment；
- Scope；
- Cohort；
- Start；
- End；
- State；
- Checkpoint；
- Counts；
- Errors；
- Retries；
- Verification；
- Rollback；
- Owner；
- Correlation ID；
- Audit。

---

## 32. Migration Lifecycle

```text
Proposed
→ Draft
→ Reviewed
→ Approved
→ Scheduled
→ Dry Run
→ Shadow
→ Ready
→ Running
→ Verifying
→ Cutover
→ Monitoring
→ Completed
→ Archived
```

异常：

```text
Paused
Failed
Rolled Back
Forward Fixing
Cancelled
Invalidated
```

---

## 33. Migration Invariants

1. Migration ID 唯一。
2. 同一对象迁移幂等。
3. Source 和 Target Version 明确。
4. 高价值迁移有前后状态。
5. 迁移失败不能删除 Source。
6. Partial Result 不可被当作完成。
7. Checkpoint 可恢复。
8. Analytics Event 不作为唯一 Source。
9. Migration 不能绕过领域不变量。
10. Rollback 和 Forward Fix 在运行前定义。
11. Shadow 结果不产生业务副作用。
12. Bulk Migration 有 Rate Limit。
13. Support 可查询迁移状态。
14. 迁移结果必须对账。
15. 旧版本删除前确认无消费者。
16. 迁移脚本执行完后应归档，不作为长期请求路径。
17. 高风险迁移有 Kill Switch。
18. 所有手工修复有 Audit。

---

## 34. Migration Scope

### 34.1 All-at-Once

风险高。

### 34.2 Cohort-Based

按账号、地区、平台、版本或对象分批。

### 34.3 Lazy Migration

对象被访问时迁移。

### 34.4 Background Migration

后台逐步迁移。

### 34.5 Hybrid

先后台迁移大部分，再 Lazy 处理剩余。

### 34.6 Scope Selection

依据：

- 数据量；
- 延迟；
- 兼容；
- 风险；
- 访问频率；
- 回滚；
- Support；
- 成本。

---

## 35. Lazy Migration

### 35.1 Advantages

- 无需一次性迁移全部；
- 只迁移活跃数据；
- 降低峰值。

### 35.2 Risks

- 请求延迟；
- 多版本长期存在；
- 代码复杂；
- 冷数据永不迁移；
- Support 难诊断。

### 35.3 Requirements

- Idempotent；
- Timeout；
- Fallback；
- Lock；
- Checkpoint；
- Version Header；
- Monitoring。

---

## 36. Bulk Migration

### 36.1 Requirements

- Dry Run；
- Cohort；
- Rate Limit；
- Capacity；
- Backup；
- Checkpoint；
- Error Queue；
- Pause；
- Resume；
- Rollback；
- Verification。

### 36.2 Blast Radius

从小范围开始。

### 36.3 Business Window

避开：

- 大型活动；
- 财务结算；
- Season End；
- Platform Launch；
- Major Campaign。

---

## 37. Online and Offline Migration

### 37.1 Online

服务运行时迁移。

需要：

- 兼容层；
- Dual Read / Write；
- Backpressure；
- Low Lock；
- Rate Limit。

### 37.2 Offline

停机窗口执行。

适合：

- 高风险强一致变化；
- 大型结构变化；
- 无法双写的系统。

### 37.3 Decision

基于：

- Downtime；
- Risk；
- Complexity；
- Volume；
- Rollback；
- Player Impact。

---

## 38. Content Versioning

### 38.1 Content Definition Version

每次内容语义变化创建新版本。

### 38.2 Content Asset Version

资源文件版本。

### 38.3 Content Pack Version

一组内容发布版本。

### 38.4 Compatibility

检查：

- Client；
- Server；
- Save；
- Localization；
- Platform；
- Entitlement；
- Quest；
- Reward。

### 38.5 Retired Content

旧版本可能需要：

- Archive；
- Legacy Access；
- Save Reference；
- Replacement Mapping。

---

## 39. Content Migration

### 39.1 Examples

- Item Rename；
- Item Merge；
- Item Split；
- Removed Content；
- Character Rework；
- Quest Rewrite；
- Map Replacement；
- Localization Key Change。

### 39.2 Player State

处理：

- Inventory；
- Loadout；
- Progress；
- Objective；
- Save；
- Reward；
- Entitlement；
- Favorite；
- History。

### 39.3 Replacement

必须定义：

- Equivalent；
- Credit；
- Refund；
- Legacy；
- Archive；
- Invalid Reference。

---

## 40. Rule Versioning

### 40.1 Rule Version

影响：

- Combat；
- Resolution；
- Match；
- Reward；
- Economy；
- Progression；
- Objective；
- Eligibility。

### 40.2 Historical Results

保存当时 Rule Version。

### 40.3 Active Session

Session 开始时冻结 Rule Version。

### 40.4 Mid-Session Change

通常禁止。

### 40.5 Replay

历史回放使用原 Rule Version 或明确标记不可重现。

---

## 41. Match and Rating Migration

### 41.1 Matchmaking Version

记录队列和分配规则。

### 41.2 Rating Version

记录算法、参数和 Season。

### 41.3 Migration Options

- Reset；
- Soft Reset；
- Recalibrate；
- Map；
- Carry；
- Placement；
- Parallel Rating。

### 41.4 Fairness

迁移需评估：

- Rank；
- Party；
- Region；
- Smurf；
- Leaderboard；
- Rewards；
- Historical Comparison。

### 41.5 No Silent Reinterpretation

旧 Rating 不能无说明直接当作新 Rating 同义值。

---

## 42. Economy Versioning

### 42.1 Versioned Elements

- Currency；
- Source；
- Sink；
- Item；
- Price；
- Drop；
- Exchange；
- Cap；
- Crafting；
- Market Rule。

### 42.2 Ledger

历史交易保留原 Economy Version。

### 42.3 Balance Migration

余额转换必须：

- 可解释；
- 可对账；
- 有汇率；
- 有舍入；
- 有上限；
- 有补偿；
- 有回滚或 Forward Fix。

### 42.4 Inflation and Fairness

迁移评估分布，不只看总量。

---

## 43. Progression Migration

### 43.1 Changes

- Level Curve；
- XP；
- Skill Tree；
- Gate；
- Mastery；
- Rank；
- Milestone；
- Prestige。

### 43.2 Mapping

可按：

- Equivalent Progress；
- Earned Value；
- Percentile；
- Milestone；
- Minimum Guarantee；
- Grandfathering。

### 43.3 Player Protection

避免：

- 已完成内容变未完成；
- 已解锁功能被收回；
- 资源投入消失；
- 付费加速价值丢失；
- 回归玩家无法理解。

### 43.4 Respec

重大树结构变化可提供免费 Respec。

---

## 44. Reward Versioning

### 44.1 Reward Definition Version

每次内容或价值变化版本化。

### 44.2 Reward Instance

记录创建时 Version。

### 44.3 Retroactive Grant

使用明确 Version。

### 44.4 Reward Replacement

旧奖励退役时：

- 保留；
- 替代；
- Convert；
- Credit；
- Legacy。

### 44.5 No Reinterpretation

已发奖励不能因定义变化自动变成不同价值。

---

## 45. Commercial Versioning

### 45.1 Product

稳定 Product ID。

### 45.2 SKU

平台和渠道版本。

### 45.3 Price

Price Snapshot 保留当时版本。

### 45.4 Offer

Offer Instance 保留版本。

### 45.5 Order

Order 引用 Product、SKU、Price、Offer 和 Entitlement Version。

### 45.6 Refund

按原订单语义处理。

---

## 46. Entitlement Migration

### 46.1 Types

- Rename；
- Merge；
- Split；
- Scope Change；
- Platform Mapping；
- Region Mapping；
- Ownership Type；
- Subscription Tier；
- Legacy Conversion。

### 46.2 Requirements

- Source；
- Target；
- Ownership Preservation；
- Expiry；
- Platform；
- Region；
- History；
- Restore；
- Rollback。

### 46.3 No Orphan Ownership

迁移后每个 Entitlement 有有效来源。

---

## 47. Analytics Schema Migration

### 47.1 Event Schema

Breaking Change 新版本。

### 47.2 Metric Version

公式变化必须版本化。

### 47.3 Dual Write

迁移期比较新旧事件。

### 47.4 Backfill

重新计算历史时标记 Metric Version。

### 47.5 Dashboard

显示：

- Version；
- Data Gap；
- Backfill；
- Compatibility；
- Freshness。

---

## 48. Experiment Versioning

### 48.1 Experiment Definition

Variant、Assignment、Exposure、Metric 和 Ramp 版本化。

### 48.2 Mid-Experiment Change

重大变化：

- 重启；
- 新 Phase；
- 或 Invalidate。

### 48.3 Assignment Salt

Salt 变化导致重分配，必须记录。

### 48.4 Archived Result

保留当时所有依赖版本。

---

## 49. Live Config Versioning

### 49.1 Immutable Config Version

已发布版本不可原地修改。

### 49.2 Activation

Published 与 Active 分离。

### 49.3 Scope

- Environment；
- Region；
- Platform；
- Client；
- Server；
- Time。

### 49.4 Rollback

恢复 LKG，不删除错误版本历史。

### 49.5 Compatibility

Config Schema 与消费者版本匹配。

---

## 50. Platform Migration

### 50.1 Types

- SDK；
- Authentication；
- Store；
- Receipt；
- Cloud Save；
- Social；
- Voice；
- Achievement；
- Notification；
- Commerce；
- Platform Generation。

### 50.2 Requirements

- Account Linking；
- Entitlement；
- Save；
- Purchase；
- Subscription；
- Friends；
- Privacy；
- Region；
- Certification；
- Rollback。

### 50.3 Legacy Platform

定义支持期和 Sunset。

---

## 51. Cross-Platform Migration

### 51.1 Account Merge

处理：

- Duplicate；
- Conflict；
- Region；
- Platform；
- Save；
- Entitlement；
- Currency；
- Subscription；
- Social；
- Ban。

### 51.2 Save Merge

通常不是简单覆盖。

### 51.3 Ownership

保留合法来源。

### 51.4 Conflict Policy

可以：

- Choose；
- Merge；
- Highest Progress；
- Latest；
- Manual Review；
- Separate Profiles。

---

## 52. Default Values

### 52.1 New Field

需要定义：

- Default；
- Derived；
- Unknown；
- Not Applicable；
- Migration Rule。

### 52.2 Avoid Ambiguous Zero

零可能不是默认。

### 52.3 Historical Accuracy

无法推断时使用 Unknown，不伪造。

### 52.4 Business Impact

Default 不能意外发放资产或权限。

---

## 53. Enum Evolution

### 53.1 Add Value

消费者必须容忍未知值。

### 53.2 Remove Value

先映射和迁移。

### 53.3 Rename Value

保持稳定内部 ID，仅改显示名优先。

### 53.4 Unknown Enum

定义 Fallback。

### 53.5 No Default-to-Privilege

未知权限值默认安全拒绝。

---

## 54. Reference Migration

### 54.1 Stable IDs

避免依赖显示名。

### 54.2 Rename

只改 Display Name。

### 54.3 Merge

多个旧 ID 映射新 ID。

### 54.4 Split

一个旧 ID 分裂多个新 ID，需要决策规则。

### 54.5 Deleted Reference

使用：

- Tombstone；
- Legacy；
- Replacement；
- Invalid；
- Archive。

---

## 55. Tombstones

### 55.1 Purpose

表示对象已删除或退役，但 ID 历史存在。

### 55.2 Fields

- Original ID；
- Removed At；
- Reason；
- Replacement；
- Access Policy；
- Migration；
- Retention。

### 55.3 Benefits

防止旧存档、订单和日志解析失败。

---

## 56. Legacy Support

### 56.1 Legacy Types

- Old Client；
- Old Save；
- Old Product；
- Old SKU；
- Old Content；
- Old Event；
- Old API；
- Old Platform。

### 56.2 Support Policy

- Full；
- Read-Only；
- Security Fix Only；
- Migration Only；
- Unsupported；
- Archived。

### 56.3 Cost Review

长期支持有成本。

### 56.4 Player Value

重要已购和存档价值优先保护。

---

## 57. Deprecation

### 57.1 Lifecycle

```text
Active
→ Deprecated
→ Migration Window
→ Unsupported
→ Retired
→ Archived
```

### 57.2 Deprecation Notice

包含：

- What；
- Why；
- Impact；
- Replacement；
- Migration；
- Deadline；
- Support；
- Owner。

### 57.3 Usage Tracking

在 Sunset 前确认剩余消费者。

### 57.4 No Silent Deprecation

高影响功能必须通知。

---

## 58. Sunset

### 58.1 Sunset Types

- API；
- Client；
- Platform；
- Content；
- Product；
- Service；
- Feature；
- Config；
- Event Schema。

### 58.2 Sunset Plan

- Timeline；
- Player Communication；
- Migration；
- Data Export；
- Entitlement；
- Refund / Credit；
- Save；
- Offline；
- Support；
- Legal；
- Archive；
- Final Shutdown。

### 58.3 Security Exception

严重安全风险可缩短窗口，但需清楚说明。

---

## 59. Rollback

### 59.1 Code Rollback

恢复旧代码。

### 59.2 Config Rollback

恢复旧配置。

### 59.3 Schema Rollback

通常高风险。

### 59.4 Data Rollback

最高风险。

### 59.5 Feature Rollback

关闭新功能。

### 59.6 Business Rollback

恢复旧规则，但需处理新状态。

---

## 60. Rollback Feasibility

发布前回答：

- 旧代码能否读取新数据；
- 新数据是否可降级；
- 是否已发生交易；
- 是否已发奖励；
- 是否产生新 Entitlement；
- 是否产生新内容；
- 是否产生不可逆用户行为；
- 是否需要 Forward Fix。

### 60.1 No Blind Rollback

回滚代码后数据可能仍不兼容。

### 60.2 Safe Rollback Window

定义多久内可直接回滚。

---

## 61. Forward Fix

### 61.1 Suitable Scenarios

- 新数据无法降级；
- 已发生合法交易；
- 已发放永久资产；
- 多平台同步完成；
- 已创建新引用；
- 数据量过大。

### 61.2 Actions

- 新 Migration；
- Corrective Transaction；
- Mapping；
- Reconciliation；
- Compensation；
- Repair Job；
- Compatibility Layer。

### 61.3 Audit

保留错误和修复历史。

---

## 62. Point-in-Time Recovery

### 62.1 Purpose

恢复数据库到某时点。

### 62.2 Risks

会丢失之后的合法交易。

### 62.3 Requirements

- Transaction Replay；
- External Provider Reconciliation；
- Orders；
- Rewards；
- Entitlements；
- Notifications；
- Audit；
- Player Communication。

### 62.4 Preferred Use

基础设施灾难，而不是普通业务错误。

---

## 63. Backup and Restore

### 63.1 Backup Types

- Full；
- Incremental；
- Snapshot；
- Object Version；
- Export；
- Ledger Archive。

### 63.2 Restore Test

备份必须定期恢复演练。

### 63.3 Encryption and Access

高风险数据严格控制。

### 63.4 Retention

符合业务、法律和隐私要求。

---

## 64. Migration Verification

### 64.1 Structural

- Schema；
- Type；
- Required；
- Reference；
- Version。

### 64.2 Quantitative

- Count；
- Sum；
- Distribution；
- Missing；
- Duplicate；
- Balance。

### 64.3 Business

- Ownership；
- Progress；
- Reward；
- Rating；
- Price；
- Access；
- Eligibility。

### 64.4 Sample Review

人工抽样。

### 64.5 End-to-End

从玩家操作验证结果。

---

## 65. Migration Acceptance Criteria

示例：

```text
- 100% 关键对象有目标版本；
- 0 个跨账户数据；
- 0 个重复永久 Entitlement；
- 余额总额与 Ledger 一致；
- Missing Rate 低于阈值；
- 99.99% 对象自动迁移成功；
- 剩余对象进入 Manual Review；
- 关键玩家路径通过；
- Support 可以诊断；
- Rollback / Forward Fix 已验证。
```

---

## 66. Dry Run

### 66.1 Purpose

在不写入正式状态时运行迁移。

### 66.2 Output

- Eligible Count；
- Transform Count；
- Skip Count；
- Error Count；
- Estimated Time；
- Storage；
- Load；
- Differences；
- Risk；
- Sample Output。

### 66.3 Blocking Errors

- Cross-Account；
- Negative Balance；
- Missing Source；
- Duplicate Ownership；
- Invalid Reference；
- Unsupported Version。

---

## 67. Canary Migration

### 67.1 Cohorts

- Internal；
- Test Accounts；
- Low-Risk Objects；
- Small Region；
- Small Platform；
- 0.1%；
- 1%；
- 5%。

### 67.2 Guardrails

- Error；
- Latency；
- Data Difference；
- Support；
- Save Failure；
- Access Failure；
- Economy；
- Transaction；
- Crash。

### 67.3 Expand

每阶段验证后扩大。

---

## 68. Migration Rate Limit

### 68.1 Purpose

保护生产系统。

### 68.2 Controls

- Records per Second；
- Concurrent Jobs；
- Database Load；
- Queue Depth；
- Error Rate；
- Retry；
- Region；
- Time Window。

### 68.3 Adaptive Rate

根据健康动态调整。

### 68.4 Backpressure

下游拥塞时暂停。

---

## 69. Checkpoint and Resume

### 69.1 Checkpoint Fields

- Last Object；
- Partition；
- Version；
- Count；
- Time；
- Error；
- Hash；
- Job Version。

### 69.2 Resume

使用同一 Migration ID 和幂等规则。

### 69.3 Checkpoint Integrity

Checkpoint 自身需持久化和审计。

---

## 70. Error Handling

### 70.1 Error Categories

- Validation；
- Transform；
- Reference；
- Permission；
- Timeout；
- Capacity；
- Data Corruption；
- Unknown；
- Business Invariant。

### 70.2 Retryable

- Timeout；
- Temporary Dependency；
- Rate Limit。

### 70.3 Non-Retryable

- Invalid Source；
- Missing Required Mapping；
- Cross-Account；
- Corrupted Object。

### 70.4 Dead Letter

无法自动处理进入受控队列。

---

## 71. Manual Review

### 71.1 Use Cases

- Conflicting Saves；
- Duplicate Ownership；
- Account Merge；
- Invalid Legacy Data；
- High-Value Asset；
- Legal Hold；
- Cross-Region Conflict。

### 71.2 Tooling

提供：

- Before；
- After；
- Source；
- Recommended Action；
- Approval；
- Audit；
- Rollback。

### 71.3 No Raw Database Edit

人工修复通过正式工具。

---

## 72. Security

### 72.1 Threats

- Migration Script Tampering；
- Unauthorized Bulk Change；
- Cross-Account Mapping；
- Privilege Escalation；
- Backup Exposure；
- Rollback Abuse；
- Audit Tampering；
- Legacy Exploit；
- Schema Injection；
- Replay；
- Double Migration；
- Admin Abuse。

### 72.2 Controls

- Signed Artifacts；
- Code Review；
- RBAC；
- MFA；
- Approval；
- Environment Separation；
- Dry Run；
- Idempotency；
- Audit；
- Encryption；
- Secret Management；
- Rate Limit；
- Break Glass；
- Anomaly Detection。

### 72.3 Least Privilege

Migration Job 只访问必要范围。

---

## 73. Privacy

### 73.1 Data Movement

迁移可能改变：

- Storage Region；
- Vendor；
- Schema；
- Purpose；
- Retention；
- Access。

需要 Privacy Review。

### 73.2 Data Minimization

迁移不应顺便复制无关数据。

### 73.3 Deletion Requests

迁移中仍需遵守删除状态。

### 73.4 Legal Hold

明确例外。

### 73.5 Test Data

生产数据进入测试前脱敏。

---

## 74. Child and Family Data

### 74.1 Additional Controls

- Age Policy；
- Parent Relationship；
- Consent；
- Sharing；
- Purchase；
- Notification；
- Social；
- Retention。

### 74.2 No Default Adult Mapping

未知年龄不能自动映射为成人。

### 74.3 Family Migration

不能泄露家庭成员购买和身份信息。

---

## 75. Accessibility

### 75.1 Player-Facing Migration

- 有进度；
- 有取消或安全退出；
- 有清楚错误；
- 支持读屏；
- 大字体；
- 不只靠颜色；
- 不要求快速操作；
- 可稍后重试。

### 75.2 Save Migration

不要求玩家理解技术版本。

### 75.3 Update Requirement

说明下载和空间需求。

### 75.4 Fallback

提供：

- Read-Only；
- Offline；
- Previous Device；
- Support；
- Backup Restore。

---

## 76. Support

### 76.1 Support View

可查看：

- Client Version；
- Server Version；
- Save Version；
- Content Version；
- Schema Version；
- Migration State；
- Checkpoint；
- Error Code；
- Before / After；
- Backup；
- Compatibility Result；
- Recommended Action。

### 76.2 Support Actions

可以：

- Retry；
- Resume；
- Restore Backup；
- Reconcile；
- Submit Manual Review；
- Apply Approved Repair；
- Generate Diagnostic。

### 76.3 Restrictions

不能：

- 任意改 Version；
- 手工加资产；
- 删除 Audit；
- 绕过兼容和所有权规则。

---

## 77. Version and Migration Incident Management

### 77.1 Incident Types

- Save Corruption；
- Cross-Account Data；
- Duplicate Asset；
- Missing Entitlement；
- Wrong Balance；
- Failed Cutover；
- Dual Write Divergence；
- Schema Break；
- Old Client Crash；
- Platform Incompatibility；
- Analytics Break；
- Rollback Failure；
- Backup Failure。

### 77.2 Severity

#### SEV-1

跨账户、资产丢失、大规模存档损坏、财务或权益错误。

#### SEV-2

高影响迁移失败或多平台不兼容。

#### SEV-3

局部 Cohort 或单版本问题。

#### SEV-4

低影响展示或文档问题。

### 77.3 Actions

- Pause Migration；
- Freeze Writes；
- Kill Switch；
- Revert Read Path；
- Restore Backup；
- Reconcile；
- Forward Fix；
- Notify；
- Support Playbook；
- Compensate；
- Postmortem。

---

## 78. Failure and Recovery

| Failure | Cause | Player Impact | Recovery | Data Guarantee |
|---|---|---|---|---|
| Save Migration Failed | Invalid Legacy Data | 无法进入存档 | Restore Backup、Manual Review | Source Save 保留 |
| Dual Write Diverged | Partial Failure | 新旧状态不同 | Reconcile、Pause、Replay | Source Authority 明确 |
| Schema Break | Breaking Change | 旧客户端失败 | Compatibility Layer、Rollback | Version Audit |
| Duplicate Migration | Retry 无幂等 | 重复资产 | Idempotency、Repair | Migration ID |
| Cross-Account Mapping | Identity Bug | 严重隐私和资产风险 | Freeze、Restore、Incident | Full Audit |
| Backfill Overload | Rate 过高 | 服务降级 | Pause、Throttle、Resume | Checkpoint |
| Rollback Incompatible | 新数据旧代码不可读 | 服务失败 | Forward Fix、Compatibility Layer | New Data 保留 |
| Missing Legacy Mapping | Old ID 无映射 | 内容或权益丢失 | Tombstone、Manual Mapping | Old ID 保留 |
| Rating Migration Bias | Mapping 不公平 | 排名异常 | Recalibrate、Season Repair | Old Rating 保留 |
| Economy Conversion Error | 汇率或舍入错误 | 余额异常 | Reconcile、Corrective Transaction | Ledger 保留 |
| Analytics Schema Loss | Event 迁移失败 | 数据缺口 | Dual Write、Backfill | Raw Event 保留 |
| Backup Restore Failed | 未验证备份 | 无法恢复 | Secondary Backup、PITR | Incident Escalation |

---

## 79. Edge Cases

### Client and Server

- Client N-2；
- Emergency Hotfix；
- Offline Client；
- Platform Delayed Patch；
- Server Partial Deployment；
- Region Version Difference。

### Save

- Very Old Save；
- Corrupted Save；
- Multiple Devices；
- Cloud Conflict；
- Partial DLC；
- Removed Content；
- Modded Save；
- Account Merge。

### Data

- Null Legacy Field；
- Unknown Enum；
- Deleted Reference；
- Duplicate ID；
- Missing Source；
- Invalid Time；
- Wrong Region；
- Legal Hold。

### Migration

- Job Restart；
- Deployment During Migration；
- Rollback Mid-Cutover；
- New Writes During Backfill；
- Concurrent Manual Repair；
- Data Deletion Request During Migration。

### Commercial

- Order Created Under Old Price；
- Refund After Product Migration；
- Subscription Tier Merge；
- SKU Sunset；
- Entitlement Split；
- Gift Pending During Migration。

---

## 80. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| System Design Framework | Foundational | Framework → Versioning | Contracts / Ownership | 无治理 |
| Integration Rules | Critical | 双向 | API / Events / Failure | 兼容失败 |
| Save and Persistence | Critical | 双向 | Save Schema / Backup | 存档损坏 |
| Analytics and Telemetry | Critical | 双向 | Event / Metric Version | 历史不可比 |
| Live Operations | Critical | 双向 | Rollout / Pause / Config | 风险扩散 |
| Experiment Management | Critical | 双向 | Variant / Exposure | 实验污染 |
| Rules and Resolution | Critical | Rules → Versioning | Rule Version | 结果不可重现 |
| Resources and Economy | Critical | 双向 | Ledger / Conversion | 资产风险 |
| Progression System | Critical | 双向 | Curve / Mapping | 进度丢失 |
| Reward System | Critical | 双向 | Definition / Instance | 重复或错发 |
| Content Lifecycle | Critical | 双向 | Content / Retirement | 引用失效 |
| Characters and Loadouts | Hard | 双向 | Build / Reference | 构筑失效 |
| Social and Multiplayer | Hard | 双向 | Protocol / Party | 无法联机 |
| Matchmaking and Competition | Critical | 双向 | Queue / Rating | 公平风险 |
| Monetization System | Critical | 双向 | Order / SKU / Fulfillment | 财务风险 |
| Offers and Pricing | Critical | 双向 | Price / Offer | 错误收费 |
| Entitlement and Ownership | Critical | 双向 | Legacy / Scope | 已购风险 |
| Support | Hard | 双向 | Diagnostics / Repair | 无法恢复 |

---

## 81. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Version Definition | 是 | Version Registry | 发布变化 | 长期 | Registry History |
| Compatibility Matrix | 是 | Versioning | 版本变化 | 长期版本 | Previous Matrix |
| Migration Definition | 是 | Versioning | 审批变化 | 长期 | Archive |
| Migration Instance | 是 | Migration Service | 状态变化 | 长期审计 | Resume |
| Checkpoint | 是 | Migration Service | 批次完成 | 至完成及审计 | Resume |
| Reconciliation Report | 是 | Domain / Versioning | 对账完成 | 审计期 | Recompute |
| Legacy Mapping | 是 | Domain / Versioning | 映射变化 | 长期 | Old Mapping |
| Tombstone | 是 | Domain | 对象退役 | 长期 | Archive |
| Deprecation State | 是 | Version Registry | 状态变化 | 长期 | Registry |
| Sunset Plan | 是 | Product / Versioning | 计划变化 | 长期 | Archive |
| Rollback Record | 是 | Audit | 回滚动作 | 长期 | Audit |
| Migration Audit | 是 | Audit System | 高风险动作 | 长期 | Append-Only |

---

## 82. Analytics and Validation

### 82.1 Key Assumptions

1. 关键版本均被注册并可追踪。
2. Compatibility Matrix 能正确阻止不兼容组合。
3. Migration 幂等、可恢复且不会破坏 Source。
4. Dual Read / Write 有明确权威和退出条件。
5. Save、Economy、Reward、Entitlement 和 Commercial 迁移可以对账。
6. Rollback 可行性在发布前评估。
7. 无法安全回滚时有 Forward Fix。
8. Legacy Mapping 和 Tombstone 能保护历史引用。
9. Support 能诊断版本和迁移状态。
10. Deprecation 和 Sunset 不会静默伤害玩家。

### 82.2 Validation Plan

| Hypothesis | Evidence | Success | Failure | Method |
|---|---|---|---|---|
| Registry 完整 | 发布审计 | 关键版本均注册 | 生产有未知版本 | Governance Audit |
| Compatibility 正确 | Matrix Test | 不兼容被阻止 | 运行时崩溃 | Integration Test |
| Migration 幂等 | 重复执行 | 状态不重复 | 重复资产 | Transaction Test |
| Source 受保护 | 故障注入 | 失败可恢复旧状态 | Source 丢失 | Recovery Test |
| Dual Write 可控 | 对账 | 一致且可退出 | 长期分叉 | Shadow Test |
| Rollback 可行 | Game Day | 在窗口内恢复 | 新数据不可读 | Rollback Drill |
| Forward Fix 有效 | Incident Simulation | 不丢合法状态 | 手工修复 | Migration Drill |
| Legacy 可解释 | Very Old Data | 正确映射 | Missing Reference | Compatibility Test |
| Support 可恢复 | Support Scenario | 能诊断和重试 | 需直接改库 | Tooling Test |
| Sunset 公平 | Player Review | 通知和迁移清楚 | 已购价值丢失 | Policy Review |

### 82.3 Behavioral Metrics

- Version Registered；
- Compatibility Evaluated；
- Migration Started；
- Migration Paused；
- Migration Resumed；
- Migration Completed；
- Migration Failed；
- Rollback Started；
- Forward Fix Applied；
- Backfill Completed；
- Legacy Mapping Used；
- Deprecation Announced；
- Sunset Completed。

### 82.4 Outcome Metrics

- Migration Success；
- Migration Duration；
- Error Rate；
- Retry Rate；
- Manual Review Rate；
- Data Difference；
- Save Recovery；
- Duplicate Asset；
- Missing Entitlement；
- Balance Reconciliation；
- Compatibility Block Accuracy；
- Rollback Success；
- Forward Fix Success；
- Legacy Access；
- Support Contacts；
- Player Data Loss；
- Cross-Account Incident。

### 82.5 Negative Metrics

- 未注册版本；
- 版本号复用；
- Schema Break；
- Save Corruption；
- 重复 Migration；
- Dual Write 长期存在；
- Missing Mapping；
- Rollback 失败；
- Backfill 触发副作用；
- 旧版本无通知失效；
- Analytics 历史断裂；
- Entitlement 丢失；
- Economy 余额错误；
- Cross-Account；
- 手工数据库修复。

### 82.6 Event Intents

| Event Intent | Trigger | Key Properties | Privacy Notes |
|---|---|---|---|
| Version State Changed | 版本状态变化 | Type, From, To, Version | 不记录个人数据 |
| Compatibility Evaluated | 兼容判断 | Source, Target, Result | 技术用途 |
| Migration State Changed | 迁移状态变化 | Migration, From, To, Scope | 审计 |
| Migration Object Failed | 对象失败 | Object Type, Error Code | 使用去标识引用 |
| Reconciliation Completed | 对账完成 | Scope, Difference, Result | 高权限 |
| Rollback Executed | 回滚 | Version, Scope, Result | 高风险审计 |
| Legacy Mapping Applied | Legacy 解析 | Source, Target | 不公开账户明细 |
| Sunset State Changed | Sunset 变化 | Target, State, Date | 产品治理 |

---

## 83. Test Strategy

### 83.1 Version Tests

- Semantic；
- Build；
- Registry；
- Immutable ID；
- Status；
- Deprecation；
- Sunset。

### 83.2 Compatibility Tests

- Client / Server；
- API；
- Protocol；
- Save；
- Content；
- Platform；
- Session；
- Read / Write；
- Unknown Capability。

### 83.3 Save Migration Tests

- Very Old；
- Current；
- Corrupted；
- Partial；
- Cloud Conflict；
- Removed Content；
- Backup；
- Retry；
- Rollback。

### 83.4 Database Migration Tests

- Expand；
- Dual Write；
- Backfill；
- Read Switch；
- Write Switch；
- Contract；
- Capacity；
- Failure；
- Resume。

### 83.5 Domain Migration Tests

- Economy；
- Progression；
- Reward；
- Content；
- Rating；
- Price；
- Order；
- Entitlement；
- Analytics；
- Experiment。

### 83.6 Rollback Tests

- Code；
- Config；
- Schema；
- Data；
- Feature；
- Mixed Version；
- New Data；
- Forward Fix。

### 83.7 Security and Privacy Tests

- Unauthorized Migration；
- Script Tamper；
- Cross-Account；
- Backup Access；
- Replay；
- Double Migration；
- Deletion During Migration；
- Legal Hold；
- Audit Tamper。

### 83.8 Accessibility Tests

- Upgrade Prompt；
- Migration Progress；
- Error；
- Retry；
- Read-Only；
- Support；
- Large Text；
- Screen Reader。

---

## 84. Compatibility Contract Template

```markdown
# Compatibility Contract

## Versions

- Producer:
- Consumer:
- Protocol:
- Schema:
- Content:
- Platform:

## Capabilities

- Required:
- Optional:
- Unsupported:

## Read / Write

- Read Compatible:
- Write Compatible:
- Session Compatible:
- Save Compatible:

## Decision

- Result:
- Reason:
- Required Action:
- Fallback:
- Expiry:

## Governance

- Owner:
- Test:
- Deprecation:
- Sunset:
```

---

## 85. Migration Contract Template

```markdown
# Migration Contract

## Definition

- Migration ID:
- Domain:
- Source Version:
- Target Version:
- Object Type:
- Owner:
- Risk Level:

## Scope

- Environment:
- Region:
- Platform:
- Cohort:
- Count:
- Window:

## Transform

- Preconditions:
- Mapping:
- Defaults:
- Unknown Handling:
- Tombstones:
- Legacy:

## Safety

- Idempotency:
- Backup:
- Checkpoint:
- Rate Limit:
- Kill Switch:
- Rollback:
- Forward Fix:

## Validation

- Structural:
- Quantitative:
- Business:
- Sample:
- Acceptance Criteria:

## Operations

- Dry Run:
- Shadow:
- Canary:
- Support:
- Communication:
- Audit:
```

---

## 86. Rollback Contract Template

```markdown
# Rollback Contract

## Target

- Current Version:
- Target Version:
- Scope:
- Reason:

## Feasibility

- Old Code Reads New Data:
- Transactions Occurred:
- Rewards Granted:
- Entitlements Created:
- New References:
- Data Downgrade:

## Plan

- Stop Writes:
- Restore Code:
- Restore Config:
- Restore Data:
- Reconcile:
- Verify:
- Communicate:

## Alternative

- Forward Fix:
- Compatibility Layer:
- Compensation:

## Governance

- Owner:
- Approver:
- Audit:
- Postmortem:
```

---

## 87. Sunset Contract Template

```markdown
# Sunset Contract

## Target

- Version / Product / Service:
- Current Status:
- Sunset Date:
- Reason:
- Owner:

## Impact

- Players:
- Platforms:
- Regions:
- Saves:
- Entitlements:
- Purchases:
- Offline:
- Social:

## Transition

- Replacement:
- Migration:
- Export:
- Refund / Credit:
- Legacy Access:
- Support:

## Communication

- Announcement:
- Reminder:
- Final Notice:
- Status Page:

## Completion

- Stop New Use:
- Stop Writes:
- Archive:
- Delete:
- Legal Retention:
- Verification:
```

---

## 88. Versioning and Migration Debt

包括：

- 多套版本命名；
- 无 Version Registry；
- 兼容规则只存在代码中；
- Save Version 与 Client Version 混用；
- 旧客户端支持无明确窗口；
- Dual Write 长期存在；
- Migration 脚本不可重入；
- Backfill 触发业务副作用；
- 无 Checkpoint；
- Legacy Mapping 分散；
- Display Name 被当 ID；
- 版本变化覆盖历史事实；
- 回滚未考虑新数据；
- Schema 变更无消费者追踪；
- Support 无法查看版本；
- Deprecation 无 Sunset；
- 旧 Flag 和兼容层永久存在。

### 88.1 Signals

- 发布前不知道旧客户端是否可用；
- Very Old Save 经常损坏；
- 团队不敢删除旧字段；
- 新旧表长期分叉；
- 迁移只能一次性执行；
- 回滚后出现更多错误；
- Legacy ID 频繁 Missing；
- Support 要求工程师直接查库；
- 同类迁移重复实现；
- 版本历史无法复现。

### 88.2 Reduction

- Version Registry；
- Compatibility Matrix；
- Migration Framework；
- Expand / Contract；
- Checkpoint；
- Reconciliation；
- Tombstone；
- Legacy Mapping；
- Rollback Review；
- Forward Fix Playbook；
- Deprecation SLA；
- Compatibility Test Suite；
- Quarterly Migration Health Review。

---

## 89. Rollout and Migration of This System

### 89.1 Rollout

Versioning and Migration 系统自身应按：

```text
Registry First
→ Read-Only Inventory
→ Compatibility Documentation
→ Shadow Decisions
→ Low-Risk Migrations
→ Domain Adoption
→ High-Risk Domains
→ Full Governance
```

### 89.2 High-Risk Changes

包括：

- Version ID；
- Compatibility Engine；
- Save Header；
- Migration Framework；
- Checkpoint；
- Dual Write；
- Legacy Mapping；
- Rollback Control；
- Backup；
- Audit；
- Cross-Platform Merge。

### 89.3 Bootstrap Migration

旧系统可能没有版本字段。

需要：

- Infer；
- Unknown；
- Default；
- Manual Review；
- Source Metadata；
- Confidence。

### 89.4 Rollback

系统自身回滚时：

- 保留 Registry History；
- 不丢 Migration State；
- 暂停新迁移；
- 恢复旧 Compatibility；
- 保留 Audit；
- 不重复 Job；
- 支持 Resume。

### 89.5 Stop Conditions

出现以下情况应停止发布：

- Version ID 冲突；
- Compatibility Decision 大面积错误；
- Save Header 写坏；
- Migration 重复执行；
- Checkpoint 丢失；
- Cross-Account；
- Backup 不可恢复；
- Rollback Control 不可用；
- Audit 缺失；
- Legacy Mapping 大面积失败；
- Support 无法诊断；
- 新框架破坏现有迁移。

---

## 90. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| 版本类型过多导致混淆 | Governance Risk | 高 | 高 | Registry + Taxonomy | Architecture |
| 旧客户端长期支持成本 | Operational Risk | 高 | 高 | Support Window | Product |
| Save 迁移损坏 | Data Risk | 严重 | 中 | Backup + Atomic Commit | Engineering |
| Dual Write 分叉 | Consistency Risk | 严重 | 中 | Authority + Reconciliation | Data |
| 回滚代码但数据不可降级 | Recovery Risk | 严重 | 中 | Rollback Feasibility Review | SRE |
| Legacy Mapping 缺失 | Access Risk | 高 | 高 | Tombstone + Mapping Registry | Content |
| Economy 转换不公平 | Player Value Risk | 严重 | 中 | Distribution Review | Economy |
| Entitlement 迁移丢失 | Commercial Risk | 严重 | 低 | Source Preservation | Commerce |
| Cross-Platform Merge 串号 | Privacy / Asset Risk | 严重 | 低 | Identity Review | Security |
| Migration Debt 持续增长 | Architecture Risk | 高 | 高 | Standard Framework | Architecture |

---

## 91. Review Checklist

### Version Registry

- [ ] 所有关键版本类型已定义；
- [ ] Version ID 稳定且不可复用；
- [ ] Display Version 与 Internal Version 分离；
- [ ] Registry 有 Owner、Status、Compatibility 和 Sunset；
- [ ] 生产无未知版本。

### Compatibility

- [ ] Compatibility Matrix 完整；
- [ ] Read、Write、Session、Save 和 Network 兼容分离；
- [ ] Capability Negotiation 可用；
- [ ] Minimum Supported Version 明确；
- [ ] Soft、Hard 和 Emergency Update 规则完整。

### Save and Schema

- [ ] Save Header 记录关键版本；
- [ ] 迁移前备份；
- [ ] Migration Atomic；
- [ ] Expand / Contract 可执行；
- [ ] Destructive Change 有审批；
- [ ] Downgrade 风险已说明。

### Migration

- [ ] Source、Target、Scope 和 Invariants 明确；
- [ ] Migration 幂等；
- [ ] Checkpoint 和 Resume 可用；
- [ ] Dry Run、Shadow 和 Canary 完成；
- [ ] Reconciliation 和 Acceptance Criteria 完整；
- [ ] Manual Review 有正式工具。

### Dual Read / Write

- [ ] 权威源明确；
- [ ] Partial Failure 有恢复；
- [ ] 对账可执行；
- [ ] 有结束条件；
- [ ] 不长期保留临时复杂度。

### Domain Versions

- [ ] Content、Rule、Economy、Progression、Reward、Commercial、Entitlement、Analytics 和 Experiment 版本化；
- [ ] 历史实例保留原版本；
- [ ] 活跃 Session 冻结规则；
- [ ] 旧订单、奖励和权益不被重新解释；
- [ ] Legacy Mapping 和 Tombstone 完整。

### Rollback and Recovery

- [ ] Rollback Feasibility 在发布前评估；
- [ ] Code、Config、Schema 和 Data Rollback 区分；
- [ ] Forward Fix 可用；
- [ ] PITR 和 Backup 已演练；
- [ ] 合法新状态不会被静默删除；
- [ ] Incident Runbook 完整。

### Deprecation and Sunset

- [ ] Deprecation Lifecycle 完整；
- [ ] 使用追踪可用；
- [ ] Replacement 和 Migration Guide 完整；
- [ ] 玩家通知和支持窗口明确；
- [ ] 已购、Save、Offline 和数据导出已处理。

### Security, Privacy and Accessibility

- [ ] Migration 权限最小化；
- [ ] Script、Backup 和 Audit 有保护；
- [ ] Cross-Account 风险已测试；
- [ ] 删除请求和 Legal Hold 可共存；
- [ ] 玩家迁移 UI 可访问；
- [ ] 儿童和家庭数据有额外保护。

### Operations

- [ ] Rate Limit、Backpressure 和 Capacity 已评估；
- [ ] Support 可查看版本和迁移状态；
- [ ] Dashboard 和 Alerts 可用；
- [ ] Migration Debt 可监控；
- [ ] Stop Conditions 明确。

---

## 92. V1 Completion Criteria

Versioning and Migration 可以被视为 V1，当：

- Client、Server、API、Protocol、Schema、Save、Content、Rule、Economy、Progression、Reward、Matchmaking、Rating、Commercial、Analytics、Experiment、Live Config 和 Platform Version 类型完整；
- Version Registry、Version Definition、Compatibility Matrix、Migration Definition、Migration Instance、Checkpoint、Legacy Mapping、Tombstone、Deprecation 和 Sunset 实体明确；
- Version ID、Display Version、Build、Semantic Version 和不可变语义规则完整；
- Backward、Forward、Read、Write、Session、Save、Network、Content 和 Commercial Compatibility 已定义；
- Compatibility Decision 能返回原因、Required Action 和 Fallback；
- 关键持久对象记录创建和结算时版本；
- Client / Server 支持窗口、Minimum Version、Soft Update、Hard Update 和 Emergency Block 可执行；
- API、Protocol、Capability Negotiation 和 Consumer Contract 规则完整；
- Save Header、Backup、Atomic Migration、Upgrade、Read-Only Legacy 和 Downgrade Policy 明确；
- Database Expand / Contract、Dual Read、Dual Write、Shadow、Backfill、Rebuild 和 Reconciliation 可执行；
- Migration Proposed、Dry Run、Shadow、Ready、Running、Verifying、Cutover、Monitoring、Completed、Paused、Failed、Rollback 和 Forward Fix 生命周期完整；
- Migration 幂等、可检查点、可恢复、有限速、有审计；
- Lazy、Bulk、Online、Offline 和 Hybrid Migration 有选择标准；
- Content、Rule、Rating、Economy、Progression、Reward、Commercial、Entitlement、Analytics、Experiment 和 Live Config 有专项版本规则；
- Reference Rename、Merge、Split、Delete、Unknown Enum、Default 和 Tombstone 处理完整；
- Legacy Support、Grandfathering、Deprecation、Usage Tracking、Sunset 和 Archive 形成闭环；
- Code、Config、Schema、Data 和 Business Rollback 的差异和可行性明确；
- 无法安全回滚时可执行 Forward Fix、Corrective Transaction 和 Compatibility Layer；
- Backup、Restore、PITR、Dry Run、Canary、Rate Limit、Checkpoint、Dead Letter 和 Manual Review 可用；
- Security、Privacy、Child、Family、Cross-Account、Support 和 Accessibility 通过专项评审；
- Version、Compatibility、Migration、Reconciliation、Rollback、Legacy 和 Sunset 有验证计划；
- Versioning and Migration Debt 有识别和治理方式；
- 本系统自身高风险变更支持 Shadow、灰度、迁移、回滚和停止条件；
- 所有下游系统可以直接引用本文件定义版本、兼容和迁移策略。

---

## 93. Related Documents

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
- [Characters and Loadouts](../content/characters-and-loadouts.md)
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
- [Experiment Management](./experiment-management.md)
