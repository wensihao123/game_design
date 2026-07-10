# Roadmap and Milestones（路线图与里程碑）

> 状态：草案  
> 适用范围：通用的软件与游戏开发项目  
> 目的：定义如何将长期方向转化为阶段目标、可交付结果和可管理的里程碑。

---

## 1. 文档目的

Roadmap and Milestones 用于帮助项目回答：

- 未来一段时间最重要的方向是什么；
- 哪些结果应该先实现；
- 哪些工作属于当前阶段；
- 哪些内容应该延后；
- 一个 Milestone 应如何定义；
- 如何判断 Milestone 是否完成；
- 如何处理延期、范围变化和依赖风险；
- 如何避免 Roadmap 变成不现实的承诺清单；
- 如何让长期目标与 Backlog、Feature 和 Release 保持一致。

Roadmap 的目标不是预测所有未来工作。

Milestone 的目标也不是简单收集一组任务。

二者共同服务于：

> 将长期方向转化为可验证、可排序、可交付的阶段性结果。

---

## 2. Roadmap 与 Milestone 的区别

### 2.1 Roadmap

Roadmap 描述：

- 长期方向；
- 主要目标；
- 重要能力；
- 阶段顺序；
- 关键依赖；
- 预期时间窗口；
- 风险和假设。

Roadmap 回答：

> 接下来应该朝什么方向推进？

---

### 2.2 Milestone

Milestone 是 Roadmap 中一个可以被明确判断完成与否的阶段目标。

Milestone 回答：

> 到这个阶段结束时，项目应具备什么可验证结果？

---

### 2.3 Backlog

Backlog 保存尚未完成的候选工作。

Backlog 回答：

> 有哪些工作可能值得做？

---

### 2.4 Release

Release 是实际交付到目标环境的版本。

Release 回答：

> 哪些完成内容会在什么时间被交付？

---

### 2.5 关系

```text
Vision
  ↓
Roadmap
  ↓
Milestones
  ↓
Features
  ↓
Tasks
  ↓
Releases
```

另一种理解：

```text
Roadmap = 方向
Milestone = 阶段结果
Backlog = 候选工作
Feature = 价值单元
Task = 执行单元
Release = 交付单元
```

---

## 3. 核心设计原则

### 3.1 Roadmap 应面向结果，而不是任务

不合格 Roadmap：

```text
完成登录页面
完成背包页面
完成设置页面
完成 20 个图标
```

这些只是工作项。

更好的 Roadmap：

```text
用户能够完成从进入产品到保存核心进度的完整闭环。
```

Roadmap 应描述：

- 能力；
- 结果；
- 风险降低；
- 用户价值；
- 产品成熟度。

---

### 3.2 Milestone 应可验收

Milestone 不能只写：

```text
完成第一阶段
核心功能差不多
进入 Beta
```

它应明确：

- 目标；
- Scope；
- Non-goals；
- 关键交付物；
- Exit Criteria；
- 风险；
- 依赖；
- 验证方式。

---

### 3.3 时间是约束，不是全部内容

Roadmap 不应只是一条时间线。

时间应与以下内容同时存在：

- 目标；
- 价值；
- 范围；
- 依赖；
- 风险；
- 置信度。

不推荐只写：

```text
Q1：功能 A
Q2：功能 B
Q3：功能 C
```

而应说明：

- 为什么是这个顺序；
- 依赖什么；
- 哪些日期是承诺；
- 哪些日期只是预测；
- 哪些目标具有较高不确定性。

---

### 3.4 越远的计划越粗

推荐使用不同精度：

```text
Now：具体 Feature 和执行目标
Next：明确目标和候选 Feature
Later：方向、问题和能力
```

未来越远，不确定性越高。

因此不应对远期工作提供虚假的细节和精确日期。

---

### 3.5 Roadmap 不是永久承诺

Roadmap 是基于当前信息的计划。

当以下条件发生变化时，可以调整：

- 用户反馈；
- 技术发现；
- 市场变化；
- 团队容量；
- 关键依赖；
- 风险；
- 发布结果；
- 项目目标。

调整 Roadmap 不是失败。

没有记录地频繁变化，才是管理问题。

