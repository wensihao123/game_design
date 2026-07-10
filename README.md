# GameDesignDocs

记录游戏设计理念与思考的文档仓库。

## 这是什么

一个个人的游戏设计知识库，用来沉淀关于游戏设计的想法、原则、案例分析与灵感。
不是某一款具体游戏的策划案，而是可跨项目复用的设计理念与方法论。

## 内容方向

- **设计理念** — 核心玩法哲学、体验目标、设计价值观
- **系统与机制** — 玩法系统、机制拆解、数值与经济思路
- **案例分析** — 对现有游戏的拆解与学习笔记
- **灵感碎片** — 零散的点子、待验证的假设

## 目录结构

```
GameDesignDocs/
├── README.md                  # 本文件（分类导航）
├── design/                    # 玩法设计理念
│   ├── philosophy/            #   核心体验、设计价值观
│   ├── systems/              #   机制 / 系统 / 数值
│   └── case-studies/         #   拆游戏的学习笔记
├── engineering/               # 工程与架构实践
│   ├── architecture/         #   SOLID、模块边界、状态机
│   ├── patterns/             #   常用模式与反模式
│   └── godot/                #   引擎特定技巧
├── process/                   # 工作流、工具链、协作规范
└── inbox/                     # 未归类的灵感碎片，定期整理
```

## 文档索引

### 工程与架构

- [如何在 Godot 中践行 SOLID 思想](engineering/architecture/solid-in-godot.md) — 把 SOLID 五原则翻译为 Godot 原生语汇（节点 / 信号 / Resource / 场景组合）
- [Godot 模块边界精讲](engineering/architecture/module-boundaries.md) — 边界划在哪、由什么构成、如何验证；SOLID 篇的配套上游

## 约定

- 文档统一 Markdown 编写，文件名用简短 kebab-case slug（如 `solid-in-godot.md`），不加日期 / 序号。
- 新增文档后，在上方「文档索引」手工登记一条，作为唯一导航入口。
- 文档间用相对链接互相引用，表达知识关联。

## 使用说明

这是一个持续生长的仓库，随想随记。每篇文档尽量自成一体，方便日后检索与复用。
