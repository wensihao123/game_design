# Systems Design（系统设计）

> Status: V1  
> Path: `design/systems/README.md`  
> Scope: 通用的软件、游戏与交互产品系统设计  
> Purpose: 定义 Systems 层的职责、目录结构、文档标准、系统关系和维护方式，使上层理念可以被转化为可执行、可验证、可实现的系统规则。

---

## 1. Systems 层是什么

Systems 层负责描述产品如何运转。

它回答：

```text
玩家如何进入系统？
系统拥有哪些状态？
状态如何变化？
规则如何结算？
资源如何流动？
系统如何与其他系统交互？
失败、中断和异常如何处理？
系统如何被验证？
```

Systems 不负责重新定义设计理念。

Systems 应当把 Philosophy 中的原则转化为：

- 系统目标；
- 状态模型；
- 规则；
- 输入；
- 输出；
- 资源关系；
- 反馈要求；
- 边界条件；
- 例外；
- 验证标准。

---

## 2. Philosophy、Systems 与 Specifications

项目设计文档分成三个主要层级。

```text
Philosophy
为什么这样设计
        ↓
Systems
产品如何运转
        ↓
Specifications
具体功能如何实现
```

### 2.1 Philosophy

回答：

- 我们相信什么；
- 首先保护什么；
- 如何判断好坏；
- 原则冲突时如何选择。

示例：

```text
高影响操作应可理解并可恢复。
```

### 2.2 Systems

回答：

- 哪些操作属于高影响；
- 需要什么确认；
- 是否允许撤销；
- 状态如何保存；
- 恢复路径是什么。

示例：

```text
稀有装备分解前显示材料结果；
已锁定物品不可分解；
分解后进入可回购状态 24 小时。
```

### 2.3 Specifications

回答：

- 页面布局；
- 字段；
- 按钮；
- 接口；
- 数据结构；
- 动画；
- 错误码；
- 验收用例。

示例：

```text
分解确认弹窗展示物品名称、品质、材料产出和不可逆提示。
```

---

## 3. Systems 层的职责

Systems 层主要承担九项职责。

### 3.1 定义系统目的

每个系统必须说明：

- 为什么存在；
- 服务什么玩家目标；
- 支持什么核心体验；
- 不负责解决什么问题。

### 3.2 定义系统边界

明确：

- 输入；
- 输出；
- 状态；
- 依赖；
- 下游影响；
- 非职责范围。

### 3.3 定义状态与流程

包括：

- 初始状态；
- 可用状态；
- 活跃状态；
- 完成状态；
- 失败状态；
- 中断状态；
- 归档状态；
- 状态转换条件。

### 3.4 定义规则

包括：

- 基础规则；
- 优先级；
- 结算顺序；
- 条件；
- 例外；
- 冲突处理。

### 3.5 定义资源流

包括：

- 资源来源；
- 资源用途；
- 产出速度；
- 消耗速度；
- 上限；
- 转换；
- 回收；
- 通胀与枯竭风险。

### 3.6 定义玩家反馈

包括：

- 必须可见的状态；
- 输入确认；
- 结果；
- 失败原因；
- 风险预警；
- 中断与恢复。

### 3.7 定义异常与边界情境

包括：

- 断线；
- 重复请求；
- 数据冲突；
- 容量不足；
- 内容过期；
- 版本变化；
- 误操作；
- 跨设备同步。

### 3.8 定义验证方式

包括：

- 关键假设；
- 成功标准；
- 失败标准；
- 定性测试；
- 定量指标；
- 负面指标；
- 长期风险。

### 3.9 定义系统关系

防止系统成为孤岛。

每个系统必须说明：

- 依赖谁；
- 向谁提供输出；
- 与谁共享状态；
- 与谁争夺资源；
- 与谁存在冲突。

---

## 4. 目录结构

建议使用以下英文目录结构：

