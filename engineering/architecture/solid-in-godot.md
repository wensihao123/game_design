# Godot 游戏开发中的 SOLID 实践指导

> **文档定位**:本文档是 Godot 4.x 项目的架构指导思想,用于指导人工编码与 AI 代理(Claude Code)的实现决策。核心主张:不照搬教科书式 OOP,而是将 SOLID 五原则"翻译"为 Godot 的原生语汇——节点、信号、Resource、场景组合。
>
> **适用范围**:GDScript 为主的 Godot 4.x 项目,尤其适用于数据驱动的模拟/管理类游戏架构(静态定义层 / 运行时状态层 / 系统层)。

---

## 0. 总纲:一个心法,两条纪律

Godot 的场景树容易诱导"一切皆节点、逻辑写在 `_process` 里"的写法。SOLID 落地的核心动作只有一个:

> **把游戏规则层做成不依赖场景树的纯对象(RefCounted / Resource),节点层退化为规则层的视图与输入适配器。**

由此推出两条最高纪律:

1. **系统层代码不出现 `get_node` / `$` / `%`** —— 规则逻辑不感知场景树的存在。
2. **节点层不写游戏规则** —— 节点只做表现(渲染/动画/音效)和输入转发。

守住这两条,五个原则的大部分要求会被自动满足。这也是 headless 测试可行性的前提:规则层可以在无渲染环境下被直接实例化和验证。

---

## 1. S — 单一职责原则(SRP)

### 1.1 Godot 语境下的表述

**一个脚本只有一个变更理由。** 场景树本身就是组合结构,Godot 天然鼓励把职责拆分到子节点或独立类。

### 1.2 核心判断标准

一个脚本如果同时在做"表现"和"逻辑",就该拆。典型拆分模式:

| 职责 | 载体 | 依赖 |
|------|------|------|
| 规则/计算(Logic) | `RefCounted` 纯类 | 仅依赖数据定义与状态,**不依赖节点** |
| 表现(View) | 节点脚本 | 依赖 Logic 的输出(通过信号或只读接口) |
| 交互(Input) | 节点脚本 / 独立输入组件 | 将输入事件翻译为对 Logic 的调用 |

### 1.3 实践规则

- **系统逻辑从节点中剥离**:tick 系统、经济系统、事件系统写成不继承 `Node` 的纯 GDScript 类。这同时服务于 SRP 和 headless 自动化测试。
- **一个场景一个关注点**:场景(`.tscn`)是复用单元,一个场景应当能被单独运行/测试(Godot 官方推荐的 "scenes should run by themselves" 惯例)。
- **`_process` / `_physics_process` 是味道高发区**:若其中出现状态机分支、资源计算、AI 决策混杂,立即拆分。

### 1.4 坏味道清单

- ❌ 一个 `Building.gd` 同时处理产出计算、tween 动画、点击响应
- ❌ 脚本超过 ~300 行且包含多个不相关的方法群
- ❌ 节点脚本中出现纯数学/纯数据变换函数(应下沉到工具类或 Logic 类)

---

## 2. O — 开闭原则(OCP)

### 2.1 Godot 语境下的表述

**新增内容 = 新增数据文件,而非修改系统代码。** 这是数据驱动架构中回报率最高的原则。

### 2.2 实现手段

**(a) 自定义 Resource 作为定义层**

```gdscript
class_name BuildingDef extends Resource

@export var id: StringName
@export var display_name: String
@export var base_output: int
@export var effect_tags: Array[StringName]  # 封闭 tag 集合中的组合
```

系统代码只面向 Def 的字段编程。新增一种建筑 = 新增一个 `.tres` 或一条 JSON 记录,零代码改动。

**(b) 封闭效果分类法(Closed Tag Taxonomy)**

- 系统对 **tag 集合关闭**:所有效果类型枚举穷尽、系统代码为每个 tag 实现一次处理逻辑,之后不再新增分支。
- 内容对 **tag 组合开放**:新内容通过组合既有 tag + 参数表达,不引入新代码路径。
- 这是 OCP 在数据驱动模拟游戏中的最佳体现:扩展发生在数据维度,修改被隔离在分类法演进时刻。

**(c) 策略对象兜底**

极少数确实需要代码级差异的内容(特殊事件脚本),在 Resource 中挂一个 `GDScript` 类型字段作为策略对象,系统统一调用约定接口:

```gdscript
@export var custom_behavior: GDScript  # 须实现 execute(ctx) -> void

func run_event(def: EventDef, ctx: EventContext) -> void:
    if def.custom_behavior:
        def.custom_behavior.new().execute(ctx)
```

### 2.3 坏味道清单

- ❌ 新增一种建筑需要在系统代码里加 `match building_id:` 分支
- ❌ 效果处理函数中出现针对具体内容 ID 的硬编码特判
- ❌ tag 集合频繁扩张(说明分类法抽象层级选错了)

---

## 3. L — 里氏替换原则(LSP)

### 3.1 Godot 语境下的表述

**少继承,多组合。** GDScript 是鸭子类型,LSP 违反不会在编译期暴露,只会在运行时炸,因此要比静态语言更克制地使用继承。

### 3.2 实践规则

