# AI Asset Workflow（AI 资产生产流程）

> 状态：草案  
> 适用范围：通用的软件、游戏、媒体与数字产品项目  
> 目的：定义使用生成式 AI 创建、编辑、扩展、变体化和辅助生产资产时的完整流程，包括需求、提示词、参考资料、生成、筛选、人工修整、验证、授权记录、集成与归档。

---

## 1. 文档目的

AI Asset Workflow 用于规范生成式 AI 在资产生产中的使用方式，确保生成结果能够：

- 服务明确的产品目标；
- 满足统一规格；
- 保持风格和身份一致；
- 具备可重复的生成过程；
- 经过必要人工筛选和修整；
- 通过技术与集成验证；
- 保留来源、模型和提示词记录；
- 避免未经审查的生成结果直接进入正式版本；
- 降低版权、授权、隐私和品牌风险；
- 能够被后续维护、修改和复用。

它回答以下问题：

- 什么场景适合使用 AI；
- 什么场景不适合使用 AI；
- 提示词和参考图如何管理；
- 如何保持角色、风格和系列资产一致；
- 如何区分探索生成与正式生产；
- 如何处理透明背景、分层和尺寸；
- AI 生成结果是否可以直接使用；
- 人工修整应做到什么程度；
- 如何验证技术质量和法律风险；
- 如何记录模型、版本、来源和决策；
- 如何归档生成过程和最终资产。

AI Asset Workflow 不是为了追求“生成更多图片”。

它的核心目标是：

> 将生成式 AI 作为受控的生产工具，产出可追踪、可审核、可修改、可集成并达到正式质量要求的资产。

---

## 2. AI Asset 的定义

AI Asset 是指在生产过程中由生成式 AI 直接生成或显著参与修改的资产。

包括：

- 文生图；
- 图生图；
- 局部重绘；
- 扩图；
- 风格变换；
- 抠图；
- 背景移除；
- 图像修复；
- 超分辨率；
- 变体生成；
- 动画帧辅助；
- 3D 生成；
- 音频生成；
- 语音生成；
- 音效生成；
- 文案生成；
- 本地化辅助；
- 数据或配置生成；
- Prompt 辅助设计。

如果 AI 只用于极轻微辅助，例如：

- 拼写检查；
- 关键词建议；
- 尺寸转换建议；
- 文件命名建议；

可以不按完整 AI Asset 流程处理，但仍应遵守项目的内容和授权规范。

---

## 3. AI Asset 生命周期总览

```text
REQUEST
  ↓
SUITABILITY CHECK
  ↓
SPECIFICATION
  ↓
REFERENCE PREPARATION
  ↓
PROMPT DESIGN
  ↓
EXPLORATION GENERATION
  ↓
SELECTION
  ↓
CONTROLLED GENERATION
  ↓
EDIT / INPAINT / VARIATION
  ↓
HUMAN CLEANUP
  ↓
STYLE & CONSISTENCY REVIEW
  ↓
TECHNICAL VALIDATION
  ↓
LEGAL / SOURCE REVIEW
  ↓
EXPORT & IMPORT
  ↓
INTEGRATION
  ↓
FINAL APPROVAL
  ↓
ARCHIVE
```

可能的回退路径：

```text
Selection → Prompt Design
Consistency Review → Controlled Generation
Technical Validation → Human Cleanup
Legal Review → Reject / Replace
Integration → Specification
```

---

# 4. Suitability Check：适用性判断

## 4.1 目标

在开始生成前，判断 AI 是否真的是合适工具。

AI 不应因为“可以生成”就默认被使用。

## 4.2 适合使用 AI 的场景

- 早期概念探索；
- Moodboard；
- 方向对比；
- 占位资源；
- 非核心背景；
- 批量变体；
- 风格测试；
- 构图草案；
- 快速视觉原型；
- 低风险内容；
- 辅助清理和修复；
- 已有资产的尺寸扩展；
- 生产前参考；
- 人工制作前的草图；
- 内部提案图。

## 4.3 需要谨慎使用的场景