---

### 3.6 限制同时进行的目标

同时开启太多 Milestone 会造成：

- 资源分散；
- 目标冲突；
- 大量半完成工作；
- 难以发布；
- 优先级失效。

应优先完成少量明确阶段目标。

---

## 4. Roadmap 的层级

建议使用以下层级：

```text
Vision
→ Strategic Theme
→ Outcome
→ Milestone
→ Feature
```

---

## 4.1 Vision

描述项目希望长期达到的状态。

Vision 应：

- 稳定；
- 简洁；
- 提供方向；
- 不包含具体任务；
- 不依赖某一实现方式。

---

## 4.2 Strategic Theme

战略主题表示一组长期关注方向。

例如：

```text
核心体验
用户留存
创作能力
平台扩展
质量与稳定性
开发效率
```

---

## 4.3 Outcome

Outcome 描述希望产生的实际结果。

例如：

```text
新用户可以在 10 分钟内完成首次核心体验。
```

Outcome 比 Feature 更关注结果，而不是实现方式。

---

## 4.4 Milestone

Milestone 是可验收的阶段目标。

例如：

```text
核心循环可完整运行，并支持保存、加载和失败恢复。
```

---

## 4.5 Feature

Feature 是实现 Milestone 所需的价值单元。

---

## 5. Roadmap 类型

---

## 5.1 Outcome Roadmap

以结果为中心。

例如：

```text
Now：完成核心使用闭环
Next：提高首次使用成功率
Later：支持高级创作和分享
```

适合：

- 产品方向；
- 游戏体验；
- 用户价值；
- 不确定性较高的项目。

---

## 5.2 Capability Roadmap

以能力为中心。

例如：

```text
基础账户能力
数据同步能力
内容管理能力
多人协作能力
```

适合：

- 平台；
- 基础设施；
- 长期系统建设。

---

## 5.3 Release Roadmap

以版本为中心。

例如：

```text
v0.1 Internal
v0.2 Alpha
v0.3 Beta
v1.0 Launch
```

适合：

- 明确版本节奏；
- 对外发布；
- 平台认证；
- 合作交付。

---

## 5.4 Technology Roadmap

以技术能力和风险为中心。

例如：

```text
统一状态管理
存档版本化
构建自动化
多平台导出
性能预算
```

适合：

- 架构演进；
- 技术债务；
- 平台升级；
- 开发效率。

---

## 5.5 Hybrid Roadmap

多数项目可以结合：

- Outcome；
- Capability；
- Release；
- Technology。

但应保持主视角清楚，避免所有内容混在同一层级。

---

# 6. Roadmap 时间模型

---

## 6.1 Now / Next / Later

推荐的通用模型：

### Now

- 已准备；
- 正在执行；
- 范围较清楚；
- 置信度高；
- 可关联具体 Milestone 和 Feature。

### Next

- 方向明确；
- 仍需进一步澄清；
- 依赖当前阶段结果；
- 置信度中等。

### Later

- 代表未来方向；
- 不承诺具体日期；
- 仍有较高不确定性；
- 可能被替代、拆分或取消。

---

## 6.2 日期型 Roadmap

当存在真实外部约束时，可以使用：

```text
2026 Q3
2026 Q4
2027 H1
```

适用：

- 合同；
- 平台窗口；
- 法规；
- 活动；
- 财务周期；
- 固定发布计划。

使用日期时，应标记：

```text
Committed
Target
Forecast
Exploratory
```

---

## 6.3 置信度

建议记录：

```text
High
Medium
Low
```

例如：

| 时间窗口 | 目标 | 置信度 |
|---|---|---|
| Now | 核心闭环 | High |
| Next | 内容扩展 | Medium |
| Later | 多人协作 | Low |

---

# 7. Milestone 的定义

Milestone 是一个阶段性结果集合。

一个好的 Milestone 应具备：

```text
Goal
Outcome
Scope
Non-goals
Entry Criteria
Exit Criteria
Dependencies
Risks
Owner
Target Window
```

Milestone 不应只是一组 Feature 名称。

---

## 7.1 Milestone Goal

描述这一阶段为什么存在。

示例：