```text
design/
├─ philosophy/
│  └─ ...
│
└─ systems/
   ├─ README.md
   ├─ system-design-framework.md
   ├─ system-map.md
   ├─ integration-rules.md
   │
   ├─ core/
   │  ├─ core-loop.md
   │  ├─ game-state-and-flow.md
   │  ├─ rules-and-resolution.md
   │  └─ input-and-interaction.md
   │
   ├─ player/
   │  ├─ tutorial-and-onboarding.md
   │  ├─ save-and-persistence.md
   │  ├─ settings-and-preferences.md
   │  └─ notification-and-reminders.md
   │
   ├─ progression/
   │  ├─ resources-and-economy.md
   │  ├─ progression-system.md
   │  ├─ reward-system.md
   │  └─ difficulty-and-challenge.md
   │
   ├─ content/
   │  ├─ content-and-unlocks.md
   │  ├─ objectives-and-quests.md
   │  ├─ characters-and-loadouts.md
   │  └─ content-lifecycle.md
   │
   ├─ social/
   │  ├─ social-and-multiplayer.md
   │  ├─ matchmaking-and-competition.md
   │  └─ moderation-and-safety.md
   │
   ├─ commercial/
   │  ├─ monetization-system.md
   │  ├─ offers-and-pricing.md
   │  └─ entitlement-and-ownership.md
   │
   └─ operations/
      ├─ analytics-and-telemetry.md
      ├─ live-operations.md
      ├─ experiment-management.md
      └─ versioning-and-migration.md
```

不是所有项目都需要全部文件。

目录应根据项目范围裁剪，但不建议随意改变每个文件的职责。

---

## 5. 顶层文件职责

### 5.1 `README.md`

负责：

- Systems 层定义；
- 目录结构；
- 阅读顺序；
- 文档标准；
- 维护规则；
- Philosophy 连接方式。

### 5.2 `system-design-framework.md`

负责定义所有系统文档共用的结构：

- System Purpose；
- Boundary；
- State Model；
- Rules；
- Data；
- Feedback；
- Edge Cases；
- Validation；
- Rollout；
- Maintenance。

### 5.3 `system-map.md`

负责展示：

- 系统全景；
- 系统依赖；
- 资源流；
- 状态共享；
- 关键循环；
- 风险集中点。

### 5.4 `integration-rules.md`

负责定义跨系统规则：

- 系统依赖方向；
- 状态所有权；
- 事件与数据边界；
- 资源职责；
- 结算顺序；
- 例外；
- 向后兼容；
- 故障隔离。

---

## 6. Core Systems

`core/` 定义所有其他系统所依赖的核心运转结构。

### 6.1 `core-loop.md`

回答：

```text
玩家反复做什么？
每一轮如何开始、行动、获得结果并进入下一轮？
```

包含：

- 核心循环；
- 会话循环；
- 长期循环；
- 循环入口；
- 循环出口；
- 动机；
- 反馈；
- 奖励；
- 恢复；
- 失败。

### 6.2 `game-state-and-flow.md`

回答：

```text
产品当前处于什么状态？
不同页面、模式和流程如何切换？
```

包含：

- 全局状态；
- 会话状态；
- 模式状态；
- 页面流；
- 中断；
- 恢复；
- 暂停；
- 后台；
- 断线。

### 6.3 `rules-and-resolution.md`

回答：

```text
规则以什么顺序执行？
多个效果冲突时如何结算？
```

包含：

- 规则层级；
- 结算顺序；
- 优先级；
- 同时事件；
- 随机性；
- 舍入；
- 例外；
- 可重现性。

### 6.4 `input-and-interaction.md`

回答：

```text
玩家如何表达意图？
系统如何确认并执行？
```

包含：

- 输入抽象；
- 操作映射；
- 平台适配；
- 输入缓冲；
- 误触保护；
- 焦点；
- 可访问性；
- 反馈；
- 取消与撤销。

---

## 7. Player Systems

`player/` 定义玩家进入、设置、保存、学习和返回体验的基础支持。

### 7.1 `tutorial-and-onboarding.md`

包含：

- 首次体验；
- 教学节奏；
- 功能解锁；
- 实际练习；
- 可跳过；
- 可回看；
- 熟练玩家路径。

