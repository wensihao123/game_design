# Godot 项目版本、Git 与 Resource 管理

> 适用范围：Godot 4.x、GDScript 项目  
> 核心目标：避免升级失控、资源共享污染、项目文件丢失和无法回滚。

---

## 1. 固定 Godot 引擎版本

“使用 Godot 4”并不足够。不同小版本之间可能存在：

- API 调整；
- 默认导入行为变化；
- 渲染结果差异；
- 物理或导航行为变化；
- 插件和 GDExtension 不兼容；
- `.tscn`、`.tres`、`project.godot` 被自动重写。

### 推荐做法

在项目文档中明确写出版本，例如：

```text
Godot Engine: 4.x-stable
Renderer: Compatibility / Mobile / Forward+
GDScript only: Yes
```

团队成员必须使用同一版本。不要让部分成员使用稳定版，部分成员使用开发版或 RC 版。

### 升级流程

1. 提交当前所有代码和资源；
2. 创建独立升级分支；
3. 备份完整项目目录；
4. 阅读目标版本迁移说明；
5. 使用新版本打开复制项目，而不是唯一主项目；
6. 检查场景和资源文件的 Git diff；
7. 重新测试：
   - 输入；
   - 物理；
   - 动画；
   - Shader；
   - UI 缩放；
   - 导航；
   - 存档；
   - 所有发布平台的导出；
8. 确认插件和 GDExtension 已兼容；
9. 通过测试后再合并。

### 不要忽略场景文件的批量改写

新版本打开项目后，即使你没有主动修改场景，编辑器也可能重写资源格式。提交前应检查：

```bash
git diff
git status
```

发现大量无关场景变化时，不要直接全部提交。

---

## 2. 从项目第一天开始使用 Git

Godot 的 `.gd`、`.tscn`、`.tres`、`.gdshader` 等主要文件通常是文本格式，适合版本管理。

### 建议提交的内容

```text
project.godot
export_presets.cfg
addons/
assets/
scenes/
scripts/
resources/
shaders/
*.gd
*.tscn
*.tres
*.gdshader
```

### 通常不应提交的内容

```gitignore
.godot/
```

`.godot/` 主要包含导入缓存和编辑器生成内容，可由 Godot 重建。

还应避免提交：

- 导出凭据；
- 平台签名密码；
- 私钥；
- 商店 API 密钥；
- 本地测试存档；
- 临时构建目录；
- IDE 用户设置。

示例：

```gitignore
.godot/
build/
dist/
*.tmp
*.log
export_credentials.cfg
```

具体文件名应根据项目实际情况调整。

### 资源文件也必须纳入版本管理

不要只提交代码而忽略：

- 原始图片；
- 音频；
- 字体；
- JSON 和 CSV；
- Blender 或 Aseprite 源文件；
- 游戏配置 Resource；
- 导出配置。

否则即使代码存在，也无法完整恢复项目。

---

## 3. 避免多人同时编辑同一大型场景

`.tscn` 虽然是文本格式，但复杂场景的合并冲突仍然很难处理。

### 容易冲突的情况

- 两人同时调整同一场景节点层级；
- 一人移动节点，另一人修改该节点属性；
- 场景中有大量内嵌子资源；
- AnimationPlayer 动画轨道被多人同时修改；
- TileMap 或大型 UI 场景被多人编辑。

### 降低冲突的方法

- 将大型场景拆成多个子场景；
- 角色、UI 面板、战斗模块独立保存；
- 数据尽量放入独立 `.tres`；
- 不要把所有东西都内嵌在一个主场景；
- 明确场景文件负责人；
- 提交前拉取最新代码；
- 小步提交，不要积累几天后一次性提交。

---

## 4. Resource 默认可能是共享对象

这是 Godot 最容易忽略的陷阱之一。

假设多个敌人都引用同一个配置：

```gdscript
@export var stats: EnemyStats
```

错误做法：

```gdscript
func take_damage(amount: int) -> void:
    stats.health -= amount
```

如果多个敌人共享同一个 `EnemyStats` Resource，修改其中一个敌人的 `stats.health`，其他敌人也可能一起变化。

### 为什么会这样

Resource 通常按路径缓存：

