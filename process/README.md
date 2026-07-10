# Process

> 本目录用于定义项目如何从「想法」推进到「可发布版本」。  
> 它描述的是工作流、协作规则、质量门槛、工具使用方式和交付规范，而不是玩法本身，也不是具体代码实现。

---

## 1. 目录定位

`process/` 负责回答以下问题：

- 一个想法如何进入项目；
- 一项功能在什么时候可以开始开发；
- 设计、原型、实现、测试和发布之间如何衔接；
- Bug、资产、版本和文档分别应该遵循什么流程；
- 一个任务在什么条件下才算真正完成；
- 项目中的决策、规范和经验如何被沉淀；
- 如何降低返工、遗漏和流程混乱。

它与其他文档目录的职责边界如下：

| 目录 | 主要回答的问题 |
|---|---|
| `design/` | 游戏做什么，为什么这样设计 |
| `engineering/` | 功能如何实现，代码如何组织 |
| `process/` | 工作如何推进、验证、交付和维护 |
| `inbox/` | 尚未分类、尚未确认、待处理的输入 |

### 示例

以“新增中毒状态”为例：

- `design/` 记录中毒的玩法目的、叠加规则、持续时间、数值和反馈；
- `engineering/` 记录状态系统的组件结构、Resource 设计、Signal、存档方式；
- `process/` 记录该需求如何进入 Backlog、何时 Ready、如何验收、何时进入发布版本；
- `inbox/` 暂存尚未澄清的想法、截图、参考资料和临时笔记。

---

## 2. Process 的核心目标

本目录的目标不是增加文档负担，而是减少以下问题：

- 需求未澄清就开始开发；
- 一边实现一边重新决定功能目标；
- 原型代码直接进入正式架构；
- 功能完成后才开始考虑测试；
- “代码写完”被误认为“功能完成”；
- 美术、UI、程序和数据之间缺少交接标准；
- 项目版本无法稳定导出；
- 决策反复讨论却没有记录；
- 临时方案长期留在主项目；
- 里程碑中积累大量半成品。

任何流程文档都应服务于：

1. 减少返工；
2. 提高可追踪性；
3. 明确质量标准；
4. 缩短从想法到可玩版本的路径；
5. 保持个人开发项目长期可维护。

---

## 3. Process Principles

本项目遵循以下流程原则：

1. **先明确价值，再进入开发**  
   功能必须说明它解决什么问题，以及对玩家或项目有什么价值。

2. **优先完成垂直切片**  
   优先打通一个完整闭环，而不是同时铺开大量只有部分完成的系统。

3. **开发前定义验收标准**  
   验收条件应在实现前明确，而不是实现完成后临时决定。

4. **设计、实现与流程分离**  
   玩法规则放在 `design/`，技术实现放在 `engineering/`，推进方式放在 `process/`。

5. **原型不是正式实现**  
   原型用于验证高风险假设。原型结束后必须明确：保留、重写、继续验证或废弃。

6. **Done 不等于代码完成**  
   只有通过验收、测试、集成和文档更新后，任务才可以进入 Done。

7. **关键决策必须记录**  
   对架构、工具、流程和资产规范产生长期影响的决策，应留下简短记录。

8. **流程复杂度与项目规模匹配**  
   不为了“规范”而增加不必要步骤。流程应尽量轻量，但不能省略关键质量门槛。

9. **每个里程碑都进行复盘和清理**  
   清理临时代码、废弃资源、过时文档、未关闭任务和无效规范。

10. **文档必须可执行**  
    流程文档应能指导实际行动，而不是只描述抽象理念。

---

## 4. 推荐目录结构

