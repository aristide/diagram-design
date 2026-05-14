# Process

**Best for:** sequential business processes with multiple actors/divisions where the reader needs to see *who* does *what*, *what data* enters and leaves each step, and *which tools* are used — not just the step order. Covers responsibility audits, data-quality gate reviews, cross-divisional handoff maps, and end-to-end workflow documentation.

Prefer swimlane (simpler) when the data types and tools don't matter. Prefer process when each step's input/output payload and responsible team must be legible at a glance.

---

## Structural elements (draw in this order)

### 1. Step-number header strip (y=4–36)
A row of circle chips above each step column. Ink-tint background for all steps; accent fill for the focal step chip.

```svg
<!-- Standard chip -->
<rect x="{chip_cx-8}" y="8" width="16" height="16" rx="8" fill="rgba(45,49,66,0.12)"/>
<text x="{chip_cx}" y="19" fill="#2d3142" font-size="7" font-family="'Geist Mono',monospace"
      text-anchor="middle" font-weight="600">{N}</text>

<!-- Focal step chip -->
<rect x="{chip_cx-8}" y="8" width="16" height="16" rx="8" fill="rgba(235,108,54,0.20)"/>
<text x="{chip_cx}" y="19" fill="#eb6c36" font-size="7" font-family="'Geist Mono',monospace"
      text-anchor="middle" font-weight="600">{N}</text>
```

### 2. Actor lanes
Horizontal swimlanes — one per actor, team, or division. Alternating subtle tint on odd lanes. Hairline dividers between every lane and at the label column boundary.

```svg
<!-- Odd-lane tint -->
<rect x="140" y="{lane_top}" width="{diagram_w-140}" height="80" fill="rgba(45,49,66,0.018)"/>

<!-- Lane dividers -->
<line x1="0" y1="{lane_y}" x2="{diagram_w}" y2="{lane_y}"
      stroke="rgba(45,49,66,0.12)" stroke-width="0.8"/>

<!-- Label column divider -->
<line x1="140" y1="36" x2="140" y2="{lanes_bottom}"
      stroke="rgba(45,49,66,0.20)" stroke-width="1"/>
```

**Label column:** 140px wide. Labels in `'Geist Mono'` 8px 500-weight uppercase, centered at x=70. Two-word labels use two stacked `<text>` elements.

**Lane sizing:** 80px tall. Label column: 140px. Content starts at x=148 (8px margin from divider).

### 3. Step nodes (w=100, h=64)

Each node is a card with four rows:

```
[NN]  badge          ← Geist Mono 6px bold in 14×10 rounded rect (top-left corner)
   Step Name         ← Geist 9px bold, text-anchor="middle" at node center-x
 Input → Output      ← Geist Mono 6.5px, text-anchor="middle" (abbreviated)
 Tool · Tool         ← Geist Mono 6.5px muted, text-anchor="middle"
```

Layout within a 64px-tall node:
- Badge top: `node_y + 4`
- Name baseline: `node_y + 26`
- Input→Output baseline: `node_y + 40`
- Tool baseline: `node_y + 52`

**Vertical centering in lane:** `node_y = lane_top + (80 - 64) / 2 = lane_top + 8`

**Standard node:**
```svg
<rect x="{nx}" y="{ny}" width="100" height="64" rx="6" fill="#f5f5f5"/>
<rect x="{nx}" y="{ny}" width="100" height="64" rx="6" fill="white"
      stroke="rgba(45,49,66,0.25)" stroke-width="1"/>
```

**Focal step node** (1 max):
```svg
<rect x="{nx}" y="{ny}" width="100" height="64" rx="6" fill="#f5f5f5"/>
<rect x="{nx}" y="{ny}" width="100" height="64" rx="6"
      fill="rgba(235,108,54,0.08)" stroke="#eb6c36" stroke-width="1.2"/>
```

**Step-number badge:**
```svg
<rect x="{nx+4}" y="{ny+4}" width="14" height="10" rx="2" fill="rgba(45,49,66,0.10)"/>
<text x="{nx+11}" y="{ny+12}" fill="#2d3142" font-size="6"
      font-family="'Geist Mono',monospace" text-anchor="middle" font-weight="600">01</text>
```

### 4. Data-type chips

Two 16×8 chips at the bottom of each node (`node_y + 54`, exactly 2px below tool text baseline). Left chip = input type; right chip = output type.

- **Skip** the input chip on the first step (no upstream).
- **Skip** the output chip on the last step (no downstream).
- **Skip both** on any node whose tool text baseline leaves < 4px to node bottom (double-line-name nodes).

