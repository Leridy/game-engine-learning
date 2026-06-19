# 2D Web Game Engine — 学习路线 v2

> 更新说明：WebGL → WebGPU，新增骨骼动画核心模块，新增 Cocos Creator 源码分析

## 学习原则

1. **理论 → 竞品 → 源码 → 动手**：每个知识点走完四步
2. **理解原理 > 写代码**：AI 时代架构判断力更重要
3. **每天更新进度**：修改 `progress/progress.md`
4. **笔记沉淀**：每周的笔记写在对应 `docs/week-X/` 目录下

## 参考项目

- **miu2d**: https://github.com/luckyyyyy/miu2d — 2D ARPG 引擎（帧动画，无骨骼动画）
- **Cocos Creator**: https://github.com/cocos/cocos-engine — 完整游戏引擎（含骨骼动画、WebGPU）

## 竞品对标清单

| 竞品 | 重点关注 |
|---|---|
| Cocos Creator | GFX 抽象层、Batcher2D、骨骼动画系统、Marionette 状态机、WebGPU 后端 |
| Unity 2D | URP 渲染管线、2D Animation Package、SpriteSkin、DOTS/ECS |
| Godot 2D | Skeleton2D/Bone2D、AnimationTree、独立 2D 引擎 |
| PixiJS | v8 WebGPU 后端、双着色器系统、实例化渲染 |
| Spine 运行时 | 骨骼动画行业标准运行时实现 |
| Babylon.js | WebGPU 先驱、Snapshot Rendering、Compute Shader |

---

## 第 1 周：WebGPU 渲染管线

> 从 WebGL 转向 WebGPU — 这是现代 GPU 编程的正确方向

### 目标
理解 WebGPU 核心概念和 2D 渲染管线，能用 WebGPU 画出 1000 个实例化 sprite。

### 每日计划

#### D1: WebGPU 基础
**理论：**
- WebGPU vs WebGL：为什么 WebGL 被替代？架构差异是什么？
- GPUAdapter / GPUDevice：异步初始化模型
- GPUBuffer：用途标志（VERTEX, UNIFORM, STORAGE, COPY_DST）
- GPUTexture：格式、Mipmap、TextureView
- GPUSampler：过滤模式、寻址模式

