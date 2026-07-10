# Release Workflow（发布流程）

> 状态：草案  
> 适用范围：通用的软件与游戏开发项目  
> 目的：定义一个版本从发布规划、范围确认、构建、验证、部署、监控到关闭的完整流程。

---

## 1. 文档目的

Release Workflow 用于确保已经完成的功能、缺陷修复、内容、资产和技术改动，能够以可控、可验证、可回滚的方式交付到目标环境。

它回答以下问题：

- 什么内容可以进入一个 Release；
- Release Scope 如何确认；
- 什么时候进行 Feature Freeze；
- Release Candidate 如何生成；
- 发布前需要完成哪些测试；
- 数据迁移如何处理；
- Feature Flag 如何使用；
- 灰度发布如何执行；
- 什么时候应该停止发布；
- 什么时候应该回滚；
- 发布后如何做 Smoke Test 和监控；
- 版本何时可以关闭；
- Hotfix 与普通 Release 有什么不同。

Release Workflow 的核心目标不是尽快“把版本发出去”。

它的核心目标是：

> 将已经完成的工作安全地交付到目标用户环境，并在出现问题时能够快速发现、控制和恢复。

---

## 2. Release 的定义

Release 是一组经过确认、构建、验证并交付到目标环境的变更集合。

Release 可以包含：

- Feature；
- Bugfix；
- Technical Work；
- Content；
- Asset；
- Configuration；
- Data Migration；
- Documentation；
- Dependency Update；
- Security Fix。

Released 表示：

> 目标用户或目标环境已经可以实际使用该版本中的内容。

Released 不等于：

- 已合并到主分支；
- 已完成开发；
- 已通过 Code Review；
- 已通过 Done Gate；
- 已生成本地构建；
- 已部署到测试环境。

---

## 3. Release 与 Done 的区别

```text
Done = 功能达到可发布质量
Released = 功能已进入目标用户环境
```

一个功能可以已经 Done，但仍等待：

- 版本窗口；
- 回归测试；
- 平台审核；
- 商店审核；
- 部署审批；
- 其他依赖；
- 数据迁移；
- 发布协调。

Release Workflow 从 Done 之后开始。

---

## 4. Release 生命周期总览

```text
RELEASE PLANNING
    ↓
SCOPE CONFIRMATION
    ↓
FEATURE FREEZE
    ↓
RELEASE CANDIDATE BUILD
    ↓
REGRESSION TESTING
    ↓
MIGRATION & CONFIGURATION CHECK
    ↓
RELEASE APPROVAL
    ↓
DEPLOY / PUBLISH
    ↓
SMOKE TEST
    ↓
MONITOR
    ↓
CLOSE
```

可能的回退路径：

```text
Regression → Fix → New Release Candidate
Approval → Scope Change → Rebuild
Deploy → Smoke Test Fail → Rollback
Monitor → Incident → Hotfix
```

---

# 5. Release Planning：发布规划

## 5.1 目标

明确：

- 这次发布为什么存在；
- 发布目标是什么；
- 目标用户是谁；
- 包含哪些内容；
- 何时发布；
- 发布到哪里；
- 风险是什么；
- 谁负责。

## 5.2 Release Planning 应包含

```text
Release Name
Version
Target Date
Target Environment
Scope
Non-goals
Owners
Risk Level
Release Type
Testing Strategy
Migration Plan
Rollback Plan
Communication Plan
```

## 5.3 发布目标

一个 Release 应有明确目标。

不合格：

```text
把最近完成的内容一起发掉
```

合格：

```text
发布新的预设管理能力，并修复旧版本数据迁移问题。
```

## 5.4 发布类型

建议区分：

```text
Major Release
Minor Release
Patch Release
Hotfix
Internal Release
Alpha
Beta
General Availability
```

---

# 6. Release Scope：发布范围

## 6.1 目标

明确本次版本包含和不包含什么。

## 6.2 Scope 内容

- Feature；
- Bugfix；
- Migration；
- Configuration；
- Content；
- Asset；
- Dependency；
- Documentation；
- Known Limitations。

## 6.3 Scope 原则

一个工作项进入 Release，应满足：

