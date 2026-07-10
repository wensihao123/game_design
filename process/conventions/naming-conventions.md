# Naming Conventions（命名规范）

> 状态：草案  
> 适用范围：通用的软件、游戏、媒体与数字产品项目  
> 目的：定义代码、文件、目录、场景、资源、数据、资产、分支、版本、文档和运行时对象的统一命名规则。

---

## 1. 文档目的

Naming Conventions 用于统一项目中的命名方式，确保名称能够：

- 表达真实职责；
- 降低理解成本；
- 提高搜索效率；
- 支持自动化处理；
- 减少重复概念；
- 避免缩写和歧义；
- 保持跨模块一致；
- 支持长期维护；
- 降低多人协作中的认知差异。

它回答以下问题：

- 文件和目录如何命名；
- 类、函数、变量、常量如何命名；
- Scene、Resource、Node、Signal 如何命名；
- UI、输入、碰撞层、配置、事件如何命名；
- 资产、动画、音频、图标如何命名；
- 分支、Commit、Tag 和 Release 如何命名；
- 文档、ADR、模板如何命名；
- 哪些缩写可以使用；
- 如何处理历史命名和重命名；
- 如何避免同一概念出现多个名称。

命名规范的目标不是让所有名称看起来一样。

它的目标是：

> 让名称稳定表达意图，使读者在不打开实现细节的情况下，也能快速理解对象是什么、负责什么、属于哪里。

---

## 2. 核心原则

### 2.1 名称应表达意图

好的名称应回答：

```text
它是什么？
它做什么？
它属于什么范围？
它与其他对象有什么区别？
```

不推荐：

```text
data
manager
helper
util
temp
thing
object
item2
new_system
```

这些词过于宽泛。

更推荐：

```text
save_slot_metadata
inventory_sort_policy
release_build_validator
audio_bus_config
```

---

### 2.2 使用项目语言，而不是实现语言

名称应优先反映业务、玩法和产品概念。

例如：

```text
apply_damage
unlock_recipe
complete_quest
save_slot
```

而不是只描述技术动作：

```text
update_value
process_data
handle_object
do_action
```

---

### 2.3 同一概念只使用一个名称

例如同一概念不要同时出现：

```text
player_profile
user_profile
account_profile
character_profile
```

除非它们确实是不同对象。

应维护统一术语。

---

### 2.4 名称应稳定

不要把短期实现方式放进长期名称。

例如：

```text
json_save_service
```

如果未来存储方式可能变化，更稳定的名称是：

```text
save_storage
```

实现细节可以存在于具体类名中：

```text
JsonSaveStorage
CloudSaveStorage
```

---

### 2.5 避免无意义长度

过短：

```text
x
tmp
mgr
svc
obj
```

过长：

```text
the_service_that_handles_all_inventory_item_sorting_operations
```

名称应在清晰和简洁之间平衡。

---

### 2.6 布尔名称应能自然回答真假

推荐：

```text
is_enabled
has_permission
can_retry
should_save
was_loaded
```

不推荐：

```text
enabled_flag
permission
retry
state
```

---

### 2.7 集合名称使用复数

推荐：

```text
items
players
active_effects
save_slots
```

单个对象使用单数：

```text
item
player
active_effect
save_slot
```

---

### 2.8 不在名称中重复上下文

在 `InventoryService` 中：

```text
add_item()
```

优于：

```text
add_inventory_item()
```

除非需要消除跨模块歧义。

---

## 3. 命名风格总览

推荐默认：

| 对象 | 风格 |
|---|---|
| 目录 | `kebab-case` 或 `snake_case`，项目统一 |
| 通用文件 | `kebab-case` |
| 代码文件 | 语言约定优先 |
| 类 / 类型 | `PascalCase` |
| 函数 / 方法 | `snake_case` 或语言约定 |
| 变量 | `snake_case` 或语言约定 |
| 常量 | `UPPER_SNAKE_CASE` |
| 枚举类型 | `PascalCase` |
| 枚举值 | `UPPER_SNAKE_CASE` 或语言约定 |
| 布尔值 | `is_` / `has_` / `can_` / `should_` |
| Signal / Event | 事件式名称 |
| Git 分支 | `type/kebab-case` |
| Tag / Release | `vMAJOR.MINOR.PATCH` |
| Markdown 文件 | `kebab-case.md` |
| Asset 文件 | `snake_case` |
| Resource ID | `snake_case` 或 namespaced ID |

如果语言或框架已有强约定，应优先遵循其生态惯例。

---

## 4. 目录命名

目录名应：

- 表达内容类别；
- 使用稳定名词；
- 避免模糊词；
- 不使用作者名；
- 不使用版本后缀，除非确实并行维护；
- 不使用 `new`、`old`、`misc`、`temp`。

