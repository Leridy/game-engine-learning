# W3: 游戏引擎核心架构

## 学习目标

1. 理解 ECS vs OOP 的核心区别和适用场景
2. 理解游戏循环、固定时间步、帧率独立
3. 理解等距坐标系统和深度排序
4. 理解 A* 寻路和空间分区碰撞检测

## 每日笔记

- [Day 1: ECS vs OOP](./day-1.md)
- [Day 2: 游戏循环](./day-2.md)
- [Day 3: 场景图与等距坐标](./day-3.md)
- [Day 4: 碰撞检测](./day-4.md)
- [Day 5: 寻路算法](./day-5.md)
- [周末: 竞品架构对标](./weekend.md)

## 知识自检

- [ ] ECS 和 OOP 继承链的核心区别？缓存友好性如何影响性能？
- [ ] A* 寻路的启发函数如何影响结果？
- [ ] 等距网格的邻居计算有什么坑？
- [ ] 游戏循环为什么要固定时间步？

## miu2d 关键文件

| 文件 | 关注点 |
|---|---|
| `character/character.ts` | 17 状态状态机 |
| `character/character-base.ts` | 属性和位置 |
| `character/character-movement.ts` | 移动和寻路 |
| `map/map-base.ts` | 等距地图逻辑 |
| `runtime/` | 主循环 |
| `npc/npc-ai.ts` | NPC AI |
| `engine-wasm/src/pathfinder.rs` | 5 种寻路策略 |
| `engine-wasm/src/collision.rs` | 空间哈希 |

## 竞品参考

- Unity DOTS: 并行 ECS
- Godot: 场景树 + Signals
- Cocos: 组件系统 + GFX 抽象
