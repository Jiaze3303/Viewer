# STEP & DXF 在线查看器 — 项目总结

## 项目概述

纯前端 3D/2D CAD 图纸在线查看器，单 HTML 文件，浏览器直接打开，无需服务器。

**在线地址**：https://jiaze3303.github.io/Viewer/
**仓库**：https://github.com/Jiaze3303/Viewer
**核心文件**：`index.html`（约 65KB）

---

## 已实现功能

### 3D 模型查看（STEP / STP / IGES / BREP）
- 使用 **occt-import-js**（OpenCascade WASM）在浏览器端解析
- 四种视图模式：实体、线框、X-Ray、线面混合
- 零件拆解视图、透明度调节
- 正视/侧视/俯视一键切换
- 自动旋转、边缘线、网格显示开关
- 零件列表（悬停高亮、点击聚焦）
- 重播动画按钮（跟随当前视图材质）
- 导出 GLTF 格式
- 入场飞入动画
- 大模型自适应相机（near/far/minDistance/maxDistance 根据模型大小自动调整）
- 鼠标悬停显示零件尺寸、顶点数、三角面数、点击坐标、距离

### 2D 图纸查看（DXF / DWG）
- 使用 **dxf-parser** 解析 DXF
- 使用 **libredwg-web**（WASM）将 DWG 转换为 DXF
- GBK / UTF-8 编码自动识别（中文 DXF 兼容）
- 支持实体：LINE、LWPOLYLINE、POLYLINE、ARC、CIRCLE、ELLIPSE、SPLINE、SOLID、3DFACE
- AutoCAD 颜色索引（ACI）256 色完整映射
- GPU 加粗线渲染（Three.js Line2，linewidth 1.0，悬停 5.0）
- 图层列表（点击切换显隐）
- 悬停高亮变红 + 浮窗信息

### 多文件管理
- 同时打开多个文件
- 左侧栏显示文件列表（160px 窄栏）
- 点击切换，保留相机位置
- 3D/2D 类型标签（红/蓝）
- 单独关闭、继续添加

### 通用功能
- 拖放/点击上传
- 4 阶段进度条
- 方向键平移（↑↓←→）
- Shift+左键平移
- 浅色技术图纸风格（anime.js 官网风格）

---

## 技术栈

| 技术 | 用途 | 版本 |
|------|------|------|
| Three.js | WebGL 3D/2D 渲染 | r170 |
| occt-import-js | STEP/IGES/BREP 解析（WASM） | 0.0.23 |
| dxf-parser | DXF 文件解析 | 1.1.2 |
| libredwg-web | DWG → DXF 转换（WASM） | 0.7.9 |
| Line2 / LineGeometry / LineMaterial | GPU 加粗线渲染 | Three.js 内置 |
| OrbitControls | 相机交互控制 | Three.js 内置 |
| GLTFLoader / GLTFExporter | GLTF 加载/导出 | Three.js 内置 |

所有依赖通过 jsDelivr CDN 加载。

---

## 已知问题 & 待优化

1. **3D 零件颜色**：所有零件统一浅灰色（#C8C4BF），没有根据 STEP 原始颜色区分
2. **DXF 文字**：MTEXT/TEXT 实体暂未渲染
3. **大型文件**：超过 50MB 的 STEP 可能因 WASM 内存限制失败
4. **GitHub Pages**：token 不能贴在聊天里，GitHub 会自动撤销

---

## 文件结构

```
workspace/
├── index.html              ← 核心文件（从 STEP-3D-Viewer.html 复制）
├── STEP-3D-Viewer.html     ← 开发版本（与 index.html 同步）
├── README.md               ← 使用说明
├── PROJECT_SUMMARY.md      ← 本文件
└── .openclaw/tmp/3d-viewer/ ← 开发临时文件
```

---

## 开发历程

1. 用户上传 STEP 文件（MV-ID5050XM 工业相机模组，16MB，93 零件）
2. 用 occt-import-js（Node.js）将 STEP 转换为 GLTF
3. 构建 Three.js 查看器，参考 anime.js 官网风格
4. 配色演变：深色紫色 → 海康橙色 → 最终浅色技术图纸风格
5. STEP 解析搬到浏览器端（WASM），做成通用工具
6. 添加 DXF 2D 图纸支持（dxf-parser）
7. 添加 DWG 支持（libredwg-web WASM 转 DXF）
8. 添加悬停测量、加粗渲染（Line2）
9. 添加多文件管理（左侧栏切换）
10. 大模型自适应相机、方向键平移、重播动画
11. 部署到 GitHub Pages

---

## 后续开发建议

- **增强 3D 零件颜色**：从 OCCT result 的 mesh.color 提取
- **DXF 文字渲染**：处理 TEXT/MTEXT 实体
- **测量工具**：两点距离、角度测量
- **剖面视图**：3D 模型剖切展示
- **Web Worker 解析**：避免大文件阻塞主线程
