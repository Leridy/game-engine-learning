# W1: WebGL 渲染管线

## 学习目标

1. 理解 WebGL 状态机模型和渲染管线
2. 能手写 GLSL 顶点/片段着色器
3. 理解批处理为什么能优化性能
4. 能用原生 WebGL 画出 1000 个 sprite

## 每日笔记

- [Day 1: WebGL 基础](./day-1.md)
- [Day 2: GLSL 着色器](./day-2.md)
- [Day 3: 纹理与纹理图集](./day-3.md)
- [Day 4: 批处理原理](./day-4.md)
- [Day 5: 状态管理与后处理](./day-5.md)
- [周末: 竞品渲染对标](./weekend.md)

## 知识自检

- [ ] draw call 的成本在哪里？为什么批处理能优化？
- [ ] 纹理图集和多纹理采样各自的优劣？
- [ ] WebGL 状态机为什么需要状态缓存？
- [ ] FBO 的用途是什么？

## miu2d 关键文件

| 文件 | 关注点 |
|---|---|
| `renderer/webgl-renderer.ts` | 上下文初始化、主渲染循环 |
| `renderer/shaders.ts` | 3 个 GLSL 着色器 |
| `renderer/sprite-batcher.ts` | 8 纹理槽位批处理 |
| `renderer/gl-state-cache.ts` | GL 状态缓存 |
| `map/map-renderer.ts` | atlas 分桶策略 |

## 竞品参考

- PixiJS: `Batcher` 类自动批处理
- Cocos: `Batcher2D` 独立 2D 批处理器
- Godot: `RenderingServerCanvas` 渲染命令抽象