推荐：

```text
assets/
scenes/
scripts/
resources/
docs/
tests/
tools/
build/
```

不推荐：

```text
stuff/
others/
new/
old/
backup2/
sihao/
```

---

## 5. 目录单复数

目录通常使用复数，表示集合：

```text
scenes/
scripts/
assets/
tests/
resources/
```

表示单一职责时可使用单数：

```text
build/
config/
docs/
```

项目应保持一致，不必机械统一全部为复数。

---

## 6. 文件命名

### 6.1 通用规则

文件名应：

- 使用小写；
- 使用短横线或下划线；
- 避免空格；
- 避免中文文件名；
- 避免大小写混用；
- 避免 `final`；
- 避免日期，除非日期本身有意义；
- 扩展名与内容一致。

---

### 6.2 文档文件

推荐：

```text
feature-lifecycle.md
release-workflow.md
save-format.md
```

---

### 6.3 代码文件

按语言生态。

GDScript 推荐：

```text
player_controller.gd
inventory_service.gd
save_slot_data.gd
```

---

### 6.4 配置文件

推荐：

```text
build-config.json
export-presets.cfg
save-schema.yaml
```

---

## 7. 类与类型命名

类名使用名词或名词短语。

推荐：

```text
PlayerController
InventoryService
SaveSlotData
DamageCalculator
AudioBusConfig
```

不推荐：

```text
HandlePlayer
DoInventory
ProcessSave
ThingManager
```

---

## 8. Controller、Service、Manager 的使用

这些后缀容易被滥用。

### 8.1 Controller

适合：

- 接收输入；
- 协调对象行为；
- 控制流程；
- 连接 View 与 Model。

例如：

```text
PlayerController
MenuController
CameraController
```

---

### 8.2 Service

适合：

- 无界面业务能力；
- 跨对象复用；
- 有明确接口；
- 可替换实现。

例如：

```text
SaveService
AudioService
LocalizationService
```

---

### 8.3 Manager

仅当对象确实管理一组同类对象或生命周期时使用。

例如：

```text
SceneTransitionManager
ConnectionManager
```

避免将任何全局对象都命名为 Manager。

---

## 9. Component 命名

组件应表达单一能力。

推荐：

```text
HealthComponent
DamageReceiver
InventoryHolder
Interactable
MovementComponent
```

避免：

```text
PlayerComponent
GeneralComponent
UtilityComponent
```

---

## 10. Interface / Protocol 命名

按语言习惯。

可使用：

```text
SaveStorage
Damageable
Serializable
Command
Repository
```

不一定需要统一使用 `I` 前缀。

重点是：

- 表达能力；
- 不表达具体实现；
- 保持稳定。

---

## 11. 抽象类命名

可以使用：

```text
BaseCharacter
AbstractRepository
BasePanel
```

但不应为了形式给所有父类加 `Base`。

如果父类本身就是明确概念，可直接命名：

```text
Character
Repository
Panel
```

---

## 12. 具体实现命名

实现类可加入实现特征：

```text
JsonSaveStorage
SqliteSaveStorage
SteamPlatformService
LocalAudioBackend
```

这类名称适合实现可替换的场景。

---

## 13. 函数与方法命名

函数应使用动词或动词短语。

推荐：

```text
load_save()
apply_damage()
calculate_total()
open_panel()
validate_input()
```

不推荐：

```text
save_data()
process()
handle()
do_it()
run()
```

后者不是绝对禁止，但应避免无上下文使用。

---

## 14. 查询函数命名

返回信息但不改变状态：

```text
get_item()
find_player()
calculate_score()
is_available()
has_space()
```

应避免查询函数产生隐藏副作用。

---

## 15. 命令函数命名

改变状态：

```text
add_item()
remove_item()
save_game()
start_battle()
set_volume()
```

名称应明确动作和对象。

---

## 16. 回调函数命名

推荐：

```text
_on_button_pressed()
_on_health_changed()
_on_request_completed()
```

内部框架回调遵循框架命名：

```text
_ready()
_process()
_physics_process()
```

---

## 17. 初始化函数

根据语义区分：

```text
initialize()
setup()
configure()
reset()
load()
```

建议：

- `initialize`：一次性初始化；
- `setup`：装配依赖或参数；
- `configure`：应用配置；
- `reset`：恢复初始状态；
- `load`：从外部读取。

不要混用。

---

## 18. 创建函数

区分：

```text
create
build
make
spawn
instantiate
```

建议：

- `create`：创建业务对象；
- `build`：按步骤组装复杂对象；
- `spawn`：在运行环境中生成实体；
- `instantiate`：从模板实例化；
- `make`：不建议作为正式项目默认词。

---

## 19. 删除函数

区分：

