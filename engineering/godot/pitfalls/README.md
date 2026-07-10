# Godot 开发避坑清单

按主题分 7 类，收录 Godot 4.x / GDScript 开发中容易踩的坑与规避做法。
"编辑器里正常、导出后坏掉"、"帧率一变就出 Bug"这类问题多能在此对号入座。

## 索引

- [场景树、节点生命周期与对象管理](scene-tree-and-node-lifecycle.md) — 节点何时存在 / 可访问 / 真正销毁，场景如何安全复用
- [游戏循环、输入与物理系统](game-loop-input-and-physics.md) — 帧率相关 Bug、输入穿透、物理抖动、暂停失效
- [架构、组件化与解耦设计](architecture-and-decoupling.md) — Manager 泛滥、场景硬引用、Signal 失控、过深继承
- [性能、资源加载与多线程](performance-and-concurrency.md) — 每帧搜索、同步加载、频繁分配、不安全线程；先测量再优化
- [文件系统、存档、导出与发布](filesystem-export-and-release.md) — "编辑器正常、导出后坏掉"的根因
- [项目版本、Git 与 Resource 管理](project-versioning-and-resources.md) — 版本控制与 Resource 的协作陷阱
- [2D 横版：UI、动画、碰撞与分辨率](2d-side-scroller-ui-animation-and-collision.md) — 横版 2D / 贴纸角色 / 桌面独立游戏专项

## 约定

- 一类一个文件，kebab-case slug。
- 与架构类文档相关时用相对链接引用
  [SOLID](../../architecture/solid-in-godot.md) /
  [模块边界](../../architecture/module-boundaries.md)。