- 核心角色；
- 品牌主视觉；
- Logo；
- 长期商业 IP；
- 高一致性系列；
- 需要精确结构的 UI；
- 需要精确动作的动画；
- 需要分层源文件的资产；
- 法律敏感内容；
- 真实人物；
- 医疗、法律、金融内容；
- 高精度产品展示；
- 需要严格真实性的内容。

## 4.4 不适合使用 AI 的场景

- 无法确认使用权；
- 需要完全原创且可证明创作链路；
- 涉及未授权个人肖像；
- 涉及敏感或受限数据；
- 需要像素级精确排版；
- 需要稳定、可编辑矢量结构；
- 需要严格物理或技术准确性；
- 生成结果无法通过人工检查；
- 团队无法承担后续清理成本；
- 模型或平台条款不允许目标用途。

## 4.5 适用性评估

```markdown
- [ ] AI 能显著降低探索或制作成本；
- [ ] 输出可以被人工验证；
- [ ] 目标资产允许一定生成不确定性；
- [ ] 模型和平台允许目标用途；
- [ ] 参考资料来源清楚；
- [ ] 不涉及未经授权的敏感素材；
- [ ] 后续有人工修整能力；
- [ ] 最终输出能满足技术要求；
```

---

# 5. Request：AI 资产需求

AI 资产需求应包含：

```text
资产名称
资产类型
使用场景
使用阶段
是否为占位
是否面向正式发布
目标规格
风格要求
一致性要求
参考资料
禁止内容
Owner
目标版本
风险等级
```

## 5.1 使用阶段

建议区分：

```text
Exploration
Prototype
Internal
Production Candidate
Final Production
Marketing
```

同一生成结果是否可以进入正式版本，取决于其使用阶段和验证结果。

---

# 6. Specification：规格定义

AI 资产必须先定义规格。

至少包括：

```text
画布尺寸
宽高比
输出格式
透明要求
背景要求
目标平台
目标显示尺寸
文件大小
风格
构图
主体数量
主体位置
视角
光照
色彩
禁止元素
源文件要求
验收标准
```

## 6.1 AI 特有规格

还应明确：

- 是否允许变体；
- 是否要求角色一致；
- 是否要求固定构图；
- 是否要求精确姿势；
- 是否要求无文字；
- 是否要求无水印；
- 是否要求透明背景；
- 是否要求分层；
- 是否允许人工重绘；
- 是否允许使用多模型；
- 是否允许风格迁移；
- 是否允许外部后处理。

## 6.2 负面要求

应明确不允许出现：

```text
额外肢体
错误手指
重复物体
文字乱码
水印
Logo
错误透视
错误比例
不一致服装
不一致角色特征
假透明背景
裁切
边缘毛刺
过度细节
```

---

# 7. Reference Preparation：参考资料准备

## 7.1 参考资料类型

- 角色设定图；
- 色板；
- 风格规范；
- 构图草图；
- 线稿；
- 姿势骨架；
- 产品截图；
- UI Wireframe；
- 现有资产；
- 禁止示例；
- 材质参考；
- 动画参考；
- 语气指南。

## 7.2 参考资料来源

每份参考资料应记录：

```text
Source
Owner
License
Usage Permission
Modification Permission
Commercial Use
Attribution
```

## 7.3 参考图使用原则

应说明：

- 参考的是风格还是结构；
- 是否允许直接复现；
- 哪些元素必须保持；
- 哪些元素只是启发；
- 哪些元素禁止复制；
- 是否涉及第三方 IP；
- 是否涉及真人肖像。

## 7.4 参考资料清理

生成前应移除：

- 无关标记；
- 水印；
- 私密信息；
- 未授权素材；
- 错误示例；
- 可能误导模型的干扰元素。

---

# 8. Prompt Design：提示词设计

## 8.1 目标

将需求转换为结构清楚、可复用、可比较的 Prompt。

## 8.2 推荐结构

```text
Asset Type
Subject
Composition
Pose / Action
Style
Color
Lighting
Materials
Technical Constraints
Consistency Constraints
Negative Constraints
Output Requirements
```

## 8.3 Prompt 模板

```markdown
## 资产类型

## 主体

## 构图

## 动作 / 姿势

## 风格

## 色彩

## 光照

## 材质

## 一致性要求

## 技术要求

## 禁止项

## 输出要求
```

## 8.4 Prompt 原则

