# Godot 文件系统、存档、导出与发布陷阱

> 核心目标：解决“编辑器里正常，导出后坏掉”的问题。

---

## 1. `res://` 与 `user://`

### `res://`

项目资源目录。

适合读取：

- 场景；
- Resource；
- 脚本；
- 图片；
- 音频；
- 内置配置。

导出后通常不应把它当成可写目录。

### `user://`

用户数据目录。

适合写入：

- 存档；
- 设置；
- 日志；
- 缓存；
- 下载内容；
- 用户生成数据。

示例：

```gdscript
const SAVE_PATH := "user://savegame.json"

func save_game(data: Dictionary) -> void:
    var file := FileAccess.open(SAVE_PATH, FileAccess.WRITE)

    if file == null:
        push_error("Failed to open save file.")
        return

    file.store_string(JSON.stringify(data))
```

---

## 2. 文件路径大小写必须完全一致

Windows 文件系统常常对大小写不敏感，而 Linux、PCK 或其他平台可能敏感。

代码：

```gdscript
load("res://Characters/Hero.tscn")
```

实际文件：

```text
res://characters/hero.tscn
```

本地可能工作，导出后失败。

建议：

- 文件和目录统一小写；
- 使用 `snake_case`；
- 不使用空格；
- 不使用容易混淆的大小写命名；
- 重命名后测试真实导出包。

---

## 3. 尽量在 Godot FileSystem Dock 中移动文件

直接在系统文件管理器中移动或重命名资源，可能造成引用路径断裂。

优先在编辑器中完成：

- Rename；
- Move；
- Delete。

移动后应检查：

- 场景引用；
- Resource 引用；
- preload 路径；
- JSON 中手写路径；
- Shader include；
- 插件配置；
- 导出过滤规则。

Git 中仅看到文件改名，不代表 Godot 中所有引用都正确更新。

---

## 4. 非资源文件可能未被导出

如果项目运行时读取：

- `.json`；
- `.csv`；
- `.txt`；
- 自定义扩展名；
- 本地化数据；
- 模板文件；

需要确认它们被加入导出包。

建议在 Export Preset 中配置：

- 资源包含过滤；
- 非资源文件包含规则；
- 排除开发文件；
- 测试导出包中能否访问。

不要只在编辑器中读取成功后就认为发布版一定成功。

---

## 5. 存档格式必须支持版本迁移

不要只保存当前字段：

```json
{
  "level": 10,
  "gold": 1000
}
```

推荐：

```json
{
  "save_version": 3,
  "game_version": "0.4.0",
  "profile": {
    "level": 10,
    "gold": 1000
  }
}
```

加载时：

```gdscript
func migrate_save(data: Dictionary) -> Dictionary:
    var version: int = data.get("save_version", 1)

    if version < 2:
        data = migrate_v1_to_v2(data)
        version = 2

    if version < 3:
        data = migrate_v2_to_v3(data)

    return data
```

---

## 6. 不要直接序列化节点和 Resource 引用

JSON 不能可靠保存：

- Node；
- Callable；
- Signal；
- Texture2D；
- PackedScene；
- 任意 Resource 引用；
- 循环引用。

存档应保存稳定 ID：

```json
{
  "equipped_weapon_id": "iron_sword",
  "active_skill_ids": ["slash", "guard"]
}
```

加载后通过数据库恢复资源：

```gdscript
var weapon_data := item_database.get_item(save_data.equipped_weapon_id)
```

---

## 7. 原子化保存，避免存档损坏

如果程序在写文件过程中崩溃，主存档可能损坏。

更安全的流程：

1. 写入临时文件；
2. 验证内容；
3. 保留旧备份；
4. 用临时文件替换主文件。

示意：

```text
savegame.tmp
savegame.json
savegame.backup.json
```

加载顺序：

1. 主存档；
2. 主存档失败则尝试备份；
3. 两者失败则创建新档；
4. 向用户说明发生了什么。

