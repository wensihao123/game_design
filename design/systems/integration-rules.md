# Integration Rules（系统集成规则）

> Status: V1  
> Path: `design/systems/integration-rules.md`  
> Scope: 通用的软件、游戏与交互产品跨系统集成  
> Purpose: 统一系统之间的状态所有权、调用方式、事件、事务、幂等、配置、故障隔离、版本兼容和例外管理，降低隐式耦合、数据冲突与故障扩散。

---

## 1. Integration Rules 的作用

Integration Rules 用于回答：

```text
系统之间如何通信？
谁可以读取或修改哪些状态？
一次跨系统操作如何保证正确？
重复请求如何处理？
事件如何命名、版本化和重放？
配置如何覆盖基础规则？
一个系统失败时，其他系统如何降级？
旧版本和新版本如何共存？
跨系统例外如何被记录和回收？
```

这份文档不替代：

- 单系统内部规则；
- 技术架构设计；
- API 详细规格；
- 数据库设计；
- 消息中间件配置；
- 部署拓扑；
- 具体代码实现。

它定义的是系统设计层的集成契约。

---

## 2. 集成的核心目标

### 2.1 Correctness

跨系统结果必须正确。

包括：

- 不重复发放；
- 不重复扣除；
- 不丢失状态；
- 不产生矛盾所有权；
- 不发生部分成功却无法解释。

### 2.2 Explainability

跨系统结果必须可追踪。

团队应能回答：

```text
谁发起了操作？
哪个系统做了判断？
哪个系统修改了状态？
为什么产生当前结果？
```

### 2.3 Isolation

一个系统失败时，应尽量限制影响范围。

### 2.4 Recoverability

跨系统操作失败后，应有：

- 重试；
- 回滚；
- 补偿；
- 恢复；
- 人工处理。

### 2.5 Evolvability

系统可以独立升级，而不要求所有依赖同时重写。

### 2.6 Observability

重要集成行为必须可记录、监控和审计。

### 2.7 Player Trust

集成问题不应转化为玩家无法理解的：

- 资产丢失；
- 重复收费；
- 任务卡死；
- 权益失效；
- 进度回退。

---

## 3. 基本原则

### 3.1 One State, One Owner

每项权威状态只有一个主拥有系统。

### 3.2 Request, Do Not Reach Through

系统应请求状态拥有者执行修改，而不是直接绕过所有者写入。

### 3.3 Events Describe Facts

事件描述已经发生的事实，不描述模糊意图。

### 3.4 Commands Express Intent

Command 表达希望某个系统执行的动作。

### 3.5 Queries Do Not Mutate

查询不应产生隐藏业务状态变化。

### 3.6 Idempotency by Default

高影响命令与事务默认应支持幂等。

### 3.7 Explicit Failure

失败必须显式表达，不依赖静默忽略。

### 3.8 Last Known Good Configuration

配置失败时优先使用最后一个有效版本。

### 3.9 High-Risk Operations Fail Closed

付费、权益、隐私和永久资产操作在不确定时优先拒绝，而不是冒险继续。

### 3.10 Observability Is Part of Design

日志、审计和追踪不是上线后补充，而是集成设计的一部分。

---

## 4. 集成对象

系统之间通常通过四种对象交互：

```text
Command
Query
Event
Configuration
```

### 4.1 Command

表示：

```text
请执行一个动作。
```

例如：

- ClaimReward；
- EquipItem；
- StartQuest；
- PurchaseOffer；
- UnlockContent。

Command 可能成功或失败。

### 4.2 Query

表示：

```text
请返回当前事实。
```

例如：

- GetResourceBalance；
- GetCurrentLoadout；
- GetAvailableContent；
- GetEntitlements。

Query 不应修改业务状态。

### 4.3 Event

表示：

```text
某个事实已经发生。
```

例如：

- RewardClaimed；
- ItemEquipped；
- QuestCompleted；
- ContentUnlocked；
- PurchaseConfirmed。

Event 不应表达请求。

### 4.4 Configuration

表示：

```text
当前规则、参数和内容定义是什么。
```

