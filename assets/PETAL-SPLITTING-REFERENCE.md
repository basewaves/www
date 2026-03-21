# Base Perception SVG — Petal Splitting Reference

## For: Manus vector generation (filling spaces between the 62 component curves)

---

## Source File

**`base_perception.svg`** (also identical: `base_curves.svg`)
- ViewBox: `0 0 3978.47 2106.27`
- 31 closed petal-shaped paths
- Original strokes: black (`#000`), no fill, `stroke-miterlimit: 10`
- Stroke widths range from ~1.5px (petal 1) to ~4.5px (petal 31), progressively thickening
- Exported from Adobe Illustrator

---

## What the 31 Source Paths Are

Each source path is a **closed petal shape** — a leaf/eye form created by two cubic bezier curves that share start and end points. The 31 petals fan out from a tight cluster (upper-right region) to a wide spread (lower-left region), creating an interference/wave pattern.

### Anatomy of a single petal:

```
Point A (start) ──── Curve 1 (outer/upper arc) ───→ Point B (apex)
    ↑                                                    │
    └──────── Curve 2 (inner/lower arc) ←───────────────┘
```

The closed path `d` attribute contains:
```
M[startX],[startY]  c/C[6 control numbers]  c/C[6 control numbers]  Z
```

- `M` = starting point (Point A)
- First `c/C` = Curve 1 (upper/outer arc) — 6 numbers defining 2 control points + endpoint (Point B)
- Second `c/C` = Curve 2 (lower/inner arc) — 6 numbers defining 2 control points + endpoint (back to Point A)
- `Z` = close path

---

## How the HTML Pages Split Them

### The Parse Logic

JavaScript function `parsePetal(d)` splits each closed path into two open curves:

1. **Clean** the path string (strip `h0`, `Z`, trailing closures)
2. **Extract** the starting M coordinates (Point A: `sx, sy`)
3. **Identify** curve commands (`c` = relative, `C` = absolute) — some paths use one command with 12 numbers, others use two separate commands
4. **Extract** 12 numbers total (6 per curve)
5. **Convert** all coordinates to absolute values
6. **Output** two separate open path strings:

```javascript
{
  base:  "M[sx],[sy] C[cp1x],[cp1y] [cp2x],[cp2y] [ex],[ey]",     // Curve 1
  waves: "M[ex],[ey] C[cp1x],[cp1y] [cp2x],[cp2y] [sx],[sy]",     // Curve 2
  full:  "[original d attribute]"                                    // Original closed path
}
```

### The 62 Component Curves

After splitting all 31 petals:
- **31 BASE curves** (upper/outer arcs) — colored coral (`#FFA565`) in the HTML page
- **31 WAVES curves** (lower/inner arcs) — colored turquoise (`#58FCC8`)
- **31 SPACE fills** (original closed paths with translucent fill) — represent the void between curves

### Visual Assignment in base-perception.html

| Element | Color | Role | CSS Class |
|---------|-------|------|-----------|
| BASE curves | `#FFA565` (coral) | Upper/outer arc of each petal | `.base-curve` |
| WAVES curves | `#58FCC8` (turquoise) | Lower/inner arc of each petal | `.waves-curve` |
| SPACE fills | `rgba(232,227,255,0.04)` | Closed petal fill (the void) | `.space-fill` |

### Stroke Width Progression

Each petal has a `data-sw` attribute controlling stroke width. They progress from thin (petal 1) to thick (petal 31):

| Petal | data-sw | Notes |
|-------|---------|-------|
| 1 | 3 | Tightest cluster (upper-right) |
| 2 | 3.2 | |
| 3 | 3.5 | |
| ... | +0.3 each | Progressive thickening |
| 30 | 11.6 | |
| 31 | 12 | Widest spread (spans full width) |

### Animation

Each curve has staggered breathing animations using CSS custom properties:
- `--bd` (breath delay): BASE curves stagger forward (0s, 0.35s, 0.7s...), WAVES curves stagger backward
- `--bo` (base opacity): Varies per curve (0.4–0.8 range)

