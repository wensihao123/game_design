# Glossary（术语表）

> Status: V1  
> Scope: `design/philosophy/`  
> Purpose: 统一设计理念、系统设计、功能规格与验证文档中的核心术语，减少同义混用、多义冲突和技术术语泄漏。

---

## 1. 使用原则

### 1.1 一词一义

一个主术语应尽量对应一个稳定概念。

例如：

```text
“难度”描述完成目标所需的综合要求；
“挑战”描述玩家需要使用什么能力克服阻力。
```

两者相关，但不能完全互换。

### 1.2 一义一词

同一个概念应优先使用一个中文主术语。

例如：

```text
统一使用“核心体验”，
不在同一体系中随意混用“主要体验”“核心感受”“核心乐趣”。
```

### 1.3 首次出现时说明

当术语首次出现在文档中时，建议使用：

```text
中文主术语（English Term）
```

后续只使用中文主术语。

### 1.4 主文档负责完整定义

每个术语应有一个主要定义来源。

其他文档可以：

- 引用；
- 摘要；
- 说明其在当前主题下的应用；

但不应重新创造相互冲突的定义。

### 1.5 技术术语不直接替代玩家术语

内部数据字段、代码名称和实现缩写，不应自动成为玩家可见名称。

例如：

```text
技术字段：energy_point
玩家术语：行动力
```

### 1.6 允许项目级扩展

具体项目可以增加：

- 世界观术语；
- 系统术语；
- 资源名称；
- 内容分类；

但不应改变本术语表中上层概念的基本含义。

---

## 2. 术语状态

术语可使用以下状态：

| Status | 含义 |
|---|---|
| Canonical | 当前推荐主术语 |
| Accepted Alias | 可以在特定语境使用的别名 |
| Context-Specific | 仅限特定系统或项目 |
| Deprecated | 不建议继续使用 |
| Reserved | 已保留但暂未正式定义 |

---

## 3. 核心理念术语

| English Term | 中文主术语 | 定义 | 不建议混用 | 主文档 |
|---|---|---|---|---|
| Design Philosophy | 设计理念 | 用于指导长期设计判断的稳定价值与方向 | 设计规范、功能需求 | [Design Principles](./foundation/design-principles.md) |
| Principle | 原则 | 在多个情境中应优先遵循的稳定判断标准 | 规则、建议、偏好 | [Design Principles](./foundation/design-principles.md) |
| Heuristic | 启发式方法 | 在信息不完整时帮助快速判断的经验方法 | 强制规则、证明 | [Design Principles](./foundation/design-principles.md) |
| Rule | 规则 | 在明确范围内必须稳定执行的约束或结算逻辑 | 原则、建议 | [Design Principles](./foundation/design-principles.md) |
| Guideline | 指导准则 | 比原则更具体、比规则更灵活的执行建议 | 强制规则 | [Design Principles](./foundation/design-principles.md) |
| Constraint | 约束 | 设计必须接受的边界条件 | 目标、偏好 | [Design Principles](./foundation/design-principles.md) |
| Exception | 例外 | 在明确范围内偏离基础规则的特殊处理 | 新规则、临时补丁 | [Consistency and Coherence](./long-term/consistency-and-coherence.md) |
| Evidence | 证据 | 用于支持或反对假设与设计判断的信息 | 意见、直觉 | [Iteration and Validation](./validation/iteration-and-validation.md) |
| Assumption | 假设 | 可以通过证据支持或推翻的判断 | 事实、结论 | [Iteration and Validation](./validation/iteration-and-validation.md) |
| Metric | 指标 | 用于量化行为、结果或状态的测量值 | 目标、体验本身 | [Iteration and Validation](./validation/iteration-and-validation.md) |

---

## 4. 玩家与体验术语

