# Git Conventions（Git 使用规范）

> 状态：草案  
> 适用范围：通用的软件、游戏、媒体与数字产品项目  
> 目的：定义 Git 仓库中的分支、提交、合并、评审、发布、回滚、大文件与敏感信息管理规范。

---

## 1. 文档目的

Git Conventions 用于统一项目中的版本控制方式，确保代码、文档、配置、资源和发布记录能够：

- 被清楚追踪；
- 被安全评审；
- 被稳定合并；
- 被快速回滚；
- 被准确关联到 Feature、Bug 和 Release；
- 减少冲突与误提交；
- 避免敏感信息泄露；
- 支持多人协作；
- 支持自动化构建与发布；
- 保持长期可维护的提交历史。

它回答以下问题：

- 分支应如何命名；
- 什么时候需要创建分支；
- Commit 应多大；
- Commit Message 如何写；
- Merge、Rebase 和 Squash 如何选择；
- Pull Request 应包含什么；
- 如何处理冲突；
- 如何管理 Release Branch 和 Hotfix；
- 如何回滚错误改动；
- 二进制资源和大文件如何管理；
- 哪些文件不应提交；
- 如何保护主分支；
- 如何保持历史清楚。

Git 规范的目标不是追求“完美提交历史”。

它的目标是：

> 建立一套清晰、稳定、可审计、可恢复的协作方式，让每一次变更都能被理解、评审、验证和安全交付。

---

## 2. 核心原则

### 2.1 每个提交应表达一个清晰意图

一个 Commit 应回答：

```text
这次改动解决了什么问题？
为什么需要它？
它影响了什么？
```

不推荐：

```text
update
fix stuff
changes
misc
```

推荐：

```text
fix(save): prevent duplicate migration on repeated startup
feat(inventory): add stack limit validation
docs(process): add release rollback checklist
```

---

### 2.2 提交应尽量小而完整

好的提交应：

- 目标单一；
- 可以独立理解；
- 可以独立评审；
- 可以独立回滚；
- 不混入无关格式化；
- 不混入临时文件；
- 不破坏基本构建。

提交过大时会导致：

- Review 困难；
- 冲突增加；
- 回滚困难；
- 问题定位困难；
- 历史不可读。

提交过小时会导致：

- 大量机械提交；
- 失去业务意义；
- 历史噪音过多。

---

### 2.3 主分支必须保持可用

主分支应尽量保持：

- 可构建；
- 可运行；
- 可测试；
- 无已知 Blocker；
- 可以生成目标环境构建。

不应将明显未完成、无法运行或破坏性改动直接推入主分支。

---

### 2.4 历史应服务于协作和恢复

Git 历史最重要的用途：

- 理解变更；
- 查找原因；
- 定位缺陷；
- 回滚；
- 生成 Release；
- 审计；
- 迁移；
- 复盘。

历史不应只服务于“看起来整齐”。

---

### 2.5 不提交无法安全共享的内容

禁止提交：

- 密钥；
- Token；
- 密码；
- 私钥；
- 生产证书；
- 用户数据；
- 客户数据；
- 个人敏感信息；
- 本地机器配置；
- 临时调试文件；
- 未授权资源；
- 超大无必要文件。

---

## 3. 仓库结构

一个仓库应有：

```text
README
.gitignore
LICENSE（如适用）
贡献或开发说明
构建说明
版本说明
主要目录结构
```

可选：

```text
CONTRIBUTING.md
CODEOWNERS
CHANGELOG.md
SECURITY.md
docs/
scripts/
tools/
```

---

## 4. 主分支命名

推荐使用：

```text
main
```

也可以使用：

```text
master
trunk
```

项目应统一，不应同时存在多个“主分支”。

主分支应代表：

- 当前稳定开发基线；
- 可持续集成的来源；
- 默认 Pull Request 目标；
- Release 或 Release Branch 的来源。

---

## 5. 分支策略

常见策略：

```text
Trunk-Based Development
Feature Branch Workflow
Git Flow
Release Branch Workflow
```

本规范推荐：

```text
短生命周期 Feature Branch
+ 稳定 main
+ 按需使用 release/*
+ 紧急使用 hotfix/*
```

---