```text
建立可以被完整体验和验证的核心流程。
```

---

## 7.2 Milestone Outcome

描述完成后项目新增了什么真实能力。

示例：

```text
用户可以进入产品、完成核心流程、保存状态，并在重新启动后继续。
```

---

## 7.3 Milestone Scope

列出实现该结果所需的主要能力。

---

## 7.4 Milestone Non-goals

明确本阶段不处理什么。

Non-goals 用于防止：

- 阶段范围持续扩大；
- 高级能力过早进入；
- 核心目标被次要工作拖延。

---

## 7.5 Entry Criteria

进入 Milestone 前应满足的条件。

例如：

- 上一个 Milestone 已关闭；
- 核心依赖可用；
- 关键技术风险已验证；
- 高优先级 Feature 已达到 Ready；
- 团队容量已确认；
- 目标环境可用。

---

## 7.6 Exit Criteria

Milestone 完成必须满足的结果。

Exit Criteria 应：

- 可验证；
- 结果导向；
- 与 Feature DoD 一致；
- 包含质量和集成要求；
- 不仅检查任务数量。

---

# 8. Milestone 状态

建议使用：

```text
Proposed
Planned
Ready
Active
At Risk
Blocked
In Review
Completed
Closed
Cancelled
```

---

## 8.1 Proposed

阶段目标正在讨论。

---

## 8.2 Planned

目标和初步范围已确认，但尚未启动。

---

## 8.3 Ready

满足进入条件，可以启动。

---

## 8.4 Active

正在执行。

---

## 8.5 At Risk

目标仍可能完成，但存在明显风险。

必须记录：

- 风险；
- 影响；
- Owner；
- 缓解措施；
- 决策时间点。

---

## 8.6 Blocked

关键依赖阻止继续推进。

---

## 8.7 In Review

正在进行阶段验收和关闭评审。

---

## 8.8 Completed

Exit Criteria 已满足。

---

## 8.9 Closed

结果、经验和 Follow-up 已记录，阶段正式关闭。

---

## 8.10 Cancelled

该 Milestone 不再继续。

---

# 9. Milestone Scope 设计

---

## 9.1 Must-have

没有这些内容，Milestone 目标无法成立。

---

## 9.2 Should-have

重要，但必要时可以调整或延后。

---

## 9.3 Stretch Goal

在核心范围稳定后，容量允许时才处理。

Stretch Goal 不应：

- 阻止 Milestone 关闭；
- 混入 Must-have；
- 在核心工作延误时继续执行；
- 被提前承诺为确定交付。

---

## 9.4 Out of Scope

明确不属于当前阶段。

---

## 9.5 Scope 分类示例

| Item | Category | Ready | Owner |
|---|---|---|---|
| 核心流程 | Must | Yes | A |
| 高级筛选 | Should | No | B |
| 分享能力 | Stretch | No | C |
| 多人协作 | Out | No | - |

---

# 10. Milestone 的大小

一个 Milestone 不应过大或过小。

---

## 10.1 过大的信号

- 持续数月且没有中间结果；
- 包含多个独立产品目标；
- 大量 Feature 彼此无关；
- Exit Criteria 很难定义；
- 风险和依赖过多；
- 无法判断真实进度；
- 所有工作都被归入同一阶段。

---

## 10.2 过小的信号

- 只是一个普通 Task；
- 一两天即可完成；
- 没有独立结果；
- 不值得单独评审；
- 不影响 Roadmap。

---

## 10.3 合理大小

一个 Milestone 应：

- 能产生一个清晰阶段结果；
- 能在合理周期内完成；
- 能被独立验证；
- 能支持一次阶段评审；
- 能为下一阶段解除重要依赖。

---

# 11. Milestone 与 Feature 的关系

每个 Feature 应明确属于：

- 某个 Milestone；
- 某个 Release；
- 或明确标记为 Unplanned。

一个 Feature 进入 Milestone，应满足：

- 直接支持 Milestone Outcome；
- 优先级合理；
- 已达到 Ready 或有明确 Ready 计划；
- 依赖可控；
- 规模适合；
- 有 Owner；
- 有验收标准。