### 7.2 `save-and-persistence.md`

包含：

- 保存粒度；
- 自动保存；
- 云存档；
- 冲突；
- 回滚；
- 备份；
- 离线；
- 跨设备；
- 删除；
- 迁移。

### 7.3 `settings-and-preferences.md`

包含：

- 输入；
- 显示；
- 音频；
- 可访问性；
- 隐私；
- 通知；
- 难度；
- 同步；
- 默认值。

### 7.4 `notification-and-reminders.md`

包含：

- 通知类型；
- 触发条件；
- 紧急程度；
- 频率；
- 静默时段；
- 玩家控制；
- 文案；
- 伦理边界。

---

## 8. Progression Systems

`progression/` 定义长期成长、资源和挑战结构。

### 8.1 `resources-and-economy.md`

包含：

- 资源分类；
- 来源；
- 消耗；
- 转换；
- 上限；
- 稀缺；
- 通胀；
- 回收；
- 付费边界。

### 8.2 `progression-system.md`

包含：

- 玩家成长；
- 角色成长；
- 能力成长；
- 横向成长；
- 纵向成长；
- 里程碑；
- 重置；
- 追赶；
- 长期上限。

### 8.3 `reward-system.md`

包含：

- 奖励类型；
- 奖励职责；
- 奖励节奏；
- 随机奖励；
- 保底；
- 重复补偿；
- 奖励展示；
- 领取；
- 过期。

### 8.4 `difficulty-and-challenge.md`

包含：

- 挑战来源；
- 难度维度；
- 难度曲线；
- 模式；
- 辅助；
- 自适应；
- 失败；
- 惩罚；
- 反制；
- 公平。

---

## 9. Content Systems

`content/` 定义内容如何进入系统、被解锁、被消费和被维护。

### 9.1 `content-and-unlocks.md`

包含：

- 内容分类；
- 解锁条件；
- 可见性；
- 依赖；
- 内容门槛；
- 过期；
- 返场；
- 永久内容；
- 限时内容。

### 9.2 `objectives-and-quests.md`

包含：

- 目标类型；
- 条件；
- 进度；
- 失败；
- 放弃；
- 重试；
- 奖励；
- 分支；
- 追踪；
- 验证。

### 9.3 `characters-and-loadouts.md`

包含：

- 角色实体；
- 队伍；
- 装备；
- 技能；
- 位置；
- 构筑；
- 推荐；
- 预设；
- 切换；
- 有效性。

### 9.4 `content-lifecycle.md`

包含：

- 创建；
- 测试；
- 发布；
- 版本；
- 下架；
- 归档；
- 返场；
- 迁移；
- 数据保留。

---

## 10. Social Systems

`social/` 仅在存在多人、社区或用户生成内容时启用。

### 10.1 `social-and-multiplayer.md`

包含：

- 好友；
- 组队；
- 邀请；
- 协作；
- 通信；
- 退出；
- 缺席；
- 贡献；
- 隐私。

### 10.2 `matchmaking-and-competition.md`

包含：

- 匹配；
- 评分；
- 延迟；
- 队伍结构；
- 排名；
- 赛季；
- 断线；
- 作弊；
- 公平。

### 10.3 `moderation-and-safety.md`

包含：

- 举报；
- 屏蔽；
- 审核；
- 处罚；
- 申诉；
- 用户生成内容；
- 儿童保护；
- 骚扰；
- 隐私。

---

## 11. Commercial Systems

`commercial/` 定义商业价值交换。

该目录必须直接引用：

- Ethical Design；
- Player First Design；
- Progression and Motivation；
- Accessibility and Inclusivity。

### 11.1 `monetization-system.md`

包含：

- 商业模式；
- 付费对象；
- 免费体验边界；
- 价值交换；
- 付费公平；
- 随机机制；
- 消费保护。

### 11.2 `offers-and-pricing.md`

包含：

- 真实价格；
- 虚拟货币；
- 套餐；
- 折扣；
- 订阅；
- 地区价格；
- 限时；
- 取消；
- 退款。