---

## 8. 导出凭据和密钥不能提交

包括：

- Android keystore 密码；
- Apple 签名证书；
- 商店上传密钥；
- 加密密钥；
- 后端 Secret；
- API 私钥。

客户端游戏中也不应内置真正机密的服务密钥，因为发布后可被提取。

公开客户端最多保存：

- 公共 API URL；
- 客户端 ID；
- 可公开的配置；
- 临时 Token。

敏感逻辑应放在服务器。

---

## 9. 插件可能只在编辑器中有效

EditorPlugin 可能只负责：

- 自定义 Inspector；
- 导入工具；
- 场景生成；
- 编辑器面板；
- 开发辅助。

它不一定会随游戏运行。

使用插件前确认：

- 是否包含运行时代码；
- 导出后是否启用；
- 是否支持目标平台；
- 是否依赖 Native Library；
- 是否需要额外导出设置；
- 是否兼容当前 Godot 版本。

---

## 10. 导出模板必须与引擎版本匹配

Godot 编辑器和 Export Template 版本不一致，可能导致：

- 无法导出；
- 导出包异常；
- 平台模块缺失；
- 调试信息不匹配。

升级引擎后，检查导出模板也已同步更新。

---

## 11. Debug 与 Release 行为可能不同

差异可能来自：

- 断言；
- 调试日志；
- 优化；
- 控制台窗口；
- 导出过滤；
- 远程调试；
- 权限；
- 文件路径；
- 插件；
- 原生库。

至少测试：

- Debug Export；
- Release Export；
- 全新用户目录；
- 离线运行；
- 无编辑器环境运行；
- 安装后首次启动；
- 多次启动和覆盖安装。

---

## 12. Windows、macOS 和 Linux 的平台差异

### Windows

- 大小写问题容易被隐藏；
- 杀毒软件可能拦截未知可执行文件；
- 路径和 DLL 依赖；
- 控制台和窗口模式差异。

### macOS

- 应用签名；
- Notarization；
- 沙盒和权限；
- Apple Silicon 与 Intel；
- 文件访问权限。

### Linux

- 文件名大小写严格；
- 动态库依赖；
- 不同发行版兼容；
- 显示服务器和驱动差异。

不要假设在 Windows 导出正常，其他平台就一定正常。

---

## 13. Steam 发布额外注意

- Steamworks 插件版本；
- App ID 配置；
- Overlay；
- 成就和云存档；
- 不同 Depot；
- Windows/Linux/macOS 包；
- 启动参数；
- 存档目录；
- Steam Deck 输入与分辨率；
- 无 Steam 客户端时的降级行为。

Steam 云存档应避免和本地原子保存策略冲突。

---

## 14. 日志与错误报告

发布版不要完全关闭所有错误信息。

建议记录：

- 游戏版本；
- 平台；
- 场景；
- 存档版本；
- 异常信息；
- 最近关键操作；
- 图形后端；
- 插件版本。

日志写入：

```text
user://logs/
```

同时注意：

- 不记录密码；
- 不记录 Token；
- 不记录过度个人数据；
- 控制日志大小；
- 定期轮换。

---

## 15. 发布前导出测试清单

- [ ] 所有路径大小写一致；
- [ ] 存档写入 `user://`；
- [ ] JSON/CSV 等文件已包含在导出包；
- [ ] 存档包含 `save_version`；
- [ ] 存档支持迁移；
- [ ] 存档写入具有临时文件和备份；
- [ ] 未提交任何导出凭据；
- [ ] Export Template 与引擎版本一致；
- [ ] 插件已验证运行时和平台兼容；
- [ ] Debug 和 Release 都测试过；
- [ ] 使用全新用户目录测试过；
- [ ] 各目标平台分别测试；
- [ ] 发布版拥有可用日志；
- [ ] Steam Deck 或目标手柄环境已测试。