- 使用具体描述；
- 避免互相冲突；
- 重要要求放在前面；
- 技术要求单独列出；
- 明确数量；
- 明确方向；
- 明确视角；
- 明确背景；
- 明确禁止项；
- 避免无意义堆叠风格词。

## 8.5 不合格 Prompt

```text
做一个好看的可爱角色，高清，酷炫，精致。
```

## 8.6 合格 Prompt

```text
单个全身角色，正侧视图，朝右，站立姿势。
圆润手绘 2D 卡通风格，粗深色描边，低饱和暖色。
保持头身比例、服装颜色和武器形状固定。
纯透明背景，不要文字，不要阴影，不要额外物体。
```

---

# 9. Prompt Versioning：提示词版本管理

每次重要修改都应生成新版本。

示例：

```text
prompt_v001
prompt_v002_pose-fix
prompt_v003_outline-lock
prompt_v004_approved
```

## 9.1 Prompt 记录内容

```text
Prompt Version
Model
Model Version
Generation Mode
Seed（如可用）
Reference Files
Settings
Date
Author
Purpose
Result
Decision
```

## 9.2 为什么要版本管理

没有 Prompt 版本会导致：

- 无法复现；
- 无法比较；
- 不知道哪次修改有效；
- 风格漂移；
- 角色一致性下降；
- 无法批量生产。

---

# 10. Exploration Generation：探索生成

## 10.1 目标

快速比较方向，不直接追求最终资产。

## 10.2 探索内容

可以测试：

- 构图；
- 风格；
- 色板；
- 比例；
- 服装；
- 镜头；
- 线条；
- 材质；
- 复杂度；
- 背景密度；
- 动作夸张程度。

## 10.3 探索原则

- 每轮只改变少量变量；
- 记录每次变化；
- 候选之间要有明确差异；
- 不要在方向未确认前过度精修；
- 不把探索图误标为正式资产。

## 10.4 探索输出

```text
Accepted Direction
Rejected Directions
Useful Elements
Problems
Next Prompt Changes
```

---

# 11. Selection：筛选

## 11.1 目标

从候选结果中选择最适合继续发展的方案。

## 11.2 筛选维度

```text
Goal Fit
Style Fit
Composition
Readability
Consistency
Technical Feasibility
Editability
Legal Risk
Production Cost
```

## 11.3 不应只选“最好看”的

还应考虑：

- 是否容易修；
- 是否符合系列；
- 是否适合目标尺寸；
- 是否存在结构错误；
- 是否容易集成；
- 是否容易生成后续变体；
- 是否可能涉及授权风险。

## 11.4 结果状态

```text
Selected
Selected with Conditions
Needs More Variants
Rejected
Archive for Reference
```

---

# 12. Controlled Generation：受控生成

## 12.1 目标

在方向确定后，提高稳定性和可重复性。

## 12.2 控制手段

- 固定参考图；
- 固定角色设定；
- 固定色板；
- 固定视角；
- 固定构图模板；
- 固定 Prompt 基础段；
- 固定随机种子；
- 固定模型版本；
- 固定输出尺寸；
- 固定负面约束；
- 限制一次只变一个变量。

## 12.3 角色一致性锁定

应记录并重复强调：

```text
头身比例
脸型
发型
服装
配色
配饰
武器
轮廓
描边
体型
标志性特征
```

## 12.4 系列资产一致性

对于一组图标、角色或背景，应先建立：

- 基准资产；
- 标准 Prompt；
- 标准色板；
- 标准尺寸；
- 标准轮廓；
- 标准阴影；
- 标准复杂度；
- 标准导出规则。

---

# 13. Image Editing：编辑、局部重绘与变体

## 13.1 适用场景

- 修复错误肢体；
- 替换局部物体；
- 调整服装；
- 修改背景；
- 修正姿势；
- 扩展画布；
- 删除多余元素；
- 改变颜色；
- 保持主体不变制作变体。

## 13.2 编辑原则

- 每次编辑目标单一；
- 遮罩范围尽量准确；
- 保留主体身份；
- 不让模型重画无关区域；
- 编辑前保存原版本；
- 编辑后重新检查全图；
- 避免连续编辑导致累积失真。

## 13.3 变体原则