### 11.3 `entitlement-and-ownership.md`

包含：

- 购买所有权；
- 账户资产；
- 平台权益；
- 恢复购买；
- 退款；
- 到期；
- 迁移；
- 撤销；
- 数据一致性。

---

## 12. Operations Systems

`operations/` 定义系统发布后的观察、实验和长期维护。

### 12.1 `analytics-and-telemetry.md`

包含：

- 事件；
- 状态；
- 指标；
- 数据最小化；
- 隐私；
- 分群；
- 数据质量；
- 留存周期；
- 使用权限。

### 12.2 `live-operations.md`

包含：

- 活动；
- 内容排期；
- 配置；
- 发布；
- 回滚；
- 补偿；
- 通知；
- 返场；
- 运营边界。

### 12.3 `experiment-management.md`

包含：

- 假设；
- 实验组；
- 暴露；
- 指标；
- 伦理；
- 停止条件；
- 分析；
- 归档。

### 12.4 `versioning-and-migration.md`

包含：

- 数据版本；
- 系统版本；
- 向后兼容；
- 配置迁移；
- 存档迁移；
- 内容替换；
- 回退；
- 弃用。

---

## 13. 推荐编写顺序

Systems 不应按页面顺序编写。

建议按依赖关系推进。

### Phase 1：建立方法和全景

```text
1. system-design-framework.md
2. system-map.md
3. integration-rules.md
```

### Phase 2：确定核心运转

```text
4. core/core-loop.md
5. core/game-state-and-flow.md
6. core/rules-and-resolution.md
7. core/input-and-interaction.md
```

### Phase 3：确定长期循环

```text
8. progression/resources-and-economy.md
9. progression/progression-system.md
10. progression/reward-system.md
11. progression/difficulty-and-challenge.md
```

### Phase 4：确定内容承载

```text
12. content/content-and-unlocks.md
13. content/objectives-and-quests.md
14. content/characters-and-loadouts.md
15. content/content-lifecycle.md
```

### Phase 5：补齐玩家支持

```text
16. player/tutorial-and-onboarding.md
17. player/save-and-persistence.md
18. player/settings-and-preferences.md
19. player/notification-and-reminders.md
```

### Phase 6：按项目启用扩展系统

```text
20. social/*
21. commercial/*
22. operations/*
```

---

## 14. 系统文档统一结构

每份系统文档应尽量使用以下结构。

```text
1. Metadata
2. System Summary
3. Purpose
4. Non-Goals
5. Governing Principles
6. Player Experience
7. System Boundary
8. Entities and State
9. State Transitions
10. Rules
11. Inputs and Outputs
12. Resource Flow
13. Feedback Requirements
14. Failure and Recovery
15. Edge Cases
16. Cross-System Dependencies
17. Data and Persistence
18. Accessibility
19. Ethical and Safety Review
20. Analytics and Validation
21. Rollout and Migration
22. Risks and Open Questions
23. Review Checklist
24. V1 Completion Criteria
25. Related Documents
```

不是所有系统都需要完整资源、商业或多人章节。

不适用时应写：

```text
Not Applicable
```

而不是删除结构后让读者无法判断是否遗漏。

---

## 15. 系统元数据标准

每份系统文档头部建议包含：

```markdown
> Status: Draft / Review / V1 / Deprecated  
> Category: Core / Player / Progression / Content / Social / Commercial / Operations  
> Owner:  
> Reviewers:  
> Last Updated:  
> Dependencies:  
> Affected Systems:  
> Risk Level: Low / Medium / High / Critical  
```

### 15.1 Status

| Status | 含义 |
|---|---|
| Draft | 正在形成方案 |
| Review | 等待跨职能评审 |
| V1 | 可以支持实现与测试 |
| Live | 已经上线并持续维护 |
| Deprecated | 不再用于新设计 |
| Archived | 只保留历史参考 |

### 15.2 Risk Level