- 已通过 Done Gate；
- 有明确 Owner；
- 有验证证据；
- 不存在发布阻塞；
- 依赖已准备；
- 数据影响已处理；
- 发布后验证方式明确。

## 6.4 Scope Freeze 前允许的变化

- 正常完成目标范围；
- 修复阻塞缺陷；
- 更新文档；
- 更新测试；
- 补充发布配置。

## 6.5 Scope Freeze 后不应加入

- 新 Feature；
- 大型重构；
- 非关键优化；
- 未评估依赖升级；
- 未完成设计的新需求；
- 无关代码清理；
- 大量内容扩展。

---

# 7. Versioning：版本管理

## 7.1 推荐格式

可使用语义化版本：

```text
MAJOR.MINOR.PATCH
```

例如：

```text
2.4.1
```

含义：

- MAJOR：不兼容变化或大型版本；
- MINOR：向后兼容的新功能；
- PATCH：向后兼容的问题修复。

## 7.2 预发布版本

```text
2.4.0-alpha.1
2.4.0-beta.2
2.4.0-rc.1
```

## 7.3 Build Number

版本号与构建号应分开。

```text
Version: 2.4.0
Build: 1842
```

构建号应：

- 唯一；
- 可追踪；
- 可关联 Commit；
- 可关联构建环境；
- 可关联发布记录。

---

# 8. Release Branch Strategy：发布分支策略

常见方式：

```text
Trunk-Based
Release Branch
Long-Lived Main + Develop
```

---

## 8.1 Trunk-Based

所有变更持续进入主分支，通过：

- Feature Flag；
- 小步提交；
- 持续集成；
- 自动化测试；

控制发布。

适合：

- 高频发布；
- 自动化成熟；
- 小批量交付。

---

## 8.2 Release Branch

从主开发分支切出：

```text
release/2.4.0
```

之后只允许：

- Bugfix；
- Version Update；
- Release Note；
- Configuration Fix；
- 必要 Migration Fix。

优点：

- 范围稳定；
- 可继续开发下一版本；
- 便于 Patch。

风险：

- 分支漂移；
- Hotfix 漏同步；
- 合并冲突。

---

## 8.3 分支原则

无论采用何种策略，都应确保：

- Release Commit 可追踪；
- Hotfix 会同步回主线；
- 发布 Tag 唯一；
- 构建来源明确；
- 未经验证的 Commit 不进入发布。

---

# 9. Feature Freeze：功能冻结

## 9.1 定义

Feature Freeze 表示：

> 从此时间点开始，Release 不再接受新的功能范围，只允许修复发布阻塞问题和完成发布准备。

## 9.2 目的

- 稳定 Scope；
- 为回归测试提供固定基线；
- 降低最后阶段风险；
- 防止持续加入新内容；
- 让发布状态可预测。

## 9.3 Freeze 后允许

- Blocker / Critical 修复；
- 发布配置修复；
- 测试修复；
- 迁移修复；
- 文档修复；
- 必要回滚。

## 9.4 Freeze 后不允许

- 新功能；
- 大型架构调整；
- 无关重构；
- 新依赖；
- 非必要视觉改动；
- 新内容批次。

---

# 10. Release Candidate：候选版本

## 10.1 定义

Release Candidate，简称 RC，是一个可能成为正式发布版本的完整构建。

例如：

```text
2.4.0-rc.1
```

## 10.2 RC 应具备

- 固定 Commit；
- 唯一构建号；
- 完整资源；
- 正式配置；
- 目标平台构建；
- 迁移脚本；
- 发布说明草稿；
- 已知问题列表；
- 测试范围；
- 回滚方案。

## 10.3 RC 原则

一旦 RC 进入正式测试，不应直接修改原构建。

任何代码、资产或配置变化都应生成新的 RC：

```text
rc.1
→ 修复
→ rc.2
```

---

# 11. Build：构建

## 11.1 构建要求

- 可重复；
- 自动化；
- 来源明确；
- 依赖锁定；
- 配置明确；
- 产物可校验；
- 版本号正确；
- 签名正确；
- 资源完整。

## 11.2 构建信息

每个发布构建应记录：

```text
Version
Build Number
Commit SHA
Branch
Build Time
Build Environment
Dependencies
Target Platform
Build Owner
Artifact Location
Checksum
```