```text
process/
├── README.md
│
├── workflow/
│   ├── feature-lifecycle.md
│   ├── bugfix-workflow.md
│   ├── asset-workflow.md
│   └── release-workflow.md
│
├── planning/
│   ├── roadmap-and-milestones.md
│   ├── backlog-management.md
│   └── task-breakdown.md
│
├── quality/
│   ├── definition-of-ready.md
│   ├── definition-of-done.md
│   ├── testing-checklist.md
│   └── review-checklist.md
│
├── tools/
│   ├── godot-workflow.md
│   ├── ai-asset-workflow.md
│   └── documentation-workflow.md
│
└── conventions/
    ├── git-conventions.md
    ├── naming-conventions.md
    └── decision-records.md
```

当前目录结构可以根据项目实际规模逐步增加，不要求一次性完成全部文件。

---

## 5. 各目录职责

## 5.1 `workflow/`

定义不同类型工作的完整生命周期。

### `feature-lifecycle.md`

描述一项功能从想法到发布的完整流程。

建议覆盖：

```text
Idea
→ Clarification
→ Ready
→ Design
→ Prototype
→ Task Breakdown
→ Implementation
→ Integration
→ Validation
→ Polish
→ Done
→ Release
→ Retrospective
```

这是 `process/` 中最核心的文档，其他流程应与它保持一致。

### `bugfix-workflow.md`

描述 Bug 从发现到关闭的过程，包括：

- 复现；
- 严重程度；
- 影响范围；
- 根因定位；
- 修复；
- 回归测试；
- 相邻系统检查；
- 是否需要补充自动化测试；
- 是否需要更新文档。

### `asset-workflow.md`

描述美术、UI、动画、特效和音频资产如何生成、处理、验收和导入。

对于本项目，重点包括：

- AI 生成资产；
- Prompt 管理；
- 风格一致性；
- 角色一致性；
- 透明背景；
- 贴纸白边；
- Sprite Sheet；
- 尺寸与 Pivot；
- Godot 导入参数；
- 源文件和最终文件归档。

### `release-workflow.md`

描述版本如何进入可发布状态，包括：

- 功能冻结；
- 回归测试；
- 存档兼容；
- 导出测试；
- 版本号；
- Changelog；
- 构建；
- 发布；
- 发布后验证；
- 回滚方案。

---

## 5.2 `planning/`

管理“做什么、何时做、先做什么”。

### `roadmap-and-milestones.md`

定义项目阶段与里程碑。

每个里程碑应至少包含：

- 目标；
- 玩家可体验到的结果；
- 必须完成的系统；
- 明确不包含的内容；
- 验收方式；
- 风险；
- 退出条件。

推荐以垂直切片组织，而不是以功能数量组织。

示例：

```text
Milestone 1：核心战斗闭环
Milestone 2：角色成长与装备
Milestone 3：城镇功能闭环
Milestone 4：内容扩展
Milestone 5：可发布 Demo
```

### `backlog-management.md`

定义需求如何进入、排序、延后和关闭。

建议状态：

```text
Inbox
Clarifying
Ready
In Progress
In Review
Testing
Done
Released
```

补充状态：

```text
Blocked
Deferred
Rejected
```

### `task-breakdown.md`

定义如何把大功能拆成可执行任务。

一个合格任务应尽量具备：

- 单一目标；
- 可独立验证；
- 明确输入和输出；
- 明确依赖；
- 明确完成标准；
- 规模足够小；
- 不同时混合设计、代码、资产和发布多个领域。

---

## 5.3 `quality/`

定义工作什么时候可以开始，以及什么时候才算完成。

### `definition-of-ready.md`

规定任务进入开发前必须满足的条件。

通常包括：

- 目标明确；
- 玩家价值明确；
- 功能范围明确；
- 不做什么已明确；
- 核心交互明确；
- 关键依赖已确认；
- 数据来源已确认；
- UI 或流程草图存在；
- 验收标准已写出；
- 不存在关键未决问题。

没有满足 Ready 的任务，不应直接进入正式开发。

### `definition-of-done.md`

规定任务进入 Done 前必须满足的条件。

通常包括：