变体应明确变化维度：

```text
颜色
状态
等级
季节
表情
装备
方向
尺寸
稀有度
```

不要让每个变体同时改变过多设计元素。

---

# 14. Transparency：透明背景

## 14.1 真透明与假透明

假透明常见表现：

- 灰白棋盘格被画进图片；
- 白底伪装成透明；
- 边缘存在灰色或白色污染；
- 半透明边缘带底色；
- 输出格式不支持 Alpha。

## 14.2 透明验证

```markdown
- [ ] 文件格式支持 Alpha；
- [ ] 背景像素 Alpha 为 0；
- [ ] 不存在棋盘格图案；
- [ ] 边缘无白边或黑边；
- [ ] 半透明边缘颜色正确；
- [ ] 在深色和浅色背景上检查；
- [ ] 正式引擎或产品中检查；
```

## 14.3 背景移除

自动背景移除后，应人工检查：

- 发丝；
- 毛发；
- 透明材质；
- 特效；
- 阴影；
- 小孔洞；
- 描边；
- 边缘残留。

---

# 15. Layering：分层

生成式图像通常是扁平图层。

如果正式生产需要：

- 角色与背景分离；
- 前景与后景分离；
- 手臂和身体分离；
- UI 元素独立；
- 动画部件分离；
- 特效独立；
- 文字独立；

则必须人工拆层或重新制作。

## 15.1 分层要求

```markdown
- [ ] 图层命名明确；
- [ ] 遮挡关系正确；
- [ ] 被遮挡区域已补全；
- [ ] 图层边缘干净；
- [ ] Pivot 和 Anchor 明确；
- [ ] 各层可独立编辑；
- [ ] 合成后与原图一致；
```

---

# 16. Human Cleanup：人工修整

AI 生成结果默认不是正式成品。

常见需要人工修整的内容：

- 手指；
- 四肢；
- 透视；
- 对称；
- 服装结构；
- 道具；
- 文字；
- 边缘；
- 描边；
- 阴影；
- 色彩；
- 重复细节；
- 纹理噪声；
- 透明通道；
- 分层；
- 构图裁切；
- 图标可读性；
- 角色身份漂移。

## 16.1 人工修整的目标

- 修正结构错误；
- 统一风格；
- 降低生成噪声；
- 提高系列一致性；
- 满足技术规格；
- 形成可维护源文件。

## 16.2 修整记录

建议记录：

```text
Generated Base
Manual Changes
Tool Used
Editor
Date
Final Version
```

---

# 17. Style Consistency Review：风格一致性评审

## 17.1 检查维度

```markdown
- [ ] 线条粗细一致；
- [ ] 描边颜色一致；
- [ ] 色板一致；
- [ ] 阴影方式一致；
- [ ] 细节密度一致；
- [ ] 材质表现一致；
- [ ] 透视一致；
- [ ] 比例一致；
- [ ] 构图语言一致；
- [ ] 光照方向一致；
```

## 17.2 系列对比

不要只单独看一张图。

应将资产与：

- 同系列资产；
- 现有 UI；
- 角色设定；
- 场景；
- 实际产品截图；

放在一起评审。

---

# 18. Identity Consistency：身份一致性

适用于：

- 角色；
- 产品；
- 品牌人物；
- 关键对象；
- 系列道具。

检查：

```markdown
- [ ] 脸型一致；
- [ ] 发型一致；
- [ ] 体型一致；
- [ ] 服装一致；
- [ ] 颜色一致；
- [ ] 配饰一致；
- [ ] 标志性细节一致；
- [ ] 正反方向合理；
- [ ] 不同姿势下仍可识别；
```

如果身份一致性无法通过受控生成稳定实现，应转为：

- 人工重绘；
- 骨骼动画；
- 3D；
- 传统 Sprite 制作；
- 基于模板的编辑。

---

# 19. Pose and Motion Accuracy：姿势与动作准确性

对于动作资产，应重点检查：

- 关节角度；
- 重心；
- 接触点；
- 步幅；
- 方向；
- 遮挡；
- 左右肢体；
- 动作连续性；
- 极端帧；
- 轮廓可读性。

AI 容易将相邻帧平均成近似姿势，因此动作帧应：