## 11.3 构建检查

```markdown
- [ ] 构建成功；
- [ ] 无阻塞警告；
- [ ] 版本号正确；
- [ ] 构建号正确；
- [ ] Commit 正确；
- [ ] 配置正确；
- [ ] 资源完整；
- [ ] 签名正确；
- [ ] 产物可启动；
- [ ] 产物可追踪；
```

---

# 12. Regression Testing：回归测试

## 12.1 目标

确认本次 Release 没有破坏已有核心功能。

## 12.2 回归范围

优先覆盖：

- 核心用户流程；
- 本次修改范围；
- 共享系统；
- 数据与存档；
- 登录与权限；
- 支付；
- 导航；
- 配置；
- 平台能力；
- 历史高频 Bug；
- 构建与安装。

## 12.3 回归分层

### Full Regression

适用于：

- Major Release；
- 核心系统变化；
- 数据迁移；
- 平台升级；
- 高风险依赖升级。

### Focused Regression

适用于：

- Minor Release；
- 局部功能；
- 影响范围明确。

### Smoke Regression

适用于：

- Hotfix；
- 紧急 Patch；
- 极小范围修复。

---

# 13. Release Blocking Defects：发布阻塞缺陷

通常以下问题应阻止发布：

- Blocker；
- Critical；
- 数据损坏；
- 安全风险；
- 无法启动；
- 核心流程不可用；
- 迁移失败；
- 正式构建失败；
- 目标平台严重异常；
- 回滚不可用；
- 关键配置错误；
- 法律或合规问题。

Major 是否阻塞，应根据：

- 用户影响；
- 是否有 Workaround；
- 是否影响核心流程；
- 是否影响数据；
- 是否高频；
- 是否可在后续 Patch 修复。

---

# 14. Migration：数据迁移

## 14.1 适用场景

- 数据库 Schema；
- 存档格式；
- 配置格式；
- 索引；
- 文件结构；
- 用户设置；
- 内容版本；
- API 合同变化。

## 14.2 迁移计划

应包含：

```text
Source Version
Target Version
Migration Steps
Execution Order
Estimated Duration
Backup
Dry Run
Validation
Rollback
Owner
```

## 14.3 迁移要求

```markdown
- [ ] 旧数据可识别；
- [ ] 迁移脚本已测试；
- [ ] Dry Run 已完成；
- [ ] 迁移可重复执行或安全阻止重复；
- [ ] 失败不会损坏原数据；
- [ ] 有备份；
- [ ] 有回滚；
- [ ] 迁移耗时可接受；
- [ ] 迁移日志完整；
- [ ] 迁移后验证明确；
```

---

# 15. Configuration：配置检查

发布问题经常来自配置，而不是代码。

需要检查：

- API Endpoint；
- Environment Variable；
- Feature Flag；
- 权限；
- 证书；
- 密钥；
- 商店信息；
- CDN；
- 数据库连接；
- 日志级别；
- 监控；
- 第三方服务；
- 支付配置；
- 域名；
- CORS；
- 时区。

## 15.1 配置原则

- 不在代码中硬编码生产配置；
- 不提交真实密钥；
- 配置应可审计；
- 配置变化应可回滚；
- 环境差异应明确；
- 发布前使用清单确认。

---

# 16. Feature Flag

## 16.1 用途

Feature Flag 可用于：

- 隐藏未开放功能；
- 灰度；
- A/B Test；
- 快速禁用；
- 按用户或地区开放；
- 降低发布风险。

## 16.2 Flag 要求

每个重要 Flag 应记录：

```text
名称
Owner
默认值
目标环境
开放条件
关闭条件
过期时间
清理计划
```

## 16.3 风险

长期未清理的 Flag 会造成：

- 分支复杂度；
- 测试组合膨胀；
- 行为不可预测；
- 旧代码残留。

---

# 17. Release Approval：发布审批

## 17.1 目标

确认当前版本已经满足发布条件。

## 17.2 审批角色

根据项目可能包括：

- Release Owner；
- Product Owner；
- Engineering Owner；
- QA；
- Security；
- Operations；
- Compliance；
- Platform Owner。

个人项目中可以由同一人完成，但仍应明确检查角色。