## 6. 分支类型

推荐分支前缀：

```text
feature/
bugfix/
hotfix/
release/
refactor/
docs/
tool/
chore/
experiment/
upgrade/
```

---

## 6.1 Feature Branch

用于新功能。

格式：

```text
feature/<topic>
```

示例：

```text
feature/save-slots
feature/inventory-filter
feature/gamepad-remapping
```

---

## 6.2 Bugfix Branch

用于普通缺陷修复。

```text
bugfix/<issue-or-topic>
```

示例：

```text
bugfix/save-migration-loop
bugfix/pause-menu-input
```

---

## 6.3 Hotfix Branch

用于生产环境紧急修复。

```text
hotfix/<issue-or-version>
```

示例：

```text
hotfix/2.4.1-save-corruption
```

Hotfix 应：

- 范围最小；
- 快速评审；
- 快速验证；
- 同步回主线；
- 必要时同步到 Release Branch。

---

## 6.4 Release Branch

用于稳定某个即将发布的版本。

```text
release/<version>
```

示例：

```text
release/2.4.0
```

Release Branch 中通常只允许：

- Bugfix；
- 文档；
- 版本号；
- 发布配置；
- 迁移修复；
- Release Note。

不应继续加入新 Feature。

---

## 6.5 Refactor Branch

用于不改变外部行为的结构调整。

```text
refactor/<topic>
```

---

## 6.6 Docs Branch

用于文档变更。

```text
docs/<topic>
```

---

## 6.7 Tool Branch

用于内部工具、构建脚本、编辑器工具。

```text
tool/<topic>
```

---

## 6.8 Upgrade Branch

用于高风险版本升级。

```text
upgrade/<dependency-or-engine>
```

例如：

```text
upgrade/godot-4-5
upgrade/postgresql-17
```

---

## 6.9 Experiment Branch

用于技术探索。

```text
experiment/<topic>
```

实验分支不应默认合并。

实验结论应转化为：

- ADR；
- Spike 结果；
- 正式 Feature；
- 正式实现分支。

---

## 7. 分支命名规则

分支名应：

- 使用小写；
- 使用短横线；
- 表达目标；
- 避免人名；
- 避免模糊词；
- 必要时包含 Issue ID。

推荐：

```text
feature/123-save-slots
bugfix/456-login-timeout
```

不推荐：

```text
sihao-work
test2
new-branch
final
my-fix
```

---

## 8. 分支生命周期

推荐流程：

```text
Create
→ Develop
→ Rebase / Update
→ Open PR
→ Review
→ Test
→ Merge
→ Delete
```

分支应尽量短生命周期。

长期分支会带来：

- 冲突；
- 漂移；
- 重复工作；
- 集成风险；
- 难以回收。

---

## 9. 创建分支前

建议先：

```bash
git checkout main
git pull --ff-only
git checkout -b feature/example
```

创建前确认：

- 基于正确分支；
- main 已更新；
- 分支目标明确；
- 不与已有工作重复；
- 相关 Issue 已存在或已确认。

---

## 10. Commit Message 规范

推荐使用 Conventional Commits 风格：

```text
<type>(<scope>): <summary>
```

例如：

```text
feat(save): add manual save slots
fix(ui): prevent hidden panel from receiving input
docs(process): add bugfix workflow
refactor(audio): extract volume persistence service
```

---

## 11. Commit Type

推荐类型：

```text
feat
fix
docs
refactor
test
perf
build
ci
chore
style
revert
```

---

## 11.1 feat

新增功能。

```text
feat(input): add controller remapping
```

---

## 11.2 fix

修复缺陷。

```text
fix(save): avoid duplicate migration
```

---

## 11.3 docs

仅文档变化。

```text
docs(api): document pagination errors
```

---

## 11.4 refactor

不改变外部行为的重构。

```text
refactor(inventory): extract item sorting policy
```

---

## 11.5 test

新增或修改测试。

```text
test(save): cover corrupted data recovery
```

---

## 11.6 perf

性能改进。

```text
perf(level): cache navigation queries
```

---

## 11.7 build

构建系统、依赖、打包。

```text
build(godot): add Windows release export preset
```

---

## 11.8 ci

持续集成。

