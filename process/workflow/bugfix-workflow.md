# Bugfix Workflow（缺陷修复流程）

> 状态：草案  
> 适用范围：通用的软件与游戏开发项目  
> 目的：定义 Bug 从发现、分诊、复现、调查、修复、验证、发布到关闭的完整处理流程。

---

## 1. 文档目的

Bugfix Workflow 用于统一缺陷处理方式，确保问题能够被：

- 正确记录；
- 快速分类；
- 合理排序；
- 稳定复现；
- 准确定位；
- 安全修复；
- 充分验证；
- 正确发布；
- 完整关闭；
- 转化为后续改进经验。

它回答以下问题：

- 什么算 Bug；
- Bug 与 Feature、Improvement、Technical Debt 有什么区别；
- Bug 应如何报告；
- Severity 和 Priority 如何定义；
- 无法复现时怎么办；
- 什么时候需要 Root Cause Analysis；
- 修复前是否需要临时缓解措施；
- 普通修复与 Hotfix 有什么区别；
- 如何控制回归风险；
- 数据损坏如何处理；
- 什么时候可以关闭 Bug；
- 发布后再次出现如何处理。

Bugfix Workflow 的目标不是简单完成“修一下”。

它的目标是：

> 在尽量降低用户影响和系统风险的同时，找出问题原因，实施可验证的修复，并防止同类问题再次发生。

---

## 2. 什么是 Bug

Bug 是指：

> 系统的实际行为与已定义、已承诺或合理预期的行为不一致。

Bug 通常具有以下至少一种特征：

- 功能无法按验收标准工作；
- 已有行为发生非预期变化；
- 系统崩溃或卡死；
- 数据错误、丢失或不一致；
- 性能明显低于目标；
- 权限或安全行为错误；
- UI 状态与内部状态不一致；
- 正式构建与开发环境行为不一致；
- 平台兼容性异常；
- 用户无法完成原本可完成的流程；
- 历史已修问题重新出现。

---

## 3. Bug 与其他工作类型的区别

### 3.1 Bug 与 Feature

```text
Bug = 已有或已承诺行为不正确
Feature = 新增此前不存在的能力
```

示例：

```text
保存按钮点击后没有保存数据 → Bug
增加自动保存功能 → Feature
```

---

### 3.2 Bug 与 Improvement

```text
Bug = 行为不符合预期
Improvement = 行为正确，但可以更好
```

示例：

```text
错误提示完全不显示 → Bug
错误提示可以写得更清楚 → Improvement
```

---

### 3.3 Bug 与 Technical Debt

```text
Bug = 已产生可观察错误
Technical Debt = 当前可运行，但实现存在维护、扩展或风险问题
```

示例：

```text
内存泄漏导致崩溃 → Bug
模块耦合过高，未来难以扩展 → Technical Debt
```

---

### 3.4 Bug 与 Support Issue

有些用户问题不是 Bug，例如：

- 使用方式不清楚；
- 配置错误；
- 权限不足；
- 环境不支持；
- 用户数据异常；
- 第三方服务不可用；
- 文档不足。

这些问题仍可能转化为：

- Documentation；
- UX Improvement；
- Configuration Fix；
- Feature；
- Monitoring Improvement。

---

## 4. Bug 生命周期总览

```text
REPORT
  ↓
TRIAGE
  ↓
REPRODUCE
  ↓
INVESTIGATE
  ↓
MITIGATE（可选）
  ↓
PLAN FIX
  ↓
IMPLEMENT
  ↓
REVIEW
  ↓
VERIFY
  ↓
REGRESSION
  ↓
RELEASE
  ↓
MONITOR
  ↓
CLOSE
```

可能的回退路径：

```text
Verify → Investigate
Regression → Implement
Release → Hotfix
Monitor → Reopen
```

---

# 5. Report：报告缺陷

## 5.1 目标

收集足够信息，让其他人可以理解问题、评估影响并尝试复现。

## 5.2 最低字段

每个 Bug 至少应包含：

```text
标题
实际结果
预期结果
复现步骤
环境
版本
发生时间
复现频率
影响范围
报告人
证据
```

## 5.3 推荐标题格式

```text
[模块/平台] 条件下出现的错误行为
```

示例：

```text
[Windows] 从暂停菜单返回后输入失效
[Save] 旧版本存档升级后背包数据丢失
[Login] Token 过期后进入无限加载
```

