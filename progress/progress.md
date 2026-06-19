# 学习进度看板

> 更新规则：每完成一个知识点，将状态从 🔲 改为 ✅，并添加完成日期和笔记链接。

## 总览

| 周 | 主题 | 进度 | 状态 |
|---|---|---|---|
| W1 | WebGPU 渲染管线 | 0/5 | 🔲 未开始 |
| W2 | Rust + WebAssembly + WebGPU 集成 | 0/5 | 🔲 未开始 |
| W3 | 骨骼动画系统（核心模块） | 0/5 | 🔲 未开始 |
| W4 | 引擎架构设计 + Cocos 源码深度分析 | 0/5 | 🔲 未开始 |

---

## W1: WebGPU 渲染管线

| 天 | 知识点 | 状态 | 完成日期 | 笔记 |
|---|---|---|---|---|
| D1 | WebGPU 基础：Adapter/Device/Buffer/Texture/Sampler | 🔲 | | |
| D1 | Cocos: `gfx/base/device.ts` + `gfx/webgpu/webgpu-device.ts` | 🔲 | | |
| D2 | WGSL 着色器语言：语法、入口点、属性、对齐规则 | 🔲 | | |
| D2 | Cocos: `gfx/webgpu/webgpu-shader.ts` | 🔲 | | |
| D3 | Pipeline State Objects + Bind Groups | 🔲 | | |
| D3 | Cocos: `gfx/base/pipeline-state.ts` + `gfx/webgpu/webgpu-pipeline-state.ts` | 🔲 | | |
| D4 | Command Encoder + 实例化渲染 | 🔲 | | |
| D4 | Cocos: `2d/renderer/batcher-2d.ts` + `rendering/render-queue.ts` | 🔲 | | |
| D5 | Compute Shader + 2D 渲染实战 | 🔲 | | |
| D5 | Cocos: `gfx/webgpu/webgpu-command-buffer.ts` + `2d/renderer/render-data.ts` | 🔲 | | |
| W1E | 竞品 WebGPU 对标：PixiJS/Cocos/Babylon.js | 🔲 | | |
| W1E | 交付：WebGPU 实例化渲染 demo | 🔲 | | |

---

## W2: Rust + WebAssembly + WebGPU 集成

| 天 | 知识点 | 状态 | 完成日期 | 笔记 |
|---|---|---|---|---|
| D1 | Rust 基础：所有权、借用、生命周期 | 🔲 | | |
| D1 | miu2d: `Cargo.toml` 依赖和编译配置 | 🔲 | | |
| D2 | Rust 类型系统：结构体、枚举、trait、泛型 | 🔲 | | |
| D2 | miu2d: `pathfinder.rs` 数据结构 | 🔲 | | |
| D3 | wasm-bindgen/wasm-pack 编译链路 | 🔲 | | |
| D3 | miu2d: `lib.rs` 导出函数 | 🔲 | | |
| D4 | JS/WASM 通信：线性内存、零拷贝 | 🔲 | | |
| D4 | miu2d: `wasm-manager.ts` + `wasm-path-finder.ts` | 🔲 | | |
| D5 | WASM 性能特性 + Compute Shader 分工 | 🔲 | | |
| D5 | miu2d: `collision.rs` 空间哈希 | 🔲 | | |
| W2E | Rust→WASM→JS demo | 🔲 | | |

---

## W3: 骨骼动画系统（核心模块）

| 天 | 知识点 | 状态 | 完成日期 | 笔记 |
|---|---|---|---|---|
| D1 | 骨骼层级、Bind Pose、FK、2D 变换矩阵 | 🔲 | | |
| D1 | Cocos: `skeletal-animation.ts` + `skeletal-animation-utils.ts` | 🔲 | | |
| D2 | 蒙皮：LBS 公式、CPU vs GPU 蒙皮、WGSL 蒙皮着色器 | 🔲 | | |
| D2 | Cocos: `skinned-mesh-renderer.ts` + Unity SpriteSkin | 🔲 | | |
| D3 | 动画剪辑：Track/Curve/Keyframe、插值方法 | 🔲 | | |
| D3 | Cocos: `animation-clip.ts` + Spine JSON 格式 | 🔲 | | |
| D4 | 动画混合：Crossfade/Additive/Layer、状态机 | 🔲 | | |
| D4 | Cocos: `animation-controller.ts` (Marionette) + 混合系统 | 🔲 | | |
| D5 | 网格变形 + IK（CCD/FABRIK）+ Sprite 换装 | 🔲 | | |
| D5 | Cocos: `cocos/spine/assembler/` + `skeleton.ts` | 🔲 | | |
| W3E | 竞品骨骼动画对标：Unity/Cocos/Godot/Spine | 🔲 | | |
| W3E | 交付：骨骼动画数据流图 | 🔲 | | |

---

## W4: 引擎架构设计 + Cocos 源码深度分析

| 天 | 知识点 | 状态 | 完成日期 | 笔记 |
|---|---|---|---|---|
| D1 | Cocos GFX 抽象层深度分析 | 🔲 | | |
| D2 | Cocos 渲染管线深度分析 | 🔲 | | |
| D3 | Cocos 骨骼动画系统深度分析 | 🔲 | | |
| D4 | 游戏编辑器 + 脚本系统 | 🔲 | | |
| D5 | 自己的架构设计文档 | 🔲 | | |
| W4E | 完善架构设计文档 | 🔲 | | |

---

## 知识自检

### WebGPU ✅/🔲
- [ ] WebGPU 和 WebGL 的核心架构区别是什么？
- [ ] Pipeline State Object 为什么是不可变的？
- [ ] BindGroup 和 BindGroupLayout 的关系？
- [ ] 实例化渲染如何用一次 draw call 画 N 个 sprite？
- [ ] Compute Shader 的典型用途？

### Rust/WASM ✅/🔲
- [ ] 所有权和借用解决了什么问题？
- [ ] JS 和 WASM 之间数据传输的几种方式及性能差异？
- [ ] 什么场景该用 WASM，什么场景该用 Compute Shader？

### 骨骼动画 ✅/🔲
- [ ] 骨骼层级的正向运动学公式是什么？
- [ ] LBS 蒙皮的完整公式？skinMatrix 是什么？
- [ ] CPU 蒙皮和 GPU 蒙皮的区别和适用场景？
- [ ] Crossfade 混合如何处理角度插值（最短路径）？
- [ ] 动画状态机的 Transition 如何触发？
- [ ] FABRIK 和 CCD IK 的核心区别？