- 功能实现；
- 验收标准通过；
- 集成完成；
- 无阻塞错误；
- UI 状态完整；
- 动画、音效和反馈完成；
- 存档行为正确；
- 不同分辨率验证完成；
- 真实导出包测试通过；
- 临时代码清理；
- 相关文档更新；
- 不留下未记录的技术债务。

### `testing-checklist.md`

定义通用测试维度：

- 正常流程；
- 失败流程；
- 边界值；
- 重复操作；
- 场景切换；
- 存档和读档；
- 暂停与时间缩放；
- 输入设备；
- 不同分辨率；
- 性能；
- 导出包；
- 回归测试。

### `review-checklist.md`

定义实现或资产评审时需要检查的内容：

- 是否满足需求；
- 是否符合架构；
- 是否产生隐藏依赖；
- 是否有重复逻辑；
- 是否使用临时硬编码；
- 是否需要测试；
- 是否影响存档；
- 是否影响旧版本；
- 是否更新文档；
- 是否符合命名和目录规范。

---

## 5.4 `tools/`

记录工具在本项目中的具体使用方式。

这里不重复官方教程，而是记录项目自己的工作约定。

### `godot-workflow.md`

建议记录：

- 固定 Godot 版本；
- Renderer；
- 项目启动方式；
- 场景目录；
- Resource 使用方式；
- Input Action 管理；
- Collision Layer 管理；
- 导出流程；
- 插件白名单；
- 引擎升级流程；
- 真实导出测试频率。

### `ai-asset-workflow.md`

建议记录：

- 哪些资产适合 AI 生成；
- 哪些资产需要手工修整；
- 基础风格 Prompt；
- 角色一致性规则；
- 动画帧一致性；
- 透明背景要求；
- 贴纸白边要求；
- 资产命名；
- 中间产物；
- Photoshop 或其他工具修整流程；
- Godot 导入和游戏内验收。

### `documentation-workflow.md`

建议记录：

- 文档放在哪个目录；
- 文档如何命名；
- 何时新建文档；
- 何时更新文档；
- 文档状态；
- 如何链接相关设计和实现；
- 何时归档；
- 如何处理过时信息。

---

## 5.5 `conventions/`

保存项目级一致性规范。

### `git-conventions.md`

建议包含：

- 分支策略；
- Commit 规范；
- 何时提交；
- 资源修改如何提交；
- 合并前检查；
- 引擎升级分支；
- 回滚流程；
- 禁止提交的敏感文件。

推荐 Commit 类型：

```text
feat: 新功能
fix: Bug 修复
refactor: 重构
docs: 文档
test: 测试
data: 数值和配置
art: 美术资源
audio: 音频资源
build: 构建和导出
chore: 非功能性维护
```

### `naming-conventions.md`

统一以下命名：

- 文件；
- 文件夹；
- 场景；
- 脚本；
- Resource；
- Godot Node；
- Signal；
- Input Action；
- Collision Layer；
- 资产；
- 版本；
- Git 分支。

### `decision-records.md`

记录长期影响项目的关键决策。

例如：

- 为什么选择 Godot；
- 为什么选择 GDScript；
- 为什么技能使用 Resource；
- 为什么角色使用组件化；
- 为什么画面基准为 1000×300；
- 为什么采用贴纸角色；
- 为什么使用某种存档格式；
- 为什么放弃某个插件或技术方案。

决策记录应说明：

```text
背景
问题
备选方案
最终决定
决定原因
影响
是否可逆
```

---

## 6. 统一工作状态

项目中的功能、Bug 和资产任务尽量使用统一状态。

```text
Inbox
→ Clarifying
→ Ready
→ In Progress
→ In Review
→ Testing
→ Done
→ Released
```

### 状态定义

#### Inbox

尚未分类或尚未确认的输入。

可能包括：

- 想法；
- 玩家反馈；
- Bug；
- 截图；
- 参考资料；
- 技术风险；
- 临时笔记。

#### Clarifying

正在澄清目标、范围、依赖和验收方式。

#### Ready

所有关键问题已经解决，可以直接进入开发。