- 使用明确骨架；
- 一帧一帧验证；
- 锁定身体比例；
- 锁定服装；
- 避免整组一次性自由生成；
- 必要时人工修正中间帧。

---

# 20. Text and Typography：文字与排版

AI 生成图中的文字通常不可靠。

正式资产中的文字应：

- 单独排版；
- 使用真实字体；
- 使用可编辑文本层；
- 通过拼写检查；
- 支持本地化；
- 避免将生成文字直接作为正式内容。

检查：

```markdown
- [ ] 无乱码；
- [ ] 无虚假文字；
- [ ] 字体授权清楚；
- [ ] 文字可编辑；
- [ ] 本地化可替换；
- [ ] 对齐和间距正确；
```

---

# 21. Technical Validation：技术验证

## 21.1 图像检查

```markdown
- [ ] 尺寸正确；
- [ ] 分辨率正确；
- [ ] 格式正确；
- [ ] Alpha 正确；
- [ ] 色彩空间正确；
- [ ] 压缩正确；
- [ ] 文件大小合理；
- [ ] 无水印；
- [ ] 无错误边缘；
- [ ] 无隐藏背景；
```

## 21.2 动画检查

```markdown
- [ ] 帧数正确；
- [ ] 帧率正确；
- [ ] 顺序正确；
- [ ] 循环正确；
- [ ] Pivot 正确；
- [ ] 无角色漂移；
- [ ] 无帧尺寸变化；
- [ ] 正式环境播放正确；
```

## 21.3 音频检查

```markdown
- [ ] 格式正确；
- [ ] 响度合理；
- [ ] 无爆音；
- [ ] 无版权风险；
- [ ] 循环点正确；
- [ ] 正式环境可播放；
```

## 21.4 构建检查

```markdown
- [ ] 资源可被正式构建包含；
- [ ] 不依赖本地临时路径；
- [ ] 不存在缺失引用；
- [ ] 目标平台可加载；
- [ ] 性能和内存可接受；
```

---

# 22. Legal and Source Review：法律与来源审查

## 22.1 必须记录

```text
Generation Platform
Model
Model Version
Account / License Type
Prompt
Reference Sources
Reference Licenses
Generation Date
Editor
Final Modifications
Intended Use
```

## 22.2 需要检查

- 平台是否允许商业使用；
- 模型是否允许商业使用；
- 输出是否有额外限制；
- 参考图是否授权；
- 是否涉及第三方商标；
- 是否涉及版权角色；
- 是否涉及真人肖像；
- 是否包含敏感数据；
- 是否需要署名；
- 是否允许再分发；
- 是否允许训练或再训练；
- 是否保留生成记录。

## 22.3 高风险信号

- 明确要求复制现有艺术家风格；
- 明确要求复制现有角色；
- 使用未经授权的真实人物照片；
- 使用第三方 Logo；
- 使用受保护品牌元素；
- 使用来源不明参考图；
- 平台条款不允许商业使用；
- 无法确认模型来源或授权。

## 22.4 处理方式

```text
Approve
Approve with Attribution
Replace Reference
Regenerate
Manually Redesign
Legal Review Required
Reject
```

---

# 23. Privacy and Sensitive Data：隐私与敏感数据

不得将以下内容上传到未经批准的生成平台：

- 个人身份信息；
- 未公开产品资料；
- 客户数据；
- 医疗数据；
- 财务数据；
- 机密源代码；
- 未公开设计；
- 合同；
- 密钥；
- 内部账号信息；
- 未授权肖像。

如需使用内部资料，应先确认：

- 平台数据处理条款；
- 是否用于模型训练；
- 数据保留时间；
- 企业账户设置；
- 是否允许上传；
- 是否需要脱敏。

---

# 24. Model and Tool Management：模型与工具管理

## 24.1 需要记录

- 工具名称；
- 模型名称；
- 模型版本；
- 生成模式；
- 订阅或许可证；
- 商业使用条款；
- 数据保留策略；
- 支持格式；
- 已知限制。

## 24.2 模型版本变化

模型升级可能导致：

- 风格变化；
- Prompt 失效；
- 角色漂移；
- 构图变化；
- 质量变化；
- 安全规则变化。

因此批量生产期间应尽量固定模型版本。

