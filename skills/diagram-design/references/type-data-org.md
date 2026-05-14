# Data Organisation

**Best for:** visualising multi-tier data storage architectures where each tier has a distinct data quality level, file format, access policy, and responsible team. The canonical use case is a data lake with Raw → Bronze → Silver → Gold → Archive tiers (the "medallion" pattern extended with optional flanking tiers). Use when the reader needs to understand **what lives where, in what format, and who can see it** — not just the flow of transformations.

Prefer standard **Medallion** (3-tier) when Bronze/Silver/Gold is sufficient. Use **Data org** when the architecture has 4–6 tiers with meaningful differences in access, format, or purpose between them.

---

## Structural elements (draw in this order)

### 1. Tier cards (vertical zone rectangles)

One tall, rounded rectangle per tier, arranged side-by-side left to right. Each card is self-contained: it holds the tier name, description, examples, and access label.

**Sizing:**
- Card width: 176px. Card height: 408px. `rx=10`.
- Gap between cards: 10px (the routing column for arrows).
- Left margin: 8px. Right margin: 72px (for visual breathing room).
- N cards in 1000px: W = 8 + N×176 + (N-1)×10 + margin.
  - 5 cards: 8 + 880 + 40 + 72 = 1000 ✓

**Colors — standard 5-tier D4N palette:**

| Tier | Fill | Stroke | Title color |
|------|------|--------|-------------|
| RAW | `rgba(220,50,100,0.06)` | `rgba(220,50,100,0.28)` | `#c73860` rose |
| BRONZE | `rgba(139,90,43,0.07)` | `rgba(139,90,43,0.28)` | `#7a6240` warm-brown |
| SILVER | `rgba(20,155,150,0.06)` | `rgba(20,155,150,0.28)` | `#0e8a85` teal |
| **GOLD** (focal) | `rgba(235,108,54,0.08)` | `#eb6c36` × 1.2 | `#eb6c36` accent |
| ARCHIVE | `rgba(100,100,110,0.04)` | `rgba(100,100,110,0.22)` | `#7a8399` muted |

Use tier colors only for: card border, title text, sub-tier label, access value text, and legend swatch. All body text stays in the global ink/muted/soft tokens.

```svg
<!-- Focal card (Gold) -->
<rect x="566" y="20" width="176" height="408" rx="10"
      fill="rgba(235,108,54,0.08)" stroke="#eb6c36" stroke-width="1.2"/>

<!-- Standard card (Silver) -->
<rect x="380" y="20" width="176" height="408" rx="10"
      fill="rgba(20,155,150,0.06)" stroke="rgba(20,155,150,0.28)" stroke-width="1"/>
```

### 2. Card content layout

All text `text-anchor="middle"` at card center-x (`card_x + 88`).

```
card_top           card y=20
  y+32  Tier title       Instrument Serif italic 20px  (or Geist Mono 12px bold uppercase for RAW/ARCHIVE)
  y+48  Sub-tier label   Geist Mono 8px  "(BRONZE)" etc — omit for RAW/ARCHIVE
  y+58  ─── separator ───
  y+72  Content group 1  Geist 9px weight=600 (group header)
  y+85  Content detail   Geist 9px weight=400
  …
  y+N   "Examples:"      Geist Mono 7.5px muted (section label)
  y+N+  Example filenames Geist Mono 7.5px soft
  …
  y+369 ─── separator ───
  y+383 "Access:"        Geist Mono 7px soft, letter-spacing 0.08em
  y+396 Access value     Geist Mono 7px, tier color
card_bottom        card y=428
```

**Typography:**
- **Tier name (medallion tier):** `font-family="'Instrument Serif', serif" font-style="italic" font-size="20"` — matches the handwritten feel of the original design.
- **Tier name (system tier — RAW, ARCHIVE):** `font-family="'Geist Mono', monospace" font-size="12" font-weight="600" letter-spacing="0.18em"` — uppercase, signals administrative/infrastructure rather than data-quality tier.
- **Sub-tier "(BRONZE)":** `font-family="'Geist Mono', monospace" font-size="8" letter-spacing="0.12em"` at 65% opacity of tier color.
- **Body (descriptors):** Geist sans 9px, weight 600 for group headers, weight 400 for details.
- **Example filenames:** Geist Mono 7.5px, fill=`#7a8399` (soft).

