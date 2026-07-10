# Asset Workflow（资产生产流程）

> 状态：草案  
> 适用范围：通用的软件、游戏、媒体与数字产品项目  
> 目的：定义视觉、音频、动画、视频、文本和其他生产资产从需求提出、规格确认、制作、评审、集成、验证到归档的完整流程。

---

## 1. 文档目的

Asset Workflow 用于统一资产生产和交付过程，确保资产能够：

- 满足实际使用需求；
- 符合统一规格；
- 保持风格和质量一致；
- 支持高效评审和修改；
- 正确进入工程或内容系统；
- 在目标环境中稳定运行；
- 保留可维护的源文件；
- 具备清晰的版本和所有权；
- 避免重复制作和错误使用。

它回答以下问题：

- 什么内容应被视为 Asset；
- 资产需求应如何提出；
- 制作前需要明确哪些规格；
- 草稿、候选稿和最终稿如何区分；
- 如何组织评审和修改；
- 资产何时可以进入集成；
- 如何验证导入、显示、性能和兼容性；
- 源文件和导出文件如何管理；
- 资产废弃或替换时如何处理；
- 外部采购、外包和生成式资产如何纳入流程。

Asset Workflow 的目标不是只完成“一个文件”。

它的目标是：

> 生产一个能够被正确使用、验证、维护、追踪和长期复用的正式资产。

---

## 2. 什么是 Asset

Asset 是被项目直接使用或支持项目交付的内容资源。

常见类型包括：

### 2.1 Visual Asset

- UI 图标；
- 插画；
- 背景；
- 角色；
- 道具；
- 纹理；
- 特效；
- 贴图；
- 3D 模型；
- 材质；
- 字体；
- Logo；
- Banner；
- 宣传图。

### 2.2 Animation Asset

- 角色动画；
- UI 动画；
- Sprite Sheet；
- 骨骼动画；
- 过场动画；
- 动态特效；
- 视频片段。

### 2.3 Audio Asset

- 音效；
- 配乐；
- 环境音；
- 语音；
- 提示音；
- 混音文件。

### 2.4 Content Asset

- 文案；
- 剧情；
- 对话；
- 本地化文本；
- 配置表；
- 数据文件；
- 关卡数据；
- 模板；
- 教程内容。

### 2.5 Production Asset

- 源文件；
- PSD；
- AI；
- Blender；
- DAW 工程；
- 视频工程；
- Prompt；
- 参考图；
- 色板；
- 字体授权文件；
- 制作说明；
- 导入预设。

---

## 3. 资产生命周期总览

```text
REQUEST
  ↓
SPECIFICATION
  ↓
REFERENCE & DIRECTION
  ↓
DRAFT
  ↓
INTERNAL REVIEW
  ↓
REVISION
  ↓
APPROVAL
  ↓
FINALIZATION
  ↓
EXPORT
  ↓
IMPORT
  ↓
INTEGRATION
  ↓
VALIDATION
  ↓
RELEASE
  ↓
ARCHIVE / MAINTAIN
```

可能的回退路径：

```text
Review → Draft
Validation → Export
Integration → Specification
Release → Replace / Deprecate
```

---

# 4. Request：资产需求

## 4.1 目标

记录为什么需要该资产，以及它将被用于什么场景。

## 4.2 最低字段

每个资产需求至少应包含：

```text
资产名称
资产类型
使用场景
需求背景
目标用户或受众
预期效果
Owner
优先级
目标版本或里程碑
```

## 4.3 不合格需求

```text
做一个好看的按钮
做一张酷一点的背景
做几个图标
找一段音乐
```

## 4.4 合格需求

```text
为账户删除流程制作一个高风险警告图标，用于确认弹窗。
图标需要在 16px、24px 和 32px 下保持清晰，并与现有系统图标风格一致。
```

## 4.5 Request 退出条件

- 使用场景明确；
- 资产类型明确；
- Owner 明确；
- 需求值得进入规格阶段；
- 没有明显重复资产。

---

# 5. Specification：规格定义

## 5.1 目标

在制作前明确资产必须满足的技术、视觉、内容和交付要求。

## 5.2 通用规格

建议包括：

```text
用途
尺寸
分辨率
比例
格式
颜色空间
透明要求
文件大小
命名规则
平台
状态数量
语言
导出方式
源文件要求
验收标准
```

