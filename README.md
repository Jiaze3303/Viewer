# STEP & DXF 在线查看器

浏览器端 3D/2D CAD 图纸在线查看器，支持 STEP、STP、IGES、BREP 等 3D 格式及 DXF、DWG 2D 图纸格式。纯前端解析，文件不离开本地。

**在线地址**：https://jiaze3303.github.io/Viewer/

## 功能

### 3D 模型查看（STEP / STP / IGES / BREP）
- 实体、线框、X-Ray、线面混合四种视图模式
- 零件拆解视图（滑块控制散开距离）
- 透明度调节
- 正视图、侧视图、俯视图一键切换
- 自动旋转、边缘线、网格显示
- 零件列表（悬停高亮、点击聚焦）
- 导出 GLTF 格式
- 入场飞入动画

### 2D 图纸查看（DXF / DWG）
- 加粗线条渲染（GPU Line2）
- 支持 LINE、LWPOLYLINE、POLYLINE、ARC、CIRCLE、ELLIPSE、SPLINE、SOLID 等实体类型
- AutoCAD 颜色索引（ACI）完整映射
- GBK / UTF-8 编码自动识别
- DWG 文件通过 LibreDwg WASM 引擎自动转换为 DXF 后渲染
- 图层列表（点击切换显隐）
- 悬停高亮 + 尺寸信息提示（长度、半径、直径、面积、角度、坐标）

### 通用功能
- 拖放或点击上传
- 文件读取实时进度条（4 阶段）
- 鼠标悬停显示零件/实体详细参数
- 左键旋转、右键平移、滚轮缩放
- 触屏单指旋转、双指缩放平移
- 浅色技术图纸风格界面

## 使用

直接用浏览器打开 `index.html`，无需安装任何软件，无需服务器。

### 支持格式

| 格式 | 类型 | 说明 |
|------|------|------|
| `.stp` `.step` | 3D | STEP 标准格式 |
| `.iges` `.igs` | 3D | IGES 格式 |
| `.brep` | 3D | OpenCascade BRep 格式 |
| `.dxf` | 2D | AutoCAD DXF 格式 |
| `.dwg` | 2D | AutoCAD DWG 格式（WASM 转 DXF） |

## 技术栈

| 技术 | 用途 | 版本 |
|------|------|------|
| Three.js | WebGL 3D/2D 渲染 | r170 |
| occt-import-js | STEP/IGES/BREP 解析（WASM） | 0.0.23 |
| dxf-parser | DXF 文件解析 | 1.1.2 |
| libredwg-web | DWG → DXF 转换（WASM） | 0.7.9 |
| Line2 / LineGeometry / LineMaterial | GPU 加粗线渲染 | Three.js 内置 |

所有依赖通过 jsDelivr CDN 加载，无需 npm install。

## 浏览器要求

- Chrome 90+ / Firefox 90+ / Safari 15+ / Edge 90+
- 需要支持 ES2020、WebAssembly、WebGL2

## 交互操作

| 操作 | 鼠标 | 触屏 |
|------|------|------|
| 旋转 | 左键拖动 | 单指拖动 |
| 平移 | 右键拖动 | 双指拖动 |
| 缩放 | 滚轮 | 双指捏合 |
| 悬停信息 | 鼠标移到零件/实体上 | - |