---

# 25. Seed and Reproducibility：种子与可复现性

如果工具支持 Seed，应记录。

但即使 Seed 相同，以下变化也可能导致结果不同：

- 模型版本；
- 参考图；
- 生成参数；
- 平台后端；
- Prompt 顺序；
- 安全过滤；
- 输出尺寸。

可复现性不应只依赖 Seed。

应保存完整生成记录。

---

# 26. Cost and Iteration Control：成本与迭代控制

AI 生成容易造成无边界试错。

应控制：

- 每轮目标；
- 每轮候选数量；
- 每轮 Prompt 变化；
- 生成预算；
- 时间预算；
- 人工清理预算；
- 最大修改轮次；
- 停止条件。

## 26.1 停止条件

可以停止 AI 迭代，当：

- 已有候选满足目标；
- 后续改进更适合人工修整；
- 生成成本超过人工制作；
- 一致性无法继续提升；
- 模型能力不足；
- 法律风险不可接受；
- 技术规格无法满足。

---

# 27. When to Switch to Manual Production：何时转人工制作

应考虑停止 AI 生成并转人工，当：

- 同一错误反复出现；
- 角色身份持续漂移；
- 精确姿势无法控制；
- 分层成本过高；
- UI 结构不精确；
- 需要矢量源文件；
- 文字无法可靠生成；
- 细节修复成本超过重画；
- 商业或授权风险不清；
- 系列一致性要求很高。

---

# 28. Batch Production：批量生产

批量生成前必须先完成：

```text
Style Lock
Prompt Lock
Reference Lock
Technical Spec Lock
Naming Rule
Review Rule
```

## 28.1 批量前样本

应先完成少量代表样本：

- 简单对象；
- 复杂对象；
- 明亮配色；
- 暗色配色；
- 小尺寸；
- 大尺寸；
- 正面；
- 侧面；
- 动作状态。

## 28.2 批量过程

- 分批生成；
- 每批评审；
- 发现漂移立即暂停；
- 不在错误方向上继续扩产；
- 保留批次记录。

---

# 29. Quality Gate：质量 Gate

AI Asset 进入正式项目前，应经过以下 Gate。

## 29.1 Content Gate

- 内容正确；
- 无错误结构；
- 无不适当元素；
- 无乱码；
- 无水印。

## 29.2 Style Gate

- 符合风格；
- 系列一致；
- 身份一致；
- 比例一致。

## 29.3 Technical Gate

- 格式正确；
- 尺寸正确；
- 透明正确；
- 可导入；
- 正式构建可用。

## 29.4 Legal Gate

- 平台条款允许；
- 参考资料来源清楚；
- 无明显侵权风险；
- 记录完整。

## 29.5 Integration Gate

- 在真实场景中可用；
- 性能可接受；
- 无缺失依赖；
- 不破坏现有体验。

---

# 30. AI Asset Definition of Done

一个 AI Asset 可以被视为 Done，当：

- 使用目的明确；
- AI 适用性已确认；
- 规格完整；
- Prompt 和参考资料有记录；
- 模型和版本有记录；
- 候选已筛选；
- 必要人工修整已完成；
- 风格和身份一致性通过；
- 技术验证通过；
- 来源和授权审查通过；
- 无水印和乱码；
- 透明、分层和格式要求满足；
- 已在真实环境集成；
- 正式构建验证通过；
- 源文件和最终文件已归档；
- Owner 已批准。

---

# 31. 状态模型

推荐使用：

```text
Requested
Suitability Review
Specified
Prompting
Exploring
Selecting
Generating
Editing
Human Cleanup
In Review
Technical Validation
Legal Review
Approved
Integrated
Done
Released
Archived
```

补充：

```text
Blocked
Deferred
Rejected
Needs Regeneration
Needs Manual Redesign
Deprecated
Replaced
```

---

# 32. 文件与目录结构

示例：

```text
ai-assets/
├── requests/
├── references/
├── prompts/
│   ├── drafts/
│   └── approved/
├── generations/
│   ├── exploration/
│   ├── selected/
│   └── rejected/
├── edits/
├── source/
├── export/
├── runtime/
├── licenses/
├── logs/
└── archive/
```

---

# 33. 命名规范