例如：

- Reward Table；
- Difficulty Curve；
- Offer Schedule；
- Content Availability；
- Feature Flag。

---

## 5. Command、Query、Event 的边界

### 5.1 不要混用

不推荐：

```text
GetAndClaimReward
```

因为它同时：

- 查询；
- 修改；
- 产生事务。

更好：

```text
GetClaimableRewards
ClaimReward
RewardClaimed
```

### 5.2 Command 由目标系统拥有

例如：

```text
Reward System
拥有 ClaimReward。

Economy System
拥有 GrantResource。

Progression System
拥有 UpgradeCharacter。
```

### 5.3 Event 由状态拥有者发布

只有权威系统可以发布其状态变化事件。

例如：

```text
Economy 发布 ResourceBalanceChanged。
Progression 发布 CharacterLeveledUp。
Content 发布 ContentUnlocked。
```

### 5.4 Query 返回的是当前事实

Query 应说明：

- 数据版本；
- 是否可能陈旧；
- 是否来自缓存；
- 是否最终一致；
- 是否包含权限过滤。

---

## 6. 状态所有权

### 6.1 Owner 的定义

状态 Owner 负责：

- 状态定义；
- 正确性；
- 修改规则；
- 持久化；
- 冲突处理；
- 迁移；
- 权威事件。

### 6.2 Reader

Reader 可以读取状态，但不能直接修改。

### 6.3 Writer

Writer 不应直接存在于 Owner 之外。

其他系统需要修改时，应：

```text
发送 Command
→ Owner 验证
→ Owner 修改
→ Owner 发布 Event
```

### 6.4 所有权表

```markdown
| State | Owner | Readers | Allowed Commands | Events |
|---|---|---|---|---|
```

### 6.5 派生状态

派生状态可以由其他系统计算或缓存，但必须说明：

- 来源；
- 失效条件；
- 更新时机；
- 权威性；
- 不一致时如何处理。

### 6.6 禁止共享写入

避免：

```text
Quest 和 Reward 都可以修改领取状态。
Progression 和 Character 都可以修改等级。
Commercial 和 Entitlement 都可以授予权益。
```

这会导致：

- 冲突；
- 重复；
- 回滚困难；
- 迁移困难；
- 审计困难。

---

## 7. 写入路径

### 7.1 标准写入路径

```text
Caller
→ Command
→ Owner Validation
→ State Mutation
→ Persistence
→ Event Publication
→ Feedback
```

### 7.2 顺序要求

高风险状态写入应优先保证：

```text
先确认权威状态成功
再展示成功结果
```

不应：

```text
先展示成功
再异步尝试写入
```

除非系统明确支持乐观更新并具有回滚。

### 7.3 乐观更新

适用于：

- 低风险；
- 可快速回退；
- 高延迟体验；
- 不涉及永久资产。

必须具备：

- 临时状态标识；
- 失败回滚；
- 玩家反馈；
- 重试；
- 冲突处理。

### 7.4 悲观确认

适用于：

- 购买；
- 资源消耗；
- 权益；
- 永久删除；
- 跨系统高影响事务。

---

## 8. 事务边界

### 8.1 单系统事务

同一系统内部的状态变化，应尽量在单一事务内完成。

### 8.2 跨系统事务

跨系统事务可能无法使用单一原子提交。

此时应定义：

- 主事务系统；
- 事务 ID；
- 步骤；
- 成功确认；
- 超时；
- 重试；
- 补偿；
- 审计。

### 8.3 示例：领取奖励

```text
1. Reward 验证奖励可领取；
2. Reward 锁定领取状态；
3. Reward 请求 Economy 发放资源；
4. Economy 幂等发放；
5. Economy 返回成功；
6. Reward 标记已领取；
7. Reward 发布 RewardClaimed；
8. UI 展示结果。
```

### 8.4 部分成功

如果步骤 3 成功但步骤 6 失败：

- 不能再次发放资源；
- 必须能够恢复 Reward 状态；
- 必须记录事务 ID；
- 必须支持人工或自动对账。

### 8.5 补偿事务

