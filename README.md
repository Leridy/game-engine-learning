# 2D Web Game Engine Learning Project

从零开始学习 2D 网页游戏引擎开发，目标是具备独立设计和实现游戏引擎 + 编辑器的能力。

## 目标

- 1 个月高强度学习核心技术原理（WebGL / Rust / WASM / 游戏引擎架构）
- 3 个月产出可演示的 2D 游戏引擎 + 编辑器原型
- 能力对标 [miu2d](https://github.com/luckyyyyy/miu2d)，架构更规范

## 学习路线

详见 [docs/roadmap.md](docs/roadmap.md)

| 周 | 主题 | 状态 |
|---|---|---|
| W1 | WebGL 渲染管线 | 🔲 未开始 |
| W2 | Rust + WebAssembly | 🔲 未开始 |
| W3 | 游戏引擎核心架构 | 🔲 未开始 |
| W4 | 编辑器 + 系统设计 + 竞品对标 | 🔲 未开始 |

## 项目结构

```
game-engine-learning/
├── CLAUDE.md                        # Claude Code 上下文文件
├── docs/
│   ├── roadmap.md                   # 完整学习路线
│   ├── week-1-webgl/                # W1: WebGL 渲染管线
│   ├── week-2-rust-wasm/            # W2: Rust + WASM
│   ├── week-3-engine-arch/          # W3: 引擎架构
│   ├── week-4-editor-design/        # W4: 编辑器 + 设计
│   ├── competitive-analysis/        # 竞品分析
│   └── specs/                       # 架构设计文档
├── progress/                        # 学习进度记录
│   └── progress.md                  # 总进度看板
└── demos/                           # 学习用的 demo 代码
```

## 竞品参考

- [miu2d](https://github.com/luckyyyyy/miu2d) — 2D ARPG 引擎，TypeScript + Rust + WASM
- Unity 2D — URP 渲染管线、DOTS/ECS
- Cocos Creator — GFX 抽象层、Batcher2D
- Godot 2D — 场景树、独立 2D 引擎
- Phaser — Web 原生、多场景架构
- PixiJS — 纯渲染库、自动批处理