```gdscript
var a := load("res://data/slime_stats.tres")
var b := load("res://data/slime_stats.tres")
```

`a` 和 `b` 很可能引用同一个对象，而不是两份独立数据。

### 正确的职责划分

Resource 保存静态配置：

```gdscript
class_name EnemyStats
extends Resource

@export var max_health: int = 100
@export var attack: int = 10
@export var move_speed: float = 60.0
```

节点保存运行时状态：

```gdscript
class_name HealthComponent
extends Node

@export var stats: EnemyStats

var current_health: int

func _ready() -> void:
    current_health = stats.max_health
```

不要把以下内容写回共享 Resource：

- 当前生命；
- 当前魔法；
- 技能冷却；
- Buff 剩余时间；
- 当前目标；
- 当前经验；
- 临时属性；
- 战斗内随机结果。

---

## 5. 什么时候复制 Resource

确实需要每个实例拥有独立 Resource 时，可以复制：

```gdscript
func _ready() -> void:
    stats = stats.duplicate(true)
```

也可以根据场景需求启用 `resource_local_to_scene`。

但是不要无脑复制所有 Resource，因为这会：

- 增加内存占用；
- 让资源引用关系更难追踪；
- 破坏本来应该共享的静态配置；
- 让 Inspector 中的修改和运行时实例产生混淆。

### 推荐规则

| 数据类型 | 推荐存放位置 |
|---|---|
| 最大生命、基础攻击 | Resource |
| 当前生命 | Node 或 RefCounted 运行时对象 |
| 技能图标、名称 | Resource |
| 当前冷却时间 | 技能运行时实例 |
| 装备基础属性 | Resource |
| 装备随机词条实例 | 独立实例数据 |
| 角色成长曲线 | Resource |
| 当前等级和经验 | 存档状态 |

---

## 6. Array 和 Dictionary 也可能共享引用

```gdscript
var a := {"hp": 100}
var b := a
b["hp"] = 10
```

此时 `a["hp"]` 也会变成 `10`。

需要复制时：

```gdscript
var b := a.duplicate(true)
```

同样要注意嵌套对象是否真的被深度复制。对于复杂运行时状态，最好定义明确的数据类，而不是无限嵌套 Dictionary。

---

## 7. 不要在 Inspector 中误改共享子资源

Material、ShaderMaterial、SpriteFrames、Curve、Gradient 等也属于 Resource。

常见现象：

- 改一个敌人的材质颜色，所有敌人一起变色；
- 修改一个 AnimatedSprite2D 的 SpriteFrames，其他实例也被影响；
- 调整某个 UI StyleBox，所有使用它的控件一起变化。

### 处理方式

需要独立修改时：

- 在 Inspector 中选择“Make Unique”；
- 创建独立资源文件；
- 或在运行时 `duplicate()`；
- 将动态参数通过 Shader uniform、节点属性或实例状态传入。

---

## 8. 推荐目录结构

```text
res://
├── assets/
│   ├── characters/
│   ├── environments/
│   ├── ui/
│   ├── audio/
│   └── fonts/
├── scenes/
│   ├── characters/
│   ├── enemies/
│   ├── levels/
│   ├── ui/
│   └── effects/
├── scripts/
│   ├── components/
│   ├── systems/
│   ├── ui/
│   └── utilities/
├── resources/
│   ├── characters/
│   ├── skills/
│   ├── items/
│   └── balance/
├── addons/
├── tests/
└── project.godot
```

不要把所有文件平铺在根目录，也不要建立过度深层、难以导航的目录。

---

## 9. 发布前检查清单

- [ ] 项目使用明确的稳定版 Godot；
- [ ] 所有成员使用相同版本；
- [ ] 项目完整纳入 Git；
- [ ] `.godot/` 未提交；
- [ ] 导出凭据和密钥未提交；
- [ ] 所有原始资源均可恢复；
- [ ] Resource 配置与运行时状态分离；
- [ ] 动态修改前确认资源是否共享；
- [ ] Material、SpriteFrames、StyleBox 未被意外共享修改；
- [ ] 升级引擎前创建分支和备份；
- [ ] 升级后完成全平台导出测试。
