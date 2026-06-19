# W2: Rust + WebAssembly + WebGPU 集成

## 学习目标

1. 理解 Rust 核心概念和 WASM 集成模式
2. 理解 WASM 和 Compute Shader 的分工
3. 能写一个 Rust→WASM→JS 的 demo

## 每日笔记

- [Day 1: Rust 基础](./day-1.md)
- [Day 2: Rust 类型系统](./day-2.md)
- [Day 3: WASM 编译链路](./day-3.md)
- [Day 4: JS/WASM 通信](./day-4.md)
- [Day 5: WASM 性能特性 + Compute Shader 分工](./day-5.md)
- [周末: Rust→WASM demo](./weekend.md)

## 知识自检

- [ ] 所有权和借用解决了什么问题？
- [ ] JS 和 WASM 之间数据传输的几种方式？
- [ ] 什么场景用 WASM，什么场景用 Compute Shader？

## 关键源码

| 文件 | 关注点 |
|---|---|
| miu2d: `engine-wasm/Cargo.toml` | 编译配置 |
| miu2d: `engine-wasm/src/pathfinder.rs` | A* 寻路（1144 行） |
| miu2d: `engine/src/wasm/wasm-manager.ts` | WASM 初始化 |
| miu2d: `engine/src/wasm/wasm-path-finder.ts` | 零拷贝桥接 |
