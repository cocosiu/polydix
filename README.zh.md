# Polydix

轻量、零依赖的多边形切割工具——用一条折线切开闭合多边形，得到两个子多边形。

---

## 概述

Polydix 沿一条折线（切割路径）将闭合的 2D 多边形一分为二，自动处理交点检测、边界插值和多边形重建。

纯 JavaScript，无任何依赖，约 220 行。

---

## 安装

```bash
npm install @cosiu/polydix
```

---

## 工作原理

```
输入: 闭合多边形 + 折线（≥2 点）
  │
  ├── 1. 判定折线各点位置：内部 / 外部 / 边上
  ├── 2. 找"进点"（外→内）和"出点"（内→外）
  ├── 3. 计算折线与多边形边界的交点
  ├── 4. 将交点插入多边形的顶点序列
  └── 5. 沿边界走两圈 → 两个闭合子多边形

输出: [多边形A, 多边形B]
```

---

## API

### `Polydix.split(polygon, polyline)`

| 参数 | 类型 | 说明 |
|---|---|---|
| `polygon` | `[x,y][]` | 闭合多边形，首尾点相同 |
| `polyline` | `[x,y][]` | 切割折线，至少 2 个点 |

**返回：** `[PolygonA, PolygonB]` — 两个闭合子多边形

### 示例

```js
const Polydix = require('@cosiu/polydix');
// 或: const Polydix = require('./polydix');

const polygon = [
  [0, 0], [10, 0], [10, 10], [0, 10], [0, 0]
];

const cutter = [
  [5, -1], [5, 11]
];

const [polyA, polyB] = Polydix.split(polygon, cutter);

console.log('左半:', polyA);
console.log('右半:', polyB);
```

---

## 特性

| 特性 | 说明 |
|---|---|
| 纯数学 | 无依赖，纯 JavaScript 几何计算 |
| 零依赖 | 任何 JS 环境都能跑（Node、浏览器） |
| 形态保持 | 子多边形保持闭合和一致的方向 |
| 边界情况 | 处理点在边上、共线段等场景 |
| 精度 | 浮点比较阈值 `1e-6` |

---

## 内部方法

| 方法 | 作用 |
|---|---|
| `_approxEqual(a, b, eps?)` | 判断两点是否重合 |
| `_closePolygon(p)` | 确保多边形闭合 |
| `_pointInPolygon(pt, poly)` | 射线法判断点是否在多边形内 |
| `_pointOnSegment(p, a, b)` | 点是否在线段上 |
| `_lineIntersection(A, B, C, D)` | 两线段求交 |
| `_interpPoint(A, B, poly)` | 找折线与多边形边的交点 |
| `_insertPoints(poly, pin, pout)` | 将交点插入多边形边界 |

---

## 适用场景

- GIS / 地图 — 地块分割
- CAD — 平面区域划分
- SVG / Canvas — 交互式图形切割
- 游戏开发 — 可破坏几何体
- 教学 — 计算几何

---

## 许可证

MIT