不推荐：

```text
有问题
功能坏了
这里不对
登录 Bug
```

## 5.4 实际结果与预期结果

不合格：

```text
不正常
```

合格：

```text
实际结果：
点击“保存”后界面显示成功，但重新进入页面后数据恢复为旧值。

预期结果：
保存成功后，新值应被持久化，并在重新进入页面后保持不变。
```

## 5.5 复现步骤

复现步骤应：

- 从明确初始状态开始；
- 使用编号；
- 尽量减少无关步骤；
- 标明关键输入；
- 标明出现问题的具体时机。

示例：

```text
1. 使用已有旧版存档启动应用；
2. 进入设置页面；
3. 修改音量为 50%；
4. 点击保存；
5. 退出并重新启动；
6. 再次进入设置页面。
```

## 5.6 证据

可包括：

- 截图；
- 视频；
- 日志；
- 崩溃堆栈；
- 存档；
- 网络请求；
- 性能数据；
- 用户会话；
- 错误代码；
- 构建号。

## 5.7 Report 退出条件

- 信息足够进行初步分诊；
- 已确认不是明显重复项；
- 已记录版本和环境；
- 已确定下一步 Owner。

---

# 6. Triage：缺陷分诊

## 6.1 目标

判断问题：

- 是否真的是 Bug；
- 是否重复；
- 影响有多大；
- 多快需要处理；
- 由谁处理；
- 是否需要立即缓解；
- 是否进入 Hotfix。

## 6.2 Triage 决策

```text
Confirmed Bug
Needs More Information
Duplicate
Cannot Reproduce
Expected Behavior
Configuration Issue
Feature Request
Improvement
Deferred
Rejected
Hotfix Required
```

## 6.3 Triage 必须确认

- 类型；
- Severity；
- Priority；
- Owner；
- 影响版本；
- 目标修复版本；
- 是否阻塞发布；
- 是否影响数据；
- 是否影响安全；
- 是否需要 Mitigation；
- 是否需要 Hotfix。

---

# 7. Severity：严重程度

Severity 表示：

> 这个问题造成的实际影响有多严重。

建议使用：

```text
S0 Blocker
S1 Critical
S2 Major
S3 Minor
S4 Trivial
```

---

## 7.1 S0 Blocker

特征：

- 产品完全不可用；
- 核心流程完全阻塞；
- 无可用替代方案；
- 发布无法继续；
- 大范围数据损坏；
- 严重安全事故；
- 大量用户受影响。

示例：

- 应用启动即崩溃；
- 所有用户无法登录；
- 保存系统导致所有存档损坏；
- 支付重复扣款。

---

## 7.2 S1 Critical

特征：

- 核心能力严重受损；
- 影响大量用户；
- 有数据、安全或财务风险；
- 可能存在有限绕过方式；
- 需要快速处理。

示例：

- 部分用户无法加载存档；
- 权限校验失效；
- 关键交易状态错误；
- 严重性能退化导致不可用。

---

## 7.3 S2 Major

特征：

- 重要功能异常；
- 影响明显；
- 主流程受损但仍可绕过；
- 不会造成重大数据或安全事故。

示例：

- 某平台无法使用特定功能；
- 编辑功能失败但可以重新创建；
- UI 卡住但重启可恢复。

---

## 7.4 S3 Minor

特征：

- 局部行为异常；
- 不影响主流程；
- 容易绕过；
- 影响有限。

示例：

- 某个非关键提示错误；
- 局部布局错位；
- 次要动画异常。

---

## 7.5 S4 Trivial

特征：

- 几乎不影响使用；
- 轻微视觉或文案问题；
- 修复优先级通常较低。

示例：

- 轻微对齐误差；
- 个别错别字；
- 非关键颜色不一致。

---

# 8. Priority：处理优先级

Priority 表示：

> 这个问题应该多快被处理。

建议使用：

```text
P0 Immediate
P1 High
P2 Medium
P3 Low
P4 Someday
```

---

## 8.1 P0 Immediate

需要立即处理。

通常包括：

- 生产事故；
- 发布阻塞；
- 严重安全问题；
- 数据损坏；
- 财务风险；
- 核心系统不可用。

---

## 8.2 P1 High

应进入最近开发窗口。

通常包括：

- 高影响用户问题；
- 当前版本关键缺陷；
- 高概率发生的 Major；
- 重要合作或交付阻塞。

---