## 5.3 视觉资产规格

可能包括：

- 画布尺寸；
- 像素密度；
- 宽高比；
- 安全区域；
- 透明背景；
- 描边；
- 阴影；
- 色板；
- 缩放规则；
- 切图规则；
- 锚点；
- 九宫格；
- 压缩要求。

## 5.4 动画资产规格

可能包括：

- 帧数；
- 帧率；
- 循环方式；
- 起止姿势；
- Root Motion；
- 动作方向；
- Pivot；
- Sprite Sheet 排列；
- 命名；
- 事件点；
- 过渡方式。

## 5.5 音频资产规格

可能包括：

- 格式；
- 采样率；
- 位深；
- 声道；
- 响度；
- 循环点；
- 长度；
- 淡入淡出；
- 压缩；
- 分类；
- 使用场景。

## 5.6 文本资产规格

可能包括：

- 语气；
- 长度；
- 术语；
- 变量占位符；
- 本地化；
- 字符限制；
- 格式；
- 禁用词；
- 法务要求。

## 5.7 规格变更

规格发生变化时，应记录：

- 变更内容；
- 原因；
- 影响；
- 是否需要返工；
- 是否影响已完成版本；
- 决策人。

---

# 6. Reference and Direction：参考与方向

## 6.1 目标

建立统一的创作方向，减少评审中的主观分歧。

## 6.2 可使用内容

- Style Guide；
- Moodboard；
- 色板；
- 参考图；
- 现有资产；
- 竞品示例；
- 禁止示例；
- 动画参考；
- 音效参考；
- 文案语气指南；
- 品牌规范。

## 6.3 参考图原则

参考应说明：

```text
参考什么
不参考什么
用于风格还是结构
是否存在授权限制
```

例如：

```text
参考其柔和配色和圆角比例，不参考具体角色造型、Logo 或构图。
```

## 6.4 风格锁定

重要资产开始批量生产前，应先确认：

- 风格；
- 比例；
- 线条；
- 色彩；
- 材质；
- 明暗；
- 复杂度；
- 识别度；
- 技术限制。

---

# 7. Draft：草稿制作

## 7.1 目标

快速验证方向，不在未经确认的方案上投入过多成本。

## 7.2 草稿可以是

- 线稿；
- 灰阶稿；
- 低保真；
- 占位图；
- 动画关键帧；
- 音频样段；
- 文案提纲；
- 低模；
- 快速原型；
- 多方案缩略图。

## 7.3 草稿原则

- 优先验证结构和方向；
- 不追求最终细节；
- 不隐藏未完成状态；
- 清楚标记 Draft；
- 每个候选方案解决明确问题；
- 避免同时提交大量无差异方案。

## 7.4 草稿数量

草稿数量应由不确定性决定。

常见方式：

```text
1 个明确方案
2–3 个有差异的方向
1 个主方案 + 1 个备选
```

---

# 8. Internal Review：内部评审

## 8.1 目标

在进一步投入前确认：

- 是否符合需求；
- 是否符合规格；
- 是否符合风格；
- 是否适合集成；
- 是否需要调整方向。

## 8.2 评审维度

```text
Goal Fit
Style
Readability
Technical Fit
Consistency
Scalability
Accessibility
Performance
Legal / License
```

## 8.3 评审结果

```text
Approved for Revision
Approved with Conditions
Needs Revision
Needs New Direction
Rejected
Blocked
```

## 8.4 评审意见要求

意见应：

- 指向明确问题；
- 区分阻塞与建议；
- 说明原因；
- 尽量关联规格；
- 避免纯主观表达。

不合格：

```text
不够好看
感觉不对
再精致一点
```

合格：

```text
图标在 16px 下内部细节无法识别，需要减少内部线条并加强外轮廓。
```

---

# 9. Revision：修改

## 9.1 目标

根据评审结果进行有范围的迭代。

## 9.2 修改原则

- 先处理 Blocker；
- 再处理 Required；
- Suggestion 根据价值决定；
- 每轮修改目标明确；
- 不在无结论情况下无限迭代；
- 重要方向变化应回到 Specification；
- 每轮保留版本记录。

## 9.3 修改记录

建议记录：

```text
Version
Date
Feedback
Changes
Open Issues
Reviewer
```

---