| English Term | 中文主术语 | 定义 | 不建议混用 | 主文档 |
|---|---|---|---|---|
| Player | 玩家 | 直接参与游戏或交互体验的人 | 用户，除非讨论通用软件 | [Player First Design](./foundation/player-first-design.md) |
| User | 用户 | 使用软件、服务或工具的人 | 玩家，除非语境为游戏 | [Player First Design](./foundation/player-first-design.md) |
| Target Player | 目标玩家 | 当前设计主要服务的人群及其情境 | 全体用户、理想用户 | [Player First Design](./foundation/player-first-design.md) |
| Player Goal | 玩家目标 | 玩家在当前情境中希望完成的事情 | 系统目标、任务条件 | [Player First Design](./foundation/player-first-design.md) |
| Player Need | 玩家需求 | 支撑目标、体验与行为的底层需要 | 玩家提出的具体方案 | [Player First Design](./foundation/player-first-design.md) |
| Player Preference | 玩家偏好 | 玩家倾向的风格、节奏或表达方式 | 必要需求 | [Player First Design](./foundation/player-first-design.md) |
| Player Cost | 玩家成本 | 玩家为完成目标付出的时间、注意力、操作、资源、风险和情绪成本 | 价格、资源消耗 | [Player First Design](./foundation/player-first-design.md) |
| Agency | 能动性 | 玩家相信自己能够通过判断与行动影响结果的感受 | 自由度、选项数量 | [Player First Design](./foundation/player-first-design.md) |
| Player Fantasy | 玩家幻想 | 玩家希望成为谁、拥有什么能力、扮演什么角色并产生什么影响 | 世界观、剧情主题 | [Core Experience and Fantasy](./foundation/core-experience-and-fantasy.md) |
| Core Experience | 核心体验 | 玩家在核心循环中反复获得、最能代表项目身份的体验 | 核心功能、玩法类型 | [Core Experience and Fantasy](./foundation/core-experience-and-fantasy.md) |
| Experience Pillar | 体验支柱 | 支撑核心体验的少量稳定体验标准 | 功能列表、卖点列表 | [Core Experience and Fantasy](./foundation/core-experience-and-fantasy.md) |
| Emotional Goal | 情绪目标 | 设计希望玩家在特定阶段产生的主要情绪状态 | 文案语气、氛围标签 | [Core Experience and Fantasy](./foundation/core-experience-and-fantasy.md) |
| Experience Promise | 体验承诺 | 产品向玩家持续提供的核心价值与预期 | 市场口号 | [Core Experience and Fantasy](./foundation/core-experience-and-fantasy.md) |
| Core Loop | 核心循环 | 玩家反复经历的目标、准备、行动、反馈、结果与新目标结构 | 单个功能流程 | [Core Experience and Fantasy](./foundation/core-experience-and-fantasy.md) |

---

## 5. 清晰度、复杂度与反馈术语

| English Term | 中文主术语 | 定义 | 不建议混用 | 主文档 |
|---|---|---|---|---|
| Clarity | 清晰度 | 玩家能否理解当前状态、可用行动、规则、风险与结果 | 简单、信息量少 | [Clarity and Feedback](./experience/clarity-and-feedback.md) |
| Feedback | 反馈 | 系统对输入、状态变化、结果和因果关系的可感知回应 | 奖励、提示 | [Clarity and Feedback](./experience/clarity-and-feedback.md) |
| State Visibility | 状态可见性 | 玩家能否及时识别当前重要状态 | 数据公开 | [Clarity and Feedback](./experience/clarity-and-feedback.md) |
| Causality | 因果关系 | 玩家能否理解什么原因导致了什么结果 | 时间先后 | [Clarity and Feedback](./experience/clarity-and-feedback.md) |
| Predictability | 可预测性 | 玩家能否根据规则和信息合理预期结果 | 结果完全确定 | [Clarity and Feedback](./experience/clarity-and-feedback.md) |
| Complexity | 复杂度 | 玩家理解、记忆、操作和管理系统所需的综合负担 | 深度、难度 | [Simplicity and Depth](./experience/simplicity-and-depth.md) |
| Depth | 深度 | 相对有限的规则在不同情境中产生持续有意义判断和组合的能力 | 内容数量、复杂度 | [Simplicity and Depth](./experience/simplicity-and-depth.md) |
| Surface Area | 系统表面积 | 玩家需要接触、学习和管理的系统入口、状态、资源和规则总量 | 内容量 | [Simplicity and Depth](./experience/simplicity-and-depth.md) |
| Progressive Disclosure | 渐进式披露 | 按时间、情境、需求和熟练度逐步展示信息与功能 | 隐藏信息 | [Simplicity and Depth](./experience/simplicity-and-depth.md) |
| Orthogonal Design | 正交设计 | 选项在不同维度上形成明确差异，而不是只做同轴数值变化 | 完全独立、互不关联 | [Simplicity and Depth](./experience/simplicity-and-depth.md) |
| Complexity Debt | 复杂度债务 | 临时规则、重复入口、例外和历史方案积累形成的理解与维护负担 | 技术债务 | [Simplicity and Depth](./experience/simplicity-and-depth.md) |