| Risk | 示例 |
|---|---|
| Low | 文案、低影响排序 |
| Medium | 常规成长、内容解锁 |
| High | 存档、经济、多人公平 |
| Critical | 付费、隐私、儿童、资产所有权 |

---

## 16. Governing Principles

每份系统文档必须明确引用上层理念。

示例：

```markdown
## Governing Principles

- [Player First Design](../philosophy/foundation/player-first-design.md)
  - 高频流程应减少无价值操作；
  - 高影响错误应可恢复。

- [Clarity and Feedback](../philosophy/experience/clarity-and-feedback.md)
  - 状态来源必须可追踪；
  - 不可用操作必须说明原因。

- [Ethical Design](../philosophy/responsibility/ethical-design.md)
  - 不通过人为摩擦出售基础修复。
```

不能只写理念文件名称。

必须说明当前系统实际遵循的具体原则。

---

## 17. System Purpose 与 Non-Goals

### 17.1 Purpose

必须说明系统提供的玩家价值。

不推荐：

```text
该系统用于管理装备。
```

更好：

```text
该系统帮助玩家根据当前目标比较、装备和保存构筑，
使队伍准备成为可理解、可调整且有后果的决策过程。
```

### 17.2 Non-Goals

明确系统不负责什么。

例如：

```text
装备系统不负责：
- 决定完整掉落经济；
- 定义角色升级；
- 处理商城所有权；
- 自动替玩家选择唯一最优构筑。
```

Non-Goals 可以防止系统职责不断扩张。

---

## 18. System Boundary

每个系统应定义：

```markdown
## System Boundary

### Inputs
- ...

### Outputs
- ...

### Owned State
- ...

### Read-Only Dependencies
- ...

### Write Dependencies
- ...

### Out of Scope
- ...
```

### 18.1 Owned State

系统必须有明确状态所有权。

例如：

```text
装备系统拥有：
- 当前装备关系；
- 装备锁定状态；
- 装备预设。

角色系统拥有：
- 角色基础属性；
- 角色等级；
- 角色身份。
```

避免多个系统同时修改同一事实。

---

## 19. 状态模型

状态模型应回答：

- 现在是什么；
- 如何进入；
- 如何退出；
- 是否可逆；
- 中断后怎么办。

示例：

```text
Locked
→ Available
→ Active
→ Completed
→ Claimed
→ Archived
```

每个转换需要：

- 触发者；
- 条件；
- 输入；
- 输出；
- 失败方式；
- 保存方式；
- 是否幂等。

---

## 20. 规则层级

系统规则建议分为：

### 20.1 Invariant

任何情况下都必须成立。

例如：

```text
已消费资源不能在同一事务中重复消费。
```

### 20.2 Core Rule

系统主要运转规则。

### 20.3 Parameter

可以调整的数值或配置。

### 20.4 Exception

特定范围内偏离基础规则的处理。

### 20.5 Temporary Override

活动、修复或运营期间的临时覆盖。

临时覆盖必须具有：

- Owner；
- 开始时间；
- 结束时间；
- 回退；
- 影响范围。

---

## 21. 资源流标准

涉及资源的系统必须记录：

```markdown
| Resource | Source | Sink | Cap | Conversion | Expiry | Owner |
|---|---|---|---|---|---|---|
```

必须检查：

- 只有来源没有用途；
- 只有用途没有稳定来源；
- 同一资源用途过多；
- 多种资源职责重复；
- 付费与免费路径不一致；
- 资源过期造成压力；
- 长期通胀；
- 新玩家无法追赶。

---

## 22. 反馈要求

Systems 文档不需要决定最终视觉样式，但必须说明信息需求。

例如：

```text
必须表达：
- 当前状态；
- 状态来源；
- 可用操作；
- 不可用原因；
- 预计结果；
- 风险；
- 实际结果；
- 是否保存成功。
```

反馈要求应区分：

- 即时反馈；
- 汇总反馈；
- 高风险确认；
- 错误反馈；
- 长期记录。

---

## 23. 失败与恢复

每个系统必须考虑：