补偿不是简单反向操作。

例如：

```text
资源已被玩家消费后，
不能直接从余额中扣回。
```

补偿策略应根据业务风险定义：

- 标记已完成；
- 发放补偿；
- 冻结异常；
- 人工审核；
- 允许系统吸收损失。

---

## 9. 幂等

### 9.1 定义

同一个操作重复执行，不应重复产生业务结果。

### 9.2 需要幂等的操作

- 领取奖励；
- 发放资源；
- 扣除资源；
- 购买；
- 恢复购买；
- 创建权益；
- 完成任务；
- 存档迁移；
- 补偿；
- 删除账户请求。

### 9.3 幂等键

幂等键应基于稳定业务身份。

例如：

```text
RewardInstanceID
PurchaseTransactionID
QuestCompletionID
MigrationJobID
CompensationBatchID
```

不推荐仅使用：

```text
当前时间
随机字符串
客户端会话 ID
```

除非这些值与业务事务稳定绑定。

### 9.4 幂等结果

重复请求应返回：

- 原成功结果；
- 当前状态；
- 已处理标识。

不应将重复成功当作普通错误，导致客户端继续重试。

### 9.5 幂等保存期限

需要根据重试窗口和风险决定。

购买与权益幂等记录通常需要长期保留。

---

## 10. 并发与竞态

### 10.1 常见竞态

- 同时领取同一奖励；
- 多设备同时修改装备；
- 两个购买请求同时扣款；
- 任务完成与活动结束同时发生；
- 资源消费与退款同时发生；
- 存档上传与迁移同时发生。

### 10.2 处理方式

可能包括：

- 版本号；
- 乐观锁；
- 悲观锁；
- 事务锁；
- 队列；
- 顺序执行；
- Compare-and-Swap；
- 最终一致与冲突合并。

### 10.3 冲突原则

高影响资产：

```text
优先正确
而不是最后写入者覆盖。
```

低风险偏好：

```text
可以使用最后写入者覆盖，
但需要明确设备同步规则。
```

### 10.4 多设备修改

需要定义：

- 权威来源；
- 版本；
- 时间；
- 合并；
- 玩家选择；
- 冲突备份。

---

## 11. 事件设计

### 11.1 事件命名

推荐过去式事实：

```text
QuestStarted
QuestCompleted
RewardGranted
ResourceSpent
CharacterLeveledUp
ContentUnlocked
```

避免：

```text
DoQuest
UpdateProgress
HandleReward
ProcessEverything
```

### 11.2 事件内容

事件应包含：

- Event ID；
- Event Type；
- Version；
- Timestamp；
- Producer；
- Entity ID；
- Correlation ID；
- Causation ID；
- 必要业务字段。

### 11.3 Correlation ID

用于追踪一次跨系统流程。

### 11.4 Causation ID

用于说明：

```text
这个事件由哪个命令或事件直接引起。
```

### 11.5 事件最小化

事件只包含消费者需要的稳定事实。

避免把整个内部对象直接发布。

### 11.6 敏感信息

事件不应包含不必要：

- 身份信息；
- 支付信息；
- 私密社交内容；
- 设备信息；
- 精确位置。

---

## 12. 事件交付语义

### 12.1 At Most Once

最多一次。

风险：

- 事件可能丢失。

### 12.2 At Least Once

至少一次。

风险：

- 事件可能重复。

要求消费者幂等。

### 12.3 Exactly Once

业务层面严格一次通常需要：

- 幂等；
- 去重；
- 事务日志；
- 业务键；

共同实现。

不应仅依赖技术中间件宣称“Exactly Once”。

### 12.4 默认建议

跨系统高价值事件按：

```text
At Least Once
+
Consumer Idempotency
```

设计。

---

## 13. 事件顺序

### 13.1 全局顺序

通常不应假设所有事件拥有全局顺序。

### 13.2 实体顺序

对同一实体，可以通过：

- Entity Version；
- Sequence Number；
- Partition Key；

保证或判断顺序。

### 13.3 乱序处理

消费者应定义：