**Separator lines** (full-width hairlines inside card, 8px inset each side):
```svg
<line x1="{card_x+8}" y1="78" x2="{card_x+168}" y2="78"
      stroke="rgba({tier_rgb},0.20)" stroke-width="0.8"/>
```

### 3. Horizontal arrows

Simple lines crossing the 10px gap between cards, drawn **before zone rects** so zones cover the embedded endpoints.

```svg
<!-- Extends 1px beyond each card edge; zones cover the tips -->
<line x1="{left_card_right+1}" y1="230"
      x2="{right_card_left}" y2="230"
      stroke="#4f5d75" stroke-width="1" marker-end="url(#arrow)"/>
```

Arrow mid-y = 230 (vertical center of card area between top separator y=78 and bottom separator y=389: (78+389)/2 ≈ 233 → use 230 for cleaner math).

**Accent the Silver→Gold arrow** to signal the step from validated data into publication-ready statistics:
```svg
<line x1="557" y1="230" x2="566" y2="230"
      stroke="#eb6c36" stroke-width="1.2" marker-end="url(#arrow-accent)"/>
```

---

## ViewBox and sizing

```
ViewBox: 0 0 1000 488
Cards: y=20, h=408, bottom=428
Legend divider: y=436
Legend text row: y=452, items y=460–470
```

With 5 cards at 176px each (10px gaps, 8px left margin, 72px right margin): total = 1000px ✓.

To use 4 or 6 tiers, adjust card width or gap to keep total ≤ 1000px, or widen the viewBox.

---

## Dark mode

- Dots pattern: `rgba(245,245,245,0.10)`.
- Card fills: double the light opacity (e.g., 0.06 → 0.12).
- Card strokes: increase to 0.35 opacity, slightly lighten hue for teal/bronze.
- Body text: ink `#f5f5f5`, muted `#bfc0c0`, soft `#8e98ac`.
- Tier title colors: use lighter variants (`#f0607a`, `#c8a060`, `#22c0bb`, `#f08a59`, `#8e98ac`).
- Arrow marker fill: `#bfc0c0` (muted), `#f08a59` (accent).

---

## Legend

5 swatch items (colored rect + label) at the bottom, plus an accent line for the "tier upgrade" arrow. All items on one row (x=104 to x=960).

```svg
<rect x="104" y="460" width="18" height="10" rx="3" fill="{tier_fill}" stroke="{tier_stroke}" stroke-width="1"/>
<text x="130" y="470" fill="#4f5d75" font-size="8.5" font-family="'Geist', sans-serif">Raw</text>
```

---

## Anti-patterns

- **More than 6 tiers** — split into two diagrams or collapse adjacent tiers. Above 6 cards the content text becomes unreadable.
- **Same access label on adjacent tiers** — if two tiers share the same access policy, they may belong in one tier.
- **Multiple focal cards (accent borders)** — only the Gold / publication-ready tier gets the accent treatment.
- **Arrows between non-adjacent tiers** — all data flows linearly; a skip-tier arrow suggests a pipeline error, not a design feature.
- **Body text in tier colors** — only the title, sub-label, and access value use the tier color; all other text stays in ink/muted/soft.
- **Instrument Serif italic for ARCHIVE/RAW titles** — use Geist Mono uppercase instead to signal they are system/infrastructure tiers, not data-quality tiers.

## Examples

- `assets/example-data-org.html` — minimal light (D4N 5-tier: Raw, Bronze, Silver, Gold, Archive)
- `assets/example-data-org-dark.html` — minimal dark
- `assets/example-data-org-full.html` — full editorial with 5 tier detail cards
