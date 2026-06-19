# W2: Rust + WebAssembly

## 学习目标

1. 理解 Rust 所有权模型和核心语法
2. 理解 WASM 编译链路和 JS/WASM 通信模式
3. 能写一个 Rust 函数编译成 WASM 在浏览器调用
4. 理解零拷贝共享内存的工作原理

## 每日笔记

- [Day 1: Rust 基础](./day-1.md)
- [Day 2: Rust 类型系统](./day-2.md)
- [Day 3: WASM 编译链路](./day-3.md)
- [Day 4: JS/WASM 通信](./day-4.md)
- [Day 5: WASM 性能特性](./day-5.md)
- [周末: 竞品 WASM 用法](./weekend.md)

## 知识自检

- [ ] 所有权和借用解决了什么问题？
- [ ] JS 和 WASM 之间数据传输的几种方式及性能差异？
- [ ] 什么场景该用 WASM，什么场景不该？
- [ ] wasm-bindgen 的 `#[wasm_bindgen]` 做了什么？

## miu2d 关键文件

| 文件 | 关注点 |
|---|---|
| `engine-wasm/Cargo.toml` | 依赖、编译优化配置 |
| `engine-wasm/src/lib.rs` | 模块导出 |
| `engine-wasm/src/pathfinder.rs` | A* 寻路（1144 行） |
| `engine-wasm/src/collision.rs` | 空间哈希 |
| `engine/src/wasm/wasm-manager.ts` | WASM 初始化 |
| `engine/src/wasm/wasm-path-finder.ts` | 零拷贝桥接 |

## 竞品参考

- Bevy: 纯 Rust ECS 引擎
- Godot GDExtension: C 绑定层设计