- 忽略旧版本；
- 暂存等待；
- 重新查询权威状态；
- 触发修复；
- 标记异常。

### 13.4 不依赖隐式顺序

不推荐：

```text
假设 ResourceGranted 一定在 RewardClaimed 前到达。
```

应通过：

- 明确状态；
- 事务 ID；
- 查询；
- 版本；

处理。

---

## 14. 事件版本

### 14.1 为什么需要版本

事件结构会变化。

### 14.2 兼容原则

优先：

- 添加可选字段；
- 保留旧字段；
- 设置默认值；
- 消费者忽略未知字段。

### 14.3 Breaking Change

包括：

- 字段删除；
- 字段含义改变；
- 类型改变；
- 单位改变；
- 事件语义改变。

Breaking Change 应：

- 新建事件版本；
- 支持双写或转换；
- 设定迁移期；
- 监控旧消费者；
- 明确弃用时间。

### 14.4 不要复用旧事件表达新含义

例如：

```text
RewardGranted
原本表示资源已到账，
后改为仅表示奖励已生成。
```

这属于语义破坏，应新建事件。

---

## 15. Query 一致性

### 15.1 Strong Consistency

适用于：

- 余额；
- 权益；
- 购买；
- 领取资格；
- 永久删除确认。

### 15.2 Eventual Consistency

适用于：

- 排行榜展示；
- 非关键推荐；
- 分析；
- 社交在线状态；
- 部分内容聚合。

### 15.3 显示陈旧状态

如果结果可能延迟，应：

- 标记更新时间；
- 提供刷新；
- 避免高影响操作依赖陈旧缓存；
- 必要时重新向 Owner 验证。

### 15.4 Read Your Writes

玩家刚完成的操作，应尽量立即在当前体验中可见。

---

## 16. Configuration

### 16.1 配置类型

- Static Configuration；
- Runtime Configuration；
- Content Configuration；
- Feature Flag；
- Experiment Variant；
- Emergency Override。

### 16.2 配置所有权

每项配置必须有：

- Owner；
- Schema；
- Version；
- Default；
- Validation；
- Rollback；
- 生效时间。

### 16.3 配置优先级

建议统一：

```text
Code Invariant
→ Product Configuration
→ Platform Override
→ Region Override
→ Experiment Variant
→ Emergency Override
```

越靠后的覆盖层风险越高。

### 16.4 不可覆盖内容

配置不得绕过：

- 数据正确性；
- 付费安全；
- 隐私；
- 儿童保护；
- 所有权；
- 核心不变量。

### 16.5 配置验证

发布前检查：

- 类型；
- 范围；
- 引用；
- 时间；
- 区域；
- 奖励；
- 资源；
- 内容依赖；
- 版本兼容。

### 16.6 最后有效配置

配置加载失败时：

- 使用 Last Known Good；
- 阻止不安全新配置；
- 记录异常；
- 通知 Owner；
- 提供回滚。

---

## 17. Feature Flags

### 17.1 用途

- 灰度发布；
- 快速关闭；
- 分平台；
- 分地区；
- 内部测试；
- 实验；
- 风险隔离。

### 17.2 Flag 不应成为永久架构

长期未清理的 Flag 会造成：

- 多分支逻辑；
- 测试组合爆炸；
- 状态不一致；
- 迁移困难。

### 17.3 每个 Flag 需要

- Owner；
- 创建原因；
- 默认值；
- 目标人群；
- 开始时间；
- 结束条件；
- 删除日期；
- 数据影响；
- 关闭后行为。

### 17.4 高风险 Flag

涉及：

- 付费；
- 权益；
- 数据格式；
- 存档；
- 经济；

时必须说明关闭后的数据处理。

---

## 18. Experiment Integration

### 18.1 实验系统职责

实验系统只负责：

- 分组；
- 变体；
- 暴露记录；
- 分析连接。

### 18.2 实验系统不应

- 直接发放资产；
- 修改业务状态；
- 绕过领域验证；
- 决定最终权益；
- 在高风险群体中无保护试验。

### 18.3 实验变体

应通过配置进入领域系统。

领域系统仍负责：