---

## 6. 选择、挑战与公平术语

| English Term | 中文主术语 | 定义 | 不建议混用 | 主文档 |
|---|---|---|---|---|
| Option | 选项 | 系统提供的可选行动或方案 | 有意义选择 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Choice | 选择 | 玩家基于目标、信息和偏好，在多个可能行动之间做出的决定 | 点击、操作 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Meaningful Choice | 有意义的选择 | 具有可理解差异、真实代价、玩家能动性和可感知后果的选择 | 选项数量 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Consequence | 后果 | 选择对状态、资源、关系、机会、情绪或未来产生的影响 | 惩罚、结果动画 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Trade-off | 权衡 | 获得一种价值时必须放弃、削弱或承担另一种价值 | 单纯成本 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Opportunity Cost | 机会成本 | 选择某个选项时放弃的其他机会价值 | 资源价格 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Reversibility | 可逆性 | 选择是否可以撤回、修改或重置 | 可恢复性 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Commitment | 承诺 | 玩家为某个方向承担调整成本和未来限制 | 永久锁定 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Effective Choice Space | 有效选择空间 | 当前情境下真正值得玩家考虑的选项集合 | 理论选项数量 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Dominant Strategy | 支配策略 | 在绝大多数相关情境中明显优于其他方案的策略 | 热门选择 | [Choice and Consequence](./experience/choice-and-consequence.md) |
| Challenge | 挑战 | 要求玩家投入理解、判断、技巧、规划或承诺才能克服的阻力 | 难度、惩罚 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Difficulty | 难度 | 完成目标所需能力、投入、时间压力和容错要求的综合程度 | 挑战类型 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Fairness | 公平 | 玩家相信规则稳定、信息足够、输入可靠、应对机会合理且结果可归因 | 所有人结果相同 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Counterplay | 反制空间 | 玩家面对威胁时可理解并可执行的应对方式 | 免费解决方案 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Failure | 失败 | 玩家未达到当前目标或进入不利结果的状态 | 惩罚 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Punishment | 惩罚 | 失败后由系统施加的时间、资源、进度或机会损失 | 挑战、后果 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Recoverability | 可恢复性 | 玩家从错误、失败、中断或不利状态中恢复的能力 | 可逆性 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Randomness | 随机性 | 由概率或不可完全预测因素改变情境或结果的机制 | 任意性 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Input Randomness | 输入随机性 | 随机生成玩家随后可以应对的情境 | 操作随机 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |
| Output Randomness | 输出随机性 | 玩家行动后由随机决定结果 | 内容生成 | [Challenge and Fairness](./experience/challenge-and-fairness.md) |

---

## 7. 成长、动机与节奏术语