## 17.3 审批检查

```markdown
- [ ] Scope 已确认；
- [ ] RC 已固定；
- [ ] 回归测试通过；
- [ ] 阻塞缺陷为零；
- [ ] Migration 已验证；
- [ ] Configuration 已确认；
- [ ] Rollback 已准备；
- [ ] Release Notes 已准备；
- [ ] Monitoring 已准备；
- [ ] 发布窗口已确认；
- [ ] Owner 已确认；
```

---

# 18. Go / No-Go Decision

发布前应做明确决定：

```text
Go
Go with Accepted Limitations
No-Go
Delay
Rollback to Previous RC
```

## 18.1 Go

所有关键条件满足，可以发布。

## 18.2 Go with Accepted Limitations

存在非阻塞限制，但：

- 已记录；
- 已接受；
- 不影响核心流程；
- 不影响数据和安全；
- 有 Follow-up。

## 18.3 No-Go

存在不可接受风险。

---

# 19. Deploy / Publish：部署与发布

## 19.1 常见方式

- 全量发布；
- 滚动发布；
- 蓝绿发布；
- Canary；
- 灰度；
- 分地区发布；
- 分平台发布；
- 商店发布；
- 手动分发；
- 内部发布。

## 19.2 发布原则

- 变更可追踪；
- 顺序明确；
- Owner 在线；
- 回滚可用；
- 监控已启动；
- 发布窗口适合；
- 关键依赖可用；
- 避免无人值守发布。

---

# 20. Progressive Delivery：渐进式发布

渐进式发布通过逐步扩大用户范围降低风险。

示例：

```text
Internal
→ 1%
→ 5%
→ 25%
→ 50%
→ 100%
```

## 20.1 每一阶段检查

- 错误率；
- 崩溃率；
- 性能；
- 数据一致性；
- 用户反馈；
- 关键业务指标；
- 告警；
- 支持工单。

## 20.2 扩大条件

- 指标稳定；
- 无新 Critical；
- 无数据异常；
- 无明显性能下降；
- 监控正常；
- Owner 确认。

---

# 21. Smoke Test：发布后冒烟测试

## 21.1 目标

快速确认发布成功，目标环境基本可用。

## 21.2 常见 Smoke Test

```markdown
- [ ] 应用可启动；
- [ ] 登录可用；
- [ ] 主页面可进入；
- [ ] 核心流程可完成；
- [ ] 新功能入口可用；
- [ ] 数据读写正常；
- [ ] 第三方依赖正常；
- [ ] 日志和监控正常；
- [ ] 无立即崩溃；
- [ ] 版本号正确；
```

Smoke Test 通过不等于完整验证完成。

---

# 22. Monitoring：发布后监控

## 22.1 观察内容

- 错误率；
- 崩溃率；
- 延迟；
- 帧率；
- 内存；
- CPU；
- 数据异常；
- 用户反馈；
- 工单；
- 转化；
- 功能使用率；
- 日志；
- 告警；
- 回滚信号。

## 22.2 监控窗口

根据风险设置：

```text
1 小时
4 小时
24 小时
3 天
1 周
一个完整业务周期
```

## 22.3 监控责任

必须明确：

- 谁观察；
- 观察什么；
- 何时升级；
- 谁决定回滚；
- 谁负责沟通。

---

# 23. Rollback：回滚

## 23.1 回滚触发条件

- 核心流程不可用；
- 错误率显著上升；
- 数据损坏；
- 安全问题；
- 性能严重退化；
- 迁移失败；
- 无法在可接受时间内修复；
- 影响范围持续扩大。

## 23.2 回滚方式

- 回滚应用版本；
- 关闭 Feature Flag；
- 恢复数据库；
- 回滚配置；
- 切换备用服务；
- 停止流量；
- 禁用入口；
- 回滚资源包。

## 23.3 回滚要求

```markdown
- [ ] 回滚目标版本明确；
- [ ] 回滚步骤已验证；
- [ ] 数据影响已评估；
- [ ] 回滚 Owner 明确；
- [ ] 回滚后 Smoke Test 明确；
- [ ] 用户沟通准备；
- [ ] 事件记录准备；
```

---

# 24. Roll Forward

