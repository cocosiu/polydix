# Polydix

A lightweight zero-dependency polygon splitting utility — cut a closed polygon with a polyline and get two polygons back.

---

## Overview

Polydix splits a closed 2D polygon along a polyline (cutting path) and returns two resulting sub-polygons. It handles intersection detection, edge interpolation, and polygon reconstruction automatically.

Pure JavaScript, no dependencies, ~220 lines.

---

## How It Works

```
Input: closed polygon + polyline (≥2 points)
  │
  ├── 1. Classify polyline points: inside / outside / on edge
  ├── 2. Find entry point (outside→inside) and exit point (inside→outside)
  ├── 3. Compute intersection points on polygon boundary
  ├── 4. Insert intersection points into boundary vertex sequence
  └── 5. Walk the boundary twice → two closed sub-polygons

Output: [PolygonA, PolygonB]
```

---

## API

### `Polydix.split(polygon, polyline)`

| Param | Type | Description |
|---|---|---|
| `polygon` | `[x,y][]` | Closed polygon, first point = last point |
| `polyline` | `[x,y][]` | Cutting path, at least 2 points |

**Returns:** `[PolygonA, PolygonB]` — two closed polygons

### Example

```js
const Polydix = require('./polydix');

const polygon = [
  [0, 0], [10, 0], [10, 10], [0, 10], [0, 0]
];

const cutter = [
  [5, -1], [5, 11]
];

const [polyA, polyB] = Polydix.split(polygon, cutter);

console.log('Left:',  polyA);  // left half
console.log('Right:', polyB);  // right half
```

---

## Features

| Feature | Description |
|---|---|
| Pure math | No dependencies, pure JavaScript geometry |
| Zero deps | Works anywhere JS runs (Node, browser) |
| Shape-preserving | Sub-polygons remain closed and orientation-consistent |
| Edge cases | Handles point-on-edge, collinear segments |
| Precision | Floating-point threshold `1e-6` |

---

## Internal Methods

| Method | Purpose |
|---|---|
| `_approxEqual(a, b, eps?)` | Point equality check |
| `_closePolygon(p)` | Ensure polygon is closed |
| `_pointInPolygon(pt, poly)` | Ray casting: is point inside? |
| `_pointOnSegment(p, a, b)` | Is point on edge segment? |
| `_lineIntersection(A, B, C, D)` | Segment-segment intersection |
| `_interpPoint(A, B, poly)` | Find cut point on polygon edge |
| `_insertPoints(poly, pin, pout)` | Insert intersection points into boundary |

---

## Use Cases

- GIS / mapping — split land parcels
- CAD — partition planar regions
- SVG / Canvas — interactive shape cutting
- Game dev — destroyable geometry
- Education — computational geometry

---

## License

MIT