---

## The 31 Source Paths (Complete Data)

Below are all 31 petal `d` attributes exactly as they appear in the source SVG. Petal 1 is the tightest cluster; Petal 31 is the widest spread.

### Petal 1 (sw: 3)
```
M2532.89,322.95c148.43,369.82-122.32,738.34-1087.76,1474.19-116.5-380.4,80.32-712.01,1087.76-1474.19Z
```

### Petal 2 (sw: 3.2)
```
M2517.11,286.82c186.95,358.56-75.83,745.07-1056.21,1546.48-153.49-371.34,32.75-715.88,1056.21-1546.48h0Z
```

### Petal 3 (sw: 3.5)
```
M2497.46,251.88c227.42,344.65-25.45,748.55-1016.95,1616.39-192.5-359.73-18.57-716.42,1016.95-1616.39Z
```

### Petal 4 (sw: 3.8)
```
M2473.77,218.4c269.8,327.94,28.85,748.5-969.59,1683.39-233.46-345.43-73.66-713.38,969.59-1683.39h0Z
```

### Petal 5 (sw: 4.1)
```
M2445.85,186.65c314,308.33,87.08,744.66-913.76,1746.93-276.33-328.3-132.51-706.5,913.76-1746.93h0Z
```

### Petal 6 (sw: 4.4)
```
M2413.52,156.92c359.93,285.67,149.27,736.78-849.11,1806.41-321.03-308.23-195.13-695.54,849.11-1806.41h0Z
```

### Petal 7 (sw: 4.7)
```
M2376.6,129.53c407.51,259.87,215.37,724.57-775.31,1861.24-367.48-285.08-261.48-680.23,775.31-1861.24h0Z
```

### Petal 8 (sw: 5)
```
M2334.95,104.77c456.61,230.81,285.37,707.78-692.03,1910.79-415.56-258.75-331.5-660.33,692.03-1910.79h0Z
```

### Petal 9 (sw: 5.3)
```
M2288.42,82.98c507.09,198.39,359.19,686.15-598.98,1954.41-465.15-229.13-405.11-635.6,598.98-1954.41h0Z
```

### Petal 10 (sw: 5.6)
```
M2236.88,64.49c558.81,162.54,436.75,659.42-495.9,1991.43-516.13-196.13-482.22-605.79,495.9-1991.43h0Z
```

### Petal 11 (sw: 5.9)
```
M2180.2,49.64c611.6,123.16,517.94,627.35-382.56,2021.17-568.34-159.65-562.69-570.68,382.56-2021.17h0Z
```

### Petal 12 (sw: 6.2)
```
M2118.29,38.78c665.27,80.2,602.62,589.69-258.75,2042.94-621.61-119.61-646.37-530.04,258.75-2042.94h0Z
```

### Petal 13 (sw: 6.5)
```
M2051.07,32.27c719.62,33.6,690.64,546.22-124.31,2056.01-675.74-75.96-733.09-483.66,124.31-2056.01h0Z
```

### Petal 14 (sw: 6.8)
```
M1978.47,30.46c774.42-16.68,781.78,496.71,20.87,2059.67-730.53-28.64-822.63-431.34-20.87-2059.67Z
```

### Petal 15 (sw: 7.1)
```
M1900.47,33.72c829.43-70.66,875.83,440.97,176.88,2053.2-785.75,22.39-914.74-372.91-176.88-2053.2h0Z
```

### Petal 16 (sw: 7.4)
```
M1817.04,42.41c884.39-128.35,972.54,378.8,343.73,2035.86-841.15,77.15-1009.17-308.19-343.73-2035.86h0Z
```

### Petal 17 (sw: 7.7)
```
M1728.21,56.9c939.01-189.73,1071.6,310.03,521.41,2006.92-896.48,135.64-1105.6-237.04-521.41-2006.92h0Z
```