有时无法或不适合回滚，例如：

- 数据已迁移；
- 用户已产生新数据；
- 外部系统已同步；
- 商店版本不可即时撤回。

此时可以 Roll Forward：

- 快速发布修复版本；
- 关闭部分能力；
- 增加兼容层；
- 数据修复；
- 配置修正。

Roll Forward 必须同样具备：

- 最小范围；
- 快速验证；
- 监控；
- 回滚或降级思路。

---

# 25. Hotfix Release

## 25.1 适用情况

- 生产事故；
- S0 / S1；
- P0；
- 安全问题；
- 数据损坏；
- 核心流程阻塞；
- 发布阻塞。

## 25.2 Hotfix 流程

```text
Incident Confirmed
→ Mitigate
→ Minimal Fix
→ Focused Review
→ Critical Test
→ Build
→ Deploy
→ Smoke Test
→ Monitor
→ Back-merge
→ RCA
```

## 25.3 Hotfix 原则

- 范围最小；
- 禁止无关改动；
- 优先可回滚；
- 保留最小 Review；
- 保留正式构建验证；
- 必须发布后监控；
- 必须同步回主线；
- 重大事故必须 RCA。

---

# 26. Release Notes

## 26.1 目标

向用户、团队或运维说明：

- 新增了什么；
- 修复了什么；
- 改变了什么；
- 已知问题；
- 是否需要迁移；
- 是否有行为变化。

## 26.2 推荐结构

```markdown
# Release Notes

## New

## Improved

## Fixed

## Changed

## Deprecated

## Removed

## Security

## Known Issues

## Migration Notes
```

## 26.3 用户版本与内部版本

用户版本应：

- 清晰；
- 避免内部术语；
- 聚焦实际影响。

内部版本可以包含：

- Ticket；
- Commit；
- Migration；
- Config；
- Risk；
- Rollback；
- Owner。

---

# 27. Changelog

Changelog 用于记录版本历史。

推荐格式：

```markdown
## [2.4.0] - 2026-07-10

### Added
- 

### Changed
- 

### Fixed
- 

### Removed
- 
```

Changelog 应：

- 与实际版本一致；
- 不提前宣称未发布内容；
- 能追踪重要变化；
- 不包含敏感信息。

---

# 28. Communication Plan

根据发布影响，可能需要通知：

- 用户；
- 客服；
- 运营；
- 销售；
- 管理层；
- 外部合作方；
- 平台审核方；
- 开发团队；
- 运维。

## 28.1 沟通内容

- 发布时间；
- 影响范围；
- 主要变化；
- 已知问题；
- 用户操作要求；
- 维护窗口；
- 回滚状态；
- 支持方式。

---

# 29. Maintenance Window

如果发布会造成中断，应明确：

- 开始时间；
- 预计时长；
- 影响范围；
- 用户通知；
- 回滚阈值；
- 延长条件；
- 结束确认。

---

# 30. Release Close：发布关闭

## 30.1 关闭条件

Release 可以关闭，当：

- 部署完成；
- Smoke Test 通过；
- 监控窗口完成；
- 无未处理 Critical；
- 数据迁移完成；
- 发布结果已记录；
- Known Issues 已记录；
- Follow-up 已创建；
- Changelog 已更新；
- Owner 已确认；
- 版本已打 Tag；
- 产物已归档。

## 30.2 关闭结果

```text
Released Successfully
Released with Known Issues
Rolled Back
Partially Released
Cancelled
Superseded
```

---

# 31. Release 状态模型

推荐状态：

```text
Planned
Scope Confirmed
In Preparation
Feature Frozen
RC Built
In Testing
Ready for Approval
Approved
Deploying
Released
Monitoring
Closed
```

补充：

```text
Blocked
Delayed
Cancelled
Rolled Back
Hotfix Required
```

---

# 32. Release Roles

---

## 32.1 Release Owner

负责：

- 推动发布；
- 维护 Scope；
- 协调时间；
- 组织 Go / No-Go；
- 跟踪阻塞；
- 确保关闭。

---

## 32.2 Engineering Owner

负责：

- 构建；
- 技术验证；
- Migration；
- Rollback；
- 技术支持。

---

## 32.3 QA Owner