```text
remove
delete
destroy
clear
dispose
release
```

建议：

- `remove`：从集合移除；
- `delete`：删除持久化记录；
- `destroy`：销毁运行时对象；
- `clear`：清空集合；
- `dispose`：释放对象持有资源；
- `release`：释放所有权或资源。

---

## 20. 变量命名

变量使用名词。

推荐：

```text
player_speed
save_slot
active_effects
retry_count
```

不推荐：

```text
thing
value
obj
data
result2
```

只有在上下文非常明确时，`value`、`data`、`result` 才可接受。

---

## 21. 局部变量

局部变量可以较短，但仍应清楚。

推荐：

```text
index
item
result
position
elapsed
```

非常短的名称只适合：

- 数学；
- 循环索引；
- 坐标；
- 标准约定。

例如：

```text
i
j
x
y
t
```

---

## 22. 私有变量

按语言约定。

GDScript 可使用：

```text
_state
_current_target
_cached_items
```

是否使用下划线前缀应项目统一。

---

## 23. 缓存变量

推荐明确表达缓存：

```text
cached_profile
_cached_nodes
texture_cache
```

避免缓存和事实源使用同一名称。

---

## 24. 临时变量

临时变量也应表达用途。

推荐：

```text
previous_state
pending_request
temporary_path
```

不推荐：

```text
tmp
temp2
aaa
```

---

## 25. 常量命名

使用：

```text
UPPER_SNAKE_CASE
```

例如：

```text
MAX_RETRY_COUNT
DEFAULT_VOLUME
SAVE_VERSION
```

常量名称应表达意义，而不是数值。

不推荐：

```text
THREE
VALUE_10
NUMBER
```

---

## 26. Magic Number

不要直接写：

```text
if retries > 3:
```

更推荐：

```text
const MAX_RETRY_COUNT := 3
```

前提是该数字具有业务含义。

---

## 27. 布尔命名

推荐前缀：

```text
is_
has_
can_
should_
was_
needs_
supports_
```

例如：

```text
is_visible
has_save_data
can_attack
should_retry
was_loaded
needs_migration
supports_gamepad
```

避免双重否定：

```text
is_not_disabled
```

更好：

```text
is_enabled
```

---

## 28. 状态变量

推荐：

```text
current_state
previous_state
next_state
```

不要同时使用：

```text
state
mode
status
phase
```

来指同一概念。

应为不同语义明确区分：

- State：运行状态；
- Status：外部或流程状态；
- Mode：运行模式；
- Phase：阶段。

---

## 29. 集合命名

数组、列表、集合使用复数：

```text
items
players
pending_requests
```

字典可使用：

```text
items_by_id
player_by_peer_id
config_by_name
```

集合名称应表达索引方式。

---

## 30. 数量命名

推荐：

```text
item_count
remaining_attempts
max_players
total_score
```

区分：

- `count`：数量；
- `index`：位置；
- `size`：容量或尺寸；
- `length`：长度；
- `total`：汇总值；
- `max`：上限。

---

## 31. 时间命名

应带单位：

```text
timeout_seconds
elapsed_ms
cooldown_seconds
created_at
updated_at
```

不推荐：

```text
timeout
delay
time
```

除非类型和上下文已经完全明确。

---

## 32. 距离与尺寸命名

带单位或语义：

```text
distance_meters
width_px
radius_units
speed_pixels_per_second
```

项目中应统一单位系统。

---

## 33. ID 命名

推荐：

```text
player_id
item_id
save_slot_id
request_id
```

如果存在不同 ID 类型，应明确：

```text
database_id
external_id
session_id
resource_id
```

---

## 34. 枚举命名

枚举类型：

```text
BattleState
SaveStatus
InputDeviceType
```

枚举值：

```text
IDLE
RUNNING
PAUSED
COMPLETED
```

或按语言约定：

```text
Idle
Running
Paused
Completed
```

同一项目应统一。

---

## 35. Event 命名

事件表示已经发生的事实。

推荐使用过去式：

```text
ItemAdded
HealthChanged
BattleStarted
SaveCompleted
```

或框架习惯的小写形式：

```text
item_added
health_changed
battle_started
save_completed
```

---

## 36. Command 命名

命令表示请求执行动作。

推荐：

```text
AddItem
StartBattle
SaveGame
OpenPanel
```

区分：

```text
StartBattle = 命令
BattleStarted = 事件
```

---

## 37. Signal 命名

Signal 应表达事件，而不是命令。

推荐：

```text
health_changed
item_added
request_failed
animation_finished
```

不推荐：

```text
change_health
add_item
do_save
```

---

## 38. Error 命名

错误类型：

```text
SaveCorruptionError
AuthenticationError
InvalidConfigError
```

错误码：