Ready 不等于“以后想做”，而是“现在开始开发不会因为基础问题而停下来”。

#### In Progress

正在进行设计、实现、资产制作或集成。

#### In Review

正在进行代码、设计、资产或流程评审。

个人项目中也应保留该阶段，用于实现后的自我检查。

#### Testing

正在按照验收标准和测试清单验证。

#### Done

已满足 Definition of Done，可以进入待发布状态。

#### Released

已经进入玩家实际可运行的构建版本。

### 补充状态

#### Blocked

由于外部依赖、技术问题或未决事项无法继续。

Blocked 必须记录阻塞原因和解除条件。

#### Deferred

确认有价值，但当前里程碑不处理。

#### Rejected

明确决定不做，应记录原因，避免未来重复评估。

---

## 7. 文档命名规范

所有文件使用小写英文与连字符：

```text
feature-lifecycle.md
definition-of-done.md
ai-asset-workflow.md
```

避免：

```text
FeatureLifecycle.md
feature_lifecycle.md
功能生命周期.md
feature lifecycle.md
```

目录同样使用小写英文：

```text
workflow/
planning/
quality/
tools/
conventions/
```

---

## 8. 文档头部规范

除 `README.md` 外，流程文档建议包含统一元信息：

```markdown
# 文档标题

> Status: Draft  
> Owner: Project  
> Last Updated: YYYY-MM-DD  
> Applies To: 当前版本或适用范围

## Purpose

说明文档解决什么问题。
```

### 推荐状态

```text
Draft
Active
Under Review
Deprecated
Archived
```

#### Draft

内容尚在讨论，不应作为强制规则。

#### Active

当前项目正在使用的规则。

#### Under Review

正在修改，旧规则仍可能部分有效。

#### Deprecated

不再推荐使用，但暂时保留用于历史参考。

#### Archived

只用于历史记录，不再参与当前流程。

---

## 9. 文档内容规范

每份流程文档至少应回答：

1. **Purpose**  
   为什么存在这份流程。

2. **Scope**  
   适用于什么，不适用于什么。

3. **Inputs**  
   进入流程前需要什么。

4. **Stages**  
   流程由哪些阶段组成。

5. **Outputs**  
   流程结束后产生什么。

6. **Entry Criteria**  
   进入每个阶段的条件。

7. **Exit Criteria**  
   离开每个阶段的条件。

8. **Roles**  
   谁负责决定、执行和验收。个人项目中也可以写成角色而不是人名。

9. **Checklists**  
   实际执行时需要逐项检查的内容。

10. **Exceptions**  
    哪些情况允许简化流程。

11. **Related Documents**  
    相关设计、工程和质量文档。

---

## 10. Process 与 Inbox 的关系

`inbox/` 是输入入口，`process/` 是处理规则。

推荐流转：

```text
新想法 / Bug / 参考资料
        ↓
      inbox/
        ↓
分类与澄清
        ↓
design/、engineering/ 或 process/
        ↓
Backlog / Ready
        ↓
进入对应生命周期
```

### Inbox 不应长期堆积

建议定期处理：

- 删除无价值内容；
- 合并重复内容；
- 将设计想法移动到 `design/`；
- 将技术问题移动到 `engineering/`；
- 将流程问题移动到 `process/`；
- 将可执行工作进入 Backlog；
- 将未决定内容标记为 Deferred。

---

## 11. Process 与 Design / Engineering 的链接方式

流程文档不重复设计和工程文档内容，而是通过链接关联。

示例：

```markdown
## Related Design

- `design/systems/combat/target-selection.md`
- `design/systems/combat/status-effects.md`

## Related Engineering

- `engineering/architecture/combat-components.md`
- `engineering/patterns/resource-strategy.md`

## Related Quality

- `process/quality/testing-checklist.md`
- `process/quality/definition-of-done.md`
```

这样可以避免同一规则在多个位置重复维护。

---

## 12. 角色定义

即使是个人项目，也建议用“角色”描述责任。