建议包含：

```text
Asset
Purpose
Variant
Prompt Version
Generation Version
Edit Version
Status
```

示例：

```text
char_mage_idle_prompt-v004_gen-v012_edit-v003_approved.png
ui_icon_warning_prompt-v002_gen-v007_final.png
```

避免：

```text
image1.png
new.png
best.png
final-final.png
```

---

# 34. Generation Log 模板

```markdown
# AI Generation Log

> Asset：
> Owner：
> Date：
> Status：

## Tool

- Platform：
- Model：
- Model Version：
- Mode：
- Account / License：

## Prompt

### Positive Prompt

### Negative Prompt

## References

| File | Source | License | Purpose |
|---|---|---|---|

## Parameters

- Size：
- Seed：
- Strength：
- Steps：
- Other：

## Output Batch

| ID | File | Result | Decision |
|---|---|---|---|

## Findings

## Next Changes
```

---

# 35. AI Asset Request 模板

```markdown
# AI Asset Request

> ID：
> Asset：
> Type：
> Owner：
> Status：
> Target Milestone：
> Target Release：
> Intended Use：

## 1. Purpose

## 2. Suitability

- 为什么使用 AI：
- 为什么不使用传统方式：
- 风险：

## 3. Specification

- Size：
- Ratio：
- Format：
- Transparency：
- Platform：
- Source File Requirement：

## 4. Subject

## 5. Style

## 6. Composition

## 7. Consistency Requirements

## 8. References

## 9. Forbidden Elements

## 10. Legal / License Requirements

## 11. Acceptance Criteria
```

---

# 36. AI Asset Review 模板

```markdown
# AI Asset Review

> Asset：
> Version：
> Reviewer：
> Date：

## Content Review

- [ ] 主体正确；
- [ ] 构图正确；
- [ ] 无额外元素；
- [ ] 无结构错误；
- [ ] 无乱码或水印。

## Consistency Review

- [ ] 风格一致；
- [ ] 色彩一致；
- [ ] 比例一致；
- [ ] 身份一致；
- [ ] 系列一致。

## Technical Review

- [ ] 尺寸正确；
- [ ] 格式正确；
- [ ] Alpha 正确；
- [ ] 分层正确；
- [ ] 可导入；
- [ ] 正式构建可用。

## Legal Review

- [ ] 模型条款允许；
- [ ] 参考资料来源清楚；
- [ ] 商业使用允许；
- [ ] 无明显第三方 IP 风险；
- [ ] 生成记录完整。

## Decision

- [ ] Approved
- [ ] Approved with Conditions
- [ ] Needs Cleanup
- [ ] Needs Regeneration
- [ ] Needs Manual Redesign
- [ ] Rejected
```

---

# 37. 通用 AI Asset Checklist

```markdown
## Suitability

- [ ] AI 是合适工具；
- [ ] 使用风险可接受；
- [ ] 输出可人工验证；
- [ ] 商业用途被允许。

## Request

- [ ] 使用场景明确；
- [ ] 使用阶段明确；
- [ ] Owner 明确；
- [ ] 目标版本明确。

## Specification

- [ ] 尺寸明确；
- [ ] 比例明确；
- [ ] 格式明确；
- [ ] 透明要求明确；
- [ ] 分层要求明确；
- [ ] 一致性要求明确；
- [ ] 禁止项明确。

## Reference

- [ ] 来源清楚；
- [ ] 授权清楚；
- [ ] 无敏感数据；
- [ ] 无未授权肖像；
- [ ] 参考用途明确。

## Prompt

- [ ] Prompt 结构清楚；
- [ ] 重要要求优先；
- [ ] 数量明确；
- [ ] 方向明确；
- [ ] 负面约束明确；
- [ ] Prompt 已版本化。

## Generation

- [ ] 工具和模型已记录；
- [ ] 模型版本已记录；
- [ ] Seed 或参数已记录；
- [ ] 候选批次已记录；
- [ ] 结果已筛选。

## Cleanup

- [ ] 结构错误已修；
- [ ] 多余元素已删除；
- [ ] 边缘已清理；
- [ ] 透明正确；
- [ ] 文字已替换；
- [ ] 源文件可编辑。

## Consistency

- [ ] 风格一致；
- [ ] 色板一致；
- [ ] 比例一致；
- [ ] 身份一致；
- [ ] 系列一致。

## Technical

- [ ] 尺寸正确；
- [ ] 格式正确；
- [ ] 压缩正确；
- [ ] 可导入；
- [ ] 正式构建可用；
- [ ] 性能可接受。

## Legal

- [ ] 平台条款已确认；
- [ ] 模型使用权已确认；
- [ ] 参考资料授权已确认；
- [ ] 无明显版权或商标风险；
- [ ] 生成记录已保存。

## Archive

- [ ] Prompt 已归档；
- [ ] Reference 已归档；
- [ ] Generation Log 已归档；
- [ ] 源文件已归档；
- [ ] 最终文件已归档；
- [ ] 使用位置已记录。
```