```text
SAVE_FILE_MISSING
INVALID_TOKEN
PERMISSION_DENIED
```

错误信息不应使用模糊编号：

```text
ERROR_1
ERROR_2
```

---

## 39. Result 命名

如果函数返回结果对象：

```text
SaveResult
ValidationResult
BuildResult
```

内部变量可以：

```text
save_result
validation_result
```

结果应包含明确状态，而不是单个模糊布尔值。

---

## 40. DTO、Model、Entity、Resource

这些词应有稳定含义。

建议：

- DTO：跨边界传输结构；
- Model：领域或界面模型；
- Entity：有身份和生命周期的对象；
- Resource：可复用配置或资产数据；
- Record：持久化记录；
- Config：配置；
- Settings：用户或系统设置。

不要随意混用。

---

## 41. Repository 命名

推荐：

```text
PlayerRepository
SaveRepository
ItemRepository
```

具体实现：

```text
SqliteSaveRepository
InMemorySaveRepository
```

---

## 42. Factory 命名

当对象创建逻辑复杂时使用：

```text
EnemyFactory
PanelFactory
SaveDataFactory
```

不要为单一构造函数创建无意义 Factory。

---

## 43. Builder 命名

适合分步骤组装复杂对象：

```text
LevelBuilder
RequestBuilder
CharacterBuildBuilder
```

避免与业务中的“建筑建造者”概念冲突。

---

## 44. Adapter 命名

用于连接不兼容接口：

```text
SteamInputAdapter
LegacySaveAdapter
PlatformAudioAdapter
```

---

## 45. Validator 命名

用于验证：

```text
SaveDataValidator
BuildConfigValidator
InputValidator
```

验证函数：

```text
validate_save_data()
is_valid_name()
```

---

## 46. Converter / Mapper / Serializer

建议区分：

- Converter：格式转换；
- Mapper：结构映射；
- Serializer：序列化；
- Parser：解析；
- Formatter：格式化；
- Encoder / Decoder：编码解码。

例如：

```text
SaveDataSerializer
ApiItemMapper
CsvConfigParser
DurationFormatter
```

---

# 47. Godot Scene 命名

Scene 文件使用：

```text
snake_case.tscn
```

例如：

```text
player_character.tscn
inventory_panel.tscn
forest_level.tscn
```

Scene 根节点类名或名称使用：

```text
PascalCase
```

例如：

```text
PlayerCharacter
InventoryPanel
ForestLevel
```

---

## 48. Godot Node 命名

Node 名称应表达角色。

推荐：

```text
CharacterSprite
CollisionShape
HealthBar
InteractionArea
AnimationPlayer
```

不推荐：

```text
Node2D
Control2
Sprite3
Thing
```

---

## 49. Node 名称与类型

名称不必重复完整类型。

在明确上下文中：

```text
Icon
Label
Button
Panel
```

通常比：

```text
IconTextureRect
TitleLabel
ConfirmButton
BackgroundPanel
```

更清楚。

但需要唯一引用时，可以更具体。

---

## 50. Unique Node Name

需要通过 `%Name` 引用时，名称应：

- 唯一；
- 稳定；
- 表达职责；
- 避免视觉位置词。

推荐：

```text
ConfirmButton
ItemList
ErrorLabel
```

不推荐：

```text
Button3
LeftLabel
TopPanel2
```

---

## 51. Godot Script 命名

Script 文件：

```text
snake_case.gd
```

类名：

```text
PascalCase
```

示例：

```text
inventory_panel.gd
class_name InventoryPanel
```

---

## 52. Godot Resource 命名

Resource 类：

```text
ItemData
EnemyConfig
SkillDefinition
```

Resource 文件：

```text
iron_sword.tres
goblin_warrior.tres
fireball_skill.tres
```

---

## 53. Autoload 命名

Autoload 名称应表达服务职责。

推荐：

```text
SaveService
AudioService
SceneRouter
PlatformService
```

不推荐：

```text
Global
Globals
Manager
Game
Main
```

---

## 54. Input Action 命名

使用行为名称，而不是物理按键。

推荐：

```text
move_left
move_right
jump
confirm
cancel
pause
open_inventory
```

不推荐：

```text
press_a
space_key
mouse_left
```

---

## 55. Collision Layer 命名

使用对象类别：

```text
world
player
enemy
projectile
interactable
trigger
```

避免：

```text
layer_1
collision2
misc
```

---

## 56. Group 命名

Group 表示能力或分类。

推荐：

```text
damageable
interactable
saveable
pause_aware
enemies
players
```

统一选择单数能力或复数集合。

---

## 57. Animation 命名

推荐：

```text
idle
walk
run
attack_light
attack_heavy
hurt
death
```

带方向：