如果一个 Feature 无法解释如何支持 Milestone，应重新评估是否加入。

---

# 12. Milestone 与 Release 的关系

Milestone 和 Release 不一定一一对应。

可能关系：

```text
一个 Milestone → 多个 Release
多个 Milestone → 一个 Release
Milestone 完成后等待 Release
Release 包含多个阶段的完成内容
```

---

## 12.1 Milestone 偏结果

Milestone 关注：

- 阶段能力；
- 目标完成；
- 风险降低；
- 项目成熟度。

---

## 12.2 Release 偏交付

Release 关注：

- 版本；
- 构建；
- 发布窗口；
- 部署；
- 平台；
- 回滚；
- 用户环境。

---

# 13. Milestone Planning

Milestone Planning 用于确认：

- 目标；
- Scope；
- Non-goals；
- Feature；
- 依赖；
- 风险；
- 容量；
- 时间窗口；
- Exit Criteria。

---

## 13.1 规划输入

- Roadmap；
- 当前项目状态；
- Backlog；
- Ready Feature；
- 上一阶段结果；
- 团队容量；
- 外部约束；
- 技术风险；
- 用户反馈；
- 发布计划。

---

## 13.2 规划输出

```text
Milestone Goal
Outcome
Must-have Scope
Should-have Scope
Stretch Goals
Non-goals
Owners
Dependencies
Risks
Entry Criteria
Exit Criteria
Target Window
Review Dates
```

---

# 14. Capacity Planning

Milestone Scope 必须考虑真实容量。

容量包括：

- 开发；
- 设计；
- 资产；
- 内容；
- 测试；
- 文档；
- 发布；
- 维护；
- Bugfix；
- 不确定性；
- 支持工作。

不应把 100% 时间都用于计划功能。

建议预留：

- Bug；
- 返工；
- Review；
- 集成；
- 测试；
- 技术风险；
- 临时支持；
- 发布问题。

---

## 14.1 容量原则

```text
计划容量 < 理论最大容量
```

避免以“所有事情都不出问题”为前提制定计划。

---

# 15. Dependency Planning

Milestone 依赖可能包括：

- 上一阶段能力；
- 外部服务；
- 平台支持；
- 资产；
- 数据；
- 技术验证；
- 团队；
- 法务；
- 发布窗口；
- 第三方审核。

---

## 15.1 依赖记录

| Dependency | Type | Owner | Needed By | Status | Fallback |
|---|---|---|---|---|---|

---

## 15.2 关键依赖要求

关键依赖必须：

- 有 Owner；
- 有目标时间；
- 有状态；
- 有风险；
- 有替代方案；
- 有超时处理方式。

---

# 16. Risk Planning

每个 Milestone 应识别：

- 价值风险；
- 范围风险；
- 技术风险；
- 依赖风险；
- 数据风险；
- 性能风险；
- 内容风险；
- 测试风险；
- 发布风险；
- 容量风险。

---

## 16.1 风险表

| Risk | Probability | Impact | Mitigation | Owner | Trigger |
|---|---|---|---|---|---|

---

## 16.2 Risk Trigger

风险应定义触发信号。

例如：

```text
如果核心技术原型在第 2 周仍未达到目标性能，则缩小 Milestone Scope。
```

---

# 17. Milestone Kickoff

Milestone 启动时应确认：

```markdown
- [ ] Goal 已理解；
- [ ] Outcome 已理解；
- [ ] Must-have Scope 已确认；
- [ ] Non-goals 已确认；
- [ ] Feature Owner 已确认；
- [ ] Entry Criteria 已满足；
- [ ] Dependencies 已确认；
- [ ] Risks 已确认；
- [ ] Review 节奏已确认；
- [ ] Exit Criteria 已确认；
```

---

# 18. Milestone Tracking

跟踪重点应包括：

- Outcome；
- Scope；
- 风险；
- 依赖；
- 阻塞；
- 质量；
- Ready 状态；
- Done 状态；
- 发布状态。

不要只跟踪：

- 完成了多少 Task；
- 工时用了多少；
- 百分比进度。

---

## 18.1 推荐状态报告