```text
ci(test): run headless checks on pull requests
```

---

## 11.9 chore

维护性工作。

```text
chore(repo): update ignore rules
```

---

## 11.10 style

不改变逻辑的格式调整。

```text
style(gdscript): apply formatter
```

不要将“视觉样式修改”误用为 `style`。

视觉样式通常仍属于：

```text
feat(ui)
fix(ui)
```

---

## 11.11 revert

回滚提交。

```text
revert: feat(save): add cloud sync
```

---

## 12. Scope

Scope 用于表示影响模块。

例如：

```text
save
inventory
ui
audio
build
docs
release
input
combat
```

Scope 应：

- 稳定；
- 可理解；
- 与项目模块对应；
- 不过度细化。

---

## 13. Summary

Summary 应：

- 使用祈使或当前时态；
- 简短；
- 描述结果；
- 不以句号结尾；
- 避免模糊词。

推荐：

```text
fix(save): preserve slot metadata during migration
```

不推荐：

```text
fixed a bug
update save system
changes
```

---

## 14. Commit Body

复杂提交建议补充 Body：

```text
fix(save): preserve slot metadata during migration

The old migration replaced the entire save header when optional metadata
was missing. Merge missing fields instead of overwriting the header.

Refs: BUG-142
```

Body 可说明：

- 背景；
- 原因；
- 实现选择；
- 风险；
- 兼容性；
- 测试；
- Issue。

---

## 15. Breaking Change

重大不兼容变更应明确：

```text
feat(api)!: replace legacy inventory response
```

或在 Body 中加入：

```text
BREAKING CHANGE:
The `items` field now returns grouped stack objects.
```

---

## 16. 一个 Commit 应包含什么

一个良好 Commit 通常包含：

- 完整的小型目标；
- 必要代码；
- 必要测试；
- 必要文档；
- 必要迁移；
- 必要配置。

不应故意拆成：

```text
先提交代码
再提交修复构建
再提交测试
再提交文档
```

如果这些内容共同构成一个完整目标，可以放在同一 Commit。

---

## 17. 不应混入同一 Commit 的内容

- 无关格式化；
- 多个独立 Feature；
- 多个不相关 Bug；
- 引擎升级与业务变更；
- 大规模重命名与功能修改；
- 资源批量替换与逻辑修改；
- 自动生成文件与手写逻辑混杂。

---

## 18. 暂存改动

推荐使用分块暂存：

```bash
git add -p
```

这样可以：

- 分离无关改动；
- 拆出清晰提交；
- 避免误提交调试代码。

---

## 19. 提交前检查

```markdown
- [ ] 目标单一；
- [ ] Diff 已检查；
- [ ] 无临时调试代码；
- [ ] 无敏感信息；
- [ ] 无无关文件；
- [ ] 基本构建通过；
- [ ] 必要测试通过；
- [ ] Commit Message 清楚；
```

---

## 20. Pull Request

Pull Request 用于：

- Review；
- 自动化检查；
- 讨论；
- 决策；
- 合并；
- 审计。

PR 不应只是“请求点击 Merge”。

---

## 21. PR 应包含

```text
Summary
Why
Scope
Non-goals
Changes
Test Evidence
Screenshots / Video
Risks
Data Impact
Migration
Performance
Security
Rollback
Related Issues
```

---

## 22. PR 标题

PR 标题建议与 Commit 规范一致：

```text
feat(save): add manual save slots
fix(input): stop hidden menu from consuming events
```

---

## 23. PR 描述模板

```markdown
## Summary

## Why

## Scope

## Non-goals

## Changes

## Test Evidence

## Screenshots / Videos

## Data / Migration Impact

## Performance Impact

## Security Impact

## Risks

## Rollback

## Related Issues

## Checklist

- [ ] Scope 清楚；
- [ ] 测试通过；
- [ ] 文档已更新；
- [ ] 无敏感信息；
- [ ] 无无关改动；
- [ ] 回滚方式明确。
```

---

## 24. PR 大小

PR 应尽量保持可评审。

过大的信号：

- 数千行无关变更；
- 多个目标；
- 多个模块；
- 同时升级依赖；
- 同时重构；
- 无法一次理解；
- Reviewer 需要多天才能完成。