## 8.3 P2 Medium

有明确价值，但不需要立即处理。

---

## 8.4 P3 Low

影响有限，可在后续优化周期处理。

---

## 8.5 P4 Someday

仅在条件允许时处理，或保留观察。

---

## 8.6 Severity 与 Priority 的区别

```text
Severity = 问题本身有多严重
Priority = 现在多快处理
```

例如：

- 极少发生但会造成崩溃的问题，可能 Severity 高、Priority 中；
- 高频出现但影响较轻的问题，可能 Severity 中、Priority 高。

---

# 9. Duplicate：重复缺陷

当新 Bug 与已有 Bug 原因和行为相同时，应标记为 Duplicate。

合并时保留：

- 原报告来源；
- 用户数量；
- 环境差异；
- 新证据；
- 复现频率；
- 影响范围。

重复报告数量本身可能提升 Priority。

---

# 10. Reproduce：复现问题

## 10.1 目标

建立稳定、最小、可重复的问题场景。

## 10.2 复现维度

应尝试确认：

- 版本；
- 平台；
- 设备；
- 账号；
- 数据状态；
- 网络状态；
- 权限；
- 输入方式；
- 首次使用或已有数据；
- Debug / Release Build；
- 特定配置；
- 发生频率。

## 10.3 最小复现

尽量缩小到：

- 最少步骤；
- 最少依赖；
- 最小数据；
- 最小场景；
- 最少系统交互。

最小复现有助于：

- 定位根因；
- 编写回归测试；
- 判断影响范围；
- 避免误修。

## 10.4 复现频率

建议记录：

```text
Always
Frequent
Intermittent
Rare
Once
Unknown
```

或：

```text
10/10
7/10
2/10
1/100
```

---

# 11. Cannot Reproduce：无法复现

无法复现不等于问题不存在。

应依次检查：

- 报告信息是否完整；
- 环境是否一致；
- 构建号是否一致；
- 用户数据是否可获得；
- 日志是否充分；
- 是否与网络、时序或性能有关；
- 是否为特定设备问题；
- 是否已被其他改动间接修复；
- 是否需要增加监控。

可能处理结果：

```text
Needs More Information
Monitoring Added
Cannot Reproduce
Closed as Unconfirmed
Deferred
```

如果关闭为无法复现，应记录：

- 已尝试的环境；
- 已尝试的步骤；
- 缺失的信息；
- 重新打开条件。

---

# 12. Investigate：调查与定位

## 12.1 目标

确定：

- 故障发生在哪里；
- 直接原因是什么；
- 根本原因是什么；
- 影响范围有多大；
- 是否存在同类风险；
- 修复会影响哪些系统。

## 12.2 调查方法

可包括：

- 日志分析；
- 堆栈分析；
- 二分定位；
- 版本对比；
- Git Bisect；
- 数据对比；
- 性能采样；
- 网络抓包；
- 状态跟踪；
- 单元隔离；
- 最小复现项目；
- 代码审查；
- 依赖版本检查。

## 12.3 Direct Cause 与 Root Cause

```text
Direct Cause = 直接触发问题的原因
Root Cause = 为什么系统允许这个问题发生
```

示例：

```text
直接原因：
空对象被访问。

根本原因：
加载流程允许未初始化数据进入业务层，且缺少边界校验与测试。
```

---

# 13. Root Cause Analysis（RCA）

## 13.1 何时需要 RCA

建议在以下情况进行：

- S0 / S1；
- 数据损坏；
- 安全事故；
- 生产事故；
- 同类问题反复出现；
- 发布后紧急回滚；
- 问题长期未被发现；
- 多系统共同失效；
- 造成重大交付延期。

## 13.2 RCA 关注点

RCA 不应只写：

```text
开发者漏写判断
测试没测到
操作失误
```

应继续追问：

- 为什么容易漏；
- 为什么评审未发现；
- 为什么测试未覆盖；
- 为什么监控未发现；
- 为什么影响可以扩大；
- 为什么恢复困难。

## 13.3 Five Whys

示例：

```text
为什么数据丢失？
因为保存时覆盖了旧数据。

为什么会覆盖？
因为新字段为空时仍执行全量写入。

为什么没有阻止？
因为缺少字段级校验。

为什么缺少校验？
因为迁移路径没有纳入验收标准。

为什么没有纳入？
因为功能设计中未识别旧数据兼容风险。
```

## 13.4 RCA 输出