- **继承深度硬上限**:引擎类 → 自定义基类 → 具体类,**不超过一层自定义继承**。
- **override 变更语义 = 改组合的信号**:若子类 override 父类方法且行为语义改变(父类 `take_damage` 扣血,子类改成反弹),立即重构为组件组合——差异行为做成挂在节点上的组件子节点。
- **抽象契约的表达方式**(按优先级):
  1. 组件化组合(最符合引擎惯例)
  2. Godot 4.5+ 的 `@abstract` 抽象类
  3. `class_name` 基类 + `push_error` 桩方法
  4. `has_method()` + 命名约定(松散接口,谨慎使用)

### 3.3 坏味道清单

- ❌ 三层以上的自定义继承链
- ❌ 子类 override 后需要调用者"知道是哪个子类"才能安全使用
- ❌ 子类中出现空实现或抛错实现的继承方法(继承了不需要的契约)

---

## 4. I — 接口隔离原则(ISP)

### 4.1 Godot 语境下的表述

**信号就是最小接口。** 消费者不应持有整个对象引用去调用其中一两个方法。

### 4.2 实践规则

- **按需订阅,而非全量引用**:UI 只需要知道"金钱变了",就连接 `EventBus.money_changed`,而不是持有整个 `GameState`。
- **拆分巨型 Autoload**:一个暴露五十个方法、被所有系统依赖的 `GameManager` 单例是 ISP 的头号违反。拆法:
  - 按领域切分为多个小 autoload;或
  - 一个**纯信号的 EventBus** + 多个非全局系统对象,由组合根(main 场景)负责装配。
- **信号命名承载语义**:`money_changed(new_amount)` 优于 `state_updated`——细粒度信号本身就是隔离后的接口。

### 4.3 坏味道清单

- ❌ 某个 autoload 被项目中 80% 的脚本引用
- ❌ 连接了信号却在回调里先做 `if not relevant: return` 过滤(说明信号粒度太粗)
- ❌ 为了调一个查询方法而持有整个系统对象的引用

---

## 5. D — 依赖倒置原则(DIP)

### 5.1 Godot 语境下的表述

**"Call down, signal up" 就是 DIP 的引擎方言。**

- 父节点可以直接调用子节点方法(高层依赖其抽象出的子节点职责)。
- 子节点**绝不** `get_parent()` 向上调用具体类型,而是发信号,由上层决定如何响应。

### 5.2 系统层的依赖注入

GDScript 里的 DI 可以非常朴素——构造函数显式注入:

```gdscript
class_name EconomySystem extends RefCounted

var _state: GameState
var _defs: DefDatabase

func _init(state: GameState, defs: DefDatabase) -> void:
    _state = state
    _defs = defs
```

由入口场景(组合根)统一构造与装配。收益:

- headless 测试可直接喂入 mock 状态,无需启动完整游戏;
- 依赖关系在装配点一目了然,而非散落在各处的全局取用。

### 5.3 坏味道清单

- ❌ `get_node("../../HUD/MoneyLabel")` 式长相对路径——低层硬编码了高层结构
- ❌ 子节点中出现 `get_parent().some_concrete_method()`
- ❌ 系统类内部直接引用 `SomeAutoload.instance` 获取依赖(隐式全局依赖,不可测)

---

## 6. 架构分层与原则映射

针对"静态定义 / 运行时状态 / 系统"三层架构,各层的 SOLID 责任:

| 层 | 载体 | 主要原则 | 关键纪律 |
|----|------|----------|----------|
| 静态定义层 | Resource / JSON | **O** | 新内容只增数据;封闭 tag 分类法 |
| 运行时状态层 | RefCounted 纯数据类 | **S** | 只存状态,不含行为逻辑 |
| 系统层 | RefCounted 纯逻辑类 | **S / D** | 构造注入依赖;不感知场景树 |
| 节点/表现层 | Node 脚本 | **I / D** | 订阅细粒度信号;call down signal up |
| 组合根 | main 场景脚本 | **D** | 唯一的装配点;唯一允许"知道一切"的地方 |

---

## 7. 快速自检清单(Code Review / AI 代理验收用)

- [ ] 系统层类是否全部不继承 `Node`、不含 `get_node`?
- [ ] 新增一条内容(建筑/事件/buff)是否零代码改动即可完成?
- [ ] 是否存在超过一层的自定义继承?能否改为组件组合?
- [ ] 是否有 autoload 被超过一半的脚本依赖?
- [ ] 是否存在 `get_parent()` 向上调用或跨层长节点路径?
- [ ] 系统对象的依赖是否全部通过 `_init` 注入、由组合根装配?
- [ ] 每个系统能否在 headless 模式下用 mock 状态独立测试?

---

## 8. 与 AI 代理协作的补充约定

供 CLAUDE.md 引用或摘录:

1. 实现任何游戏规则前,先确认它属于系统层,产出不依赖节点的纯类。
2. 需要新内容时,优先检查能否用既有 tag 组合 + 数据文件表达;若必须扩展 tag 集合,须先在设计文档中更新分类法。
3. 禁止在节点脚本中新增规则计算;发现既有违反时标记为技术债,不顺手扩大。
4. 所有新系统类必须附带可 headless 运行的最小测试脚本。