可以拆分为：

```text
Preparation PR
Data Model PR
Core Logic PR
UI Integration PR
Cleanup PR
```

前提是每个 PR 仍然安全、可理解。

---

## 25. Draft PR

适合：

- 提前共享方向；
- 请求早期反馈；
- 展示进行中设计；
- 运行 CI；
- 暴露依赖。

Draft PR 不应被误认为已准备合并。

---

## 26. Review 要求

Reviewer 应检查：

- 目标；
- 正确性；
- Scope；
- 架构；
- 安全；
- 数据；
- 性能；
- 测试；
- 回滚；
- 文档。

Review 意见应标记：

```text
Blocker
Required
Suggestion
Question
Nit
Praise
```

---

## 27. PR 合并前检查

```markdown
- [ ] Review 已完成；
- [ ] Required 已解决；
- [ ] CI 通过；
- [ ] 测试证据完整；
- [ ] 分支已更新；
- [ ] 无未解决冲突；
- [ ] Scope 未失控；
- [ ] 文档已更新；
- [ ] 数据迁移已验证；
- [ ] Merge 方式已确认；
```

---

## 28. Merge、Squash 与 Rebase

---

## 28.1 Merge Commit

保留完整分支历史。

适合：

- 多个有意义提交；
- 需要保留分支上下文；
- Release Branch；
- 长期协作分支。

优点：

- 历史完整；
- 分支边界清楚。

缺点：

- 历史可能复杂；
- 噪音较多。

---

## 28.2 Squash Merge

将一个 PR 合并成单个 Commit。

适合：

- 短生命周期 Feature Branch；
- 分支中有 WIP、fixup；
- 希望主分支保持简洁；
- PR 本身是一个独立目标。

优点：

- 主分支历史清楚；
- 回滚简单。

缺点：

- 丢失内部提交粒度；
- 不适合保留关键演进过程。

---

## 28.3 Rebase Merge

将提交线性地放到主分支。

适合：

- 每个 Commit 都已经清理；
- 每个 Commit 都有独立价值；
- 团队熟悉 Rebase。

优点：

- 历史线性；
- 保留提交。

风险：

- 重写历史；
- 容易误操作；
- 共享分支上不适合随意 Rebase。

---

## 28.4 推荐策略

对于多数短 Feature Branch：

```text
Squash Merge
```

对于有高质量独立提交的变更：

```text
Rebase Merge
```

对于 Release、Hotfix 同步：

```text
Merge Commit
```

项目应统一默认策略。

---

## 29. Rebase 规范

Rebase 适合：

- 更新个人分支；
- 清理本地提交；
- 合并前整理历史。

不应对已被多人使用的共享分支随意强制 Rebase。

常用：

```bash
git fetch origin
git rebase origin/main
```

---

## 30. Force Push

只允许在明确情况下使用：

```bash
git push --force-with-lease
```

不推荐使用：

```bash
git push --force
```

`--force-with-lease` 可以减少覆盖他人更新的风险。

禁止对以下分支 Force Push：

- main；
- release；
- hotfix 共享分支；
- 受保护分支。

---

## 31. 更新分支

推荐：

```bash
git fetch origin
git rebase origin/main
```

或：

```bash
git merge origin/main
```

团队应统一方式，避免同一分支反复混用。

---

## 32. 冲突处理

冲突处理应：

1. 理解双方变更；
2. 不只选择 `ours` 或 `theirs`；
3. 重新运行测试；
4. 检查资源和配置；
5. 检查是否丢失行为；
6. 检查最终 Diff；
7. 必要时请求原作者协助。

---

## 33. 二进制冲突

常见于：

- 图片；
- 音频；
- 视频；
- PSD；
- 3D 文件；
- Godot 二进制资源。

降低方式：

- 明确 Owner；
- 避免多人同时编辑；
- 锁定文件；
- 拆小资产；
- 使用文本格式；
- 使用 Git LFS；
- 提前协调。

---

## 34. Git LFS

适合：

- 大型图片；
- 音频；
- 视频；
- 3D 模型；
- PSD；
- 二进制源文件；
- 大型数据集。

不应盲目将所有资源都放入 LFS。

使用前确认：