```text
Incident Summary
Impact
Timeline
Detection
Direct Cause
Root Cause
Contributing Factors
Mitigation
Permanent Fix
Prevention Actions
Owner
Due Date
```

---

# 14. Mitigate：临时缓解

## 14.1 目标

在永久修复完成前，先降低用户影响或系统风险。

## 14.2 缓解方式

- Feature Flag 关闭；
- 回滚版本；
- 降级功能；
- 禁止特定入口；
- 临时配置调整；
- 数据备份；
- 限流；
- 停止自动任务；
- 临时提示；
- 客服通知；
- 手工修复；
- 切换备用服务。

## 14.3 Mitigation 不等于 Fix

Mitigation 只降低影响，不代表根因已解决。

必须继续追踪：

- 永久修复；
- 回归测试；
- 数据修复；
- 预防措施。

---

# 15. Plan Fix：制定修复方案

## 15.1 修复方案需要说明

- 根因；
- 修改范围；
- 预计影响；
- 数据影响；
- 兼容性；
- 回归风险；
- 测试计划；
- 回滚方式；
- 是否需要迁移；
- 是否需要监控；
- 是否需要 Hotfix。

## 15.2 最小修复与系统修复

可能存在两种方案：

### 最小修复

- 快速；
- 范围小；
- 风险低；
- 适合紧急处理；
- 可能保留部分技术债务。

### 系统修复

- 解决根因；
- 范围更大；
- 需要更多测试；
- 可能影响更多模块；
- 适合正常发布周期。

有时可以：

```text
先最小修复止血
→ 再创建 Follow-up 完成系统改进
```

---

# 16. Implement：实施修复

## 16.1 实施原则

- 修复根因，不只掩盖症状；
- 控制变更范围；
- 避免顺便大规模重构；
- 保留可回滚性；
- 增加必要日志；
- 增加自动化回归测试；
- 更新相关文档；
- 记录临时方案；
- 检查同类代码。

## 16.2 Bugfix Scope

修复中不应无控制地加入：

- 新功能；
- 无关重构；
- 大范围风格调整；
- 非必要依赖升级；
- 与 Bug 无关的优化。

如必须扩大范围，应重新评估：

- 风险；
- Track；
- 测试范围；
- 发布方式。

---

# 17. Review：修复评审

## 17.1 评审重点

- 是否真正解决根因；
- 是否只是掩盖症状；
- 是否可能引入新问题；
- 是否存在数据影响；
- 是否增加回归测试；
- 是否需要兼容旧版本；
- 是否可回滚；
- 是否存在隐藏范围扩大。

## 17.2 Bugfix Review Checklist

```markdown
- [ ] 根因已说明；
- [ ] 修复与根因对应；
- [ ] 变更范围合理；
- [ ] 没有无关改动；
- [ ] 新增或更新回归测试；
- [ ] 数据影响已检查；
- [ ] 性能影响已检查；
- [ ] 安全影响已检查；
- [ ] 回滚方式明确；
- [ ] 日志与监控已考虑；
```

---

# 18. Verify：验证修复

## 18.1 目标

确认原 Bug 已被修复。

## 18.2 验证要求

使用原复现步骤：

- 在原问题环境验证；
- 在修复版本验证；
- 使用原始数据或等价数据；
- 检查原证据对应行为；
- 验证成功与失败流程；
- 检查副作用。

## 18.3 修复验证结果

```text
Fixed
Not Fixed
Partially Fixed
Cannot Verify
New Issue Found
```

## 18.4 验证证据

应记录：

- 修复版本；
- 构建号；
- 环境；
- 测试步骤；
- 结果；
- 截图、日志或视频；
- 测试人；
- 时间。

---

# 19. Regression：回归测试

## 19.1 目标

确认修复没有破坏已有功能。

## 19.2 回归范围

至少覆盖：

- 原问题流程；
- 相邻流程；
- 共享模块；
- 公共接口；
- 数据读写；
- 权限；
- 构建；
- 历史相关 Bug；
- 目标平台。

## 19.3 回归范围判断

根据：

- 修改文件；
- 修改模块；
- 共享依赖；
- 数据结构；
- 公共接口；
- 状态机；
- 平台差异；
- 变更风险。

## 19.4 回归检查