# 10. Approval：批准

## 10.1 目标

确认资产方向和主要质量已经满足正式制作要求。

## 10.2 批准不等于最终交付

Approval 表示：

- 方向确认；
- 主要视觉或内容确认；
- 可以进入最终制作；
- 不应再进行大范围方向变化。

## 10.3 批准条件

```markdown
- [ ] 需求符合；
- [ ] 规格符合；
- [ ] 风格符合；
- [ ] 主要内容确认；
- [ ] 关键技术限制满足；
- [ ] 无方向级未决问题；
- [ ] Owner 已确认；
```

---

# 11. Finalization：最终制作

## 11.1 目标

完成正式质量所需的细节、清理和生产处理。

## 11.2 常见工作

- 清理线条；
- 完成细节；
- 统一色彩；
- 完成阴影；
- 完成动画中间帧；
- 修复音频噪声；
- 混音；
- 校正文案；
- 完成本地化；
- 清理图层；
- 优化拓扑；
- 烘焙；
- 压缩；
- 完成元数据；
- 移除占位内容。

## 11.3 最终化检查

```markdown
- [ ] 无占位内容；
- [ ] 无临时标记；
- [ ] 无多余图层；
- [ ] 无损坏引用；
- [ ] 命名正确；
- [ ] 色彩和格式正确；
- [ ] 源文件可继续编辑；
- [ ] 导出前质量已确认；
```

---

# 12. Export：导出

## 12.1 目标

生成符合目标系统要求的正式交付文件。

## 12.2 导出要求

- 格式正确；
- 尺寸正确；
- 压缩正确；
- 透明正确；
- 色彩空间正确；
- 元数据正确；
- 命名正确；
- 文件大小合理；
- 无隐藏辅助内容；
- 无错误裁切；
- 无多余边缘；
- 可重复导出。

## 12.3 导出预设

建议为重复资产类型建立导出预设，例如：

```text
UI Icon Export
Background Export
Sprite Sheet Export
Audio SFX Export
Video Web Export
```

## 12.4 导出文件与源文件

应区分：

```text
Source File
Working File
Review Export
Final Export
Runtime Asset
```

---

# 13. Import：导入

## 13.1 目标

将正式文件导入项目，并应用正确的导入设置。

## 13.2 常见导入设置

- Filter；
- Compression；
- Mipmap；
- Sampling；
- Loop；
- Normalization；
- Scale；
- Pivot；
- Slice；
- Import Preset；
- Color Space；
- Mesh Optimization；
- Audio Streaming。

## 13.3 Import Checklist

```markdown
- [ ] 文件位于正确目录；
- [ ] 命名符合规范；
- [ ] 导入设置正确；
- [ ] 缩放正确；
- [ ] Pivot 正确；
- [ ] 切片正确；
- [ ] 压缩正确；
- [ ] 透明正确；
- [ ] 资源引用正常；
- [ ] 无重复资源；
```

---

# 14. Integration：集成

## 14.1 目标

确认资产在真实产品流程中正确工作。

## 14.2 需要检查

- 实际显示；
- 实际缩放；
- 实际颜色；
- 动画播放；
- 音频触发；
- UI 状态；
- 资源加载；
- 平台差异；
- 本地化；
- 性能；
- 内存；
- 构建。

## 14.3 集成原则

资产在独立预览中正确，不代表集成后正确。

例如：

- UI 图标在编辑器中清晰，但缩小后模糊；
- 音效单独播放正常，但混音后过响；
- 动画单独播放正常，但状态切换不自然；
- 透明图在某平台出现黑边；
- 字体支持不足导致缺字；
- 视频在正式构建中无法解码。

---

# 15. Validation：验证

## 15.1 目标

确认资产同时满足：

- 需求；
- 规格；
- 视觉或内容质量；
- 技术要求；
- 集成要求；
- 发布要求。

## 15.2 视觉验证

```markdown
- [ ] 风格一致；
- [ ] 比例一致；
- [ ] 色彩一致；
- [ ] 细节适合目标尺寸；
- [ ] 识别度正确；
- [ ] 无明显瑕疵；
- [ ] 无错误透明边；
```

## 15.3 技术验证