- 仓库平台额度；
- Clone 成本；
- CI 支持；
- 备份；
- 迁移；
- 锁文件能力。

---

## 35. LFS 跟踪示例

```bash
git lfs track "*.psd"
git lfs track "*.wav"
git lfs track "*.blend"
```

需要提交：

```text
.gitattributes
```

---

## 36. 大文件规范

提交前应检查：

- 是否真的需要进入 Git；
- 是否可压缩；
- 是否可生成；
- 是否应放 Artifact Storage；
- 是否应放对象存储；
- 是否应使用 LFS；
- 是否包含中间文件。

不应提交：

- 构建产物；
- 导出包；
- 安装包；
- 缓存；
- 临时视频；
- 重复源文件。

---

## 37. `.gitignore`

应忽略：

- 编辑器缓存；
- 构建目录；
- 导出目录；
- 临时文件；
- 日志；
- 本地配置；
- 密钥；
- 操作系统文件；
- IDE 文件；
- 依赖缓存。

示例：

```text
build/
export/
dist/
tmp/
logs/
.env
.env.*
*.log
.DS_Store
Thumbs.db
```

具体规则应根据工具调整。

---

## 38. `.gitattributes`

可用于：

- 行尾；
- Diff；
- Merge；
- LFS；
- 文本识别；
- 二进制识别。

示例：

```text
*.gd text eol=lf
*.md text eol=lf
*.png binary
*.wav binary
```

---

## 39. 行尾规范

推荐统一：

```text
LF
```

尤其是跨平台项目。

可以使用：

```text
.editorconfig
.gitattributes
```

避免因 CRLF / LF 产生大量无意义 Diff。

---

## 40. 文件编码

推荐：

```text
UTF-8
```

避免同一仓库混用：

- UTF-8；
- UTF-16；
- 本地编码；
- 带 BOM 与不带 BOM 的混乱。

---

## 41. 敏感信息

禁止提交：

- `.env`；
- API Key；
- Secret；
- 私钥；
- Signing Key；
- 生产数据库地址；
- 真实账户；
- 用户数据；
- OAuth Secret。

应使用：

- 环境变量；
- Secret Manager；
- CI Secret；
- 本地未追踪配置；
- 示例配置文件。

---

## 42. 示例配置

推荐提交：

```text
.env.example
config.example.json
```

示例中只包含：

- 字段；
- 占位值；
- 说明。

不要包含真实 Secret。

---

## 43. Secret 泄露处理

如果敏感信息已提交：

1. 立即撤销或轮换；
2. 不要只删除最新 Commit；
3. 评估历史泄露；
4. 清理 Git 历史；
5. 通知相关 Owner；
6. 更新 Secret 管理方式；
7. 记录 Incident。

仅执行：

```text
git rm
```

不能消除历史中的 Secret。

---

## 44. 生成文件

自动生成文件是否提交，应明确。

可以提交：

- 运行时必须；
- 消费者无法生成；
- 构建环境需要；
- 版本差异有意义。

不建议提交：

- 可稳定重建；
- 构建产物；
- 缓存；
- 本地临时生成；
- 巨大无价值 Diff。

---

## 45. 依赖锁文件

通常应提交：

- `package-lock.json`；
- `pnpm-lock.yaml`；
- `yarn.lock`；
- 其他语言锁文件。

锁文件用于：

- 固定依赖；
- 可重复构建；
- 安全审计；
- CI 一致性。

---

## 46. 依赖升级

依赖升级建议单独 PR。

应包含：

- 版本变化；
- Changelog；
- Breaking Change；
- 测试结果；
- 安全影响；
- 回滚方式。

不要将大型依赖升级混入普通 Feature。

---

## 47. Tag

Tag 用于标记：

- Release；
- 里程碑；
- 稳定基线；
- 迁移点。

推荐：

```text
v2.4.0
v2.4.1
```

预发布：

```text
v2.4.0-rc.1
v2.4.0-beta.2
```

---

## 48. Annotated Tag

推荐发布使用 Annotated Tag：

```bash
git tag -a v2.4.0 -m "Release v2.4.0"
git push origin v2.4.0
```

---

## 49. Release Commit

Release Commit 可包含：

