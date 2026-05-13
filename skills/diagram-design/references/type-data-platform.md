# Data Platform

**Best for:** end-to-end data stack overviews — ingestion → storage → query → analytics → visualization — deployed on a container orchestrator (Kubernetes, ECS, Nomad). Combines a phase banner, deployment boundary, orchestration bar, and identity footer into a single readable architecture template.

## Structural elements (draw in this order)

### 1. Phase chevron banner
A horizontal band of 5 chevron polygons at the top of the SVG (y=4, h=28). Each chevron covers one functional phase: **Data sources · Ingestion · Storage · Transformation & Analysis · Visualization**. Alternating fills (`#2d3142` / `#3d4460`), paper-colored labels.

```svg
<!-- Section 1 of 5 — leftmost (no left notch) -->
<polygon points="0,4 188,4 200,18 188,32 0,32" fill="#2d3142"/>
<text x="100" y="21" fill="#f5f5f5" font-size="7" font-family="'Geist Mono', monospace"
      letter-spacing="0.14em" text-anchor="middle">DATA SOURCES</text>
<!-- Section 2 of 5 — middle (notch left, arrow right) -->
<polygon points="200,4 388,4 400,18 388,32 200,32 212,18" fill="#3d4460"/>
```

Tip: sections 2–4 have a left notch at `{left},4 ... {left},32 {left+12},18`; section 5 has a plain right edge.

### 2. External zone (dashed border)
Data sources live **outside** the cluster. Signal this with a dashed stroke — the dashed border is the only visual difference from a standard zone.

```svg
<rect x="4" y="40" width="152" height="336" rx="6"
      fill="rgba(45,49,66,0.02)" stroke="rgba(45,49,66,0.20)"
      stroke-width="0.8" stroke-dasharray="6,3"/>
```

### 3. Cluster boundary (solid border)
The Kubernetes (or equivalent) cluster gets a solid, slightly heavier border than individual zones.

```svg
<rect x="164" y="40" width="832" height="336" rx="8"
      fill="rgba(45,49,66,0.02)" stroke="rgba(45,49,66,0.18)" stroke-width="1.2"/>
```

Place a small Kubernetes icon + "Kubernetes" label at the bottom-left inside the boundary (not on the border line itself).

### 4. Orchestration bar
A full-width bar at the top of the cluster interior (below the chevron banner) for the workflow scheduler (Airflow, Prefect, etc.). This bar spans the cluster width. Tool icon placed at the far right; text centered.

```svg
<rect x="176" y="52" width="808" height="44" rx="4"
      fill="rgba(45,49,66,0.05)" stroke="rgba(45,49,66,0.18)" stroke-width="0.8"/>
```

Arrows from the orchestration bar to tool nodes use `stroke-dasharray="4,3"` — they signal *trigger*, not data flow.

### 5. Nodes
Standard architecture node pattern with role badge top-left, icon top-right, name and subtitle centered.

**Focal node** (central storage — MinIO, S3, etc.) gets accent tint + accent border:
```svg
<rect x="…" y="…" width="160" height="80" rx="8"
      fill="rgba(235,108,54,0.08)" stroke="#eb6c36" stroke-width="1.2"/>
```

### 6. Identity / security bar
A full-width bar **below** the cluster boundary, spanning both the external zone and the cluster. Distinct from the cluster by sitting outside it.

```svg
<rect x="4" y="388" width="992" height="40" rx="6"
      fill="rgba(45,49,66,0.05)" stroke="rgba(45,49,66,0.20)" stroke-width="0.8"/>
```

---

## Connector styles

| Connection type | Style |
|----------------|-------|
| Data flow (primary path) | `stroke="#eb6c36"` `stroke-width="1.2"` solid — accent arrow |
| Data flow (secondary) | `stroke="#4f5d75"` `stroke-width="1"` solid — muted arrow |
| Orchestration trigger | `stroke="#4f5d75"` `stroke-width="1"` `stroke-dasharray="4,3"` |
| Query / read-back | `stroke="rgba(45,49,66,0.30)"` `stroke-width="1"` `stroke-dasharray="4,3"` |

All connectors use **orthogonal elbow routing** (see `type-architecture.md` for the two-bend path pattern). When gap between adjacent nodes is tight (< 16px), route via the zone boundary x-coordinate using an 8px-radius Q-bezier corner.

---

## Layout grid

- **4px grid** applies to all x/y/width/height values.
- Nodes inside the cluster sit 12px from the zone boundary edges.
- External source nodes are typically 112–132px wide × 52–72px tall.
- Cluster nodes: 140–164px wide × 72–88px tall (focal node slightly taller).

---

## Dark mode

- Chevrons: lighten fills (`#3d4460` / `#4a5270`).
- Dashed border: `stroke="rgba(245,245,245,0.22)"`.
- Cluster boundary: `stroke="rgba(245,245,245,0.18)"`.
- Nodes: `fill="rgba(245,245,245,0.06)"` `stroke="rgba(245,245,245,0.20)"`.
- Focal node: `fill="rgba(240,138,89,0.12)"` `stroke="#f08a59"`.
- Dot pattern: `fill="rgba(245,245,245,0.10)"`.

---

## Anti-patterns
- Chevron banner omitted — it's the key that maps visual columns to functional phases.
- All arrows solid — orchestration triggers must be dashed to distinguish from data flow.
- More than one focal node — MinIO/S3 (the storage hub) is the focal point; everything else is muted.
- External zone with solid border — dashed border is the signal that these components are outside the cluster.
- Identity bar inside the cluster boundary — it applies to all components and must span the full width.

## Examples
- `assets/example-data-platform.html` — minimal light
- `assets/example-data-platform-dark.html` — minimal dark
- `assets/example-data-platform-full.html` — full editorial