```markdown
- [ ] 原问题不再出现；
- [ ] 原有正常流程仍可用；
- [ ] 相邻功能未受影响；
- [ ] 数据行为正确；
- [ ] 共享模块行为正确；
- [ ] 正式构建通过；
- [ ] 目标平台通过；
```

---

# 20. Data Repair：数据修复

如果 Bug 已经造成错误数据，仅修代码可能不够。

需要评估：

- 哪些数据受影响；
- 如何识别；
- 是否可恢复；
- 是否需要批量修复；
- 是否需要用户确认；
- 是否需要备份；
- 是否需要审计；
- 是否需要补偿；
- 是否需要通知用户。

## 20.1 数据修复流程

```text
Identify
→ Backup
→ Dry Run
→ Validate
→ Execute
→ Verify
→ Audit
```

## 20.2 数据修复要求

```markdown
- [ ] 受影响数据可识别；
- [ ] 修复规则明确；
- [ ] 已备份；
- [ ] 已进行 Dry Run；
- [ ] 修复结果可验证；
- [ ] 可回滚；
- [ ] 有审计记录；
```

---

# 21. Release：发布修复

## 21.1 普通发布

适用于：

- 风险可控；
- 不需要立即修复；
- 可进入正常版本周期；
- 有完整回归时间。

## 21.2 Patch Release

适用于：

- 小范围修复；
- 不增加新功能；
- 兼容性稳定；
- 需要较快发布。

## 21.3 Hotfix

适用于：

- P0 / P1；
- 生产事故；
- 发布阻塞；
- 严重数据或安全风险；
- 无法等待正常发布周期。

---

# 22. Hotfix Workflow

```text
Incident Confirmed
→ Mitigate
→ Create Hotfix Branch
→ Minimal Fix
→ Focused Review
→ Critical Verification
→ Build
→ Deploy
→ Smoke Test
→ Monitor
→ Back-merge
→ RCA
```

## 22.1 Hotfix 原则

- 变更范围最小；
- 禁止无关重构；
- 优先可回滚；
- 必须有明确 Owner；
- 必须验证正式构建；
- 必须发布后监控；
- 必须同步回主开发分支；
- 重大事故必须做 RCA。

## 22.2 Hotfix 不应省略

即使紧急，也不应完全省略：

- Review；
- 最低验证；
- 正式构建；
- 回滚方案；
- 发布后 Smoke Test；
- 监控；
- 修复同步。

---

# 23. Rollback：回滚

## 23.1 何时回滚

- 修复引入更严重问题；
- 正式环境验证失败；
- 数据风险扩大；
- 性能严重下降；
- 无法快速定位；
- 发布影响超出预期。

## 23.2 回滚方式

- 回滚版本；
- 关闭 Feature Flag；
- 回滚配置；
- 回滚数据库；
- 恢复备份；
- 切换备用服务；
- 禁用入口。

## 23.3 回滚后

必须：

- 确认系统恢复；
- 记录影响；
- 更新 Bug 状态；
- 重新调查；
- 重新制定修复；
- 必要时通知用户。

---

# 24. Monitor：发布后监控

## 24.1 目标

确认修复在真实环境中有效，且没有产生新问题。

## 24.2 观察内容

- 错误率；
- 崩溃率；
- 性能；
- 用户反馈；
- 日志；
- 告警；
- 数据异常；
- 客服工单；
- 功能使用率；
- 回滚信号。

## 24.3 监控窗口

根据风险定义：

- 数小时；
- 24 小时；
- 3 天；
- 1 周；
- 一个完整业务周期。

---

# 25. Close：关闭缺陷

## 25.1 关闭条件

Bug 可以关闭，当：

- 原问题已修复；
- 修复已验证；
- 必要回归通过；
- 必要发布完成；
- 正式环境表现正常；
- 数据修复完成；
- 已知限制已记录；
- Follow-up 已创建；
- RCA 已完成或不需要；
- 文档已更新。

## 25.2 关闭结果

```text
Fixed
Fixed with Limitations
Duplicate
Cannot Reproduce
Expected Behavior
Won’t Fix
Deferred
Obsolete
```

---

# 26. Reopen：重新打开

Bug 应重新打开，当：

- 原问题再次出现；
- 修复不完整；
- 只修复部分环境；
- 回归失败；
- 正式环境出现同类问题；
- 数据影响仍然存在；
- 根因判断错误。

重新打开时应记录：

- 新版本；
- 新环境；
- 新证据；
- 与原问题的差异；
- 是否为同一根因；
- 是否需要提高 Severity 或 Priority。