```text
Status
Outcome Progress
Completed Results
Current Risks
Blocked Items
Scope Changes
Decisions Needed
Next Checkpoint
```

---

## 18.2 状态颜色

可以使用：

```text
Green
Yellow
Red
```

### Green

- 目标可达；
- 风险可控；
- 无关键阻塞。

### Yellow

- 存在风险；
- 可能需要调整 Scope 或时间；
- 需要管理关注。

### Red

- 当前计划不可达；
- 存在关键阻塞；
- 必须重新规划。

颜色必须有客观解释，不能只表达感觉。

---

# 19. Milestone Review

建议定期检查：

- Outcome 是否仍然正确；
- 当前 Scope 是否仍然必要；
- 风险是否变化；
- 依赖是否变化；
- 质量是否达标；
- 是否需要移除 Stretch Goal；
- 是否需要拆分；
- 是否需要重新排序；
- 是否仍符合 Roadmap。

---

## 19.1 Review 节奏

可选择：

- 每周；
- 每两周；
- 每个 Sprint；
- 每个关键 Checkpoint；
- 风险触发时。

---

# 20. Scope Change

Milestone 执行中可能发生范围变化。

---

## 20.1 允许的小型变化

- 不改变 Outcome；
- 不增加关键依赖；
- 不改变 Exit Criteria；
- 不显著增加成本；
- 不扩大发布风险。

---

## 20.2 重大变化

以下变化应正式评审：

- Outcome 改变；
- Must-have 增加；
- Non-goals 被加入；
- 新增关键系统；
- 数据模型改变；
- 发布目标改变；
- 时间显著变化；
- 资源显著变化；
- Exit Criteria 改变。

---

## 20.3 Scope Change 记录

```text
Change
Reason
Impact
Decision
Owner
Date
```

---

# 21. Scope Freeze

当 Milestone 接近完成时，可以进入 Scope Freeze。

Scope Freeze 表示：

- 不再加入新 Feature；
- 只允许完成 Must-have；
- 只修复阻塞问题；
- 集中完成集成、测试和关闭。

Scope Freeze 的目的：

- 防止最后阶段持续扩张；
- 稳定验收基线；
- 提高关闭概率。

---

# 22. Delay Management

延期不应只通过修改日期处理。

必须判断：

- 为什么延期；
- 是否是 Scope 问题；
- 是否是依赖问题；
- 是否是容量问题；
- 是否是估算问题；
- 是否是质量问题；
- 是否是优先级变化；
- 是否仍值得继续。

---

## 22.1 延期处理选项

```text
Reduce Scope
Move Should-have
Remove Stretch Goals
Split Milestone
Add Capacity
Resolve Dependency
Change Target Window
Cancel
```

---

## 22.2 不推荐

```text
保持全部 Scope，同时不断顺延日期
```

这会隐藏真实问题。

---

# 23. Split Milestone

当 Milestone 过大时，可以拆分：

```text
Milestone A：核心闭环
Milestone B：体验完善
Milestone C：扩展能力
```

拆分后每个 Milestone 都应：

- 有独立 Outcome；
- 有独立 Exit Criteria；
- 有明确依赖；
- 可以单独评审。

---

# 24. Milestone Cancellation

Milestone 可以取消，当：

- 目标不再有价值；
- 已被其他方案替代；
- 成本不可接受；
- 依赖无法解决；
- 项目方向变化；
- 风险超过收益。

取消时应记录：

- 原目标；
- 取消原因；
- 已完成工作；
- 可复用成果；
- 清理工作；
- 后续影响；
- 是否需要替代 Milestone。

---

# 25. Milestone Completion

Milestone 可以标记 Completed，当：

- Exit Criteria 全部满足；
- Must-have 全部完成；
- 相关 Feature 已通过 Done；
- 必要集成已完成；
- 关键测试通过；
- 不存在阻塞缺陷；
- 结果可 Demo 或验证；
- 后续工作已拆分；
- 已知限制已记录。

---

# 26. Milestone Closure

Completed 不等于 Closed。

关闭前还应完成：

- 结果复盘；
- 实际与计划对比；
- 延期和 Scope 变化记录；
- 学习结论；
- Follow-up；
- Roadmap 更新；
- 文档归档。