| English Term | 中文主术语 | 定义 | 不建议混用 | 主文档 |
|---|---|---|---|---|
| Progression | 成长 | 玩家、角色、选择空间、内容、世界或身份随时间产生的可感知变化 | 数值提升 | [Progression and Motivation](./long-term/progression-and-motivation.md) |
| Motivation | 动机 | 推动玩家开始、继续或重新进入体验的原因 | 奖励、留存 | [Progression and Motivation](./long-term/progression-and-motivation.md) |
| Intrinsic Motivation | 内在动机 | 玩家因为行为本身具有掌握、好奇、表达或关系价值而参与 | 免费动机 | [Progression and Motivation](./long-term/progression-and-motivation.md) |
| Extrinsic Motivation | 外在动机 | 玩家因为资源、奖励、排名、成就或解锁而参与 | 商业动机 | [Progression and Motivation](./long-term/progression-and-motivation.md) |
| Reward | 奖励 | 系统对行动、目标、发现或成就提供的反馈与价值 | 成长、反馈 | [Progression and Motivation](./long-term/progression-and-motivation.md) |
| Retention | 留存 | 玩家愿意在未来再次返回体验的结果 | 动机、强制登录 | [Progression and Motivation](./long-term/progression-and-motivation.md) |
| Grind | 重复劳动 | 主要依赖重复相同行为获取进度，且缺少新判断、学习或表达的过程 | 所有重复 | [Progression and Motivation](./long-term/progression-and-motivation.md) |
| Catch-up | 追赶机制 | 帮助中断、新加入或落后玩家重新进入当前有效内容的机制 | 直接复制全部成果 | [Progression and Motivation](./long-term/progression-and-motivation.md) |
| Pacing | 节奏 | 行动、决策、信息、反馈、等待和情绪随时间展开的速度与结构 | 速度 | [Pacing and Rhythm](./experience/pacing-and-rhythm.md) |
| Rhythm | 韵律 | 体验中的重复、对比、停顿、加速和回落模式 | 音乐节拍 | [Pacing and Rhythm](./experience/pacing-and-rhythm.md) |
| Tension | 张力 | 风险、不确定性、压力和高影响选择形成的情绪强度 | 难度 | [Pacing and Rhythm](./experience/pacing-and-rhythm.md) |
| Release | 释放 | 高张力后通过成功、安全、奖励或停顿形成的情绪回落 | 奖励 | [Pacing and Rhythm](./experience/pacing-and-rhythm.md) |
| Recovery Space | 恢复空间 | 低强度、低风险、低认知负担，用于整理和消化的体验阶段 | 空白内容 | [Pacing and Rhythm](./experience/pacing-and-rhythm.md) |
| Decision Density | 决策密度 | 单位时间内需要进行判断与承担后果的频率 | 操作次数 | [Pacing and Rhythm](./experience/pacing-and-rhythm.md) |
| Natural Stopping Point | 自然停止点 | 玩家可以清楚、安全且无额外损失地结束当前会话的节点 | 流程终点 | [Pacing and Rhythm](./experience/pacing-and-rhythm.md) |

---

## 8. 一致性、责任与验证术语

| English Term | 中文主术语 | 定义 | 不建议混用 | 主文档 |
|---|---|---|---|---|
| Consistency | 一致性 | 相同或相似事物遵循相同术语、操作、规则和反馈 | 完全相同 | [Consistency and Coherence](./long-term/consistency-and-coherence.md) |
| Coherence | 整体协调性 | 不同系统与表现共同服务同一核心体验并形成完整整体 | 一致性 | [Consistency and Coherence](./long-term/consistency-and-coherence.md) |
| Consistency Debt | 一致性债务 | 历史系统、局部方案和例外积累形成的认知与维护负担 | 复杂度债务 | [Consistency and Coherence](./long-term/consistency-and-coherence.md) |
| Accessibility | 可访问性 | 减少感知、理解、输入和操作中的非核心障碍 | 简单模式 | [Accessibility and Inclusivity](./responsibility/accessibility-and-inclusivity.md) |
| Inclusivity | 包容性 | 避免在目标用户、表达和系统假设中无意排斥不同玩家 | 角色外观多样性 | [Accessibility and Inclusivity](./responsibility/accessibility-and-inclusivity.md) |
| Assistance | 辅助 | 调整输入、信息、速度、惩罚或自动化以支持参与的设计 | 降低一切挑战 | [Accessibility and Inclusivity](./responsibility/accessibility-and-inclusivity.md) |
| Redundant Coding | 冗余编码 | 使用颜色、图标、形状、声音等多个通道表达同一关键信息 | 重复提示 | [Accessibility and Inclusivity](./responsibility/accessibility-and-inclusivity.md) |
| Ethical Design | 伦理设计 | 在商业与参与目标中保护玩家自主、知情、时间、金钱、隐私与尊严 | 仅法律合规 | [Ethical Design](./responsibility/ethical-design.md) |
| Manipulation | 操纵性设计 | 依赖误导、压力、损失恐惧或认知偏差替代玩家真实意愿的影响方式 | 所有行为引导 | [Ethical Design](./responsibility/ethical-design.md) |
| FOMO | 错过恐惧 | 玩家因担心失去内容、进度、资格或群体关系而被推动参与 | 所有限时内容 | [Ethical Design](./responsibility/ethical-design.md) |
| Dark Pattern | 暗黑模式 | 通过界面与流程误导玩家做出不符合真实意愿的选择 | 复杂界面 | [Ethical Design](./responsibility/ethical-design.md) |
| Data Minimization | 数据最小化 | 只收集实现明确功能所必需的数据 | 不收集数据 | [Ethical Design](./responsibility/ethical-design.md) |
| Validation | 验证 | 检查关键假设是否得到真实证据支持 | 测试功能是否完成 | [Iteration and Validation](./validation/iteration-and-validation.md) |
| Iteration | 迭代 | 通过假设、原型、证据、判断和调整逐步降低不确定性 | 持续增加功能 | [Iteration and Validation](./validation/iteration-and-validation.md) |
| Prototype | 原型 | 以最低必要成本验证特定假设的设计表达 | 半成品 | [Iteration and Validation](./validation/iteration-and-validation.md) |
| Qualitative Evidence | 定性证据 | 用于解释玩家为何产生某种行为和感受的观察与叙述 | 主观意见 | [Iteration and Validation](./validation/iteration-and-validation.md) |
| Quantitative Evidence | 定量证据 | 用于描述行为规模、分布和趋势的数据 | 事实原因 | [Iteration and Validation](./validation/iteration-and-validation.md) |
| Success Criterion | 成功标准 | 在测试前定义、用于判断假设获得支持的条件 | 目标口号 | [Iteration and Validation](./validation/iteration-and-validation.md) |
| Failure Criterion | 失败标准 | 在测试前定义、用于判断方案不成立或风险不可接受的条件 | Bug 列表 | [Iteration and Validation](./validation/iteration-and-validation.md) |