---

# 27. Bug 状态模型

推荐状态：

```text
New
Triaged
Needs Information
Ready to Reproduce
Reproduced
Investigating
Ready for Fix
In Progress
In Review
Ready for Verification
Verified
Ready for Release
Released
Monitoring
Closed
```

补充：

```text
Blocked
Duplicate
Cannot Reproduce
Deferred
Rejected
Reopened
```

---

# 28. Workflow Track

---

## 28.1 Fast Track

适用于：

- 明确的小型 Bug；
- 影响范围小；
- 易回滚；
- 不涉及数据或安全；
- 复现稳定。

流程：

```text
Report
→ Confirm
→ Fix
→ Verify
→ Release
→ Close
```

---

## 28.2 Standard Track

适用于普通缺陷。

流程：

```text
Report
→ Triage
→ Reproduce
→ Investigate
→ Plan
→ Fix
→ Review
→ Verify
→ Regression
→ Release
→ Close
```

---

## 28.3 Critical / Hotfix Track

适用于：

- S0 / S1；
- P0；
- 生产事故；
- 安全问题；
- 数据风险。

流程：

```text
Confirm Incident
→ Mitigate
→ Assign Incident Owner
→ Minimal Fix
→ Critical Review
→ Focused Verification
→ Deploy
→ Monitor
→ RCA
→ Permanent Follow-up
→ Close
```

---

# 29. Bug Report 模板

```markdown
# Bug 标题

> ID：
> 状态：
> Severity：
> Priority：
> Owner：
> 报告人：
> 发现版本：
> 影响版本：
> 目标修复版本：
> 平台：
> 环境：
> 创建日期：

## 1. Summary

## 2. Actual Result

## 3. Expected Result

## 4. Reproduction Steps

1.
2.
3.

## 5. Reproduction Rate

## 6. Environment

- OS：
- Device：
- Build：
- Account：
- Network：
- Configuration：

## 7. Impact

## 8. Evidence

- Screenshot：
- Video：
- Log：
- Crash Dump：
- Save / Data：

## 9. Workaround

## 10. Related Issues
```

---

# 30. Investigation 模板

```markdown
# Bug Investigation

> Bug：
> Owner：
> Date：

## 1. Reproduction Result

## 2. Minimal Reproduction

## 3. Affected Scope

## 4. Direct Cause

## 5. Root Cause

## 6. Contributing Factors

## 7. Related Risk

## 8. Candidate Fixes

| 方案 | 优点 | 风险 | 成本 |
|---|---|---|---|

## 9. Recommended Fix

## 10. Regression Scope

## 11. Data Impact

## 12. Rollback Plan
```

---

# 31. Bugfix Verification 模板

```markdown
# Bugfix Verification

> Bug：
> Fix Version：
> Build：
> Environment：
> Tester：
> Date：

## 1. Original Reproduction

## 2. Verification Result

- [ ] Fixed
- [ ] Not Fixed
- [ ] Partially Fixed
- [ ] Cannot Verify
- [ ] New Issue Found

## 3. Regression Scope

## 4. Data Verification

## 5. Build Verification

## 6. Evidence

## 7. Known Limitations

## 8. Final Decision
```

---

# 32. RCA 模板

```markdown
# Root Cause Analysis

> Incident：
> Severity：
> Date：
> Owner：
> Status：

## 1. Summary

## 2. User / Business Impact

## 3. Timeline

| 时间 | 事件 |
|---|---|

## 4. Detection

## 5. Direct Cause

## 6. Root Cause

## 7. Contributing Factors

## 8. Why Existing Controls Failed

## 9. Mitigation

## 10. Permanent Fix

## 11. Prevention Actions

| Action | Owner | Due Date | Status |
|---|---|---|---|

## 12. Lessons Learned
```

---

# 33. 通用 Bugfix Checklist

