# 2D Web Game Engine — 4 周学习路线

## 学习原则

1. **理论 → 竞品 → miu2d → 动手**：每个知识点走完四步
2. **理解原理 > 写代码**：AI 时代架构判断力更重要
3. **每天更新进度**：修改 `progress/progress.md`
4. **笔记沉淀**：每周的笔记写在对应 `docs/week-X/` 目录下

## 参考项目

- **miu2d**: https://github.com/luckyyyyy/miu2d
  - 17.6 万行 2D ARPG 引擎，TypeScript + Rust + WASM + 原生 WebGL
  - 17 个引擎子系统，13 个编辑模块
  - 先 clone 到本地：`git clone https://github.com/luckyyyyy/miu2d.git`

## 竞品对标清单

| 竞品 | 重点关注 |
|---|---|
| Unity 2D | URP 渲染管线、Sorting Layer、Sprite Atlas、DOTS/ECS |
| Cocos Creator | GFX 抽象层、Batcher2D、`.effect` 着色器格式、Electron 编辑器 |
| Godot 2D | 场景树、独立 2D 引擎、Signals、编辑器即引擎 |
| Phaser | 多场景架构、SpriteGPULayer、init/preload/create/update 生命周期 |
| PixiJS | 统一 WebGL/WebGPU 抽象、自动批处理、RenderGroup 缓存 |

---

## 第 1 周：WebGL 渲染管线

### 目标
理解 WebGL 渲染管线的每一层，能用原生 WebGL 画出 1000 个 sprite。

### 每日计划

#### D1: WebGL 基础
**理论：**
- WebGL 是什么：浏览器对 OpenGL ES 的绑定，状态机模型
- 上下文获取：`canvas.getContext('webgl2')`
- 着色器程序：顶点着色器（位置变换）vs 片段着色器（像素颜色）
- 缓冲区：VBO（顶点缓冲对象）存储顶点数据
- 绘制调用：`gl.drawArrays()` / `gl.drawElements()`