---

## 9. 容易混淆的术语对

### 9.1 Challenge vs Difficulty

```text
挑战：
玩家需要运用什么能力。

难度：
完成目标所需能力、压力和容错的综合程度。
```

### 9.2 Consequence vs Punishment

```text
后果：
选择造成的任何实际影响，可以是正面、负面或中性。

惩罚：
失败后由系统施加的损失。
```

### 9.3 Reversibility vs Recoverability

```text
可逆性：
能否撤回或修改原选择。

可恢复性：
能否从错误或不利结果中重新建立可玩状态。
```

### 9.4 Complexity vs Depth

```text
复杂度：
理解和操作负担。

深度：
持续产生有意义判断的能力。
```

### 9.5 Feedback vs Reward

```text
反馈：
说明系统发生了什么。

奖励：
向玩家提供价值或认可。
```

### 9.6 Motivation vs Retention

```text
动机：
为什么行动或返回。

留存：
最终是否返回的结果。
```

### 9.7 Accessibility vs Assistance

```text
可访问性：
整体减少非核心障碍的设计目标。

辅助：
实现该目标的一类具体手段。
```

### 9.8 Consistency vs Coherence

```text
一致性：
相似事物是否按相同方式工作。

整体协调性：
不同事物是否共同服务同一体验。
```

### 9.9 Evidence vs Metric

```text
指标：
一个测量值。

证据：
经过情境解释后，可以支持或反对判断的信息。
```

---

## 10. 项目术语扩展模板

```markdown
## Project Term

| Field | Content |
|---|---|
| English Term |  |
| 中文主术语 |  |
| Status | Canonical / Alias / Deprecated |
| Definition |  |
| Scope |  |
| Not Equivalent To |  |
| Player-Facing Example |  |
| Technical Mapping |  |
| Owner |  |
| Review Condition |  |
```

---

## 11. 术语变更记录

```markdown
| Date | Term | Old Usage | New Usage | Reason | Affected Documents | Owner |
|---|---|---|---|---|---|---|
```

重大术语变更应同步检查：

- README；
- Principle Map；
- Philosophy 正文；
- Systems 文档；
- UI 文案；
- 数据字段映射；
- 本地化术语表；
- 分析事件名称。

---

## 12. 术语审计清单

- [ ] 同一个概念只有一个中文主术语；
- [ ] 同一个术语没有承担多个无关含义；
- [ ] 英文和中文映射稳定；
- [ ] 主定义文档明确；
- [ ] 技术术语没有直接泄漏到玩家界面；
- [ ] 缩写均有解释；
- [ ] 本地化术语与主定义一致；
- [ ] 弃用术语有迁移记录；
- [ ] 新术语没有重复现有概念；
- [ ] 术语变化已同步全部相关文档。

---

## 13. Related Documents

- [Philosophy README](./README.md)
- [Principle Map](./principle-map.md)
- [Conflict Resolution](./conflict-resolution.md)
- [Design Principles](./foundation/design-principles.md)
- [Consistency and Coherence](./long-term/consistency-and-coherence.md)
