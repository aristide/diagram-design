# Architecture

**Best for:** system overviews, data-flow diagrams, integration maps, infra topology.

## Layout conventions
- Group components by tier or trust boundary (frontend → backend → data; public → private).
- Primary flow runs left→right or top→down. Pick one and hold it.
- Draw arrows before boxes so z-order puts connections behind components.
- 1–2 coral focal nodes: the primary integration point, the primary data store, or the key decision node.
- Dashed boundary rectangles mark regions (VPC, security group, trust zone); labels sit on a paper-colored mask over the boundary line.

## Connector style

**Prefer orthogonal (right-angle) connectors** over diagonal lines for all non-horizontal/vertical connections. Two-bend elbow path with `r=8`:

```svg
<!-- right+down: from (x1,y1) to (x2,y2), mid = (x1+x2)/2 -->
<path d="M x1,y1 H mid-8 Q mid,y1 mid,y1+8 V y2-8 Q mid,y2 mid+8,y2 H x2"
      fill="none" stroke="…" stroke-width="1.2" marker-end="url(#arrow)"/>
```

Flip the vertical signs for right+up. Use a plain `<line>` only when endpoints share the same x or y. Arrow labels sit on the vertical segment, centered horizontally on `mid` and vertically between the two corners.

## Zone grouping

Group 2+ nodes that serve the same tier or trust boundary with a zone rect — drawn **before** arrows and nodes (z-order: bg → zones → arrows → nodes):

```svg
<rect x="{x}" y="{y}" width="{w}" height="{h}" rx="8"
      fill="rgba(45,49,66,0.02)" stroke="rgba(45,49,66,0.10)" stroke-width="0.8"/>
<rect x="{label_x}" y="{y+4}" width="{label_w}" height="12" rx="2" fill="{paper}"/>
<text x="{label_cx}" y="{y+13}" fill="rgba(45,49,66,0.40)" font-size="7"
      font-family="'Geist Mono', monospace" text-anchor="middle" letter-spacing="0.14em">LAYER</text>
```

Rules:
- Leave 12–16px above the first enclosed node — the eyebrow label sits in this margin.
- Zone fill: `rgba(45,49,66,0.02)` (2% ink wash). Any stronger competes with node fills.
- Max 3 zones per diagram. More and it reads like a swimlane (use that type instead).
- Dark mode: swap `rgba(45,49,66,…)` → `rgba(245,245,245,…)` same opacities; label mask fill = `paper` (dark).

## Anti-patterns
- Every box in coral ("this is important too") — hierarchy collapses.
- Bidirectional arrow when one direction is obvious from context.
- Legend floating inside the diagram area.

## Examples
- `assets/example-architecture.html` — minimal light
- `assets/example-architecture-dark.html` — minimal dark
- `assets/example-architecture-full.html` — full editorial