```text
walk_left
walk_right
attack_up
```

带武器或状态：

```text
sword_attack_light
bow_aim
stunned_idle
```

---

## 58. Animation State 命名

状态机名称应与逻辑状态一致：

```text
Idle
Move
Attack
Hurt
Dead
```

不要出现同一状态多种名称：

```text
Walking
Move
RunState
Locomotion
```

除非语义确实不同。

---

## 59. Shader 命名

推荐：

```text
outline_2d.gdshader
dissolve_2d.gdshader
water_surface.gdshader
```

Shader 参数：

```text
outline_width
dissolve_amount
edge_color
```

---

# 60. UI 命名

UI 组件名称应表达功能，而不是位置。

推荐：

```text
SettingsPanel
InventoryList
ConfirmDialog
SearchField
```

不推荐：

```text
LeftPanel
TopBox
Container2
```

位置可以变化，职责更稳定。

---

## 61. UI 状态命名

推荐：

```text
default
hover
pressed
focused
disabled
loading
empty
error
success
```

不要使用：

```text
normal2
active_new
selected_final
```

---

## 62. UI Token 命名

设计 Token 可以使用语义化命名：

```text
color_surface_primary
color_text_muted
spacing_md
radius_lg
shadow_panel
```

优于：

```text
gray_2
orange_3
padding_12
```

基础色板可以保留数值层级，但组件应使用语义 Token。

---

# 63. Asset 命名

推荐结构：

```text
<category>_<subject>_<variant>_<state>_<size>_<version>
```

示例：

```text
ui_icon_warning_default_24_v003.png
char_knight_idle_right_v012.png
bg_forest_day_wide_v005.png
sfx_ui_confirm_short_v004.wav
```

不是每个字段都必须出现，但顺序应稳定。

---

## 64. Asset Category

可使用：

```text
ui
char
enemy
npc
item
weapon
bg
fx
sfx
music
voice
font
video
```

项目应建立自己的固定词表。

---

## 65. Asset Subject

Subject 应使用稳定对象名称：

```text
knight
forest
warning
health_potion
```

不要同一对象同时出现：

```text
potion
health_pot
hp_bottle
healing_item
```

---

## 66. Asset Variant

例如：

```text
red
rare
winter
damaged
large
```

---

## 67. Asset State

例如：

```text
default
hover
pressed
disabled
idle
attack
hurt
destroyed
```

---

## 68. Asset Direction

统一使用：

```text
left
right
up
down
front
back
```

或：

```text
n
s
e
w
```

不要混用两套方向命名。

---

## 69. Asset Size

可使用：

```text
16
24
32
64
small
medium
large
```

建议数字表示精确像素，语义词表示设计规格。

---

## 70. Asset Version

推荐：

```text
v001
v002
v003
```

避免：

```text
final
final2
final_new
final_final
```

---

## 71. Sprite Sheet 命名

推荐：

```text
char_knight_walk_right_8f_v004.png
fx_slash_horizontal_6f_v002.png
```

其中：

```text
8f = 8 frames
6f = 6 frames
```

---

## 72. 音频命名

推荐：

```text
sfx_ui_confirm_short_v003.wav
sfx_sword_hit_heavy_02.wav
music_forest_day_loop_v005.ogg
voice_merchant_greeting_01.wav
```

---

## 73. 音频变体

同类随机变体：

```text
sfx_footstep_grass_01.wav
sfx_footstep_grass_02.wav
sfx_footstep_grass_03.wav
```

版本号和随机变体编号应区分。

例如：

```text
sfx_footstep_grass_01_v003.wav
```

---

# 74. 数据 ID 命名

数据 ID 应：

- 稳定；
- 唯一；
- 不依赖显示名称；
- 不使用本地化文本；
- 不随重命名轻易改变；
- 可用于存档和引用。

推荐：

```text
item.health_potion
enemy.goblin_warrior
skill.fireball
quest.main_001
```

或：

```text
health_potion
goblin_warrior
fireball
main_quest_001
```

---

## 75. Namespaced ID

大型项目推荐：

```text
domain.category.name
```

例如：

```text
core.item.health_potion
combat.skill.fireball
world.enemy.goblin_warrior
```

---

## 76. 显示名称与内部 ID

应分离：

```text
id: health_potion
display_name_key: item.health_potion.name
```

不要使用显示文本作为内部 ID。

---

# 77. 配置字段命名

配置字段应：

- 表达单位；
- 表达范围；
- 表达是否可选；
- 与代码一致。

推荐：

```text
timeout_seconds
max_retry_count
default_language
enable_cloud_save
```

---

## 78. 环境变量命名

使用：

```text
UPPER_SNAKE_CASE
```

例如：

```text
API_BASE_URL
DATABASE_URL
LOG_LEVEL
ENABLE_ANALYTICS
```

