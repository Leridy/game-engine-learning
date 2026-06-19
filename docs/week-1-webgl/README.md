# W1: WebGPU 渲染管线

> 注意：已从 WebGL 转向 WebGPU

## 学习目标

1. 理解 WebGPU 核心概念（Adapter/Device/Buffer/Texture/Pipeline/BindGroup）
2. 掌握 WGSL 着色器语言
3. 理解实例化渲染和 Compute Shader
4. 能用 WebGPU 实例化画出 1000 个 sprite

## 每日笔记

- [Day 1: WebGPU 基础](./day-1.md)
- [Day 2: WGSL 着色器语言](./day-2.md)
- [Day 3: Pipeline State Objects + Bind Groups](./day-3.md)
- [Day 4: Command Encoder + 实例化渲染](./day-4.md)
- [Day 5: Compute Shader + 2D 渲染实战](./day-5.md)
- [周末: 竞品 WebGPU 对标](./weekend.md)

## 知识自检

- [ ] WebGPU 和 WebGL 的核心架构区别？
- [ ] Pipeline State Object 为什么不可变？
- [ ] BindGroup 和 BindGroupLayout 的关系？
- [ ] 实例化渲染如何一次 draw call 画 N 个 sprite？
- [ ] Compute Shader 的典型用途？

## 关键源码

| 文件 | 关注点 |
|---|---|
| Cocos: `gfx/base/device.ts` | Device 抽象工厂 |
| Cocos: `gfx/webgpu/webgpu-device.ts` | WebGPU 后端初始化 |
| Cocos: `gfx/base/pipeline-state.ts` | Pipeline 抽象 |
| Cocos: `2d/renderer/batcher-2d.ts` | 2D 批处理器 |
| Cocos: `rendering/render-queue.ts` | 渲染队列排序 |
