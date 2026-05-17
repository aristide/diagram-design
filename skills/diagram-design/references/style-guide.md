# Style Guide

**The single source of truth for colors, typography, and tokens.** Every diagram draws from this — not from hex values inlined in other reference files. If you want to change the visual skin of Schematic, change this file.

Default skin is a cool editorial palette — white-smoke paper, jet-black ink, atomic-tangerine accent, blue-slate muted. It's designed to look good out of the box; swap these values (or run [`onboarding.md`](onboarding.md)) and every new diagram inherits the new skin without touching any type-specific logic.

To generate your own from a website URL, see [`onboarding.md`](onboarding.md).

---

## Tokens

### Semantic roles

Every token is referred to by **semantic role**, not by its hex value. Type references (`type-*.md`) and SKILL.md say `accent`, not `#f7591f`.

| Role | Purpose | Default (light) | Default (dark) |
|---|---|---|---|
| `paper` | Page background, default node fill | `#F4F6FA` (D4N paper) | `#0B1F38` (D4N ink) |
| `paper-2` | Diagram container bg, secondary fill | `#E4E9F0` (D4N fog) | `#1a2e48` |
| `ink` | Primary text, primary stroke | `#0B1F38` (D4N ink) | `#F4F6FA` (D4N paper) |
| `muted` | Secondary text, default arrow stroke | `#5A6B82` (D4N slate) | `#8A99AE` (D4N steel) |
| `soft` | Sublabels, boundary labels | `#8A99AE` (D4N steel) | `#C8D1DD` (D4N mist) |
| `rule` | Hairline borders | `rgba(11,31,56,0.12)` | `rgba(244,246,250,0.12)` |
| `rule-solid` | Stronger borders, baselines | `#C8D1DD` (D4N mist) | `rgba(200,209,221,0.25)` |
| `accent` | Focal / 1–2 max per diagram | `#E63558` (D4N magenta) | `#EE5570` |
| `accent-tint` | Fill for accent-bordered boxes | `rgba(230,53,88,0.07)` | `rgba(238,85,112,0.10)` |
| `link` | HTTP/API calls, external arrows | `#167C7C` (D4N teal-deep) | `#1FA0A0` (D4N teal) |

> **Brand palette source:** this skin is sourced from the **D4N (Data4Now) brand** — navy `#0F3D6E` (letterforms, headlines), teal `#1FA0A0` (secondary structure), teal-deep `#167C7C` (links/external), magenta `#E63558` (single focal accent), near-black `#0B1F38` (ink), and cool-paper `#F4F6FA` (background). Fonts: **Montserrat** display/labels, **Roboto** body/callouts, **JetBrains Mono** technical sublabels. Onboarded from `data4now-design` skill — `colors_and_type.css`.

> **Note:** The pre-baked example HTML files in `assets/` were built under an earlier skin (Geist/Instrument Serif). New diagrams the skill produces will use the current D4N tokens above (Montserrat/JetBrains Mono).

### Inversion rule (light → dark)

Any `rgba(11,31,56, X)` in light becomes `rgba(244,246,250, X)` in dark. Same opacities, RGB flipped. The accent gets a slight hue-shift brighter to read on dark paper.

### Series palette (multi-series chart types only)

A small set of desaturated, editorial-tone colors for chart types that genuinely need to distinguish multiple overlapping entities (currently: **radar**). The "1-focal" rule still holds — `accent` is reserved for the focal series; the palette below covers the rest.

| Token | Light | Dark | Notes |
|---|---|---|---|
| `series-1` | `#7c8f6f` (sage) | `#9caf8f` | Non-focal series |
| `series-2` | `#5e7a9b` (dusty-blue) | `#82a0c0` | Non-focal series |
| `series-3` | `#b8915a` (mustard) | `#d3ad7a` | Non-focal series |
| `series-4` | `#9c6b50` (rust-brown) | `#b88670` | Non-focal series |
| `series-5` | `#6e6479` (slate) | `#8d8298` | Non-focal series |