环境变量应带命名空间，避免冲突：

```text
GAME_API_BASE_URL
GAME_SAVE_BUCKET
```

---

## 79. Feature Flag 命名

推荐：

```text
enable_cloud_save
enable_new_inventory_ui
rollout_advanced_search
```

或集中前缀：

```text
ff_cloud_save
ff_inventory_v2
```

每个 Flag 应有 Owner 和清理计划。

---

## 80. API Endpoint 命名

推荐资源式路径：

```text
/users
/save-slots
/inventory-items
```

避免动词式：

```text
/getUsers
/createSave
/doDelete
```

动作无法自然建模时可使用：

```text
/orders/{id}/cancel
```

---

## 81. API 字段命名

应统一：

```text
snake_case
```

或：

```text
camelCase
```

项目不可混用。

应明确日期和单位：

```text
created_at
timeout_seconds
file_size_bytes
```

---

## 82. 数据库表命名

推荐复数或单数，项目统一。

例如复数：

```text
users
save_slots
inventory_items
```

字段：

```text
id
user_id
created_at
updated_at
```

---

## 83. 数据库索引命名

推荐：

```text
idx_<table>_<columns>
```

例如：

```text
idx_save_slots_user_id
idx_items_category_rarity
```

唯一约束：

```text
uq_<table>_<columns>
```

外键：

```text
fk_<table>_<referenced_table>
```

---

## 84. Migration 命名

推荐：

```text
20260710_add_save_slot_metadata.sql
```

或序号：

```text
0042_add_save_slot_metadata.sql
```

必须：

- 唯一；
- 有顺序；
- 描述变化；
- 不使用 `fix2`。

---

# 85. 测试命名

测试名称应表达：

```text
given
when
then
```

例如：

```text
test_save_fails_when_slot_is_read_only
test_damage_is_clamped_at_zero_health
test_migration_preserves_existing_metadata
```

---

## 86. 测试夹具命名

推荐：

```text
valid_save_data
corrupted_save_data
empty_inventory
player_with_full_health
```

不要使用：

```text
data1
test_obj
dummy2
```

---

## 87. Mock、Fake、Stub

应区分：

- Stub：返回预设结果；
- Fake：简化但可运行实现；
- Mock：验证交互；
- Spy：记录调用。

命名示例：

```text
FakeSaveStorage
StubPlatformService
MockAnalyticsClient
```

---

# 88. Git 分支命名

格式：

```text
<type>/<kebab-case-topic>
```

例如：

```text
feature/save-slots
bugfix/login-timeout
hotfix/2-4-1-save-corruption
docs/release-workflow
upgrade/godot-4-5
```

---

## 89. Commit Scope 命名

Scope 应对应稳定模块：

```text
save
inventory
ui
audio
build
docs
release
```

不要使用：

```text
misc
general
stuff
```

---

## 90. Tag 和 Release 命名

推荐：

```text
v2.4.0
v2.4.1
v2.5.0-beta.1
v2.5.0-rc.2
```

内部 Build：

```text
build-1842
```

---

## 91. Milestone 命名

Milestone 应表达阶段结果，而不是时间。

推荐：

```text
Core Loop Complete
Save and Recovery Ready
Public Beta Readiness
Multi-platform Export Ready
```

不推荐：

```text
Phase 2
July Work
Sprint 14
Next Version Stuff
```

可以带编号，但名称仍应表达结果。

---

## 92. Release 名称

可以使用：

```text
2.4.0
Public Beta 1
Summer Content Update
```

内部仍应保留正式版本号。

---

# 93. 文档命名

Markdown 文件：

```text
kebab-case.md
```

推荐：

```text
feature-lifecycle.md
decision-records.md
save-format.md
```

---

## 94. ADR 命名

推荐：

```text
adr-0001-use-godot.md
adr-0002-save-format-versioning.md
```

或：

```text
0001-use-godot.md
0002-save-format-versioning.md
```

---

## 95. 模板命名

推荐：

```text
feature-template.md
bug-report-template.md
release-plan-template.md
```

---

## 96. 会议和复盘文档

日期有意义时可以使用：

```text
2026-07-10-release-retro.md
2026-07-10-incident-review.md
```

---

# 97. 缩写规范

缩写只应在以下情况下使用：

- 行业通用；
- 项目内高频；
- 不会产生歧义；
- 已在 Glossary 定义。

常见可接受：

```text
API
UI
UX
ID
URL
HTTP
JSON
CPU
GPU
FPS
```

谨慎使用：

```text
mgr
svc
cfg
ctx
util
repo
```

项目应统一是否接受。

---

## 98. 首字母缩写大小写

类名中推荐按单词处理：