```markdown
## Report

- [ ] 标题清楚；
- [ ] Actual Result 明确；
- [ ] Expected Result 明确；
- [ ] 复现步骤明确；
- [ ] 版本和环境明确；
- [ ] 证据已附上。

## Triage

- [ ] 类型已确认；
- [ ] Severity 已确认；
- [ ] Priority 已确认；
- [ ] Owner 已确认；
- [ ] 是否 Hotfix 已判断；
- [ ] 是否需要 Mitigation 已判断。

## Reproduce

- [ ] 原环境已尝试；
- [ ] 复现频率已记录；
- [ ] 最小复现已建立；
- [ ] 影响范围已确认；
- [ ] 无法复现时已记录尝试过程。

## Investigation

- [ ] Direct Cause 已识别；
- [ ] Root Cause 已识别；
- [ ] 数据影响已检查；
- [ ] 安全影响已检查；
- [ ] 性能影响已检查；
- [ ] 同类风险已检查。

## Fix

- [ ] 修复方案与根因对应；
- [ ] 变更范围最小合理；
- [ ] 无无关重构；
- [ ] 回归测试已增加；
- [ ] 回滚方式明确；
- [ ] 日志与监控已考虑。

## Verification

- [ ] 原复现步骤通过；
- [ ] 修复版本已记录；
- [ ] 原环境已验证；
- [ ] 失败流程已验证；
- [ ] 相邻流程已回归；
- [ ] 正式构建已验证。

## Release

- [ ] 目标版本明确；
- [ ] 发布方式明确；
- [ ] Patch / Hotfix 已确认；
- [ ] 发布后 Smoke Test 通过；
- [ ] 监控正常；
- [ ] Hotfix 已同步回主分支。

## Close

- [ ] 正式环境结果正常；
- [ ] 数据修复完成；
- [ ] 已知限制已记录；
- [ ] Follow-up 已创建；
- [ ] RCA 已完成或不需要；
- [ ] 最终状态已记录。
```

---

# 34. 常见失败模式

## 34.1 直接修复，不先复现

结果：

- 修错问题；
- 无法证明修复有效；
- 回归测试无法建立；
- 根因判断错误。

---

## 34.2 只修症状，不修根因

示例：

```text
出现 Null 就加一个 if
```

但没有解释：

- 为什么会出现 Null；
- 为什么数据能进入非法状态；
- 为什么测试没有发现。

---

## 34.3 所有 Bug 都设为 High Priority

结果：

- 优先级失效；
- 真正紧急问题无法突出；
- 团队频繁中断。

---

## 34.4 无法复现就立即关闭

结果：

- 间歇性问题被忽略；
- 用户问题反复出现；
- 缺少监控导致长期未知。

---

## 34.5 Hotfix 中加入无关重构

结果：

- 变更范围扩大；
- 回归风险上升；
- 发布变慢；
- 回滚困难。

---

## 34.6 修复后没有回归测试

结果：

- 同类问题再次出现；
- 历史 Bug 复发；
- 修复破坏相邻功能。

---

## 34.7 只修代码，不修数据

结果：

- 新数据正常；
- 历史错误数据仍然存在；
- 用户问题没有真正解决。

---

## 34.8 Hotfix 未同步回主分支

结果：

- 下一版本重新引入问题；
- 分支行为不一致；
- 修复丢失。

---

## 34.9 Bug 长期停留在 Blocked

改进：

必须记录：

- 阻塞原因；
- Owner；
- 解除条件；
- 下一次检查时间；
- 超时处理方式。

---

## 34.10 关闭 Bug 但没有发布

`Verified` 不等于 `Released`。

如果用户环境仍然没有修复，就不能宣称问题已经完全解决。

---

# 35. Bugfix 指标

可参考：

- 新增 Bug 数；
- 已关闭 Bug 数；
- Open Bug 数；
- S0 / S1 数量；
- 平均首次响应时间；
- 平均复现时间；
- 平均修复时间；
- 平均验证时间；
- Reopen Rate；
- Regression Rate；
- Escaped Defects；
- Cannot Reproduce 比例；
- Hotfix 数量；
- Rollback 数量；
- 相同根因重复发生次数。

指标应帮助改善：

- 质量；
- 发现能力；
- 修复效率；
- 发布稳定性。

不应只追求：

- 关闭数量；
- 修复速度；
- Bug 清零。

---

# 36. 最终原则

Bugfix Workflow 的本质不是：

> 尽快把 Bug 状态改成 Closed。

它的本质是：

> 准确理解问题，控制影响，解决根因，验证修复，并降低同类问题再次发生的概率。

一个有效的缺陷修复流程应做到：

- 报告清晰；
- 分诊准确；
- Severity 与 Priority 分离；
- 复现可靠；
- 根因明确；
- 修复范围可控；
- 验证有证据；
- 回归充分；
- 发布可回滚；
- 结果可监控；
- 经验可沉淀。