- 验证；
- 不变量；
- 状态；
- 权限；
- 正确性。

### 18.4 暴露事件

只有玩家真正接触变体时才记录暴露。

不要在后台分组时直接视为已暴露。

---

## 19. Live Operations Integration

### 19.1 Live Operations 可以控制

- 活动时间；
- 内容开放；
- 奖励配置；
- 入口；
- 文案；
- 资源倍率；
- 返场。

### 19.2 Live Operations 不应直接修改

- 玩家余额；
- 权益；
- 购买记录；
- 存档结构；
- 账号身份；
- 核心安全规则。

### 19.3 资产操作

运营需要补偿时，应调用领域系统正式 Command。

例如：

```text
CreateCompensationGrant
```

而不是直接改数据库。

### 19.4 活动结束

应定义：

- 未领取奖励；
- 活动货币；
- 未完成任务；
- 排名；
- 购买权益；
- 返场；
- 数据归档。

---

## 20. 外部系统集成

外部系统包括：

- 支付平台；
- 平台账号；
- 推送服务；
- 云存档；
- 社交平台；
- 第三方分析；
- 广告；
- 客服。

### 20.1 外部系统原则

- 不假设永远可用；
- 不假设回调只到一次；
- 不假设顺序；
- 不信任未经验证输入；
- 不把外部状态直接当作内部权威；
- 保留审计和恢复路径。

### 20.2 支付

支付应至少区分：

```text
Purchase Initiated
Payment Pending
Payment Confirmed
Entitlement Granted
Purchase Completed
Refunded
Revoked
```

不能将：

```text
客户端返回成功
```

直接视为购买完成。

### 20.3 外部超时

需要：

- Pending 状态；
- 重试；
- 查询；
- 对账；
- 玩家说明；
- 客服恢复。

---

## 21. 故障隔离

### 21.1 核心原则

非关键系统失败不应阻断核心系统。

### 21.2 可降级系统

通常包括：

- Analytics；
- Notification；
- Recommendation；
- Social Presence；
- Non-Critical Live Content；
- Cosmetic Preview。

### 21.3 不可静默降级

以下失败必须明确：

- 购买；
- 权益；
- 存档；
- 资源扣除；
- 任务领取；
- 隐私；
- 数据删除。

### 21.4 Circuit Breaker 设计意图

当依赖持续失败时：

- 停止重复调用；
- 使用降级；
- 避免故障扩散；
- 定期探测恢复。

系统设计文档不需要规定技术参数，但应说明：

- 触发条件；
- 降级结果；
- 玩家影响；
- 恢复条件。

---

## 22. 超时与重试

### 22.1 超时

每个外部或跨系统请求应有明确超时。

### 22.2 重试条件

只重试：

- 短暂网络错误；
- 服务暂时不可用；
- 可幂等操作；
- 明确可恢复错误。

### 22.3 不重试条件

- 参数错误；
- 权限不足；
- 状态不满足；
- 资源不足；
- 永久业务拒绝。

### 22.4 退避

避免立即高频重复。

### 22.5 玩家反馈

长时间 Pending 时，应展示：

- 当前状态；
- 是否仍在处理；
- 是否可以退出；
- 结果如何恢复；
- 是否会重复扣除。

---

## 23. 回滚与补偿

### 23.1 回滚

回滚适用于尚未被其他系统消费的状态。

### 23.2 补偿

补偿适用于无法安全回滚的已发生结果。

### 23.3 补偿原则

- 不重复；
- 可审计；
- 有事务 ID；
- 可批量；
- 有 Owner；
- 有到期；
- 有玩家说明。

### 23.4 不要用补偿代替正确性

补偿是最后手段，不应成为正常流程。

---

## 24. 审计

以下操作需要长期审计：

- 购买；
- 退款；
- 权益；
- 高价值资源；
- 管理员操作；
- 补偿；
- 存档迁移；
- 账号删除；
- 隐私授权；
- 封禁与申诉。

审计记录应包含：

- 谁；
- 何时；
- 为什么；
- 修改前；
- 修改后；
- 来源；
- 事务；
- 版本。