```text
ApiClient
HttpRequest
JsonParser
UiController
```

而不是：

```text
APIClient
HTTPRequest
JSONParser
UIController
```

但应服从语言和项目惯例。

---

## 99. 禁用词

建议避免：

```text
data
info
thing
object
stuff
misc
general
common
helper
util
manager
temp
new
old
final
test2
v2
```

这些词并非永远禁止，但使用时必须有明确上下文。

---

# 100. `Common` 和 `Utils`

`common`、`utils` 容易变成无边界目录。

应优先按职责拆分：

```text
formatting/
validation/
serialization/
math/
platform/
```

而不是：

```text
utils/
common/
helpers/
```

---

# 101. `New`、`Old`、`Legacy`

`new` 和 `old` 会随时间失效。

应使用：

```text
inventory_v2
legacy_save_reader
modern_renderer
```

但版本后缀也应有淘汰计划。

---

# 102. `Temp` 和 `Prototype`

临时内容应：

- 放在明确目录；
- 有 Owner；
- 有清理日期；
- 不进入正式发布；
- 不被长期引用。

例如：

```text
prototype/
experiments/
```

---

# 103. 数字后缀

避免：

```text
button2
panel3
manager4
```

数字只应表达真实序号或版本：

```text
chapter_02
phase_03
player_2
v2
```

---

# 104. 方位命名

不要使用易失效位置名称：

```text
left_panel
top_button
```

更推荐职责：

```text
navigation_panel
primary_action_button
```

只有位置本身是稳定概念时使用：

```text
left_hand
top_bar
```

---

# 105. 颜色命名

基础色板可使用：

```text
gray_100
gray_200
orange_500
```

业务组件应使用语义：

```text
text_primary
surface_muted
status_error
action_primary
```

避免直接将业务含义绑定具体颜色：

```text
red_button
green_text
```

---

# 106. 命名冲突

发现名称冲突时，优先通过上下文区分：

```text
SaveConfig
SaveData
SaveMetadata
SaveResult
```

不要使用无意义后缀：

```text
SaveData2
SaveDataNew
SaveDataObj
```

---

# 107. 重命名流程

重命名可能影响：

- 代码；
- 文件路径；
- Scene 引用；
- Resource；
- 存档；
- 数据库；
- API；
- 文档；
- 构建脚本；
- 外部链接。

---

## 107.1 重命名前

```markdown
- [ ] 确认新名称更准确；
- [ ] 搜索全部引用；
- [ ] 检查持久化 ID；
- [ ] 检查 API 兼容；
- [ ] 检查存档兼容；
- [ ] 检查外部依赖；
- [ ] 评估迁移成本；
```

---

## 107.2 重命名后

```markdown
- [ ] 全仓库引用已更新；
- [ ] 构建通过；
- [ ] 测试通过；
- [ ] 数据迁移完成；
- [ ] 文档更新；
- [ ] 旧名称有兼容策略；
- [ ] 无残留别名；
```

---

# 108. 持久化名称

以下名称一旦进入正式数据，应慎重修改：

- 数据库字段；
- API 字段；
- 存档 Key；
- Resource ID；
- Analytics Event；
- Feature Flag；
- 配置字段；
- 文件路径；
- 本地化 Key。

这些名称属于外部契约。

重命名需要：

- Migration；
- Alias；
- Backward Compatibility；
- Versioning；
- Deprecation。

---

# 109. Analytics Event 命名

推荐：

```text
feature_object_action
```

例如：

```text
inventory_item_added
save_slot_created
battle_completed
```

事件属性：

```text
item_id
source
duration_seconds
result
```

不要包含敏感个人信息。

---

# 110. Log Event 命名

结构化日志事件：

```text
save_load_failed
migration_completed
asset_import_failed
```

字段保持稳定：

```text
request_id
user_id
error_code
duration_ms
```

---

# 111. 本地化 Key 命名

推荐：

```text
screen.component.element
```

例如：

```text
settings.audio.master_volume
inventory.empty.title
dialog.delete.confirm
```

不要使用显示文本本身作为 Key。

---

# 112. CSS / UI Class 命名

如果适用，可以使用：

- BEM；
- Utility-first；
- Component-scoped；
- CSS Modules。

应统一一种主策略。

BEM 示例：

```text
inventory-panel
inventory-panel__item
inventory-panel__item--selected
```

---

# 113. 文件扩展名和大小写

扩展名统一小写：

```text
.png
.wav
.md
.json
```

避免：

```text
.PNG
.Wav
.MD
```

跨平台文件系统大小写差异可能造成构建问题。

---

# 114. Glossary

项目应维护术语表。

模板：

```markdown
| Term | Definition | Avoid |
|---|---|---|
| Save Slot | 单个独立存档位置 | Save File、Profile |
| Player | 实际操作者对应实体 | User、Hero |
```