### Petal 18 (sw: 8)
```
M1634.01,77.55c993-254.77,1172.71,234.53,709.82,1965.66-951.43,197.84-1203.7-159.34-709.82-1965.66Z
```

### Petal 19 (sw: 8.3)
```
M1534.51,104.72c1046.03-323.42,1275.5,152.15,908.83,1911.36-1005.71,263.73-1303.1-74.99-908.83-1911.36h0Z
```

### Petal 20 (sw: 8.6)
```
M1429.82,138.77c1097.76-395.6,1379.58,62.81,1118.22,1843.31-1059,333.24-1403.4,16.09-1118.22-1843.31h0Z
```

### Petal 21 (sw: 8.9)
```
M1320.09,180.04c1147.84-471.21,1484.51-33.58,1337.72,1760.81-1110.94,406.3-1504.15,113.95-1337.72-1760.81h0Z
```

### Petal 22 (sw: 9.2)
```
M1205.47,228.86c1195.89-550.13,1589.84-137.05,1566.98,1663.21-1161.17,482.8-1604.88,218.6-1566.98-1663.21h0Z
```

### Petal 23 (sw: 9.5)
```
M1086.19,285.56c1241.51-632.21,1695.06-247.62,1805.57,1549.85-1209.32,562.63-1705.07,330.01-1805.57-1549.85h0Z
```

### Petal 24 (sw: 9.8)
```
M962.5,350.43c1284.28-717.27,1799.64-365.26,2053,1420.12-1254.97,645.61-1804.19,448.15-2053-1420.12h0Z
```

### Petal 25 (sw: 10.1)
```
M834.68,423.79c1323.78-805.11,1902.98-489.91,2308.66,1273.46-1297.72,731.58-1901.65,572.91-2308.66-1273.46h0Z
```

### Petal 26 (sw: 10.4)
```
M703.09,505.87c1359.57-895.49,2004.49-621.46,2571.9,1109.31-1337.12,820.33-1996.83,704.17-2571.9-1109.31h0Z
```

### Petal 27 (sw: 10.7)
```
M568.09,596.94c1391.17-988.15,2103.5-759.77,2841.96,927.2-1372.73,911.6-2089.08,841.75-2841.96-927.2h0Z
```

### Petal 28 (sw: 11)
```
M430.11,697.2c1418.12-1082.79,2199.33-904.65,3117.97,726.7-1404.09,1005.14-2177.72,985.42-3117.97-726.7h0Z
```

### Petal 29 (sw: 11.3)
```
M289.63,806.84C1729.56-372.23,2580.88-249.02,3688.63,1314.27c-1430.7,1100.63-2262.01,1134.93-3399-507.43h0Z
```

### Petal 30 (sw: 11.6)
```
M147.16,926.02C1603.26-350.62,2525.67-287.1,3831.15,1195.11,2379.07,2392.86,1489.93,2485.07,147.16,926.02h0Z
```

### Petal 31 (sw: 12)
```
M3.29,1054.84c1466.13-1375.11,2460.32-1376.08,3971.82,11.45C2507.37,2362.42,1560.56,2516.42,3.29,1054.84h0Z
```

---

## Key Geometric Properties

- **All 31 petals share a visual convergence zone** near the upper-right (approximately x:2000-2500, y:30-320)
- **Petals fan outward and downward** — the apex of each petal spreads progressively toward the lower-left
- **Petal 14** (sw: 6.8) is the approximate **centerline** — where the M point is closest to the top of the viewBox (y:30.46)
- **Petals 1-13:** Start points move from upper-right toward center-top
- **Petals 15-31:** Start points move from center-top toward lower-left
- **The void between the two curves of each petal** is what we call "SPACE" — the channel between BASE (outer arc) and WAVES (inner arc)

---

## What Manus Should Generate

### The Goal

Generate **60 closed fill shapes** — curved triangular regions that fill the gaps between adjacent component curves on each side of the split petals.

After the 31 closed petals are split, there are:
- **31 BASE curves** (upper/outer arcs) on the left side
- **31 WAVES curves** (lower/inner arcs) on the right side

