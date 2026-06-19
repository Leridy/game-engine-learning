# W4: 编辑器 + 系统设计 + 竞品对标

## 学习目标

1. 理解游戏编辑器的核心架构
2. 理解脚本系统设计（DSL、Lua、GameAPI）
3. 理解资源管线和游戏子系统
4. 输出自己的引擎架构设计文档

## 每日笔记

- [Day 1: 游戏编辑器架构](./day-1.md)
- [Day 2: 脚本系统设计](./day-2.md)
- [Day 3: 资源管线](./day-3.md)
- [Day 4: 游戏子系统](./day-4.md)
- [Day 5: 架构设计文档](./day-5.md)
- [周末: 竞品编辑器对标](./weekend.md)

## 知识自检

- [ ] 阻塞式脚本如何在单线程游戏循环中实现？
- [ ] GameAPI 如何同时服务 DSL 和 Lua 两种运行时？
- [ ] 二进制格式解析的基本结构是什么？
- [ ] 编辑器和引擎的几种关系模式？

## miu2d 关键文件

| 文件 | 关注点 |
|---|---|
| `script/executor.ts` | DSL 执行器 |
| `script/parser.ts` | 脚本解析器 |
| `script/commands/` | 9 组命令 |
| `script/api/game-api.ts` | 170+ GameAPI |
| `script/lua/lua-executor.ts` | Lua 5.4 运行时 |
| `dashboard/src/` | 13 个编辑模块 |
| `resource/` | 8 种格式解码器 |
| `audio/` | Web Audio |
| `weather/` | 天气/粒子 |
| `gui/` | HUD/对话框 |

## 交付物

- [ ] 竞品编辑器对标笔记
- [ ] 自己的引擎架构设计文档 (`docs/specs/`)