审计数据本身也应遵循隐私最小化。

---

## 25. 数据隐私

跨系统传递数据时，应检查：

### 25.1 Purpose Limitation

数据是否只用于明确目的。

### 25.2 Data Minimization

是否只传必要字段。

### 25.3 Access Control

谁可以读取。

### 25.4 Retention

保存多久。

### 25.5 Deletion

删除请求如何跨系统传播。

### 25.6 Derived Data

删除原始数据后，派生数据如何处理。

### 25.7 Third Party

传给第三方的数据是否必要、透明且可控制。

---

## 26. 删除与遗忘传播

账户或数据删除通常是跨系统事务。

应定义：

```text
Deletion Requested
→ Identity Verified
→ Account Locked
→ Systems Notified
→ Data Deleted or Anonymized
→ Third Parties Notified
→ Completion Recorded
```

### 26.1 删除状态

- Requested；
- Verified；
- In Progress；
- Partially Completed；
- Completed；
- Failed。

### 26.2 删除失败

必须：

- 可重试；
- 可审计；
- 不静默完成；
- 向用户说明；
- 支持人工处理。

---

## 27. 版本兼容

### 27.1 API Version

跨系统接口应有兼容策略。

### 27.2 Data Version

持久化状态需要版本。

### 27.3 Event Version

事件需要版本。

### 27.4 Configuration Version

配置需要版本。

### 27.5 Client Compatibility

旧客户端可能：

- 不认识新字段；
- 不支持新状态；
- 使用旧规则。

需要定义：

- 最低版本；
- 兼容窗口；
- 强制升级；
- 降级表现；
- 数据转换。

---

## 28. Migration

### 28.1 迁移原则

- 可重复执行；
- 可暂停；
- 可恢复；
- 可审计；
- 有进度；
- 有失败列表；
- 有回滚或补偿。

### 28.2 迁移前

必须检查：

- 数据备份；
- 目标版本；
- 兼容；
- 性能；
- 锁；
- 玩家影响；
- 客服准备。

### 28.3 迁移期间

需要：

- 状态；
- 监控；
- 错误处理；
- 流量控制；
- 旧新双读或双写条件。

### 28.4 迁移后

需要：

- 对账；
- 抽样；
- 完整性；
- 旧路径关闭；
- Flag 清理；
- 复盘。

---

## 29. 跨系统例外

### 29.1 定义

跨系统例外是：

```text
为了特定内容、平台、活动或修复，
临时偏离标准集成规则。
```

### 29.2 例外风险

- 所有权模糊；
- 状态不同步；
- 事件遗漏；
- 迁移困难；
- 运营依赖；
- 难以回收。

### 29.3 例外记录

```markdown
## Integration Exception

- Base Rule:
- Exception:
- Scope:
- Reason:
- Systems:
- Data Impact:
- Risk:
- Start:
- Expiry:
- Owner:
- Removal Plan:
- Review Condition:
```

### 29.4 禁止永久临时例外

例外到期后必须：

- 删除；
- 正式升级为规则；
- 迁移数据；
- 记录原因。

---

## 30. 跨系统契约模板

```markdown
# Integration Contract

## 1. Parties

- Producer:
- Consumer:
- Owner:
- Risk Level:

## 2. Purpose

- Business Purpose:
- Player Value:

## 3. Interaction Type

- Command / Query / Event / Configuration

## 4. Input

| Field | Meaning | Required | Version |
|---|---|---|---|

## 5. Output

| Field | Meaning | Guarantee | Version |
|---|---|---|---|

## 6. State Ownership

- Owned State:
- Read State:
- Write Restrictions:

## 7. Correctness

- Invariants:
- Idempotency Key:
- Transaction Boundary:
- Ordering:
- Consistency:

## 8. Failure

- Timeout:
- Retry:
- Permanent Failure:
- Partial Failure:
- Recovery:
- Player Feedback:

## 9. Versioning

- Current Version:
- Compatibility:
- Deprecation:
- Migration:

## 10. Observability

- Correlation ID:
- Audit:
- Metrics:
- Alerts:

## 11. Privacy and Safety

- Personal Data:
- Retention:
- Access:
- High-Risk Conditions:

## 12. Owner and Review

- Owner:
- Review Date:
- Open Questions:
```