### 23.1 Player Failure

玩家未达到目标。

### 23.2 Input Error

玩家误触、误解或输入错误。

### 23.3 System Failure

网络、服务、数据或客户端错误。

### 23.4 Partial Failure

部分操作成功，部分失败。

### 23.5 Recovery

包括：

- 重试；
- 撤销；
- 回滚；
- 恢复购买；
- 重新同步；
- 补偿；
- 客服路径。

高影响系统必须避免让玩家承担无法理解的资产损失。

---

## 24. Edge Cases

至少检查：

- 重复点击；
- 重复请求；
- 页面快速切换；
- 断线；
- 后台恢复；
- 跨日；
- 跨时区；
- 容量不足；
- 数据冲突；
- 内容过期；
- 版本更新；
- 多设备；
- 多语言；
- 大字体；
- 低性能；
- 权限拒绝；
- 支付失败；
- 退款。

---

## 25. 跨系统依赖

依赖应区分：

### 25.1 Hard Dependency

缺失时系统无法运行。

### 25.2 Soft Dependency

缺失时体验退化，但仍可运行。

### 25.3 Event Dependency

通过事件接收变化。

### 25.4 Data Dependency

读取其他系统数据。

### 25.5 Write Dependency

需要修改其他系统状态。

写依赖应尽量少于读依赖。

---

## 26. 系统集成原则

### 26.1 Single Source of Truth

每项关键状态只有一个主拥有系统。

### 26.2 Explicit Contracts

系统之间通过明确输入、输出和事件交互。

### 26.3 Idempotency

重复执行同一事务不应重复产生结果。

### 26.4 Failure Isolation

一个系统失败不应无边界破坏其他系统。

### 26.5 Observable State

重要状态变化可被记录、调试和解释。

### 26.6 Backward Compatibility

长期数据和存档需要考虑旧版本兼容。

### 26.7 Reversible Rollout

高风险系统应支持逐步发布与快速回退。

---

## 27. 数据与持久化

设计层不需要定义完整数据库结构，但必须说明：

- 哪些状态需要保存；
- 保存粒度；
- 保存时机；
- 本地还是服务端；
- 数据冲突；
- 数据版本；
- 删除；
- 隐私；
- 保留期限；
- 审计要求。

示例：

```markdown
| State | Persistence | Authority | Save Trigger | Recovery |
|---|---|---|---|---|
```

---

## 28. 可访问性检查

每个系统都应检查：

- 是否依赖颜色；
- 是否依赖声音；
- 是否要求精确输入；
- 是否要求持续按住；
- 是否支持重绑定；
- 是否支持字幕；
- 是否支持放大文本；
- 是否可以暂停；
- 是否可以减速；
- 是否存在认知负担；
- 是否允许辅助。

详见：

- [Accessibility and Inclusivity](../philosophy/responsibility/accessibility-and-inclusivity.md)

---

## 29. 伦理与安全检查

涉及以下内容时必须增加专项评审：

- 付费；
- 概率；
- 儿童；
- 隐私；
- 通知；
- 社交；
- 排名；
- 用户生成内容；
- 永久损失；
- 自动续费；
- 限时；
- FOMO。

详见：

- [Ethical Design](../philosophy/responsibility/ethical-design.md)
- [Conflict Resolution](../philosophy/conflict-resolution.md)

---

## 30. 分析与验证

每个系统必须定义：

### 30.1 Key Assumptions

系统依赖哪些尚未被完全证明的判断。

### 30.2 Success Criteria

什么结果表示系统支持了预期体验。

### 30.3 Failure Criteria

什么结果表示系统不成立或风险不可接受。

### 30.4 Behavioral Metrics

玩家做了什么。

### 30.5 Outcome Metrics

玩家是否达到目标。

### 30.6 Negative Metrics

是否造成：

- 误操作；
- 放弃；
- 投诉；
- 退款；
- 压力；
- 不公平；
- 数据损失。

### 30.7 Qualitative Validation

通过：

- 观察；
- 访谈；
- 复述；
- 长期日记；
- 回顾；