**资源：**
- [WebGL2 Fundamentals - How It Works](https://webgl2fundamentals.org/webgl/lessons/webgl-how-it-works.html)
- [WebGL2 Fundamentals - Shaders](https://webgl2fundamentals.org/webgl/lessons/webgl-shaders-and-glsl.html)

**miu2d 对照：**
- 读 `packages/engine/src/renderer/webgl-renderer.ts`
  - 上下文创建参数：`alpha: false, antialias: false, powerPreference: "high-performance"`
  - 为什么这些参数这样设置？

#### D2: GLSL 着色器
**理论：**
- GLSL 语法：`attribute`, `uniform`, `varying`, `precision`
- 顶点着色器职责：模型坐标 → 裁剪坐标（MVP 变换）
- 片段着色器职责：计算每个像素的颜色
- 纹理采样：`texture2D(sampler, uv)`

**资源：**
- [WebGL2 Fundamentals - Shaders and GLSL](https://webgl2fundamentals.org/webgl/lessons/webgl-shaders-and-glsl.html)

**miu2d 对照：**
- 读 `packages/engine/src/renderer/shaders.ts`
  - Sprite 着色器：像素坐标 → 裁剪坐标的变换
  - 8 纹理槽位的 if/else 采样链（WebGL1 兼容方案）
  - 4 种颜色滤镜：grayscale, frozen, poison

#### D3: 纹理与纹理图集
**理论：**
- 纹理是什么：GPU 上的图片数据
- 纹理单元：`gl.TEXTURE0` ~ `gl.TEXTURE15`，同时绑定多个纹理
- 纹理图集（Atlas）：把多张小图合并为一张大图
  - 好处：减少纹理绑定切换 → 减少 draw call
  - 工具：TexturePacker, ShoeBox
- 纹理过滤：`NEAREST`（像素风）vs `LINEAR`（平滑）

**资源：**
- [WebGL2 Fundamentals - Textures](https://webgl2fundamentals.org/webgl/lessons/webgl-3d-textures.html)

**miu2d 对照：**
- 读 `packages/engine/src/renderer/sprite-batcher.ts`
  - `MAX_TEXTURE_SLOTS = 8`：一次绑定 8 个纹理
  - 每个 sprite 的 `texIndex` 属性告诉着色器采哪个纹理
  - 新纹理只有在 8 个槽位都满时才强制 flush

#### D4: 批处理原理
**理论：**
- Draw call 的成本：CPU→GPU 通信开销、状态切换
- 批处理策略：把多个 sprite 合并到一个 VBO，一次 draw 画完
- 批处理中断：不同纹理/着色器/混合模式会中断批次
- 排序策略：按纹理/材质排序 sprite，最大化批次大小

**资源：**
- [PixiJS Batching Deep Dive](https://pixijs.com/guides/concepts/rendering/batching)

**miu2d 对照：**
- 读 `packages/engine/src/map/map-renderer.ts`
  - `_batchBuckets` 按 atlas 索引分桶
  - 同一个 atlas 的 tile 连续绘制 → 不触发 flush
  - 结果：~4800 tiles → 1-5 draw calls

#### D5: 状态管理与后处理
**理论：**
- WebGL 状态机：程序、VAO、缓冲区、混合模式都是状态
- 状态缓存：记录当前状态，避免重复设置
- 混合模式：alpha blend、additive、multiply、screen
- FBO（帧缓冲对象）：渲染到纹理，用于后处理效果

**资源：**
- [WebGL2 Fundamentals - Framebuffers](https://webgl2fundamentals.org/webgl/lessons/webgl-render-to-texture.html)

**miu2d 对照：**
- 读 `packages/engine/src/renderer/gl-state-cache.ts`
  - 缓存 `currentProgram`, `currentVAO`, `currentArrayBuffer`
  - 跳过重复的 GL 调用
- 读水面着色器（在 `shaders.ts` 中）
  - FBO 渲染场景 → UV 扰动 → 水波纹效果

### 周末任务
- [ ] 竞品渲染对标笔记（PixiJS/Cocos/Godot 的批处理策略对比）
- [ ] 用原生 WebGL 写一个最小 demo：画 1000 个彩色方块
- [ ] 更新 `progress/progress.md`

---

## 第 2 周：Rust + WebAssembly

### 目标
理解 Rust 核心概念和 WASM 集成模式，能写一个 Rust 函数编译成 WASM 在浏览器调用。

### 每日计划

#### D1: Rust 基础
**理论：**
- 为什么用 Rust：内存安全、零成本抽象、WASM 一等公民
- 所有权系统：每个值有且只有一个所有者
- 借用规则：同一时间只能有一个可变引用，或多个不可变引用
- 生命周期：编译器如何确保引用有效性

**资源：**
- [The Rust Book Ch1-4](https://doc.rust-lang.org/book/ch01-00-getting-started.html)
- [Rust by Example - Ownership](https://doc.rust-lang.org/rust-by-example/scope/ownership.html)

**miu2d 对照：**
- 读 `packages/engine-wasm/Cargo.toml`
  - `wasm-bindgen`, `hashbrown`, `ruzstd`, `web-sys` 依赖
  - 编译优化：`opt-level = 3, lto = true, codegen-units = 1`

#### D2: Rust 类型系统
**理论：**
- 结构体（struct）：命名字段的数据聚合
- 枚举（enum）：带数据的变体类型（Rust 枚举比其他语言强大得多）
- trait：类似接口，定义共享行为
- 泛型：类型参数化

**资源：**
- [The Rust Book Ch5-10](https://doc.rust-lang.org/book/ch05-00-structs.html)
- [Rust by Example - Generics](https://doc.rust-lang.org/rust-by-example/generics.html)

**miu2d 对照：**
- 读 `packages/engine-wasm/src/pathfinder.rs`
  - `Vec2` 结构体：坐标表示
  - `Pathfinder` 结构体：状态和方法
  - `PathStrategy` 枚举：5 种寻路策略

#### D3: WASM 编译链路
**理论：**
- wasm-bindgen：Rust ↔ JS 的桥接层
  - `#[wasm_bindgen]` 属性宏自动生成 JS 胶水代码
  - 支持导出函数、结构体、枚举
- wasm-pack：构建工具
  - `wasm-pack build --target web` 生成 .wasm + JS glue + .d.ts
- 编译流程：Rust → LLVM IR → WASM 字节码 → wasm-opt 优化

**资源：**
- [wasm-bindgen Book](https://rustwasm.github.io/docs/wasm-bindgen/)
- [wasm-pack Book](https://rustwasm.github.io/docs/wasm-pack/)

**miu2d 对照：**
- 读 `packages/engine-wasm/src/lib.rs`
  - `#[wasm_bindgen]` 导出的函数
  - 模块初始化：`#[wasm_bindgen(start)]`

#### D4: JS/WASM 通信
**理论：**
- WASM 线性内存：一块连续的 ArrayBuffer
- 数据传输方式：
  1. 函数参数/返回值（简单但有序列化开销）
  2. 共享内存（零拷贝，JS 创建 TypedArray 视图）
  3. SharedArrayBuffer（多线程场景）
- 零拷贝模式：Rust 暴露指针 → JS 创建视图 → 双方直接读写同一块内存

**资源：**
- [MDN - WebAssembly](https://developer.mozilla.org/en-US/docs/WebAssembly)
- [wasm-bindgen - Accessing memory](https://rustwasm.github.io/docs/wasm-bindgen/reference/accessing-memory.html)

**miu2d 对照：**
- 读 `packages/engine/src/wasm/wasm-manager.ts`
  - 动态导入 WASM 模块
  - 获取 `WebAssembly.Memory`
- 读 `packages/engine/src/wasm/wasm-path-finder.ts`
  - `obstacle_bitmap_ptr()` → `new Uint8Array(buffer, ptr, size)`
  - staging buffer 模式：JS 写临时缓冲 → 一次性 `.set()` 到 WASM 视图

#### D5: WASM 性能特性
**理论：**
- WASM 擅长：紧密循环、浮点运算、内存密集型算法
- WASM 不擅长：DOM 操作、网络 I/O、需要浏览器 API 的场景
- 函数调用开销：JS↔WASM 调用约 1-2ns，热循环中累积可观
- 内存增长成本：`memory.grow` 有开销，预分配更好

**资源：**
- [WebAssembly High-Level Goals](https://github.com/WebAssembly/design/blob/main/HighLevelGoals.md)

**miu2d 对照：**
- 读 `packages/engine-wasm/src/collision.rs`
  - 空间哈希：适合 WASM 的计算密集型场景
  - 批量查询减少 JS↔WASM 调用次数

### 周末任务
- [ ] 竞品 WASM 用法笔记（Bevy/Godot GDExtension）
- [ ] 写一个 Rust 函数编译成 WASM 在浏览器调用
- [ ] 更新 `progress/progress.md`

---

## 第 3 周：游戏引擎核心架构

### 目标
理解游戏引擎的核心架构模式，能画出 miu2d 的模块依赖图。

### 每日计划

#### D1: ECS vs OOP
**理论：**
- OOP 继承链的问题：深继承 → 僵化、菱形继承、缓存不友好
- ECS 模型：
  - Entity = 唯一 ID
  - Component = 纯数据（Position, Velocity, Sprite）
  - System = 处理函数（MovementSystem 处理所有有 Position+Velocity 的实体）
- 数据布局：Struct-of-Arrays（SoA）→ 缓存行友好 → SIMD 友好
- 混合方案：内部用 ECS 保证性能，对外暴露组件式 API

**资源：**
- [Game Programming Patterns - Component](https://gameprogrammingpatterns.com/component.html)
- [ECS FAQ](https://github.com/SanderMertens/ecs-faq)

**miu2d 对照：**
- 读 `packages/engine/src/character/` 目录
  - 4 层继承：Sprite → CharacterBase → CharacterMovement → CharacterCombat → Character
  - 思考：如果用 ECS 重构，组件和系统怎么划分？

#### D2: 游戏循环
**理论：**
- 游戏循环的本质：while(running) { update(dt); render(); }
- 固定时间步 vs 可变时间步
  - 固定时间步：物理模拟稳定，帧率独立
  - 插值渲染：在固定时间步之间平滑视觉
- `requestAnimationFrame` vs `setTimeout`

**资源：**
- [Game Programming Patterns - Game Loop](https://gameprogrammingpatterns.com/game-loop.html)
- [Fix Your Timestep!](https://gafferongames.com/post/fix_your_timestep/)

**miu2d 对照：**
- 读 `packages/engine/src/runtime/` 目录
  - 主循环如何组织？update 和 render 的调用顺序？

#### D3: 场景图与等距坐标
**理论：**
- 场景图：父子层级、变换继承
- 坐标系统：世界坐标、屏幕坐标、tile 坐标
- 等距投影：菱形 tile，64px 列宽，16px 行高
  - tile→pixel: `x = (row%2)*32 + 64*col`, `y = 16*row`
  - pixel→tile: 反向计算
- 深度排序：按行（row）排序，同行内按列排序

**资源：**
- [Red Blob Games - Isometric Grids](https://www.redblobgames.com/grids/hexagons/)

**miu2d 对照：**
- 读 `packages/engine/src/map/map-base.ts`
  - 等距坐标转换函数
  - 5 层地图结构：ground, object, top, trap, obstacle
  - 障碍物类型：NONE, CAN_OVER, TRANS, OBSTACLE（字节值匹配）

#### D4: 碰撞检测
**理论：**
- AABB（轴对齐包围盒）：最简单的碰撞检测
- 圆形碰撞：距离比较
- 空间分区：
  - 空间哈希：把空间划分为网格，每个实体放入对应格子
  - 四叉树：递归分割空间，适合不均匀分布
- 宽阶段 vs 窄阶段：先粗筛，再精确检测

**资源：**
- [Red Blob Games - Board and Tiles](https://www.redblobgames.com/)
- [Game Programming Patterns - Spatial Partition](https://gameprogrammingpatterns.com/spatial-partition.html)

**miu2d 对照：**
- 读 `packages/engine-wasm/src/collision.rs`
  - 空间哈希实现：实体按格子分桶
  - `query_radius`, `detect_all_collisions` 接口
- 读 `packages/engine/src/wasm/wasm-collision.ts`
  - JS 侧如何调用 WASM 碰撞检测

#### D5: 寻路算法
**理论：**
- A* 算法：开放列表 + 关闭列表 + 启发函数
  - 启发函数：曼哈顿距离、欧几里得距离、切比雪夫距离
  - 等距网格的邻居计算：奇偶行偏移
- 导航网格（NavMesh）：不规则区域的寻路
- 性能优化：分层寻路、跳点搜索

**资源：**
- [Red Blob Games - A* Pathfinding](https://www.redblobgames.com/pathfinding/a-star/introduction.html)
- [Red Blob Games - Implementation](https://www.redblobgames.com/pathfinding/a-star/implementation.html)

**miu2d 对照：**
- 读 `packages/engine-wasm/src/pathfinder.rs`
  - 5 种策略：PathOneStep, SimpleMaxNpcTry, PerfectMaxNpcTry, PerfectMaxPlayerTry, PathStraightLine
  - 等距邻居计算：奇偶行不同的偏移量
  - 位图障碍物：3 个 bitmap（static, hard, dynamic）

### 周末任务
- [ ] 竞品架构对标笔记（Unity DOTS / Godot 场景树 / Cocos GFX）
- [ ] 画出 miu2d 模块依赖图和数据流图
- [ ] 更新 `progress/progress.md`

---

## 第 4 周：编辑器 + 系统设计 + 竞品对标

### 目标
理解游戏编辑器和脚本系统设计，输出自己的引擎架构设计文档。

### 每日计划

#### D1: 游戏编辑器架构
**理论：**
- 核心面板：场景视图、属性检查器、层级树、资源浏览器、控制台
- 编辑器和引擎的关系：
  - Unity：编辑器用 C++/C# IMGUI，独立于游戏运行时
  - Godot：编辑器用引擎自身的 UI 系统（dogfooding）
  - Cocos：Electron + TypeScript
- 序列化：游戏数据的存储格式（JSON、二进制、数据库）

**资源：**
- [Godot Editor Architecture](https://docs.godotengine.org/en/stable/tutorials/editor/index.html)

**miu2d 对照：**
- 读 `packages/dashboard/src/` 目录
  - VS Code 风格布局：ActivityBar + Sidebar + Content
  - 13 个编辑模块的组织方式
- 读 `packages/server/` 目录
  - Hono + tRPC + Prisma + PostgreSQL

#### D2: 脚本系统设计
**理论：**
- 脚本引擎的角色：游戏逻辑的容器，设计师/策划的工具
- DSL（领域特定语言）：自定义语法，针对游戏场景优化
- 通用脚本语言：Lua、Python、JavaScript
- 阻塞式脚本在游戏循环中的实现：
  - Promise/async 模式：脚本命令返回 Promise，执行器暂停直到 resolve
  - 协程模式：Lua 的 coroutine.yield

**资源：**
- [Game Scripting in Python](https://www.lua.org/gems/sample.pdf)

**miu2d 对照：**
- 读 `packages/engine/src/script/executor.ts`
  - async while 循环：`await executeCommand()` 暂停执行
  - `BlockingResolver`：阻塞操作的 Promise 管理
  - 调用栈嵌套：RunScript 推栈，Return 弹栈
- 读 `packages/engine/src/script/lua/lua-executor.ts`
  - wasmoon 的 `:await()` 协程桥接
  - 170+ GameAPI 函数注册为 Lua 全局函数

#### D3: 资源管线
**理论：**
- 资源类型：图片、音频、地图、精灵帧、配置数据
- 二进制格式解析：文件头 → 元数据 → 数据区
- RLE 压缩：行程编码，适合像素数据
- 异步加载：fetch → decode → upload to GPU
- 资源缓存：已加载的资源复用，避免重复加载

**miu2d 对照：**
- 读 `packages/engine/src/resource/` 目录
  - 8 种格式解码器：ASF, MPC, MAP, SHD, XNB, MSF, MMF, INI/OBJ
  - ASF：16 字节头 + 64 字节元数据 + 调色板 + RLE 帧数据
  - MSF v2：Zstd 压缩的精灵帧格式
- 读 `packages/engine-wasm/src/asf_decoder.rs` + `mpc_decoder.rs`
  - Rust 实现的解码器性能对比

#### D4: 游戏子系统
**理论：**
- 音频系统：Web Audio API、OGG 解码、混音、音量控制
- 粒子系统：发射器参数、粒子生命周期、物理模拟
- 天气系统：雨、雪、闪电、屏幕效果
- UI 系统：HUD、对话框、菜单、文字渲染

**miu2d 对照：**
- 读 `packages/engine/src/audio/` — Web Audio 集成
- 读 `packages/engine/src/weather/` — 雨/雪/粒子
- 读 `packages/engine/src/gui/` — HUD、对话框

#### D5: 架构设计文档
**输出：** 自己的引擎架构设计文档初稿，包含：
- 技术选型（WebGL vs WebGPU、Rust vs TS、ECS vs OOP）
- 模块划分和接口定义
- 渲染管线设计
- 脚本系统设计
- 编辑器设计
- 与 miu2d 和竞品的差异点

### 周末任务
- [ ] 竞品编辑器对标笔记（Unity/Cocos/Godot 编辑器对比）
- [ ] 完善架构设计文档
- [ ] 更新 `progress/progress.md`