---

## 31. 典型流程示例：奖励领取

### 31.1 系统

- Objectives；
- Reward；
- Economy；
- Progression；
- Save；
- Analytics。

### 31.2 流程

```text
QuestCompleted
→ Reward 创建 RewardInstance
→ 玩家发送 ClaimReward
→ Reward 验证资格
→ Reward 使用 RewardInstanceID 调用 Economy
→ Economy 幂等发放资源
→ Reward 标记 Claimed
→ Reward 发布 RewardClaimed
→ Progression 根据资源变化更新可升级状态
→ Save 持久化
→ Analytics 记录
```

### 31.3 不变量

- 同一 RewardInstance 只能领取一次；
- Economy 发放结果可重复查询；
- Analytics 失败不影响领取；
- UI 不直接修改领取状态。

### 31.4 故障

如果 Analytics 失败：

```text
领取继续成功。
```

如果 Economy 超时：

```text
Reward 保持 Pending；
使用相同幂等键查询或重试；
不允许再次创建新奖励实例。
```

---

## 32. 典型流程示例：购买与权益

### 32.1 系统

- Offers；
- Payment Platform；
- Purchase；
- Entitlement；
- Content；
- Audit。

### 32.2 流程

```text
PurchaseInitiated
→ Payment Pending
→ Payment Platform Confirmed
→ Purchase 验证交易
→ Entitlement 幂等授予
→ Content 读取权益
→ PurchaseCompleted
```

### 32.3 不变量

- 客户端成功不等于支付成功；
- 同一交易只能授予一次权益；
- 权益状态由 Entitlement 拥有；
- 退款和撤销必须产生对应状态；
- 购买记录长期审计。

### 32.4 Fail Closed

无法确认支付时：

```text
保持 Pending，
不授予永久权益，
支持恢复购买和对账。
```

---

## 33. 典型流程示例：活动结束

### 33.1 系统

- Time；
- Live Operations；
- Objectives；
- Reward；
- Economy；
- Content；
- Notification。

### 33.2 需要定义

- 已开始任务；
- 已完成未领取；
- 活动货币；
- 排名；
- 商店；
- 购买权益；
- 返场；
- 通知。

### 33.3 推荐原则

活动结束不应由每个系统独立判断。

应由：

```text
权威 Time
+
活动生命周期状态
```

统一驱动。

### 33.4 事件

```text
EventEnteringGracePeriod
EventEnded
EventRewardsFinalized
EventArchived
```

---

## 34. 典型流程示例：存档同步

### 34.1 系统

- Local Save；
- Cloud Save；
- Account；
- Versioning；
- Game State。

### 34.2 冲突

本地和云端同时修改。

### 34.3 处理

应比较：

- Save Version；
- Last Modified；
- Session ID；
- Progress Summary。

高影响冲突应让玩家：

- 看到两份摘要；
- 选择；
- 保留备份；
- 支持恢复。

不应只用时间戳静默覆盖所有情况。

---

## 35. 评审问题

### 所有权

```text
每项状态是否有唯一 Owner？
是否有系统绕过 Owner 直接写入？
```

### 命令与事件

```text
Command 是否表达意图？
Event 是否描述事实？
Query 是否无隐藏写入？
```

### 正确性

```text
重复请求是否安全？
部分成功如何恢复？
是否有事务 ID 和幂等键？
```

### 事件

```text
是否考虑重复、乱序、延迟和版本？
消费者是否依赖隐式顺序？
```

### 配置

```text
配置能否绕过核心不变量？
是否有 Last Known Good？
是否能快速回滚？
```

### 故障

```text
非关键依赖失败是否阻断核心体验？
高风险操作是否 Fail Closed？
```

### 数据

```text
是否传输了不必要敏感数据？
删除请求是否能传播到全部系统？
```

### 版本

```text
旧消费者如何处理新事件？
旧客户端如何处理新状态？
```