- 版本号；
- Changelog；
- Release Note；
- 配置；
- 锁文件；
- 迁移版本。

不应包含未评审的新功能。

---

## 50. Release Branch 流程

```text
main
→ create release/2.4.0
→ freeze
→ fix release blockers
→ build RC
→ verify
→ tag
→ release
→ merge fixes back to main
→ delete branch when safe
```

---

## 51. Hotfix 流程

```text
released tag / production branch
→ create hotfix/*
→ minimal fix
→ focused review
→ test
→ release
→ tag patch version
→ merge back to main
→ merge to active release branch
```

---

## 52. Revert

优先使用：

```bash
git revert <commit>
```

而不是重写已共享历史。

Revert 优点：

- 保留历史；
- 可审计；
- 适合主分支；
- 适合生产修复。

---

## 53. Reset

`git reset` 适合本地未共享历史。

常见模式：

```bash
git reset --soft
git reset --mixed
git reset --hard
```

`--hard` 会丢失未提交内容，应谨慎使用。

禁止用 Reset 重写已共享主分支历史。

---

## 54. Cherry-pick

适合：

- 将 Hotfix 同步到其他分支；
- 将明确独立提交移植；
- 发布分支选择性修复。

风险：

- 重复提交；
- 历史分叉；
- 后续合并冲突；
- 漏同步。

应记录 Cherry-pick 来源。

---

## 55. Bisect

用于定位引入问题的 Commit。

```bash
git bisect start
git bisect bad
git bisect good <known-good-tag>
```

Bisect 更有效的前提：

- 每个 Commit 可构建；
- 提交目标单一；
- 测试可自动运行。

---

## 56. Stash

`git stash` 适合短期保存未完成改动。

不适合：

- 长期存储；
- 团队共享；
- 代替 Commit；
- 保存关键工作。

Stash 应及时恢复或清理。

---

## 57. Worktree

`git worktree` 适合：

- 同时维护多个分支；
- 快速处理 Hotfix；
- 保持当前工作不被打断；
- 并行构建 Release。

---

## 58. 主分支保护

建议启用：

- 禁止直接 Push；
- 必须通过 PR；
- 必须通过 CI；
- 必须 Review；
- 禁止 Force Push；
- 禁止删除；
- 必须解决对话；
- 必须保持分支更新；
- 必须通过安全检查。

---

## 59. CODEOWNERS

适合定义：

- 关键模块 Reviewer；
- 安全文件 Owner；
- 构建配置 Owner；
- 数据迁移 Owner；
- 发布配置 Owner。

示例：

```text
/docs/process/ @process-owner
/build/ @build-owner
/security/ @security-owner
```

---

## 60. CI 检查

PR 可要求：

- Lint；
- Format；
- Unit Test；
- Integration Test；
- Build；
- Export；
- Link Check；
- Secret Scan；
- Dependency Scan；
- License Check；
- Artifact Validation。

---

## 61. Commit 签名

高安全项目可要求：

- GPG；
- SSH Signing；
- Verified Commit；
- Verified Tag。

用于确认提交来源。

---

## 62. Fork Workflow

适合外部贡献者。

流程：

```text
Fork
→ Branch
→ Commit
→ Push
→ Pull Request
→ Review
→ Merge
```

需要明确：

- 贡献协议；
- License；
- DCO / CLA；
- 分支规则；
- 测试要求。

---

## 63. Monorepo

Monorepo 需要额外规范：

- Scope；
- CODEOWNERS；
- 依赖边界；
- 构建缓存；
- 受影响测试；
- 发布版本；
- 包管理；
- 路径过滤。

Commit Scope 应能清楚表示影响子项目。

---

## 64. Submodule

Git Submodule 适合：

- 独立版本库；
- 明确外部依赖；
- 需要固定 Commit。

风险：

- Clone 不完整；
- CI 配置；
- 更新复杂；
- 开发者忘记初始化；
- 权限问题。

没有明确必要时，不应优先使用 Submodule。

---

## 65. Subtree

Subtree 可用于整合外部仓库，同时避免 Submodule 使用复杂度。

使用前应明确：

- 更新方式；
- 上游同步；
- 历史保留；
- 冲突处理。

---