```markdown
- [ ] 格式正确；
- [ ] 尺寸正确；
- [ ] 导入正确；
- [ ] 构建中可加载；
- [ ] 内存可接受；
- [ ] 性能可接受；
- [ ] 平台兼容；
- [ ] 无缺失依赖；
```

## 15.4 内容验证

```markdown
- [ ] 内容正确；
- [ ] 无错别字；
- [ ] 无占位符；
- [ ] 术语一致；
- [ ] 本地化正确；
- [ ] 敏感内容已检查；
- [ ] 授权信息完整；
```

---

# 16. Asset Definition of Done

一个资产可以被视为 Done，当：

- 已批准方向；
- 最终文件完成；
- 源文件完整；
- 导出格式正确；
- 命名正确；
- 导入设置正确；
- 已集成到真实使用场景；
- 已在目标环境验证；
- 无阻塞问题；
- 授权和来源清楚；
- 版本记录完整；
- 归档位置明确。

---

# 17. Asset 状态模型

推荐使用：

```text
Requested
Specified
In Direction
Drafting
In Review
Revising
Approved
Finalizing
Exported
Imported
Integrated
Validated
Done
Released
Archived
```

补充：

```text
Blocked
Deferred
Rejected
Deprecated
Replaced
Cancelled
```

---

# 18. Asset Priority

建议根据：

- 是否阻塞核心功能；
- 是否阻塞测试；
- 是否阻塞发布；
- 是否可以使用占位资源；
- 是否依赖其他资产；
- 是否需要外部制作；
- 制作周期；
- 风险；
- 用户可见程度；

确定优先级。

不应仅根据“视觉重要性”排序。

---

# 19. Placeholder Asset

占位资产用于：

- 提前集成；
- 验证布局；
- 验证流程；
- 降低依赖阻塞；
- 支持并行开发。

## 19.1 占位资产要求

- 明确标记；
- 不应与正式资产混淆；
- 有替换 Owner；
- 有替换时间；
- 有构建检查；
- 发布前必须清理或正式接受。

## 19.2 占位资产风险

- 被误发布；
- 尺寸与正式资产不同；
- 性能差异；
- 视觉评审失真；
- 交互布局返工。

---

# 20. Versioning：资产版本管理

推荐版本格式：

```text
asset-name_v001
asset-name_v002
asset-name_final
asset-name_final-final
```

不推荐后两种。

推荐：

```text
asset-name_v001
asset-name_v002
asset-name_v003_approved
```

或由版本管理系统追踪。

## 20.1 版本记录

应记录：

- 版本；
- 日期；
- 修改内容；
- 作者；
- Reviewer；
- 状态；
- 使用位置。

---

# 21. Naming：命名

建议命名包含：

```text
Category
Subject
Variant
State
Size
Version
```

示例：

```text
ui_icon_warning_default_24_v003.png
char_knight_idle_right_v012.png
sfx_ui_confirm_short_v004.wav
```

命名应：

- 可搜索；
- 可预测；
- 不依赖个人记忆；
- 不使用空格；
- 不使用模糊词；
- 不使用 final-final。

---

# 22. Folder Structure：目录结构

示例：

```text
assets/
├── source/
│   ├── ui/
│   ├── character/
│   ├── environment/
│   ├── audio/
│   └── video/
├── export/
│   ├── review/
│   └── final/
├── runtime/
├── references/
├── licenses/
└── archive/
```

实际结构应与项目工具和版本控制方式匹配。

---

# 23. Source File Management：源文件管理

源文件应满足：

- 可继续编辑；
- 图层结构清楚；
- 外部链接完整；
- 字体可获得；
- 插件依赖可追踪；
- 版本可打开；
- 作者明确；
- 不包含无关敏感内容。

## 23.1 源文件缺失风险

如果只保留导出文件，未来可能无法：

- 修改尺寸；
- 更新文案；
- 调整颜色；
- 修复动画；
- 重新导出；
- 支持新平台；
- 证明来源。

---

# 24. External Asset：外部资产

外部资产包括：

- 商店购买；
- 开源资源；
- 外包；
- 素材库；
- 音乐库；
- 字体；
- 第三方模型；
- 生成式资产。

## 24.1 必须记录

```text
Source
Author
License
Purchase Proof
Usage Scope
Modification Permission
Redistribution Restriction
Attribution Requirement
```

## 24.2 使用前检查