Between each pair of adjacent curves on the same side, there is a curved triangular void. Manus should generate closed vector paths that fill these voids:
- **30 BASE-side fills** (between adjacent BASE curves)
- **30 WAVES-side fills** (between adjacent WAVES curves)
- **Total: 60 fill shapes**

These are NOT additional petals. They are **gap-fill regions** bounded by adjacent component curves.

---

### How to Split Each Petal (Required First Step)

Before generating fills, Manus must split each of the 31 source petal paths into their two component curves. Each petal `d` attribute contains:

```
M[startX],[startY]  c/C[6 numbers]  c/C[6 numbers]  Z
```

**Splitting procedure:**

1. Parse the `M` coordinates → **Point A** (start point: `sx, sy`)
2. Extract the first 6 numbers after the first `c/C` command → **BASE curve** (outer/upper arc)
   - If lowercase `c`: add to Point A to get absolute coordinates
   - Endpoint of this curve = **Point B** (apex: `ex, ey`)
3. Extract the second 6 numbers after the second `c/C` command → **WAVES curve** (inner/lower arc)
   - If lowercase `c`: add to Point B to get absolute coordinates
   - Endpoint returns to Point A

**Result per petal:**
- **BASE curve N:** `M[sx],[sy] C[cp1x],[cp1y] [cp2x],[cp2y] [ex],[ey]` (Point A → Point B)
- **WAVES curve N:** `M[ex],[ey] C[cp1x],[cp1y] [cp2x],[cp2y] [sx],[sy]` (Point B → Point A)

Where:
- **Point A** = start/end (convergence zone, upper-right region)
- **Point B** = apex (fans outward toward lower-left)

---

### Geometry of Each Fill Shape

#### BASE-Side Fill (between BASE curve N and BASE curve N+1)

```
BASE curve N:     A_N ──────curve──────→ B_N
                                              \
                                         (straight line or short arc)
                                              \
BASE curve N+1:   A_{N+1} ←──curve──── B_{N+1}
                  /
            (near-zero edge at convergence)
                  \
                  A_N (close)
```

The fill shape has 4 edges:
1. **BASE curve N forward** — the outer arc of petal N (from A_N to B_N)
2. **Far edge** — straight line from B_N to B_{N+1} (the apex endpoints, which are spread apart)
3. **BASE curve N+1 reversed** — the outer arc of petal N+1, traversed backwards (from B_{N+1} to A_{N+1})
4. **Near edge** — straight line from A_{N+1} back to A_N (the start points, which are close together in the convergence zone)

This creates a **curved triangular shape** — roughly triangular because the "near edge" is very short (start points cluster together) while the "far edge" is wider (apex points spread apart), and two sides are curves.

#### WAVES-Side Fill (between WAVES curve N and WAVES curve N+1)

Same logic, using the inner/lower arcs:

1. **WAVES curve N forward** — the inner arc of petal N (from B_N to A_N)
2. **Near edge** — straight line from A_N to A_{N+1} (start points, close together)
3. **WAVES curve N+1 reversed** — the inner arc of petal N+1, traversed backwards (from A_{N+1} to B_{N+1})
4. **Far edge** — straight line from B_{N+1} back to B_N (apex points, spread apart)

---

### How to Reverse a Cubic Bezier

To reverse a cubic bezier `M[start] C[cp1] [cp2] [end]`:

**Forward:** `M sx,sy C cp1x,cp1y cp2x,cp2y ex,ey`
**Reversed:** `C cp2x,cp2y cp1x,cp1y sx,sy` (swap control point order, endpoint becomes the original start)

The control points swap positions (cp1↔cp2) and the direction reverses.

---

### Output Format

Each fill shape is a closed path with this structure:

