# Project Context

2D 网页游戏引擎学习项目。用户是前端开发者，目标转型游戏引擎开发。

## 当前状态

- 学习阶段：第 1 周 — WebGPU 渲染管线
- 参考项目：miu2d（已 clone 到 `demos/miu2d/`）、Cocos Creator（需 clone）
- 进度文件：`progress/progress.md`

## 重要：方向变更

- ~~WebGL~~ → **WebGPU**：现代 GPU 编程方向
- ~~帧动画~~ → **骨骼动画**：核心能力，重中之重
- 新增 **Cocos Creator 源码分析**：GFX 抽象层、骨骼动画系统、Marionette 状态机

## 学习工作流

当用户说"开始学习"或"继续学习"时：

### 1. 确认当前进度
读取 `progress/progress.md`，找到下一个未完成的知识点。

### 2. 准备源码
- miu2d: 已在 `demos/miu2d/`
- Cocos Creator: 需要时 clone `https://github.com/cocos/cocos-engine.git` 到 `demos/cocos-engine/`

### 3. 生成 lessons.json
从源码中读取当天涉及的文件，组织成 `viewer/lessons.json` 格式：

```json
{
  "title": "W1D1: WebGPU 基础",
  "chapters": [{ "id": "...", "title": "...", "sections": [...] }],
  "files": { "path/to/file.ts": "完整内容..." }
}
```

section 类型：heading / paragraph / code-reference / table / list / code-block / note

### 4. 启动查看器
```bash
lsof -ti:8765 | xargs kill -9 2>/dev/null
cd ~/game-engine-learning/viewer && python3 -m http.server 8765 &
open http://localhost:8765
```

### 5. 确认完成后更新进度
用户说"学完了"后：更新 `progress/progress.md`，写笔记到 `docs/week-X/day-X.md`，git commit + push。

## 学习原则

- 理解原理比写代码重要（AI 辅助编码时代）
- 每个理论模块立刻用源码印证（miu2d + Cocos）
- 竞品对标：Cocos、Unity、Godot、PixiJS、Spine
- 用户确认学完才更新进度，不要自动标记

## 关键资源

- WebGPU: https://webgpufundamentals.org/
- WGSL: https://google.github.io/tour-of-wgsl/
- Rust: https://doc.rust-lang.org/book/
- wasm-bindgen: https://rustwasm.github.io/docs/wasm-bindgen/
- Spine Runtime: http://esotericsoftware.com/spine-runtime-guide
- Game Programming Patterns: https://gameprogrammingpatterns.com/
- Red Blob Games: https://www.redblobgames.com/

## 4 周学习路线

| 周 | 主题 | 交付物 |
|---|---|---|
| W1 | WebGPU 渲染管线 | WebGPU 实例化渲染 demo |
| W2 | Rust + WASM + WebGPU 集成 | Rust→WASM demo |
| W3 | 骨骼动画系统（核心） | 骨骼动画数据流图 |
| W4 | 引擎架构设计 + Cocos 源码分析 | 完整架构设计文档 |

详见 `docs/roadmap.md`
