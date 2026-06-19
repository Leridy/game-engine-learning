# D1: WebGL 基础

> 日期：2026-06-20
> 状态：✅ 已完成

## 理论笔记

### 核心概念

- **WebGL 是什么：** 浏览器对 OpenGL ES 2.0/3.0 的绑定，直接操作 GPU 画图
- **状态机模型：** WebGL 是一个状态机，你设置状态（着色器、缓冲区、纹理、混合模式），然后调用 drawArrays/drawElements 画一次
- **上下文获取：** `canvas.getContext("webgl2", options)` 返回 GL 上下文
- **着色器程序：** 顶点着色器（位置变换）+ 片段着色器（像素颜色）= 一个 WebGLProgram
- **绘制调用（draw call）：** `gl.drawArrays()` / `gl.drawElements()` — CPU 告诉 GPU "画"
- **批处理：** 不立刻画，攒够了一起画，减少 CPU→GPU 通信次数

### 关键 API

```javascript
// 获取上下文
const gl = canvas.getContext("webgl2", { alpha: false, antialias: false });

// 编译着色器
const vs = gl.createShader(gl.VERTEX_SHADER);
gl.shaderSource(vs, vertexShaderSource);
gl.compileShader(vs);

// 创建程序
const program = gl.createProgram();
gl.attachShader(program, vs);
gl.attachShader(program, fs);
gl.linkProgram(program);

// 使用程序
gl.useProgram(program);

// 绘制
gl.drawArrays(gl.TRIANGLES, 0, vertexCount);
```

## miu2d 对照

### webgl-renderer.ts 分析

**上下文创建参数（第 146-152 行）：**
- `alpha: false` — 不需要透明背景，全屏游戏场景
- `antialias: false` — 像素风格，抗锯齿反而模糊
- `premultipliedAlpha: true` — 预乘 alpha 混合更正确
- `preserveDrawingBuffer: true` — 保留帧缓冲用于截图
- `powerPreference: "high-performance"` — 用独立显卡

**状态设置（第 182-200 行）：**
- `gl.enable(gl.BLEND)` — 开启透明度混合
- `gl.blendFunc(SRC_ALPHA, ONE_MINUS_SRC_ALPHA)` — 标准 alpha 混合
- `gl.disable(gl.DEPTH_TEST)` — 2D 不需要深度测试
- `gl.disable(gl.CULL_FACE)` — 2D 不需要面剔除

**帧控制：**
- `beginFrame()` — 清屏、重置统计
- `endFrame()` — flush 所有待绘制批次、汇总统计
- 关键理解：`drawSprite()` 不立刻画，攒到 `flush()` 一起画

**混合模式：**
- normal: `SRC_ALPHA, ONE_MINUS_SRC_ALPHA` — 标准半透明
- multiply: `DST_COLOR, ZERO` — 正片叠底（变暗）
- additive: `SRC_ALPHA, ONE` — 叠加（变亮，火焰/光效）
- screen: `ONE, ONE_MINUS_SRC_COLOR` — 滤色（更柔和的变亮）

**纹理管理：**
- `NEAREST` 过滤 — 像素风格必须用
- `CLAMP_TO_EDGE` — 防止边缘渗色
- `texSubImage2D` — 增量上传，避免 GPU 重新分配内存
- `FinalizationRegistry` — GC 时自动清理 GPU 纹理

## 竞品参考

- PixiJS 也用类似的状态机模型，但自动处理批处理
- Cocos Creator 的 GFX 抽象层把 WebGL 和 Vulkan 统一到一个 API
- Godot 的 RenderingServer 把渲染命令抽象成独立的服务器

## 疑问 & 待深入

- [x] WebGL2 和 WebGL1 的主要区别？（miu2d 用 WebGL2 但有 WebGL1 兼容代码）
- [ ] 着色器的具体语法？（D2 深入）
- [ ] 批处理的具体实现？（D4 深入）

## 今日总结

WebGL 是一个状态机：设置状态 → 发出绘制命令。miu2d 的 `WebGLRenderer` 是整个渲染系统的核心，负责上下文初始化、纹理管理、混合模式、帧控制。批处理是性能关键 — `drawSprite()` 只是把数据加入队列，`flush()` 时才真正画到屏幕。4 种混合模式对应不同的视觉效果。
