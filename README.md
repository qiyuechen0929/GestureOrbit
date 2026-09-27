# GestureOrbit · 手势全向卡牌环

![banner](banner.svg)


> 纯前端 · 无构建 · 单文件 WebAR 手势交互应用

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Three.js](https://img.shields.io/badge/Three.js-r128-000000.svg)](https://threejs.org/)
[![MediaPipe](https://img.shields.io/badge/MediaPipe-Hands-00e5ff.svg)](https://developers.google.com/mediapipe)
[![Status](https://img.shields.io/badge/Status-Demo-green.svg)](#)

## 简介

`GestureOrbit` 是一个基于浏览器摄像头的**手势驱动的 3D 全向交互应用**。它将你的手掌变成一个"全向摇杆"：

- **手掌移动** → 控制 3D 卡牌环的左右旋转与上下倾斜
- **握拳手势** → 磁吸锁定当前卡牌，前移放大并翻面查看

项目采用 **MediaPipe Hands** 完成实时手部关键点追踪，**Three.js** 负责 3D 渲染，搭配科幻风格的 HUD 界面、霓虹光效与粒子星空，全部代码压缩在**单个 HTML 文件**中，无需任何构建工具即可运行。

## 功能特性

- ✋ **掌上全向摇杆**：基于手掌 XY 位置的二维摇杆控制，带死区与速度平滑，操控细腻
- 🎯 **磁吸对齐**：松手后自动吸附到最近的卡牌，环形浏览停靠精准
- 🤛 **握拳聚焦**：握拳将当前卡牌拉近放大并翻面显示详情，松拳恢复环绕浏览
- 🃏 **程序化卡牌纹理**：14 张卡牌完全由 Canvas 动态生成，无需任何外部图片资源
- 🕹️ **科幻 HUD 界面**：顶部状态徽章、四向方向指示、实时手势反馈视窗
- 🌌 **沉浸式视觉**：粒子星空、指数雾效、霓虹光环，赛博氛围拉满
- ⚡ **零构建单文件**：一个 HTML 搞定全部逻辑与样式，复制即用

## 技术栈

| 技术 | 用途 |
| --- | --- |
| [MediaPipe Hands](https://developers.google.com/mediapipe) | 实时手部关键点追踪、握拳识别 |
| [Three.js r128](https://threejs.org/) | 3D 场景构建与渲染 |
| Canvas 2D | 程序化纹理生成（卡牌正反面） |
| 原生 HTML / CSS / JS | 界面与交互逻辑 |

## 快速开始

### 方式一：直接打开

将 `index.html` 下载到本地，**双击用浏览器打开**即可。

> ⚠️ **注意**：手势识别依赖摄像头，请使用 **HTTPS** 环境或本地文件访问，并授予浏览器摄像头权限。部分浏览器要求本地文件（`file://`）环境下摄像头可用，若无法调用，请使用方式二。

### 方式二：本地服务器运行

```bash
# Python
python -m http.server 8080

# 或 Node.js
npx serve .

# 然后访问
open http://localhost:8080
```

### 方式三：在线部署

将 `index.html` 直接拖入任意静态托管平台（GitHub Pages、Vercel、Netlify、Cloudflare Pages 等）即可上线。

## 使用说明

1. **激活**：页面加载后举起手掌，等待右上角手势视窗出现手部骨架线框
2. **旋转 / 倾斜**：手掌在画面中移动，卡牌环跟随转动——左右移动控制旋转，上下移动控制俯仰
3. **聚焦查看**：**握拳**锁定当前正前方的卡牌，卡牌会前移放大并翻面显示详情；松开手掌返回环绕浏览
4. **待机**：手掌静止在屏幕中央死区内，视角自动磁吸对齐最近的卡牌

> 界面底部与顶部徽章会实时提示当前状态（`待机中` / `调整视角中` / `正在读取` 等）。

## 项目结构

```
gesture-orbit/
└── index.html   # 全部代码（样式 + 逻辑 + 3D 场景），单文件即项目
```

## 核心机制

```
摄像头输入
   ↓ MediaPipe Hands 手部关键点追踪
   ↓ 提取手掌中心 (x, y) + 握拳判定
   ↓ 映射为全向摇杆输入
   ├─ 左右偏移 → 卡牌环 Y 轴旋转（带速度平滑 + 磁吸）
   ├─ 上下偏移 → 卡牌环 X 轴俯仰（带角度限制）
   └─ 握拳     → 聚焦当前卡牌：前移 + 放大 + 翻面
   ↓ Three.js 渲染 → 屏幕
```

## 自定义配置

所有可调参数集中在 `CONFIG` 对象中：

```js
const CONFIG = {
    cardCount: 14,       // 卡牌数量
    ringRadius: 7.0,     // 环形半径
    deadZone: 0.12,      // 摇杆死区半径
    maxSpeedX: 0.05,     // 左右最大转速
    maxSpeedY: 0.02,     // 上下最大转速
    minRotX: -0.4,       // 上仰角度限制（弧度）
    maxRotX: 0.4,        // 下俯角度限制（弧度）
    snapSpeed: 0.1,      // 磁吸吸附速度
    inspectScale: 1.1,   // 聚焦放大倍率
    inspectPull: 2.0     // 聚焦前移距离
};
```

## 浏览器兼容性

- 支持 WebGL 与 `getUserMedia` 的现代浏览器（Chrome / Edge / Firefox / Safari）
- 建议在**桌面端**使用以获得最佳追踪体验
- 移动端可通过 HTTPS 访问，但需注意性能表现

## License

[MIT](LICENSE) © 2025 GestureOrbit Contributors

## 致谢

- [MediaPipe](https://developers.google.com/mediapipe) — 强大的跨平台机器学习解决方案
- [Three.js](https://threejs.org/) — 易用的 3D JavaScript 库

## 作者

ChenQiyue

---

最后更新：2026-08-06
