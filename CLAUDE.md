# Project Context

这是一个 2D 网页游戏引擎的学习项目。用户是前端开发者，目标是转型做游戏引擎开发。

## 当前状态

- 学习阶段：第 1 周 — WebGL 渲染管线
- 参考项目：https://github.com/luckyyyyy/miu2d（已 clone 到 `demos/miu2d/`）
- 进度文件：`progress/progress.md`
- 架构设计文档：`docs/specs/`

## 学习工作流

当用户说"开始学习"或"继续学习"时，按以下流程操作：

### 1. 确认当前进度

```bash
# 读取进度文件，找到下一个未完成的知识点
cat progress/progress.md
```

### 2. 获取源码并结构化

从 `demos/miu2d/` 中读取当天涉及的源文件，组织成 `viewer/lessons.json` 格式：

```json
{
  "title": "W1D2: GLSL 着色器",
  "chapters": [
    {
      "id": "section-id",
      "title": "章节标题",
      "sections": [
        { "type": "heading", "level": 2, "text": "标题" },
        { "type": "paragraph", "text": "讲解内容..." },
        { "type": "code-reference", "file": "path/to/file.ts", "startLine": 10, "endLine": 20, "caption": "说明", "highlights": [10, 11, 12] },
        { "type": "table", "headers": ["列1", "列2"], "rows": [["值1", "值2"]] },
        { "type": "list", "items": ["要点1", "要点2"] },
        { "type": "code-block", "language": "typescript", "code": "const x = 1;" },
        { "type": "note", "text": "关键提示" }
      ]
    }
  ],
  "files": {
    "path/to/file.ts": "文件完整内容..."
  }
}
```

**关键规则：**
- `files` 字段嵌入源文件完整内容（确保 code-reference 的行号准确）
- `code-reference` 的 `startLine`/`endLine` 必须与嵌入的文件内容对应
- 文档内容用中文，代码注释保留原文
- 每个 lesson 覆盖一天的学习内容

### 3. 启动查看器

```bash
# 杀掉旧的 HTTP 服务（如果有）
lsof -ti:8765 | xargs kill -9 2>/dev/null

# 启动 HTTP 服务
cd ~/game-engine-learning/viewer && python3 -m http.server 8765 &

# 打开浏览器
open http://localhost:8765
```

### 4. 用户学习

用户通过浏览器学习，左侧文档右侧代码，点击代码引用可跳转定位。

### 5. 确认完成后更新进度

用户说"学完了"或"可以更新进度"后：
1. 更新 `progress/progress.md` 中对应行的状态（🔲 → ✅）
2. 将学习笔记写入 `docs/week-X/day-X.md`
3. `git commit && git push`

## 查看器技术说明

- 单文件 HTML：`viewer/index.html`
- 数据文件：`viewer/lessons.json`
- 需要 HTTP 服务（`file://` 协议有 CORS 限制）
- Prism.js 从 CDN 加载（TypeScript/JavaScript/Rust/GLSL/C 语法高亮）
- 进度持久化到 localStorage

## 学习原则

- 理解原理比写代码重要（AI 辅助编码时代）
- 每个理论模块立刻用 miu2d 源码印证
- 竞品对标不能少：Unity、Cocos、Godot、Phaser、PixiJS
- 用户确认学完才更新进度，不要自动标记

## 关键资源

- miu2d 仓库：https://github.com/luckyyyyy/miu2d
- WebGL 教程：https://webgl2fundamentals.org/
- Rust Book：https://doc.rust-lang.org/book/
- wasm-bindgen：https://rustwasm.github.io/docs/wasm-bindgen/
- Game Programming Patterns：https://gameprogrammingpatterns.com/
- Red Blob Games：https://www.redblobgames.com/

## 4 周学习路线

| 周 | 主题 | 交付物 |
|---|---|---|
| W1 | WebGL 渲染管线 | 渲染管线笔记 + 最小 WebGL demo |
| W2 | Rust + WebAssembly | Rust/WASM 笔记 + Rust→WASM demo |
| W3 | 游戏引擎核心架构 | 引擎架构笔记 + miu2d 模块依赖图 |
| W4 | 编辑器 + 系统设计 + 竞品对标 | 完整架构设计文档 |

详见 `docs/roadmap.md`