---

# 27. Milestone Retro

Retro 应回答：

- 目标是否达成；
- 哪些假设正确；
- 哪些假设错误；
- Scope 是否合理；
- 哪些依赖造成问题；
- 哪些风险提前识别；
- 哪些问题发现太晚；
- 哪些流程有效；
- 下一阶段应改变什么。

---

## 27.1 Retro 输出

```text
What Worked
What Did Not Work
Unexpected Findings
Scope Changes
Quality Findings
Process Improvements
Follow-up Actions
```

---

# 28. Roadmap Review

Roadmap 应定期评审。

建议频率：

- 每月；
- 每季度；
- 每个 Milestone；
- 重大变化后；
- 重要 Release 后。

---

## 28.1 Roadmap Review 内容

```markdown
- [ ] Vision 是否仍然成立；
- [ ] Strategic Themes 是否仍然正确；
- [ ] Now / Next / Later 是否需要调整；
- [ ] 当前 Milestone 是否仍然重要；
- [ ] 新风险是否出现；
- [ ] 新依赖是否出现；
- [ ] 用户反馈是否改变方向；
- [ ] 技术发现是否改变顺序；
- [ ] 是否有目标应取消；
- [ ] 是否需要新增 Milestone；
```

---

# 29. Roadmap 变更记录

重要变更应记录：

```text
Date
Previous Plan
New Plan
Reason
Impact
Decision Owner
```

这样可以避免：

- 反复讨论已解决问题；
- 不清楚为什么改方向；
- 误认为计划从未变化；
- 无法复盘决策质量。

---

# 30. Roadmap 模板

```markdown
# Roadmap

> Owner：
> Period：
> Last Updated：
> Status：

## 1. Vision

## 2. Strategic Themes

- 

## 3. Now

| Outcome | Milestone | Target Window | Confidence |
|---|---|---|---|

## 4. Next

| Outcome | Candidate Milestone | Dependencies | Confidence |
|---|---|---|---|

## 5. Later

| Direction | Problem / Opportunity | Notes |
|---|---|---|

## 6. Major Dependencies

## 7. Major Risks

## 8. Assumptions

## 9. Change Log

| Date | Change | Reason |
|---|---|---|
```

---

# 31. Milestone 模板

```markdown
# Milestone

> Name：
> Status：
> Owner：
> Target Window：
> Confidence：
> Related Roadmap Theme：
> Last Updated：

## 1. Goal

## 2. Outcome

## 3. Entry Criteria

- [ ]

## 4. Must-have Scope

- [ ]

## 5. Should-have Scope

- [ ]

## 6. Stretch Goals

- [ ]

## 7. Non-goals

- [ ]

## 8. Features

| Feature | Priority | Ready | Owner | Status |
|---|---|---|---|---|

## 9. Dependencies

| Dependency | Owner | Needed By | Status | Fallback |
|---|---|---|---|---|

## 10. Risks

| Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|

## 11. Exit Criteria

- [ ]

## 12. Review Schedule

## 13. Scope Changes

| Date | Change | Reason | Impact |
|---|---|---|---|

## 14. Final Result

## 15. Learnings

## 16. Follow-up
```

---

# 32. Milestone Planning Checklist

```markdown
## Goal

- [ ] Goal 明确；
- [ ] Outcome 可验证；
- [ ] 与 Roadmap 一致；
- [ ] 价值明确。

## Scope

- [ ] Must-have 明确；
- [ ] Should-have 明确；
- [ ] Stretch Goal 明确；
- [ ] Non-goals 明确；
- [ ] Scope 规模合理。

## Readiness

- [ ] Entry Criteria 明确；
- [ ] 关键 Feature 已 Ready；
- [ ] Owner 明确；
- [ ] 容量已确认；
- [ ] 目标时间窗口合理。

## Dependencies

- [ ] 关键依赖已识别；
- [ ] 依赖有 Owner；
- [ ] 依赖有时间；
- [ ] Fallback 已考虑；
- [ ] 无隐藏阻塞。

## Risks

- [ ] 关键风险已识别；
- [ ] 风险有缓解方案；
- [ ] 风险有触发条件；
- [ ] 高风险项已优先验证。

## Completion

- [ ] Exit Criteria 可测试；
- [ ] 质量要求明确；
- [ ] Release 关系明确；
- [ ] Review 节奏明确；
- [ ] 关闭方式明确。
```