## 66. 文档与 Git

文档应：

- 与实现一起提交；
- 使用清楚 Commit；
- 通过 Review；
- 更新索引；
- 避免复制冲突；
- 保持链接有效。

---

## 67. 资产与 Git

资产提交应：

- 使用规范命名；
- 避免无意义重复导出；
- 保留源文件；
- 使用 LFS；
- 明确授权；
- 检查文件大小；
- 检查引用；
- 避免一次提交大量无关资产。

---

## 68. Godot 项目注意事项

通常提交：

- `.gd`；
- `.tscn`；
- `.tres`；
- `project.godot`；
- 原始资产；
- 插件源码；
- Export Preset（不含 Secret）。

通常忽略：

- `.godot/`；
- 导出产物；
- 本地缓存；
- 调试日志；
- 本地证书；
- 私密配置。

大型 `.tscn` 冲突应通过：

- 拆场景；
- 提前协调；
- 减少无关重排；
- 检查文本 Diff。

---

## 69. Commit 拆分示例

不推荐：

```text
feat: inventory update
```

其中同时包含：

- 新 UI；
- 存档迁移；
- 音频；
- 文档；
- 引擎升级；
- 格式化全仓库。

推荐拆为：

```text
refactor(inventory): extract stack policy
feat(inventory): add item stack limits
feat(ui): show stack capacity
test(inventory): cover stack merge boundaries
docs(inventory): document stack rules
```

---

## 70. PR 拆分示例

大型 Feature 可以拆成：

```text
PR 1：数据模型与接口
PR 2：核心逻辑
PR 3：UI 集成
PR 4：存档迁移
PR 5：测试与文档
```

拆分原则：

- 每个 PR 有明确目标；
- 不破坏主分支；
- 依赖清楚；
- 能独立评审；
- 能安全合并。

---

## 71. WIP Commit

本地可以使用：

```text
WIP
fixup!
squash!
```

但合并前应：

- Squash；
- Reword；
- 清理历史；
- 确保主分支没有 WIP Commit。

---

## 72. Autosquash

可以使用：

```bash
git commit --fixup <commit>
git rebase -i --autosquash origin/main
```

适合清理 Review 修改。

---

## 73. Commit History 清理

合并前可以：

- Reword；
- Squash；
- Fixup；
- Reorder；
- Drop。

目标是让提交历史：

- 可理解；
- 可构建；
- 有逻辑；
- 不包含临时噪音。

---

## 74. Git Hook

可使用 Hook 自动检查：

- 格式；
- Lint；
- 测试；
- Commit Message；
- Secret；
- 大文件；
- 生成文件；
- 文档链接。

Hook 不应：

- 运行时间过长；
- 依赖不可用环境；
- 与 CI 结果不一致。

CI 仍应是最终检查来源。

---

## 75. Pre-commit Checklist

```markdown
- [ ] 已检查 Diff；
- [ ] 无调试代码；
- [ ] 无敏感信息；
- [ ] 无巨大无关文件；
- [ ] 文件命名正确；
- [ ] 格式通过；
- [ ] 测试通过；
- [ ] Commit Message 正确；
```

---

## 76. Pre-merge Checklist

```markdown
- [ ] PR 目标清楚；
- [ ] Scope 没有失控；
- [ ] Review 已完成；
- [ ] CI 通过；
- [ ] 测试证据完整；
- [ ] 文档已更新；
- [ ] Migration 已验证；
- [ ] Rollback 已说明；
- [ ] 无未解决冲突；
- [ ] Merge 方式正确；
```

---

## 77. Release Git Checklist

```markdown
- [ ] Release Branch 正确；
- [ ] Scope 已冻结；
- [ ] RC Commit 已固定；
- [ ] Version 已更新；
- [ ] Changelog 已更新；
- [ ] Release Build 通过；
- [ ] Tag 已创建；
- [ ] Tag 已推送；
- [ ] Hotfix 已同步；
- [ ] Release Branch 关闭策略已确认；
```

---

## 78. Git Incident Checklist

当发生错误 Push、Secret 泄露或历史损坏时：