```svg
<path d="
  M [A_N.x],[A_N.y]
  C [BASE_N.cp1x],[BASE_N.cp1y] [BASE_N.cp2x],[BASE_N.cp2y] [B_N.x],[B_N.y]
  L [B_{N+1}.x],[B_{N+1}.y]
  C [BASE_{N+1}.cp2x],[BASE_{N+1}.cp2y] [BASE_{N+1}.cp1x],[BASE_{N+1}.cp1y] [A_{N+1}.x],[A_{N+1}.y]
  Z
" />
```

Breaking that down:
- `M` — start at Point A of petal N
- `C` — follow BASE curve N forward to Point B of petal N
- `L` — straight line across to Point B of petal N+1
- `C` — follow BASE curve N+1 **reversed** back to Point A of petal N+1
- `Z` — close path (straight line back to start, which is the short "near edge")

For WAVES-side fills, same structure but using the WAVES curve control points:

```svg
<path d="
  M [B_N.x],[B_N.y]
  C [WAVES_N.cp1x],[WAVES_N.cp1y] [WAVES_N.cp2x],[WAVES_N.cp2y] [A_N.x],[A_N.y]
  L [A_{N+1}.x],[A_{N+1}.y]
  C [WAVES_{N+1}.cp2x],[WAVES_{N+1}.cp2y] [WAVES_{N+1}.cp1x],[WAVES_{N+1}.cp1y] [B_{N+1}.x],[B_{N+1}.y]
  Z
" />
```

### Output SVG Structure

Deliver as a single SVG file containing all 60 fill shapes in two groups:

```svg
<svg viewBox="0 0 3978.47 2106.27" xmlns="http://www.w3.org/2000/svg">
  <g id="base-fills">
    <!-- 30 BASE-side fills (between adjacent outer arcs) -->
    <path d="..." data-between="1-2" />
    <path d="..." data-between="2-3" />
    ...
    <path d="..." data-between="30-31" />
  </g>
  <g id="waves-fills">
    <!-- 30 WAVES-side fills (between adjacent inner arcs) -->
    <path d="..." data-between="1-2" />
    <path d="..." data-between="2-3" />
    ...
    <path d="..." data-between="30-31" />
  </g>
</svg>
```

### Styling

- `fill: none; stroke: #000; stroke-miterlimit: 10` — matching the original source paths
- No fill colors (downstream HTML/CSS handles all color and opacity)
- `data-between` attribute on each path indicating which two petals it sits between (for identification)

### Technical Constraints

- **ViewBox must remain:** `0 0 3978.47 2106.27`
- **Path format:** Standard SVG — `M`, `C`, `L`, `Z` commands with absolute coordinates
- **All paths must be closed** (terminated with `Z`)
- **All coordinates must be absolute** (uppercase commands only) for maximum clarity
- **Maintain precision** — use same decimal precision as the source paths (2 decimal places)
- **The `L` (straight line) edges** connecting apex-to-apex and start-to-start may need to be curves (`C`) if the straight line doesn't visually match the gap shape — use judgment based on how the points align

### Suggested Approach for Manus

1. **Parse all 31 petal `d` attributes** into absolute coordinates:
   - Point A (start: sx, sy)
   - BASE curve: cp1, cp2, Point B (apex: ex, ey)
   - WAVES curve: cp1, cp2, back to Point A

2. **For each adjacent pair (N, N+1) from 1-30:**

   a. **BASE-side fill:**
   - Start at A_N
   - Follow BASE curve N forward (C with N's control points to B_N)
   - Line to B_{N+1}
   - Follow BASE curve N+1 reversed (C with swapped control points to A_{N+1})
   - Close (Z — auto-line back to A_N)

   b. **WAVES-side fill:**
   - Start at B_N
   - Follow WAVES curve N forward (C with N's control points to A_N)
   - Line to A_{N+1}
   - Follow WAVES curve N+1 reversed (C with swapped control points to B_{N+1})
   - Close (Z — auto-line back to B_N)

3. **Output all 60 paths** in a single SVG file with the grouping structure shown above.

This generates exactly **60 curved triangular fill shapes** (30 BASE-side + 30 WAVES-side) that tile the gaps between all adjacent component curves.
