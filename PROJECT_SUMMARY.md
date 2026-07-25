# STEP & DXF 在线查看器 — 项目总结

## 项目概述

一个纯前端的 3D/2D CAD 图纸在线查看器，单 HTML 文件，浏览器直接打开，无需服务器。

**在线地址**：https://jiaze3303.github.io/Viewer/
**仓库**：https://github.com/Jiaze3303/Viewer
**核心文件**：`index.html`（约 57KB）

---

## 已实现功能

### 3D 模型查看（STEP / STP / IGES / BREP）
- 使用 **occt-import-js**（OpenCascade WASM）在浏览器端解析 STEP 文件
- 四种视图模式：实体、线框、X-Ray、线面混合
- 零件拆解视图（滑块控制散开距离）
- 透明度调节
- 正视/侧视/俯视一键切换
- 自动旋转、边缘线、网格显示开关
- 零件列表（悬停高亮、点击聚焦）
- 导出 GLTF 格式
- 入场飞入动画
- 鼠标悬停显示零件尺寸（宽×高×深）、顶点数、三角面数、点击坐标、距离

### 2D 图纸查看（DXF / DWG）
- 使用 **dxf-parser** 库解析 DXF 文件
- 使用 **libredwg-web**（WASM）将 DWG 转换为 DXF 后渲染
- GBK / UTF-8 编码自动识别（支持中文 DXF）
- 支持实体类型：LINE、LWPOLYLINE、POLYLINE、ARC、CIRCLE、ELLIPSE、SPLINE、SOLID、3DFACE
- AutoCAD 颜色索引（ACI）完整映射（256 色）
- GPU 加粗线渲染（Three.js Line2，linewidth 1.0，悬停时加粗到 5.0）
- 图层列表（点击切换显隐）
- 悬停高亮变红 + 浮窗显示：实体类型、图层、长度、半径/直径、面积、角度、坐标

### 通用功能
- 拖放或点击上传
- 4 阶段进度条（读取文件 → 加载引擎 → 解析 CAD → 构建场景）
- 浅色技术图纸风格界面（anime.js 官网风格：暖米灰底 #E8E4DF，红色强调 #E53935）
- 鼠标操作：左键旋转、右键平移、滚轮缩放
- 触屏操作：单指旋转、双指缩放平移
- 2D 模式自动切换正交视图，左键直接平移
- 重新上传 3D 文件自动恢复透视视图

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

所有依赖通过 jsDelivr CDN 加载，无需 npm install。

---

## 已知问题 & 待优化

1. **3D 模型材质**：目前所有零件统一浅灰色（#C8C4BF），没有根据 STEP 文件中的原始颜色区分零件。需要从 OCCT 解析结果中提取每个零件的颜色并应用。
2. **DXF 标注**：已实现但后来移除了自动工程标注（尺寸线+箭头），因为效果不够专业。如果需要可以重新加。
3. **大型文件性能**：超过 50MB 的 STEP 文件可能因 WASM 内存限制导致解析失败。
4. **DXF 文字**：MTEXT/TEXT 实体暂未渲染，只渲染几何图形。
5. **GitHub Pages 部署**：已配置但可能需要手动在 Settings → Pages 确认构建状态。

---

## 文件结构

```
workspace/
├── index.html              ← 核心文件，单文件包含全部功能
├── README.md               ← 使用说明
├── STEP-3D-Viewer.html     ← 早期开发版本（可忽略）
└── .openclaw/tmp/3d-viewer/ ← 开发过程中的临时文件
    ├── model.stp           ← 用户上传的原始 STEP 文件
    ├── model.gltf          ← 转换后的 GLTF
    ├── model.bin           ← GLTF 二进制数据
    ├── mesh_info.json      ← 零件信息
    └── *.mjs               ← Node.js 转换脚本
```

---

## 开发历程

1. 用户上传了 STEP 文件（MV-ID5050XM 工业相机模组，16MB，93 个零件）
2. 用 occt-import-js（Node.js）将 STEP 转换为 GLTF
3. 构建 Three.js 查看器，参考 anime.js 官网风格
4. 从深色紫色主题 → 海康机器人橙色主题 → 最终浅色技术图纸风格
5. 将 STEP 解析能力搬到浏览器端（WASM），做成通用工具
6. 添加 DXF 2D 图纸支持
7. 添加悬停测量、加粗渲染、自动标注（后移除）
8. 部署到 GitHub Pages

---

## 如果需要继续开发

- **增强3D零件颜色**：修改 `buildGLTF` 函数，从 OCCT result 的 `mesh.color` 提取颜色
- **添加 DXF 文字渲染**：在 `buildDXFScene` 中处理 TEXT/MTEXT 实体
- **添加测量工具**：两点间距离测量、角度测量
- **添加剖面视图**：3D 模型的剖切面展示
- **优化大文件**：使用 Web Worker 解析，避免阻塞主线程
- **添加3D标注**：重新实现工程标注功能