负责：

- 测试计划；
- 回归；
- 缺陷状态；
- 发布建议。

---

## 32.4 Product Owner

负责：

- Scope；
- 用户价值；
- 已知限制接受；
- 发布时机。

---

## 32.5 Operations Owner

负责：

- 部署；
- 监控；
- 告警；
- 事故响应；
- 恢复。

个人项目中可以由同一人承担多个角色，但责任仍应区分。

---

# 33. Release Plan 模板

```markdown
# Release Plan

> Release：
> Version：
> Type：
> Target Date：
> Environment：
> Release Owner：
> Status：

## 1. Goal

## 2. Scope

| Item | Type | Owner | Done Status |
|---|---|---|---|

## 3. Non-goals

## 4. Risk Level

## 5. Dependencies

| Dependency | Status | Owner | Blocking |
|---|---|---|---|

## 6. Feature Freeze

- Date：
- Allowed Changes：

## 7. Build

- Branch：
- Commit：
- Build Number：
- Platforms：

## 8. Testing

- Regression Scope：
- Acceptance：
- Performance：
- Security：
- Compatibility：

## 9. Migration

## 10. Configuration

## 11. Feature Flags

## 12. Rollout Strategy

## 13. Smoke Test

## 14. Monitoring

## 15. Rollback Plan

## 16. Communication

## 17. Approval

## 18. Known Issues
```

---

# 34. Go / No-Go 模板

```markdown
# Go / No-Go Review

> Release：
> Version：
> Date：
> Owner：

## Scope

## RC

- Version：
- Build：
- Commit：

## Quality Status

| Category | Result | Evidence |
|---|---|---|
| Acceptance | | |
| Regression | | |
| Performance | | |
| Security | | |
| Migration | | |
| Build | | |

## Open Defects

| Defect | Severity | Status | Decision |
|---|---|---|---|

## Risks

## Rollback Readiness

## Monitoring Readiness

## Final Decision

- [ ] Go
- [ ] Go with Accepted Limitations
- [ ] No-Go
- [ ] Delay
- [ ] New RC Required

## Conditions

## Sign-off
```

---

# 35. Release Checklist

```markdown
## Planning

- [ ] Release Goal 明确；
- [ ] Release Type 明确；
- [ ] Target Date 明确；
- [ ] Owner 明确；
- [ ] Scope 明确；
- [ ] Non-goals 明确。

## Scope

- [ ] 所有 Item 已通过 Done Gate；
- [ ] 依赖已准备；
- [ ] Known Limitations 已记录；
- [ ] 未完成项已移出；
- [ ] Feature Freeze 已生效。

## Build

- [ ] 版本号正确；
- [ ] 构建号正确；
- [ ] Commit 正确；
- [ ] Release Build 成功；
- [ ] 目标平台构建成功；
- [ ] 资源完整；
- [ ] 签名正确；
- [ ] 构建可追踪。

## Testing

- [ ] Acceptance 通过；
- [ ] Regression 通过；
- [ ] 正式构建验证通过；
- [ ] 平台验证通过；
- [ ] 性能验证通过；
- [ ] 安全验证通过；
- [ ] 阻塞缺陷为零。

## Data

- [ ] Migration 已测试；
- [ ] Dry Run 已完成；
- [ ] Backup 已准备；
- [ ] Rollback 已验证；
- [ ] Migration 验证步骤明确。

## Configuration

- [ ] Environment Variables 正确；
- [ ] API Endpoint 正确；
- [ ] Feature Flag 正确；
- [ ] 权限正确；
- [ ] 证书和密钥正确；
- [ ] 监控配置正确。

## Approval

- [ ] Go / No-Go 已完成；
- [ ] 风险已接受；
- [ ] Known Issues 已接受；
- [ ] 发布窗口已确认；
- [ ] 沟通已准备。

## Deployment

- [ ] 部署步骤明确；
- [ ] Owner 在线；
- [ ] Rollback 可执行；
- [ ] Smoke Test 已准备；
- [ ] Monitoring 已启动。

## Post-release

- [ ] Smoke Test 通过；
- [ ] 核心指标稳定；
- [ ] 无 Critical Incident；
- [ ] 用户反馈已观察；
- [ ] Changelog 已更新；
- [ ] Tag 已创建；
- [ ] Follow-up 已创建；
- [ ] Release 已关闭。
```