| 角色 | 责任 |
|---|---|
| Product / Design | 定义目标、玩家价值、范围和验收结果 |
| Engineering | 定义技术方案、实现、集成和技术风险 |
| Art / UI | 定义资产需求、风格、导入和表现质量 |
| QA | 验证验收标准、边界情况和回归问题 |
| Release | 管理版本、构建、导出和发布 |
| Project Owner | 处理优先级、范围冲突和最终决策 |

同一个人可以承担全部角色，但每个阶段仍应明确“当前正在以哪个角色进行判断”。

这有助于避免：

- 开发者视角替代玩家视角；
- 为了实现方便而改变玩法目标；
- 自己写完代码后跳过测试；
- 只检查技术正确性，不检查体验。

---

## 13. 轻量流程与完整流程

并非所有任务都需要完整生命周期。

### 可使用轻量流程的情况

- 文案修正；
- 单个图标替换；
- 明确且低风险的小 Bug；
- 不影响存档和架构的小调整；
- 开发工具改进；
- 文档拼写修正。

轻量流程：

```text
Clarify
→ Implement
→ Verify
→ Done
```

### 必须使用完整流程的情况

- 新系统；
- 核心玩法；
- 存档结构变化；
- 架构调整；
- 大范围 UI 重构；
- 战斗规则变化；
- 新资产管线；
- 发布版本；
- 引擎升级；
- 跨多个模块的改动。

---

## 14. 流程变更规则

流程本身也需要版本和维护。

当准备修改流程时，应回答：

- 当前流程造成了什么问题；
- 哪一步成本过高；
- 哪一步经常被跳过；
- 哪一步没有产生价值；
- 新流程如何减少返工；
- 是否会影响已有任务；
- 是否需要迁移旧文档。

不要因为一次特殊情况就立即重写整个流程。应先确认问题是否重复出现。

---

## 15. 维护节奏

建议在以下时间检查 `process/`：

### 每周或每个开发周期

- 清理 Inbox；
- 更新正在进行的任务状态；
- 处理 Blocked；
- 检查 Ready 队列；
- 更新短期优先级。

### 每个里程碑结束

- 复盘返工原因；
- 更新 Definition of Done；
- 清理失效规范；
- 检查是否出现新的共性问题；
- 更新测试清单；
- 归档废弃流程；
- 检查文档与实际工作是否一致。

### 每次发布后

- 检查发布流程遗漏；
- 记录构建问题；
- 更新平台检查项；
- 更新存档和版本策略；
- 记录需要自动化的步骤。

---

## 16. 当前优先建设顺序

`process/` 建议按以下顺序逐步完善：

### Phase 1：建立主干

1. `workflow/feature-lifecycle.md`
2. `quality/definition-of-ready.md`
3. `quality/definition-of-done.md`
4. `planning/backlog-management.md`

### Phase 2：建立质量控制

5. `quality/testing-checklist.md`
6. `quality/review-checklist.md`
7. `workflow/bugfix-workflow.md`

### Phase 3：建立资产与工具流程

8. `workflow/asset-workflow.md`
9. `tools/ai-asset-workflow.md`
10. `tools/godot-workflow.md`

### Phase 4：建立版本与长期维护

11. `workflow/release-workflow.md`
12. `planning/roadmap-and-milestones.md`
13. `conventions/git-conventions.md`
14. `conventions/decision-records.md`

---

## 17. 当前下一步

本目录下一份需要深入定义的文档是：

```text
process/workflow/feature-lifecycle.md
```

它将详细说明：

- 功能从哪里进入；
- 每个阶段的目标；
- 每个阶段的输入和输出；
- Entry Criteria；
- Exit Criteria；
- 原型与正式实现的边界；
- 验收与 Polish 的区别；
- Done 与 Released 的区别；
- 什么时候允许使用轻量流程；
- 功能生命周期模板。

`feature-lifecycle.md` 将作为后续 Backlog、Definition of Ready、Definition of Done、测试和发布流程的主干文档。