---

# 38. 常见失败模式

## 38.1 把 AI 输出直接当成正式成品

结果：

- 结构错误；
- 边缘问题；
- 假透明；
- 角色漂移；
- 法律来源不清；
- 正式构建失败。

---

## 38.2 没有 Prompt 版本

结果：

- 无法复现；
- 无法比较；
- 不知道哪次修改有效；
- 批量资产不一致。

---

## 38.3 一次修改太多变量

结果：

- 无法知道哪项修改有效；
- 风格漂移；
- 迭代不可控。

---

## 38.4 只看单张图，不看系列

结果：

- 每张单独好看；
- 放在一起风格不一致；
- 比例、线条、色彩漂移。

---

## 38.5 角色一致性完全依赖 Prompt

结果：

- 脸型变化；
- 服装变化；
- 身材变化；
- 配饰消失；
- 左右方向错误。

改进：

- 使用设定图；
- 固定模型；
- 固定参考；
- 人工修整；
- 必要时改用传统制作。

---

## 38.6 把棋盘格当透明

结果：

- 正式场景出现灰白背景；
- 边缘污染；
- 导出不可用。

---

## 38.7 生成文字直接使用

结果：

- 乱码；
- 拼写错误；
- 本地化困难；
- 字体授权不清。

---

## 38.8 无限生成，不进入人工修整

结果：

- 成本持续增加；
- 质量提升有限；
- 问题反复出现；
- 无法形成正式资产。

---

## 38.9 来源和授权不记录

结果：

- 无法确认商业使用权；
- 无法证明参考来源；
- 发布时出现法律风险。

---

## 38.10 模型升级后继续批量生产

结果：

- 风格突然变化；
- Prompt 失效；
- 角色不一致；
- 批次无法统一。

---

## 38.11 AI 生成取代设计判断

问题：

- 生成结果看起来丰富；
- 但没有解决实际需求；
- 规格和用户价值被忽略。

AI 只能辅助生产，不能替代：

- 目标定义；
- 风格决策；
- 产品判断；
- 质量验收。

---

# 39. 指标

可参考：

- 平均生成轮次；
- 平均候选数量；
- 首轮选中率；
- 人工修整时间；
- Prompt 复用率；
- 系列一致性返工率；
- 技术验证失败率；
- 假透明问题数；
- 法律审查拒绝率；
- 模型切换影响数；
- AI 成本；
- 单资产总生产成本；
- AI 转人工比例。

指标应帮助判断：

- AI 是否真的节省成本；
- 哪类资产适合 AI；
- 哪类资产应转人工；
- Prompt 和参考体系是否稳定；
- 法律和技术风险是否可控。

---

# 40. 最终原则

AI Asset Workflow 的本质不是：

> 用 AI 尽快生成大量内容。

它的本质是：

> 在明确目标、规格、来源、风险和质量要求的前提下，将生成式 AI 纳入受控生产流程，并通过人工判断、修整、验证和归档，将不稳定输出转化为可正式使用的资产。

一个有效的 AI 资产流程应做到：

- 先判断是否适合使用 AI；
- 需求和规格清楚；
- Prompt 可版本化；
- 参考资料来源清楚；
- 模型和参数可追踪；
- 生成过程可比较；
- 人工修整不可缺失；
- 风格和身份一致；
- 技术验证充分；
- 法律风险可审查；
- 正式环境可用；
- 全部过程可归档和复现。