---

# 36. Hotfix Checklist

```markdown
- [ ] Incident 已确认；
- [ ] Severity / Priority 已确认；
- [ ] Mitigation 已执行；
- [ ] Hotfix Owner 已确认；
- [ ] 修复范围最小；
- [ ] 无无关重构；
- [ ] Focused Review 已完成；
- [ ] 原问题已验证；
- [ ] Critical Regression 已完成；
- [ ] Release Build 成功；
- [ ] Rollback 可用；
- [ ] 发布后 Smoke Test 通过；
- [ ] Monitoring 正常；
- [ ] 修复已同步回主线；
- [ ] RCA 已安排。
```

---

# 37. Release Report 模板

```markdown
# Release Report

> Release：
> Version：
> Date：
> Owner：
> Result：

## 1. Summary

## 2. Included Changes

## 3. Deployment Timeline

| Time | Event |
|---|---|

## 4. Build Information

## 5. Migration Result

## 6. Smoke Test Result

## 7. Monitoring Result

## 8. Incidents

## 9. Rollback / Hotfix

## 10. Known Issues

## 11. Follow-up Actions

| Action | Owner | Due Date | Status |
|---|---|---|---|

## 12. Lessons Learned
```

---

# 38. 常见失败模式

## 38.1 Done 就直接发布

问题：

- 没有完整回归；
- 没有正式构建验证；
- 没有迁移检查；
- 没有 Rollback；
- 没有 Monitoring。

---

## 38.2 Freeze 后继续加入功能

结果：

- 测试基线不断变化；
- 发布延期；
- 回归范围失控；
- 风险无法判断。

---

## 38.3 RC 构建被直接修改

结果：

- 测试版本与发布版本不一致；
- 证据失效；
- 无法追踪。

正确做法：

```text
任何变化都生成新的 RC。
```

---

## 38.4 只验证 Debug Build

结果：

- 正式资源缺失；
- 签名问题；
- 配置不同；
- 性能不同；
- 权限不同。

---

## 38.5 Migration 没有回滚

结果：

- 部署失败后无法恢复；
- 数据被部分转换；
- 新旧版本都不可用。

---

## 38.6 Feature Flag 永久不清理

结果：

- 代码分支复杂；
- 测试组合增加；
- 行为不可预测。

---

## 38.7 Go / No-Go 只是形式

表现：

- 无论风险如何都选择 Go；
- 缺陷没有明确决策；
- Owner 不清楚；
- 条件没有跟踪。

---

## 38.8 发布后无人监控

结果：

- 问题由用户先发现；
- 回滚时机延误；
- 影响扩大。

---

## 38.9 Hotfix 没有同步回主线

结果：

- 下一版本重新出现问题；
- 分支不一致；
- 修复丢失。

---

## 38.10 Release 永远不关闭

表现：

- 状态长期停留 Monitoring；
- Follow-up 未创建；
- Changelog 未更新；
- 经验未记录。

---

# 39. Release 指标

可参考：

- 发布频率；
- Lead Time；
- Change Failure Rate；
- Rollback Rate；
- Hotfix Rate；
- 发布后缺陷数；
- Smoke Test 失败率；
- 平均恢复时间；
- Migration 失败率；
- 发布延期率；
- RC 数量；
- Scope Change 次数；
- 发布后 24 小时事故数。

指标用于改善：

- 稳定性；
- 自动化；
- 测试质量；
- Scope 控制；
- 恢复能力。

不应只追求：

- 发布越快越好；
- 发布越多越好；
- RC 越少越好。

---

# 40. 最终原则

Release Workflow 的本质不是：

> 把构建上传到目标环境。

它的本质是：

> 将一个已经完成的变更集合，以可验证、可观察、可回滚的方式安全交付，并确认其在真实环境中稳定运行。

一个有效的发布流程应做到：

- Scope 稳定；
- 构建可追踪；
- 测试充分；
- 数据安全；
- 配置正确；
- 决策明确；
- 发布可渐进；
- 失败可回滚；
- 结果可监控；
- 经验可沉淀。
