# W3: 骨骼动画系统（核心模块）

> 重中之重 — 骨骼动画是现代 2D 游戏引擎的核心能力

## 学习目标

1. 深入理解骨骼动画的每一层：骨骼层级 → FK → 蒙皮 → 动画混合 → 状态机
2. 理解 LBS 蒙皮公式和 GPU 蒙皮着色器
3. 理解动画状态机和 Crossfade 混合
4. 理解网格变形和 IK

## 每日笔记

- [Day 1: 骨骼动画基础](./day-1.md)
- [Day 2: 蒙皮（Skinning）](./day-2.md)
- [Day 3: 动画剪辑与关键帧插值](./day-3.md)
- [Day 4: 动画混合与状态机](./day-4.md)
- [Day 5: 网格变形与 IK](./day-5.md)
- [周末: 竞品骨骼动画对标](./weekend.md)

## 知识自检

- [ ] 正向运动学公式？
- [ ] LBS 蒙皮完整公式？
- [ ] CPU vs GPU 蒙皮的区别？
- [ ] Crossfade 如何处理角度插值？
- [ ] 动画状态机 Transition 如何触发？
- [ ] FABRIK vs CCD IK 的核心区别？

## 关键源码

| 文件 | 关注点 |
|---|---|
| Cocos: `3d/skeletal-animation/skeletal-animation.ts` | SkeletalAnimation 组件 |
| Cocos: `3d/skinned-mesh-renderer/skinned-mesh-renderer.ts` | SkinnedMeshRenderer |
| Cocos: `animation/animation-clip.ts` | Track 系统 |
| Cocos: `animation/skeletal-animation-utils.ts` | IJointTransform |
| Cocos: `animation/marionette/animation-controller.ts` | Marionette 状态机 |
| Cocos: `3d/skeletal-animation/skeletal-animation-blending.ts` | 混合系统 |
| Cocos: `cocos/spine/skeleton.ts` | Spine 集成 |