```markdown
- [ ] 授权允许当前用途；
- [ ] 允许商业使用；
- [ ] 允许修改；
- [ ] 允许随产品分发；
- [ ] 是否需要署名；
- [ ] 是否禁止单独转售；
- [ ] 来源可追踪；
- [ ] 购买凭证已保存；
```

---

# 25. Outsourcing：外包资产

## 25.1 需求包

外包前应提供：

- Brief；
- 规格；
- 风格参考；
- 禁止项；
- 文件格式；
- 源文件要求；
- 评审节点；
- 修改轮次；
- 交付时间；
- 授权和所有权；
- 验收标准。

## 25.2 里程碑交付

推荐：

```text
Direction
→ Draft
→ Mid Review
→ Final Review
→ Source Delivery
```

不建议等到最终成品才第一次评审。

---

# 26. Localization Asset Workflow

```text
Source Text Freeze
→ Context Export
→ Translation
→ Linguistic Review
→ Import
→ In-context Review
→ Fix
→ Final Validation
```

## 26.1 本地化资产需要

- 上下文；
- 字符限制；
- 变量；
- 性别；
- 复数；
- 语气；
- 截图；
- 使用位置；
- 禁止翻译项；
- 术语表。

---

# 27. Audio Workflow

```text
Request
→ Direction
→ Sample
→ Review
→ Edit
→ Mix
→ Export
→ Import
→ In-context Test
→ Loudness Check
→ Final
```

检查：

```markdown
- [ ] 采样率正确；
- [ ] 格式正确；
- [ ] 声道正确；
- [ ] 循环无断点；
- [ ] 音量层级合理；
- [ ] 无爆音；
- [ ] 无多余静音；
- [ ] 正式构建可播放；
```

---

# 28. Animation Workflow

```text
Motion Brief
→ Key Poses
→ Timing
→ Blocking
→ In-between
→ Polish
→ Export
→ Import
→ State Integration
→ Validation
```

检查：

```markdown
- [ ] 动作意图清楚；
- [ ] 关键姿势正确；
- [ ] 节奏正确；
- [ ] 循环自然；
- [ ] 方向正确；
- [ ] Pivot 正确；
- [ ] 过渡正确；
- [ ] 无漂移；
- [ ] 正式构建播放正确；
```

---

# 29. Asset Review 模板

```markdown
# Asset Review

> Asset：
> Type：
> Version：
> Owner：
> Reviewer：
> Status：

## 1. Purpose

## 2. Usage

## 3. Specification

## 4. Reference

## 5. Review Criteria

## 6. Findings

| ID | Level | Finding | Owner | Status |
|---|---|---|---|---|

## 7. Decision

- [ ] Approved
- [ ] Approved with Conditions
- [ ] Needs Revision
- [ ] Needs New Direction
- [ ] Rejected
- [ ] Blocked

## 8. Conditions

## 9. Next Version
```

---

# 30. Asset Request 模板

```markdown
# Asset Request

> ID：
> Name：
> Type：
> Owner：
> Priority：
> Target Milestone：
> Target Release：
> Status：

## 1. Background

## 2. Purpose

## 3. Usage Scenario

## 4. Specification

- Size：
- Ratio：
- Format：
- Color Space：
- Transparency：
- Platform：
- File Size：
- Source File：

## 5. Style Direction

## 6. References

## 7. Non-goals

## 8. States / Variants

## 9. Acceptance Criteria

## 10. Dependencies

## 11. License Requirements

## 12. Delivery Date
```

---

# 31. Asset Delivery 模板

```markdown
# Asset Delivery

> Asset：
> Version：
> Author：
> Date：
> Status：

## Source Files

## Final Exports

## Runtime Files

## Import Settings

## Usage Locations

## License / Source

## Known Limitations

## Validation Result

## Archive Location
```

---

# 32. Asset Checklist