Glossary 用于：

- 统一产品概念；
- 统一代码命名；
- 统一文档；
- 统一 UI 文案；
- 统一数据字段。

---

# 115. 命名 Review

评审名称时应问：

```text
读者能否理解？
是否表达职责？
是否与项目术语一致？
是否会快速过时？
是否隐藏副作用？
是否与其他名称冲突？
是否需要单位？
是否需要单复数？
是否属于外部契约？
```

---

# 116. 命名 Checklist

```markdown
## 通用

- [ ] 名称表达意图；
- [ ] 使用项目术语；
- [ ] 无歧义；
- [ ] 无无意义缩写；
- [ ] 无过时实现细节；
- [ ] 单复数正确；
- [ ] 大小写风格正确。

## 类与函数

- [ ] 类名为名词；
- [ ] 函数名为动词；
- [ ] 布尔值可自然判断；
- [ ] 查询与命令清楚；
- [ ] 副作用可从名称理解。

## 文件与目录

- [ ] 路径职责清楚；
- [ ] 文件名可搜索；
- [ ] 无空格；
- [ ] 无 final；
- [ ] 无 new / old；
- [ ] 扩展名小写。

## 数据

- [ ] ID 稳定；
- [ ] 显示名与内部 ID 分离；
- [ ] 单位明确；
- [ ] 时间字段明确；
- [ ] 外部契约已考虑兼容。

## Asset

- [ ] Category 明确；
- [ ] Subject 明确；
- [ ] Variant 明确；
- [ ] State 明确；
- [ ] Version 明确；
- [ ] 无 final-final。

## Git

- [ ] 分支类型正确；
- [ ] Topic 清楚；
- [ ] Tag 符合版本规范；
- [ ] Commit Scope 稳定。

## 文档

- [ ] 文件名表达主题；
- [ ] 标题清楚；
- [ ] ADR 编号唯一；
- [ ] 模板名称清楚。
```

---

# 117. 常见失败模式

## 117.1 所有东西都叫 Manager

结果：

- 职责不清；
- 对象边界模糊；
- 容易形成 God Object。

---

## 117.2 使用 `data` 表示不同概念

例如：

```text
player_data
save_data
request_data
temp_data
```

应进一步明确：

```text
player_profile
save_payload
request_body
migration_buffer
```

---

## 117.3 布尔变量没有前缀

例如：

```text
active
permission
ready
```

容易与对象或状态混淆。

---

## 117.4 文件使用 `final`

结果：

```text
final
final2
final-new
final-final
```

无法确定正式版本。

---

## 117.5 名称包含视觉位置

UI 调整后名称失效：

```text
left_button
bottom_panel
```

---

## 117.6 内部 ID 使用显示文本

结果：

- 本地化后引用失效；
- 改文案导致存档损坏；
- 空格和特殊字符问题。

---

## 117.7 缩写只有作者能理解

结果：

- 新成员理解困难；
- 搜索困难；
- 同一缩写多义。

---

## 117.8 单位不明确

例如：

```text
timeout = 5000
```

无法判断是秒、毫秒还是帧。

---

## 117.9 同一概念多套名称

例如：

```text
quest
mission
task
objective
```

如果它们不是不同层级，应统一。

---

## 117.10 无意义数字后缀

```text
service2
panel3
new_system4
```

说明名称或结构尚未整理。

---

## 117.11 重命名只改显示部分

结果：

- 内部引用残留；
- 数据迁移缺失；
- 文档冲突；
- 旧 API 失效。

---

# 118. 命名规范模板

```markdown
# Project Naming Policy

> Project：
> Owner：
> Last Updated：

## General Case Rules

## File and Folder Rules

## Code Rules

## Godot Rules

## Data ID Rules

## Asset Rules

## Git Rules

## Document Rules

## Approved Abbreviations

| Abbreviation | Meaning |
|---|---|

## Glossary

| Term | Definition | Avoid |
|---|---|---|

## Deprecated Names

| Old Name | New Name | Migration |
|---|---|---|
```

---

# 119. 最终原则

Naming Conventions 的本质不是：

> 强制所有名称满足形式上的统一。

它的本质是：

> 通过稳定、明确、可搜索的名称，把项目中的概念、职责、行为和边界表达出来。

一个健康的命名体系应做到：

- 以意图为中心；
- 使用统一项目语言；
- 同一概念一个名称；
- 类名表达对象；
- 函数名表达动作；
- 布尔名表达判断；
- 集合使用复数；
- 单位明确；
- ID 稳定；
- 文件可搜索；
- 资产可追踪；
- 分支和版本可识别；
- 外部契约谨慎重命名；
- 避免无意义缩写和数字后缀。
