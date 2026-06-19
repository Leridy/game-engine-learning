# 学习进度看板

> 更新规则：每完成一个知识点，将状态从 🔲 改为 ✅，并添加完成日期和笔记链接。

## 总览

| 周 | 主题 | 进度 | 状态 |
|---|---|---|---|
| W1 | WebGL 渲染管线 | 0/5 | 🔲 未开始 |
| W2 | Rust + WebAssembly | 0/5 | 🔲 未开始 |
| W3 | 游戏引擎核心架构 | 0/5 | 🔲 未开始 |
| W4 | 编辑器 + 系统设计 + 竞品对标 | 0/5 | 🔲 未开始 |

---

## W1: WebGL 渲染管线

| 天 | 知识点 | 状态 | 完成日期 | 笔记 |
|---|---|---|---|---|
| D1 | WebGL 基础：上下文、着色器、缓冲区、绘制调用 | 🔲 | | |
| D1 | miu2d: `webgl-renderer.ts` 上下文初始化 | 🔲 | | |
| D2 | GLSL 顶点/片段着色器、坐标变换 | 🔲 | | |
| D2 | miu2d: `shaders.ts` 3 个着色器 | 🔲 | | |
| D3 | 纹理、纹理图集、多纹理采样 | 🔲 | | |
| D3 | miu2d: `sprite-batcher.ts` 8 纹理槽位 | 🔲 | | |
| D4 | 批处理原理：draw call 成本 | 🔲 | | |
| D4 | miu2d: `map-renderer.ts` atlas 分桶 | 🔲 | | |
| D5 | WebGL 状态管理、混合模式、FBO | 🔲 | | |
| D5 | miu2d: `gl-state-cache.ts` + 水面着色器 | 🔲 | | |
| W1E | 竞品渲染对标：PixiJS/Cocos/Godot | 🔲 | | |
| W1E | 交付：最小 WebGL 2D 渲染 demo | 🔲 | | |

---

## W2: Rust + WebAssembly

| 天 | 知识点 | 状态 | 完成日期 | 笔记 |
|---|---|---|---|---|
| D1 | Rust 基础：所有权、借用、生命周期 | 🔲 | | |
| D1 | miu2d: `Cargo.toml` 依赖和编译配置 | 🔲 | | |
| D2 | Rust 结构体、枚举、trait、泛型 | 🔲 | | |
| D2 | miu2d: `pathfinder.rs` 数据结构 | 🔲 | | |
| D3 | wasm-bindgen/wasm-pack 编译链路 | 🔲 | | |
| D3 | miu2d: `lib.rs` 导出函数 | 🔲 | | |
| D4 | JS/WASM 通信：线性内存、零拷贝 | 🔲 | | |
| D4 | miu2d: `wasm-manager.ts` + `wasm-path-finder.ts` | 🔲 | | |
| D5 | WASM 性能特性：何时用/不用 WASM | 🔲 | | |
| D5 | miu2d: `collision.rs` 空间哈希 | 🔲 | | |
| W2E | 竞品 WASM 用法：Bevy/Godot GDExtension | 🔲 | | |
| W2E | 交付：Rust→WASM→JS demo | 🔲 | | |

---

## W3: 游戏引擎核心架构

| 天 | 知识点 | 状态 | 完成日期 | 笔记 |
|---|---|---|---|---|
| D1 | ECS vs OOP：数据布局、缓存友好 | 🔲 | | |
| D1 | miu2d: `character/` 4 层继承链 | 🔲 | | |
| D2 | 游戏循环：固定时间步、插值 | 🔲 | | |
| D2 | miu2d: `runtime/` 主循环 | 🔲 | | |
| D3 | 场景图、坐标系统、等距投影 | 🔲 | | |
| D3 | miu2d: `map-base.ts` 等距坐标 | 🔲 | | |
| D4 | 碰撞检测：AABB、空间哈希、四叉树 | 🔲 | | |
| D4 | miu2d: `collision.ts` + `wasm-collision.ts` | 🔲 | | |
| D5 | 寻路算法：A*、等距网格适配 | 🔲 | | |
| D5 | miu2d: `pathfinder.rs` 5 种策略 | 🔲 | | |
| W3E | 竞品架构对标：Unity DOTS/Godot/Cocos | 🔲 | | |
| W3E | 交付：miu2d 模块依赖图和数据流图 | 🔲 | | |

---

## W4: 编辑器 + 系统设计 + 竞品对标

| 天 | 知识点 | 状态 | 完成日期 | 笔记 |
|---|---|---|---|---|
| D1 | 游戏编辑器架构：Inspector/Hierarchy/SceneView | 🔲 | | |
| D1 | miu2d: `dashboard/` 13 个编辑模块 | 🔲 | | |
| D2 | 脚本系统设计：DSL、Lua、GameAPI | 🔲 | | |
| D2 | miu2d: `script/` 双执行模型 | 🔲 | | |
| D3 | 资源管线：格式解析、图集、异步加载 | 🔲 | | |
| D3 | miu2d: `resource/` 8 种解码器 | 🔲 | | |
| D4 | 音频、粒子、天气、UI 系统 | 🔲 | | |
| D4 | miu2d: `audio/` `weather/` `gui/` | 🔲 | | |
| D5 | 自己的引擎架构设计文档初稿 | 🔲 | | |
| D5 | 对标 miu2d 和竞品，确定技术选型 | 🔲 | | |
| W4E | 竞品编辑器对标：Unity/Cocos/Godot | 🔲 | | |
| W4E | 交付：完整架构设计文档 | 🔲 | | |

---

## 知识自检

学完每个模块后，能回答这些问题才算通过：

### WebGL ✅/🔲
- [ ] draw call 的成本在哪里？为什么批处理能优化？
- [ ] 纹理图集和多纹理采样各自的优劣？
- [ ] WebGL 状态机为什么需要状态缓存？

### Rust/WASM ✅/🔲
- [ ] 所有权和借用解决了什么问题？
- [ ] JS 和 WASM 之间数据传输的几种方式及性能差异？
- [ ] 什么场景该用 WASM，什么场景不该？

### 游戏引擎架构 ✅/🔲
- [ ] ECS 和 OOP 继承链的核心区别？缓存友好性如何影响性能？
- [ ] A* 寻路的启发函数如何影响结果？等距网格的邻居计算有什么坑？
- [ ] 游戏循环为什么要固定时间步？

### 编辑器/脚本 ✅/🔲
- [ ] 阻塞式脚本如何在单线程游戏循环中实现？
- [ ] GameAPI 如何同时服务 DSL 和 Lua 两种运行时？