### 例外

```text
跨系统例外是否有到期和移除计划？
```

---

## 36. 检查清单

### Ownership

- [ ] 每项权威状态只有一个 Owner；
- [ ] 修改通过 Owner 的 Command；
- [ ] 派生状态有失效规则；
- [ ] 不存在共享写入。

### Commands and Queries

- [ ] Command、Query、Event 职责分离；
- [ ] Query 不产生隐藏修改；
- [ ] Command 有明确成功和失败；
- [ ] 高风险 Command 支持幂等。

### Transactions

- [ ] 事务边界明确；
- [ ] 部分成功路径明确；
- [ ] 补偿策略明确；
- [ ] 事务 ID 可以追踪；
- [ ] 不重复发放或扣除。

### Events

- [ ] 事件命名描述事实；
- [ ] 权威发布者明确；
- [ ] 至少一次交付下消费者可幂等；
- [ ] 乱序和版本已考虑；
- [ ] 事件不包含不必要敏感数据。

### Configuration

- [ ] 配置有 Owner、Schema 和版本；
- [ ] 配置有默认值和范围；
- [ ] Last Known Good 可用；
- [ ] 高风险不变量不可被覆盖；
- [ ] Feature Flag 有删除计划。

### Failure

- [ ] 超时明确；
- [ ] 重试条件明确；
- [ ] 非关键系统可降级；
- [ ] 高风险系统 Fail Closed；
- [ ] 玩家能理解 Pending 和失败状态。

### External Systems

- [ ] 外部回调可重复处理；
- [ ] 外部状态不是未经验证的内部权威；
- [ ] 有查询、对账和恢复路径；
- [ ] 支付与权益分离。

### Data and Privacy

- [ ] 数据最小化；
- [ ] 访问权限明确；
- [ ] 保留期限明确；
- [ ] 删除传播明确；
- [ ] 第三方共享明确。

### Versioning

- [ ] API、Event、Data、Configuration 有版本；
- [ ] Breaking Change 有迁移计划；
- [ ] 旧客户端兼容范围明确；
- [ ] 迁移可重复、可恢复、可审计。

### Exceptions

- [ ] 例外有明确范围；
- [ ] 例外有 Owner；
- [ ] 例外有到期时间；
- [ ] 例外有移除或正式化计划。

---

## 37. V1 Completion Criteria

Integration Rules 可以被视为 V1，当：

- Command、Query、Event、Configuration 的职责明确；
- 所有关键状态遵循唯一 Owner；
- 跨系统写入通过正式契约；
- 高影响命令具备幂等设计；
- 跨系统事务具有部分失败和补偿方案；
- 事件命名、字段、交付语义、顺序和版本规则明确；
- Query 一致性要求得到区分；
- 配置、Feature Flag 和实验覆盖层有统一优先级；
- Live Operations 不直接绕过领域状态；
- 外部支付、平台和第三方回调具有验证与对账；
- 故障隔离、降级、超时和重试规则明确；
- 高风险操作使用 Fail Closed；
- 审计、隐私、删除传播和数据保留得到定义；
- API、数据、事件、配置和客户端兼容策略存在；
- Migration 可重复、可恢复并可审计；
- 跨系统例外有记录、Owner、到期和清理计划；
- 下游系统文档能够直接引用本规则。

---

## 38. Related Documents

### Philosophy

- [Design Principles](../philosophy/foundation/design-principles.md)
- [Clarity and Feedback](../philosophy/experience/clarity-and-feedback.md)
- [Consistency and Coherence](../philosophy/long-term/consistency-and-coherence.md)
- [Ethical Design](../philosophy/responsibility/ethical-design.md)
- [Iteration and Validation](../philosophy/validation/iteration-and-validation.md)

### Systems

- [Systems README](./README.md)
- [System Design Framework](./system-design-framework.md)
- [System Map](./system-map.md)
- `core/game-state-and-flow.md`
- `core/rules-and-resolution.md`
- `player/save-and-persistence.md`
- `commercial/entitlement-and-ownership.md`
- `operations/versioning-and-migration.md`
