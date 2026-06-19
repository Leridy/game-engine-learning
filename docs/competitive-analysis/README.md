# 竞品分析

对比维度：渲染架构、批处理策略、编辑器设计、脚本系统、ECS 支持、Web 适配

## 竞品对标矩阵

| 维度 | Unity 2D | Cocos Creator | Godot 2D | Phaser | PixiJS | miu2d |
|---|---|---|---|---|---|---|
| 渲染后端 | URP (GL/VK/Metal) | GFX 抽象层 | RenderingServer | PixiJS | WebGL2/WebGPU | 原生 WebGL2 |
| 2D 渲染模型 | 独立 2D 管线 | 独立 2D 管线 | 独立 2D 引擎 | PixiJS 批处理 | 批处理渲染器 | 自研批处理 |
| 批处理策略 | 材质/纹理排序 | Batcher2D | Canvas 命令合并 | 自动（via PixiJS） | 自动 | 多纹理 8 槽位 |
| 编辑器 | 完整 IDE | Electron IDE | 引擎原生 IDE | 无（纯代码） | 无（库） | React 13 模块 |
| 脚本语言 | C# (Burst) | TypeScript/JS | GDScript/C#/C++ | JS/TS | N/A | DSL + Lua 5.4 |
| ECS 支持 | DOTS 并行 ECS | ECS 启发 | 组件式 | 无 | 无 | 无（OOP 继承） |
| 2D 光照 | URP 2D Lights | 基础 | 内置 2D 光照 | 插件 | N/A | 无 |
| Tilemap | 内置 + 扩展 | 内置 | 内置 + 地形 | Tiled 集成 | N/A | 自研等距 |
| 物理 | Box2D | Box2D | Godot/Jolt | Arcade+Matter.js | N/A | 空间哈希 |
| 平台 | 40+ (原生+Web) | 原生+Web+小程序 | 原生+Web+编辑器 | Web only | Web only | Web only |
| Web 适配度 | 低 | 中 | 中 | 高 | 高 | 高 |

## 详细分析

- [Unity 2D 分析](./unity.md)
- [Cocos Creator 分析](./cocos.md)
- [Godot 2D 分析](./godot.md)
- [Phaser 分析](./phaser.md)
- [PixiJS 分析](./pixijs.md)
- [miu2d 分析](./miu2d.md)

## 关键收获

### 从 Unity 学到的
- Sorting Layer + Order in Layer 直觉化的深度排序
- Sprite Atlas 构建时优化对批处理至关重要
- ScriptableRenderPass 注入模型给高级用户扩展能力
- DOTS/ECS 是数据密集型游戏逻辑的性能天花板

### 从 Cocos Creator 学到的
- GFX 抽象层是多后端支持的正确架构
- Batcher2D 作为独立的 2D 批处理器是干净的设计
- `.effect` 着色器格式（JSON + GLSL）支持跨平台编译
- 预分配池化的 RenderData 避免 GC 压力

### 从 Godot 学到的
- "一切皆场景"的组合模式创造极致的可复用性
- 独立 2D 引擎（不是 3D 的扁平化）架构更优
- Signals 作为解耦通信模式避免紧耦合
- 编辑器用引擎自身系统构建 → dogfooding + 减少维护

### 从 Phaser 学到的
- 多场景架构支持同时堆叠多个游戏状态
- SpriteGPULayer 证明百万级 sprite 在单次 draw call 中可行
- 与 React/Vue 集成降低 Web 开发者门槛

### 从 PixiJS 学到的
- 统一的 WebGL/WebGPU 渲染器抽象是面向未来的架构
- 自动批处理对开发者透明是可用性的关键
- 纯渲染层（不含游戏逻辑）创造最大灵活性
- RenderGroup 缓存静态子树大幅减少每帧工作量