```markdown
## Request

- [ ] 使用场景明确；
- [ ] 资产类型明确；
- [ ] Owner 明确；
- [ ] 优先级明确；
- [ ] 目标版本明确。

## Specification

- [ ] 尺寸明确；
- [ ] 比例明确；
- [ ] 格式明确；
- [ ] 平台明确；
- [ ] 透明要求明确；
- [ ] 源文件要求明确；
- [ ] 验收标准明确。

## Direction

- [ ] 风格参考明确；
- [ ] 禁止项明确；
- [ ] 与现有资产一致；
- [ ] 授权风险已检查。

## Production

- [ ] 草稿已评审；
- [ ] 修改意见已处理；
- [ ] 最终方向已批准；
- [ ] 占位内容已清理；
- [ ] 源文件可编辑。

## Export

- [ ] 格式正确；
- [ ] 尺寸正确；
- [ ] 命名正确；
- [ ] 压缩正确；
- [ ] 透明正确；
- [ ] 色彩空间正确。

## Import

- [ ] 目录正确；
- [ ] 导入设置正确；
- [ ] Pivot / Slice 正确；
- [ ] 资源引用正确；
- [ ] 无重复资产。

## Validation

- [ ] 真实场景显示正确；
- [ ] 正式构建可加载；
- [ ] 性能可接受；
- [ ] 平台兼容；
- [ ] UI / 动画 / 音频行为正确；
- [ ] 无阻塞问题。

## Archive

- [ ] 源文件已保存；
- [ ] 授权文件已保存；
- [ ] 版本记录完整；
- [ ] 使用位置已记录；
- [ ] 归档路径明确。
```

---

# 33. 常见失败模式

## 33.1 没有规格就开始制作

结果：

- 尺寸错误；
- 格式错误；
- 风格不一致；
- 反复返工；
- 无法集成。

---

## 33.2 只评审“好不好看”

问题：

- 忽略可读性；
- 忽略尺寸；
- 忽略平台；
- 忽略性能；
- 忽略真实使用场景。

---

## 33.3 最终才第一次评审

结果：

- 方向错误时返工成本极高；
- 修改次数增加；
- 交付延期。

---

## 33.4 只有导出文件，没有源文件

结果：

- 无法修改；
- 无法支持新尺寸；
- 无法修复；
- 来源难以证明。

---

## 33.5 占位资产进入正式版本

原因：

- 标记不清；
- 没有 Owner；
- 没有替换检查；
- 发布前未清理。

---

## 33.6 资产在预览中正确，集成后错误

原因：

- 未做真实场景验证；
- 缩放不同；
- 压缩不同；
- 混音不同；
- 构建环境不同。

---

## 33.7 版本命名混乱

表现：

```text
final
final2
final-new
final-final
```

结果：

- 不知道哪个版本正式；
- 修改丢失；
- 错误文件被使用。

---

## 33.8 外部资产授权不清

风险：

- 无法商业使用；
- 需要署名但未署名；
- 不允许修改；
- 不允许分发；
- 无购买凭证。

---

## 33.9 无限修改

表现：

- 没有批准节点；
- 意见不断变化；
- 每轮目标不清；
- 个人偏好主导。

改进：

- 明确 Specification；
- 使用版本；
- 设 Approval Gate；
- 区分 Blocker 和 Suggestion。

---

## 33.10 删除旧资产但未处理引用

结果：

- 构建失败；
- 缺图；
- 运行时错误；
- 存档引用失效。

---

# 34. 资产替换与废弃

资产被替换时，应：

- 标记 Deprecated；
- 查找全部引用；
- 替换引用；
- 验证正式构建；
- 保留迁移说明；
- 确认不再使用；
- 再进行归档或删除。

不应直接删除仍可能被引用的正式资产。

---

# 35. Asset Metrics

可参考：

- 平均制作周期；
- 平均修改轮次；
- 首轮通过率；
- 返工率；
- 规格错误率；
- 导入错误率；
- 正式构建缺失率；
- 占位资产遗留数；
- 重复资产数量；
- 未授权资产数量；
- 资产复用率；
- 废弃资产比例。

指标用于改善：

- Brief 质量；
- 风格一致性；
- 评审效率；
- 导入自动化；
- 版本管理；
- 资产复用。

---

# 36. 最终原则

Asset Workflow 的本质不是：

> 完成一张图、一段音频或一个文件。

它的本质是：

> 生产一个符合目标、规格、质量、技术和授权要求，并能够被正确集成、验证、维护和复用的正式资产。

一个有效的资产流程应做到：

- 需求清楚；
- 规格明确；
- 方向统一；
- 尽早评审；
- 修改有边界；
- 源文件完整；
- 导出可重复；
- 导入正确；
- 集成真实；
- 验证充分；
- 授权可追踪；
- 版本可维护。