Fills sit at `0.18` opacity light, `0.22` dark; strokes use the full color. **Don't backfill these tokens to non-chart types** — architecture, swimlane, etc. continue to use muted-ink variants. The series palette is opt-in for diagrams where overlapping shapes demand distinguishable color, not a license to add color elsewhere.

---

## Typography

| Role | Family | Size | Weight | Usage |
|---|---|---|---|---|
| `title` | Montserrat | 1.75rem | 700 | Page H1 |
| `node-name` | Montserrat | 12px | 600 | Human-readable labels |
| `sublabel` | JetBrains Mono | 9px | 400 | Port, protocol, URL, field type |
| `eyebrow` | JetBrains Mono | 7–8px | 500, tracked 0.18em, uppercase | Type tags, axis labels |
| `arrow-label` | JetBrains Mono | 8px | 400, tracked 0.06em | Arrow annotations |
| `callout` | Roboto *italic* | 14px | 400 | Editorial asides only |

### Font stack

```html
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&family=Roboto:ital,wght@0,400;0,500;1,400&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

**Load-bearing rule:** JetBrains Mono is for *technical* content (ports, commands, URLs, field types). Names go in Montserrat. Page title is Montserrat bold. Roboto italic is reserved for annotation callouts (see [primitive-annotation.md](primitive-annotation.md)).

---

## Stroke, radius, spacing

| Token | Value | Use |
|---|---|---|
| `stroke-thin` | `0.8` | Tag-box outlines, leaf nodes |
| `stroke-default` | `1` | Most strokes |
| `stroke-strong` | `1.2` | Emphasis strokes |
| `radius-sm` | `4` | Small tags |
| `radius-md` | `6` | Node boxes |
| `radius-lg` | `8` | Containers, rings |
| `grid` | `4` | Every coord, size, and gap is divisible by 4 (hard rule) |

---

## Node type → treatment

Semantic role combinations — reference these by name in type specs.

| Type | Fill | Stroke |
|---|---|---|
| `focal` (1–2 max) | `accent-tint` | `accent` |
| `backend` | `#ffffff` (white) | `ink` |
| `store` | `ink @ 0.05` | `muted` |
| `external` | `ink @ 0.03` | `ink @ 0.30` |
| `input` | `muted @ 0.10` | `soft` |
| `optional` | `ink @ 0.02` | `ink @ 0.20` dashed `4,3` |
| `security` | `accent @ 0.05` | `accent @ 0.50` dashed `4,4` |

---

## Customizing the skin

Three options:

1. **Run onboarding** — see [`onboarding.md`](onboarding.md). Drop a URL; the skill extracts the palette + fonts and rewrites this file.
2. **Edit by hand** — change the hex values in the tables above. Run the pre-output taste gate afterward to verify the accent still reads as "focal" against the new paper color.
3. **Brand handoff** — paste your existing design-token JSON into a new section here and map its tokens to the semantic roles above.

### Constraints (don't break these)

- **Contrast**: `ink` must hit WCAG AA on `paper`. `muted` must hit AA on `paper` for 11px+ text.
- **One accent**: pick one color for `accent`. Two accents erases the focal signal.
- **No rainbow palette**: if your brand ships 8 colors, pick 3 (paper, ink, accent). The rest become `muted` variants.
- **Serif + sans + mono**: three families, not more. If brand typography is all sans, keep Instrument Serif for `title` and `callout` anyway — the contrast is load-bearing.
- **Paper is warm-neutral, not pure white**: pure white turns the design sterile. Pick a cream, bone, or light grey with a hint of warmth.
- **Dot pattern is optional, not default**: the 22×22 dot pattern is an opt-in "dotted paper" variant (good for long-form editorial hero diagrams). The default background is a clean `paper` fill, no pattern. When the pattern is enabled, it should sit at ~10% opacity of `ink` on `paper` — visible but quiet.
- **Container is clean by default**: the diagram sits directly on the page paper, no secondary container background or border. A framed variant (`paper-2` bg + `rule` border + 8px radius + padding) is available as an opt-in for card-heavy layouts, but don't reach for it by default — the extra chrome fights the figure.
