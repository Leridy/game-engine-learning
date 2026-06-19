# 2D Web Game Engine Learning Roadmap — Design Document

> 日期：2026-06-20
> 状态：✅ 已批准

## 背景

用户是前端开发者，目标转型 2D 网页游戏引擎 + 编辑器开发。要求 1 个月高强度学习原理，3 个月产出可演示版本。

## 参考项目

**miu2d** (https://github.com/luckyyyyy/miu2d)
- 17.6 万行 2D ARPG 引擎
- TypeScript + Rust + WebAssembly + 原生 WebGL
- 17 个引擎子系统，13 个编辑模块
- 11 个 pnpm monorepo 包

### miu2d 架构要点

**渲染管线：**
- 原生 WebGL2，3 个 GLSL 着色器（Sprite/Rect/Water）
- SpriteBatcher：8 纹理槽位，4800 tiles → 1-5 draw calls
- GLStateCache：消除冗余 GL 调用
- atlas 分桶排序策略

**Rust/WASM：**
- 6 个 Rust 模块：A* 寻路、ASF/MPC/MSF 解码、空间碰撞、Zstd 解压
- 零拷贝共享内存：Rust 暴露指针 → JS 创建 TypedArray 视图
- 编译优化：opt-level=3, lto=true, wasm-opt -O4 --enable-simd

**脚本系统：**
- 双执行模型：自定义 DSL（218 命令）+ Lua 5.4（via wasmoon）
- 共享 GameAPI（170+ 函数）
- 阻塞式脚本通过 Promise + BlockingResolver 实现

**编辑器：**
- React 19 + VS Code 风格布局
- 13 个编辑模块
- 后端：Hono + tRPC + Prisma + PostgreSQL

### miu2d 优劣势

**优势：** 渲染性能优秀、逆向工程忠实、WASM 集成干净、双脚本模型、内存管理严谨
**劣势：** 无渲染测试、WebGL1 兼容代码遗留、模块级可变状态、4 层继承链过深、Rust 覆盖率仅 4.1%

## 竞品分析结论

| 竞品 | 关键收获 |
|---|---|
| Unity | Sorting Layer、Sprite Atlas、ScriptableRenderPass、DOTS/ECS |
| Cocos Creator | GFX 抽象层、Batcher2D、.effect 着色器、Electron 编辑器 |
| Godot | 一切皆场景、独立 2D 引擎、Signals、编辑器即引擎 |
| Phaser | 多场景架构、SpriteGPULayer、与 React/Vue 集成 |
| PixiJS | 统一 WebGL/WebGPU 抽象、自动批处理、纯渲染层 |

## 学习路线方案

**选定方案：A + C 结合**
- A（自底向上）：理论为主线，知识体系完整
- C（并行双线）：每个理论模块立刻用 miu2d 源码印证

**节奏：** 每天上午理论 → 竞品做法 → miu2d 做法 → 动手验证

## 4 周计划

### W1: WebGL 渲染管线
- D1: WebGL 基础 + `webgl-renderer.ts`
- D2: GLSL 着色器 + `shaders.ts`
- D3: 纹理/图集 + `sprite-batcher.ts`
- D4: 批处理原理 + `map-renderer.ts`
- D5: 状态管理/FBO + `gl-state-cache.ts`
- 周末：竞品渲染对标 + 最小 WebGL demo

### W2: Rust + WebAssembly
- D1: Rust 所有权/借用 + `Cargo.toml`
- D2: Rust 类型系统 + `pathfinder.rs`
- D3: wasm-bindgen/wasm-pack + `lib.rs`
- D4: JS/WASM 通信 + `wasm-manager.ts` + `wasm-path-finder.ts`
- D5: WASM 性能特性 + `collision.rs`
- 周末：竞品 WASM 用法 + Rust→WASM demo

### W3: 游戏引擎核心架构
- D1: ECS vs OOP + `character/` 继承链
- D2: 游戏循环 + `runtime/`
- D3: 场景图/等距坐标 + `map-base.ts`
- D4: 碰撞检测 + `collision.ts`
- D5: A* 寻路 + `pathfinder.rs`
- 周末：竞品架构对标 + 模块依赖图

### W4: 编辑器 + 系统设计
- D1: 编辑器架构 + `dashboard/`
- D2: 脚本系统 + `script/`
- D3: 资源管线 + `resource/`
- D4: 游戏子系统 + `audio/` `weather/` `gui/`
- D5: 架构设计文档初稿
- 周末：竞品编辑器对标 + 完善设计文档

## 交付物

| 周 | 交付物 |
|---|---|
| W1 | 渲染管线笔记 + 最小 WebGL demo |
| W2 | Rust/WASM 笔记 + Rust→WASM demo |
| W3 | 引擎架构笔记 + miu2d 模块依赖图 |
| W4 | 完整架构设计文档 |

## 项目仓库

https://github.com/Leridy/game-engine-learning

每次 Claude Code 加载时，读取 `progress/progress.md` 恢复进度。