---

# 33. Milestone Closure Checklist

```markdown
- [ ] Must-have 全部完成；
- [ ] Exit Criteria 全部通过；
- [ ] 相关 Feature 已 Done；
- [ ] 必要 Release 已完成或已计划；
- [ ] Blocker 和 Critical 为零；
- [ ] 已知限制已记录；
- [ ] Follow-up 已创建；
- [ ] Scope Change 已记录；
- [ ] Retro 已完成；
- [ ] Roadmap 已更新；
- [ ] 文档已归档；
- [ ] Milestone 已关闭。
```

---

# 34. 常见失败模式

## 34.1 Roadmap 变成功能列表

表现：

```text
功能 A
功能 B
功能 C
功能 D
```

问题：

- 看不出目标；
- 看不出顺序理由；
- 看不出结果；
- 看不出依赖。

---

## 34.2 远期计划过度精确

表现：

- 为半年后的 Task 指定具体日期；
- 假设所有依赖不变；
- 产生虚假承诺。

改进：

- 使用 Now / Next / Later；
- 标记 Confidence；
- 越远越粗。

---

## 34.3 Milestone 没有 Exit Criteria

结果：

- 无法判断完成；
- 不断加入内容；
- 阶段长期不关闭。

---

## 34.4 Milestone 只按任务完成率跟踪

问题：

- Task 完成不代表 Outcome 达成；
- 集成和质量可能仍未完成；
- 大量低价值任务提高了表面进度。

---

## 34.5 Stretch Goal 变成隐性 Must-have

结果：

- 核心目标被拖延；
- Scope 不受控；
- Milestone 无法关闭。

---

## 34.6 所有工作都放进当前 Milestone

结果：

- 优先级失效；
- 容量超载；
- 大量工作半完成。

---

## 34.7 延期只改日期

结果：

- 根因不解决；
- Scope 不变；
- 风险继续累积；
- Roadmap 失去可信度。

---

## 34.8 Ready 状态未检查

表现：

- 高优先级 Feature 尚未 Ready 就进入 Milestone；
- 执行中持续等待澄清；
- 任务拆分反复变化。

---

## 34.9 Roadmap 被当作永久承诺

结果：

- 团队不敢调整错误方向；
- 新信息无法影响计划；
- 为守日期牺牲价值和质量。

---

## 34.10 Milestone 完成后不复盘

结果：

- 相同估算错误反复发生；
- 依赖问题重复出现；
- Scope 管理没有改善；
- Roadmap 无法学习。

---

# 35. Roadmap 和 Milestone 指标

可参考：

- Milestone Completion Rate；
- Planned vs Actual Duration；
- Scope Change Count；
- Must-have Completion Rate；
- Stretch Goal Completion Rate；
- Blocked Time；
- Dependency Delay；
- Ready Feature Ratio；
- Reopened Feature Count；
- Release Delay；
- Milestone Carry-over；
- Outcome Achievement；
- Defect Escape Rate。

指标用于改善：

- 规划质量；
- Scope 控制；
- 依赖管理；
- 风险识别；
- 交付稳定性。

不应只追求：

- 100% 按原日期完成；
- 完成更多 Milestone；
- Stretch Goal 全部完成；
- Scope 从不变化。

---

# 36. 最终原则

Roadmap 的本质不是：

> 预测未来所有要做的事情。

Milestone 的本质也不是：

> 把一批任务装进同一个时间段。

二者的本质是：

> 明确长期方向，将其拆成可验证的阶段结果，并在真实约束和新信息下持续调整优先级、范围和顺序。

一个有效的 Roadmap 和 Milestone 体系应做到：

- 结果导向；
- 方向清楚；
- 越远越粗；
- 置信度透明；
- Scope 有边界；
- 依赖可见；
- 风险可控；
- Exit Criteria 明确；
- 允许调整；
- 能够复盘；
- 与 Backlog、Feature 和 Release 保持一致。