```svg
<!-- Input type chip -->
<rect x="{nx+4}" y="{ny+54}" width="16" height="8" rx="2" fill="{type_color}"/>
<text x="{nx+12}" y="{ny+60}" fill="white" font-size="5"
      font-family="'Geist Mono',monospace" text-anchor="middle" font-weight="700">DB</text>

<!-- Output type chip -->
<rect x="{nx+80}" y="{ny+54}" width="16" height="8" rx="2" fill="{type_color}"/>
<text x="{nx+88}" y="{ny+60}" fill="white" font-size="5"
      font-family="'Geist Mono',monospace" text-anchor="middle" font-weight="700">TB</text>
```

**Data-type colour palette** (reuses `style-guide.md` series tokens):

| Code | Light | Dark | Meaning |
|------|-------|------|---------|
| `LS` | `#7c8f6f` sage | `#9caf8f` | List / assignment / task |
| `DB` | `#5e7a9b` dusty-blue | `#82a0c0` | Dataset / tabular records |
| `TB` | `#b8915a` mustard | `#d3ad7a` | Table (analysis-ready) |
| `FL` | `#9c6b50` rust-brown | `#b88670` | File / document / report |
| `WB` | `#6e6479` slate | `#8d8298` | Web / press / public release |
| N/A | omit chip entirely | — | Unknown or not applicable |

---

## Connector rule

**All inter-lane arrows: exit RIGHT → single right-angle bend → enter TOP (↓) or BOTTOM (↑).**

The decision is: if the destination is below, enter its top; if above, enter its bottom.

```svg
<!-- Downward (dest below source) -->
<path d="M {rx},{sy} H {dest_cx-8} Q {dest_cx},{sy} {dest_cx},{sy+8} V {dest_top}"
      fill="none" stroke="…" stroke-width="1" marker-end="url(#arrow)"/>

<!-- Upward (dest above source) -->
<path d="M {rx},{sy} H {dest_cx-8} Q {dest_cx},{sy} {dest_cx},{sy-8} V {dest_bottom}"
      fill="none" stroke="…" stroke-width="1" marker-end="url(#arrow)"/>

<!-- Same lane (horizontal) -->
<line x1="{src_right}" y1="{lane_cy}" x2="{dest_left}" y2="{lane_cy}"
      stroke="…" stroke-width="1" marker-end="url(#arrow)"/>
```

- Corner radius: 8px (Q bezier).
- All coordinates on **4px grid**. Node center-x values that fall off-grid: round to nearest multiple of 4.
- `rx` = source node right edge = `node_x + 100`.
- `dest_cx` = destination node center-x ≈ `node_x + 50` (rounded to 4px grid).
- `dest_top` = destination node `node_y`. `dest_bottom` = `node_y + 64`.

**Connector styles:**

| Connection | Stroke | Width | Dash |
|-----------|--------|-------|------|
| Normal data flow | `#4f5d75` | 1 | solid |
| Into/out of focal step | `#eb6c36` | 1.2 | solid |
| Orchestration trigger (scheduler→tool) | `#4f5d75` | 1 | `4,3` |

---

## ViewBox and sizing

```
W = 140 (label col) + N × 112 (slot) + 28 (right margin)
H = 36 (header strip) + M × 80 (lanes) + 60 (legend row 1) + 20 (legend row 2)
```

- **Slot width = 112px** = 100px node + 12px connector gap.
- The 12px gap between adjacent nodes is the routing column for connector bends.
- Expand `H` by 20px if a second legend row (data-type key) is added.

**Legend (two rows):**
- Row 1: shape key (focal node, step node, connectors).
- Row 2: data-type chip key (`LS · list / assignment`, `DB · dataset`, etc.) + note "left chip = input type · right chip = output type".

---

## Anti-patterns

- **Diagonal arrows.** Every connector must have exactly one right-angle bend. No direct straight lines between nodes in different lanes.
- **Left/right port entry on a vertical-dominant arrow.** Always exit right, enter top or bottom.
- **Data-type chips in a double-line-name node** when there's less than 4px margin to node bottom — skip them or shorten the name to a single line.
- **More than one focal step** — pick the single most critical operation.
- **More than 12 steps** without splitting — use an overview diagram + a separate detail diagram.
- **Unlabelled lanes** — every swimlane must identify its actor.
- **All arrows the same style** — orchestration triggers must be dashed to distinguish them from data-flow connectors.

## Examples

- `assets/example-process.html` — minimal light (Jamaica LFS: 11 steps, 6 divisions, data-type chips)
- `assets/example-process-dark.html` — minimal dark
- `assets/example-process-full.html` — full editorial with subtitle + 3 callout cards