**资源：**
- [WebGPU Fundamentals](https://webgpufundamentals.org/)
- [MDN WebGPU API](https://developer.mozilla.org/en-US/docs/Web/API/WebGPU_API)

**源码对照：**
- Cocos: `cocos/gfx/base/device.ts` — Device 抽象工厂
- Cocos: `cocos/gfx/webgpu/webgpu-device.ts` — WebGPU 后端初始化

#### D2: WGSL 着色器语言
**理论：**
- WGSL 语法 vs GLSL：类型声明、函数、结构体
- `@vertex` / `@fragment` / `@compute` 入口点
- `@location` / `@builtin` / `@group` / `@binding` 属性
- 变量：`const` / `override` / `let` / `var` 的区别
- 纹理采样：`textureSample(tex, sampler, uv)`
- 对齐规则：vec3 对齐到 16 字节

**资源：**
- [Google Tour of WGSL](https://google.github.io/tour-of-wgsl/)
- [W3C WGSL Spec](https://www.w3.org/TR/WGSL/)

**源码对照：**
- Cocos: `cocos/gfx/webgpu/webgpu-shader.ts` — WGSL 编译

#### D3: Pipeline State Objects + Bind Groups
**理论：**
- RenderPipeline：不可变的渲染状态集合
- ComputePipeline：计算管线
- BindGroupLayout：定义资源绑定结构
- BindGroup：实际资源绑定实例
- 与 WebGL 的区别：WebGL 每次 draw 前设置状态，WebGPU 创建时验证一次

**源码对照：**
- Cocos: `cocos/gfx/base/pipeline-state.ts` — Pipeline 抽象
- Cocos: `cocos/gfx/webgpu/webgpu-pipeline-state.ts` — WebGPU 实现

#### D4: Command Encoder + 实例化渲染
**理论：**
- CommandEncoder → RenderPassEncoder → draw 命令 → finish → queue.submit()
- 实例化渲染：`drawIndexed(6, instanceCount)` — 一次 draw 画 N 个 sprite
- 顶点缓冲 vs 实例缓冲：stepMode 'vertex' vs 'instance'
- 纹理图集：所有 sprite 共享一张大纹理

**资源：**
- [WebGPU Fundamentals - Instancing](https://webgpufundamentals.org/webgpu/lessons/webgpu-instanced-rectangles.html)

**源码对照：**
- Cocos: `cocos/2d/renderer/batcher-2d.ts` — 2D 批处理器
- Cocos: `cocos/rendering/render-queue.ts` — 渲染队列排序

#### D5: Compute Shader + 2D 渲染实战
**理论：**
- Compute Shader 基础：workgroup、dispatch、storage buffer
- 粒子系统：compute 做物理模拟 → render 读同一个 buffer 画粒子
- Compute-to-Render 同步：WebGPU 自动处理
- 完整的 WebGPU 2D 渲染流程：初始化 → 创建资源 → 编码 → 提交

**源码对照：**
- Cocos: `cocos/gfx/webgpu/webgpu-command-buffer.ts` — 命令编码
- Cocos: `cocos/2d/renderer/render-data.ts` — 2D 渲染数据

### 周末任务
- [ ] 竞品 WebGPU 对标：PixiJS v8 双后端、Cocos GFX、Babylon.js Snapshot Rendering
- [ ] 用原生 WebGPU 写一个最小 demo：实例化画 1000 个 sprite
- [ ] 更新 `progress/progress.md`

---

## 第 2 周：Rust + WebAssembly + WebGPU 集成

### 目标
理解 Rust 核心概念和 WASM 集成模式，理解 WASM 在游戏引擎中的定位。

### 每日计划

#### D1: Rust 基础
**理论：** 所有权、借用、生命周期
**源码对照：** miu2d `Cargo.toml` — 编译配置

#### D2: Rust 类型系统
**理论：** 结构体、枚举、trait、泛型
**源码对照：** miu2d `pathfinder.rs` — 数据结构设计

#### D3: WASM 编译链路
**理论：** wasm-bindgen、wasm-pack、编译流程
**源码对照：** miu2d `lib.rs` — 导出函数

#### D4: JS/WASM 通信
**理论：** 线性内存、零拷贝、SharedArrayBuffer
**源码对照：** miu2d `wasm-manager.ts` + `wasm-path-finder.ts`

#### D5: WASM 性能特性 + Compute Shader
**理论：** WASM 适合什么、不适合什么、与 Compute Shader 的分工
**源码对照：** miu2d `collision.rs` — 空间哈希

### 周末任务
- [ ] Rust→WASM→JS demo
- [ ] 更新 `progress/progress.md`

---

## 第 3 周：骨骼动画系统（核心模块）

> 这是重中之重 — 骨骼动画是现代 2D 游戏引擎的核心能力

### 目标
深入理解骨骼动画的每一层，从数据结构到 GPU 渲染。

### 每日计划

#### D1: 骨骼动画基础
**理论：**
- 骨骼层级：Bone { name, parentIndex, localPosition, localRotation, localScale }
- Bind Pose / Rest Pose / Inverse Bind Matrix
- 正向运动学（FK）：worldMatrix[i] = worldMatrix[parent[i]] * localMatrix[i]
- 2D 变换矩阵：3x3 仿射矩阵（translate + rotate + scale）

**资源：**
- [Spine Runtime Architecture](http://esotericsoftware.com/spine-runtime-guide)
- Cocos: `cocos/animation/skeletal-animation-utils.ts` — IJointTransform

**源码对照：**
- Cocos: `cocos/3d/skeletal-animation/skeletal-animation.ts` — SkeletalAnimation 组件
- Godot: `Skeleton2D` / `Bone2D` 节点设计

#### D2: 蒙皮（Skinning）
**理论：**
- 蒙皮权重：每个顶点最多受 4 个骨骼影响
- LBS（线性混合蒙皮）：finalPos = Σ weight[i] * (boneWorld[i] * invBind[i]) * restPos
- 2D 蒙皮比 3D 简单：旋转是单角度，LBS 够用
- CPU 蒙皮 vs GPU 蒙皮：CPU 上传变形顶点 vs GPU 上传骨骼矩阵

**GPU 蒙皮着色器（WGSL）：**
```wgsl
struct BoneMatrices { matrices: array<mat3x3<f32>, 128> };
@group(0) @binding(0) var<uniform> bones: BoneMatrices;

@vertex fn vs_main(input: VertexInput) -> @builtin(position) vec4<f32> {
    var skin = mat3x3<f32>();
    skin = input.weights.x * bones.matrices[input.bones.x]
         + input.weights.y * bones.matrices[input.bones.y];
    let pos = skin * vec3<f32>(input.position, 1.0);
    return vec4<f32>(pos.xy, 0.0, 1.0);
}
```

**源码对照：**
- Cocos: `cocos/3d/skinned-mesh-renderer/skinned-mesh-renderer.ts` — SkinnedMeshRenderer
- Unity: SpriteSkin 组件 + GPU Deformation 路径

#### D3: 动画剪辑与关键帧插值
**理论：**
- AnimationClip：tracks → channels → curves → keyframes
- 关键帧插值：Step / Linear / Cubic Hermite / Bezier
- Catmull-Rom 样条：自动切线计算
- Spine 格式：bones.rotate[] / bones.translate[] / bones.scale[]
- DragonBones 格式：tweenEasing 曲线

**源码对照：**
- Cocos: `cocos/animation/animation-clip.ts` — Track 系统
- Spine: JSON 格式的 bones/animations 结构

#### D4: 动画混合与状态机
**理论：**
- 交叉淡入（Crossfade）：lerp(poseA, poseB, alpha)
- 角度插值：最短路径 lerpAngle
- 叠加混合（Additive）：delta = sample - reference
- 层混合（Layer Blend）：权重 + 遮罩
- 动画状态机（ASM）：State / Transition / Condition
- Marionette 系统：AnimationGraph + PoseGraph

**源码对照：**
- Cocos: `cocos/animation/marionette/animation-controller.ts` — Marionette 控制器
- Cocos: `cocos/3d/skeletal-animation/skeletal-animation-blending.ts` — 混合系统
- Unity: Animator + AnimationClip + AnimatorController

#### D5: 网格变形与 IK
**理论：**
- 2D 网格变形：三角网格 + UV + 骨骼权重
- Spine Mesh Attachment：10-50 三角形/sprite 区域
- Sprite 换装：每个骨骼挂载独立 sprite
- IK（逆运动学）：CCD / FABRIK 算法
- 2D 两点 IK 解析解：余弦定理

**源码对照：**
- Spine: SkeletonRenderer — 遍历 slots，region attachment / mesh attachment
- Cocos: `cocos/spine/assembler/` — Spine 网格转 2D 批次
- Cocos: `cocos/spine/skeleton.ts` — Spine 组件

### 周末任务
- [ ] 竞品骨骼动画对标：Unity 2D Animation / Cocos Spine / Godot Skeleton2D
- [ ] 画出骨骼动画数据流图：Clip → Sample → Blend → FK → Skinning → Render
- [ ] 更新 `progress/progress.md`

---

## 第 4 周：引擎架构设计 + Cocos 源码深度分析

### 目标
输出自己的引擎架构设计文档，对标 miu2d 和 Cocos。

### 每日计划

#### D1: Cocos GFX 抽象层深度分析
**理论：** GFX 如何统一 WebGL/WebGPU/Vulkan/Metal
**源码对照：**
- `cocos/gfx/base/device.ts` — 抽象工厂
- `cocos/gfx/base/buffer.ts` / `texture.ts` / `shader.ts` — 资源抽象
- `cocos/gfx/base/pipeline-state.ts` — Pipeline 抽象

#### D2: Cocos 渲染管线深度分析
**理论：** RenderPipeline → RenderFlow → RenderStage 三级架构
**源码对照：**
- `cocos/rendering/render-pipeline.ts` — 管线核心
- `cocos/rendering/forward/forward-pipeline.ts` — 前向管线
- `cocos/rendering/custom/` — 自定义渲染图

#### D3: Cocos 骨骼动画系统深度分析
**理论：** SkeletalAnimation + SkinnedMeshRenderer + Marionette 完整链路
**源码对照：**
- `cocos/3d/skeletal-animation/skeletal-animation.ts`
- `cocos/3d/skinned-mesh-renderer/skinned-mesh-renderer.ts`
- `cocos/animation/marionette/animation-controller.ts`

#### D4: 游戏编辑器 + 脚本系统
**理论：** 编辑器架构、脚本系统设计、资源管线
**源码对照：**
- miu2d: `dashboard/` 13 个编辑模块
- miu2d: `script/` 双执行模型（DSL + Lua）

#### D5: 自己的架构设计文档
**输出：** 完整的引擎架构设计文档，包含：
- 技术选型：WebGPU + WGSL + Rust/WASM + ECS
- 渲染管线：实例化渲染 + 纹理图集 + Compute Shader
- 骨骼动画系统：骨骼层级 + 蒙皮 + 状态机 + Spine 兼容
- 编辑器设计：React + WebGPU 实时预览
- 与 miu2d 和竞品的差异分析

### 周末任务
- [ ] 完善架构设计文档
- [ ] 更新 `progress/progress.md`

---

## 关键源码文件索引

### Cocos Creator（需要 clone）

| 模块 | 路径 |
|---|---|
| GFX 基类 | `cocos/gfx/base/device.ts`, `buffer.ts`, `texture.ts`, `shader.ts`, `pipeline-state.ts` |
| WebGPU 后端 | `cocos/gfx/webgpu/webgpu-device.ts` (24 files) |
| 渲染管线 | `cocos/rendering/render-pipeline.ts`, `render-flow.ts`, `render-stage.ts` |
| 2D 批处理器 | `cocos/2d/renderer/batcher-2d.ts` |
| 2D 渲染数据 | `cocos/2d/renderer/render-data.ts` |
| 骨骼动画 | `cocos/3d/skeletal-animation/skeletal-animation.ts` |
| 蒙皮渲染器 | `cocos/3d/skinned-mesh-renderer/skinned-mesh-renderer.ts` |
| 动画剪辑 | `cocos/animation/animation-clip.ts` |
| 动画状态 | `cocos/animation/animation-state.ts` |
| 蒙皮工具 | `cocos/animation/skeletal-animation-utils.ts` |
| Marionette | `cocos/animation/marionette/animation-controller.ts` |
| Spine 集成 | `cocos/spine/skeleton.ts`, `cocos/spine/assembler/` |

### miu2d（已 clone）

| 模块 | 路径 |
|---|---|
| WebGL 渲染器 | `packages/engine/src/renderer/webgl-renderer.ts` |
| 着色器 | `packages/engine/src/renderer/shaders.ts` |
| 批处理器 | `packages/engine/src/renderer/sprite-batcher.ts` |
| 状态缓存 | `packages/engine/src/renderer/gl-state-cache.ts` |
| 精灵系统 | `packages/engine/src/sprite/sprite.ts` |
| 角色状态机 | `packages/engine/src/character/character.ts` |
| 寻路（Rust） | `packages/engine-wasm/src/pathfinder.rs` |
| 碰撞（Rust） | `packages/engine-wasm/src/collision.rs` |