解释行为原因。

---

## 31. Rollout 与 Migration

高风险系统需要定义：

- 内部测试；
- 小范围发布；
- 分群；
- 配置开关；
- 数据迁移；
- 回滚；
- 补偿；
- 旧版本兼容；
- 停止条件；
- 全量条件。

不能仅写：

```text
上线后观察。
```

必须提前定义观察什么、何时停止和如何回退。

---

## 32. 系统文档成熟度

### Level 0：Concept

只有问题、目标和初步方向。

### Level 1：Model

已有：

- 边界；
- 状态；
- 规则；
- 依赖。

### Level 2：Implementable

已有：

- 完整流程；
- 异常；
- 数据；
- 反馈；
- 验证；
- 边界情境。

### Level 3：Validated

关键假设已通过原型或测试。

### Level 4：Live

已上线并具有监控、版本和维护记录。

---

## 33. 系统评审顺序

建议按以下顺序评审：

```text
1. 系统是否真的需要存在
2. 是否支持核心体验
3. 边界是否清晰
4. 状态与规则是否完整
5. 是否可理解、可控制、可恢复
6. 跨系统关系是否健康
7. 长期资源和内容是否可持续
8. 可访问性是否成立
9. 伦理、安全和隐私是否通过
10. 是否可以验证、发布和回退
```

不要先讨论：

- 按钮颜色；
- 动画；
- 文案细节；
- 实现类名；

而忽略系统目的和规则。

---

## 34. 系统文档检查清单

### Purpose

- [ ] 系统目的清晰；
- [ ] 玩家价值明确；
- [ ] 与核心体验有关；
- [ ] Non-Goals 已定义。

### Boundary

- [ ] 输入和输出明确；
- [ ] 状态所有权明确；
- [ ] 依赖明确；
- [ ] 无重复职责。

### State and Rules

- [ ] 状态完整；
- [ ] 转换条件明确；
- [ ] 规则优先级明确；
- [ ] 例外有限且有记录；
- [ ] 重复执行安全。

### Player Experience

- [ ] 入口清楚；
- [ ] 当前状态可见；
- [ ] 行动后果可理解；
- [ ] 错误可恢复；
- [ ] 存在合理退出。

### Cross-System

- [ ] 资源流闭合；
- [ ] 依赖方向合理；
- [ ] 写依赖受到控制；
- [ ] 故障不会无限扩散；
- [ ] 数据来源唯一。

### Responsibility

- [ ] 可访问性已检查；
- [ ] 伦理与安全已检查；
- [ ] 隐私最小化；
- [ ] 儿童和脆弱用户风险已处理。

### Validation

- [ ] 假设明确；
- [ ] 成功标准明确；
- [ ] 失败标准明确；
- [ ] 负面指标明确；
- [ ] 发布和回退方式明确。

---

## 35. V1 完成标准

Systems 目录可以被视为 V1 建立完成，当：

- Systems 层职责与 Philosophy、Specifications 清楚区分；
- 目录结构已经确定；
- `system-design-framework.md` 已建立；
- `system-map.md` 已建立；
- `integration-rules.md` 已建立；
- 核心循环、状态、规则和输入系统已完成；
- 关键资源、成长、奖励和难度系统已完成；
- 系统之间的依赖与状态所有权明确；
- 每个系统引用具体 Governing Principles；
- 高风险系统包含可访问性、伦理和隐私评审；
- 每个系统具有成功、失败和回退标准；
- 目录可以根据项目范围裁剪；
- 下游功能规格可以直接引用系统规则。

---

## 36. Related Documents

### Philosophy

- [Philosophy README](../philosophy/README.md)
- [Glossary](../philosophy/glossary.md)
- [Principle Map](../philosophy/principle-map.md)
- [Conflict Resolution](../philosophy/conflict-resolution.md)

### Systems

- `system-design-framework.md`
- `system-map.md`
- `integration-rules.md`

### Validation

- [Iteration and Validation](../philosophy/validation/iteration-and-validation.md)