```markdown
- [ ] 停止继续 Push；
- [ ] 确认影响范围；
- [ ] 通知相关 Owner；
- [ ] 保护主分支；
- [ ] 撤销 Secret；
- [ ] 选择 Revert 或历史清理；
- [ ] 验证远端状态；
- [ ] 验证 CI 和 Release；
- [ ] 记录 Incident；
- [ ] 更新预防措施；
```

---

## 79. 常见失败模式

### 79.1 直接在 main 上开发

结果：

- 未完成内容进入主线；
- 无 Review；
- 难以回滚；
- CI 被破坏。

---

### 79.2 一个 Commit 包含所有内容

结果：

- 无法理解；
- 无法局部回滚；
- Review 困难；
- Bisect 困难。

---

### 79.3 Commit Message 过于模糊

例如：

```text
update
fix
changes
```

结果：

- 无法理解历史；
- Changelog 无法生成；
- Bug 定位困难。

---

### 79.4 长期不更新分支

结果：

- 大量冲突；
- 集成风险；
- 重复工作。

---

### 79.5 在共享分支 Force Push

结果：

- 他人提交丢失；
- 历史混乱；
- CI 和 PR 失效。

---

### 79.6 Secret 删除后认为安全

Secret 即使从最新版本删除，仍可能存在于：

- Git 历史；
- Fork；
- Clone；
- CI Log；
- Cache；
- Artifact。

必须轮换。

---

### 79.7 构建产物进入 Git

结果：

- 仓库膨胀；
- Diff 无意义；
- Merge 冲突；
- Clone 缓慢。

---

### 79.8 二进制资源不使用 LFS

结果：

- 仓库迅速增大；
- 历史无法清理；
- Clone 成本高。

---

### 79.9 Hotfix 没有合回主线

结果：

- 后续版本重新出现问题；
- 分支行为不一致。

---

### 79.10 依赖升级混入业务 PR

结果：

- 问题来源不清；
- 回滚困难；
- Review 范围扩大。

---

### 79.11 PR 只有代码，没有上下文

结果：

- Reviewer 不理解目标；
- 评审变慢；
- 风险被遗漏。

---

### 79.12 所有 Review 修改都新增无意义 Commit

例如：

```text
fix review
fix review again
final fix
```

应在合并前清理。

---

## 80. Git 指标

可参考：

- PR Cycle Time；
- Review Time；
- Merge Conflict Rate；
- Revert Rate；
- Hotfix Rate；
- Failed CI Rate；
- PR Size；
- Commit Size；
- Direct Push Count；
- Secret Incident Count；
- Long-lived Branch Count；
- Reopened PR Count；
- Release Tag Error Count。

指标应帮助改善：

- 协作；
- Review；
- 稳定性；
- 交付速度；
- 恢复能力。

不应只追求：

- 更多 Commit；
- 更短 PR；
- 更少分支；
- 更快 Merge。

---

## 81. 默认推荐策略

对于中小型项目，推荐：

```text
main 受保护
短生命周期 feature/*
PR Review
CI 必须通过
默认 Squash Merge
release/* 按需创建
hotfix/* 用于紧急修复
vX.Y.Z Tag 标记发布
大文件使用 Git LFS
Secret 使用外部管理
```

---

## 82. Git 规范模板

```markdown
# Repository Git Policy

> Repository：
> Owner：
> Default Branch：
> Merge Strategy：
> Last Updated：

## Branch Types

## Commit Convention

## PR Requirements

## Required CI Checks

## Protected Branches

## Release Branch Policy

## Hotfix Policy

## Tag Convention

## LFS Rules

## Secret Management

## Required Reviewers

## Rollback Procedure
```

---

## 83. 最终原则

Git Conventions 的本质不是：

> 规定每个人必须使用完全相同的命令。

它的本质是：

> 让每一次变更都拥有清楚意图、可追踪历史、可靠评审、稳定集成和安全恢复能力。

一个健康的 Git 工作方式应做到：

- 分支短生命周期；
- 主分支可用；
- Commit 目标单一；
- Message 清楚；
- PR 上下文完整；
- Review 有效；
- CI 可靠；
- Merge 策略统一；
- Release 可追踪；
- Hotfix 可同步；
- Secret 不进入仓库；
- 大文件被正确管理；
- 错误改动可安全回滚。
