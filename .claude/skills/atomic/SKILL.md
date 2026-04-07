---
name: atomic
description: Generate HTML using Atomic CSS utility classes. Use when user asks to create UI components, layouts, pages, or any HTML structure. Atomic CSS = class names that map directly to CSS properties using first-letter abbreviation rules.
argument-hint: "[describe the component or layout to build]"
---

# Atomic CSS Component Generator

> **One rule**: combine first letters of CSS property + value = class name.
> `display: flex` → `df`, `justify-content: center` → `jcc`, `align-items: center` → `aic`

For the complete class reference, see [reference.md](reference.md).
**Do NOT guess class names.** When unsure, ALWAYS use MCP `lookup_class` or `validate_classes` to verify before outputting.

## Absolute Rules

1. **HTML only** — never create separate CSS files
2. **Classes only** — never use inline `style` attributes
3. **Grid first** — use `dg` + `gtc` for layouts, not legacy float/flex grids
4. **Semantic tags** — use `header`, `nav`, `main`, `section`, `article`, `footer`
5. **Responsive** — always add `sm-`, `md-` prefixes for mobile/tablet
6. **4px spacing rhythm** — all spacing values in multiples of 4
7. **HEX 6-digit uppercase** — `cFFFFFF`, `bg000000`

## MCP Server

**MUST use** the `atomic-css` MCP server when you don't know a class. Never guess — always verify:
- `lookup_class` — class → CSS mapping (verify before using)
- `validate_classes` — validity check + suggestions for invalid classes
- `css_to_classes` — convert CSS declarations → Atomic classes
- `search_by_css` — find classes by CSS property name

```json
{ "mcpServers": { "atomic-css": { "type": "sse", "url": "https://mcp.atomiccss.dev/sse" } } }
```

---

## Core Naming Rule

**First letters of CSS property + value = class name.**

```
display: flex           → df        justify-content: center → jcc
align-items: flex-start → aifs      flex-direction: column  → fdc
white-space: nowrap     → wsn       text-overflow: ellipsis → toe
object-fit: cover       → ofc       border-collapse: collapse → bcc
table-layout: fixed     → tlf       user-select: none       → usn
pointer-events: none    → pen       background-size: cover  → bsc
```

This rule covers ALL CSS properties. If unsure, try the first-letter abbreviation first, then verify with MCP.

### Ambiguous Prefixes (unit changes meaning)
- `fs0` = `flex-shrink: 0` (no unit → flex-shrink) vs `fs16px` = `font-size: 16px` (with unit → font-size)
- `fw700` = `font-weight: 700` (number only, 100-900 in steps of 100)
- `o50` = `opacity: 0.5` (number 0-100, divided by 100)
- `zi10` = `z-index: 10` (number only)
- `order1` = `order: 1` (number only)

## Units

| Unit | Notation | Example |
|------|----------|---------|
| px | `px` | `w100px`, `gap8px` |
| % | `p` | `w50p` → `width: 50%` |
| rem | `rem` | `fs1-5rem` → `1.5rem` |
| em | `em` | `p1em` |
| vh/vw | `vh`, `vw` | `h100vh`, `w100vw` |
| vmin/vmax | `vmin`, `vmax` | `w50vmin` |
| fr | `fr` | `gtc1fr-2fr` |

> **rem base**: `html { font-size: 10px }` → `1rem = 10px`

### Unit Size Convention
| Size | Unit | Example |
|------|------|---------|
| Under 20px | `px` | `gap8px`, `p12px`, `br4px` |
| 20px+ | `rem` | `gap2rem`(20px), `p3-2rem`(32px) |

## Hyphens & Special Notation

| Use | Example | Result |
|-----|---------|--------|
| Decimal | `gap1-5rem` | `1.5rem` |
| Value separator | `gtc1fr-2fr-1fr` | `1fr 2fr 1fr` |
| Media prefix | `sm-dn` | `@media(max-width:768px)` |
| Pseudo-class | `hover-bg000000` | `:hover` |
| `i` suffix | `w100pxi` | `!important` |
| `neg-` prefix | `neg-mt10px` | `margin-top: -10px` |

---

## Unit-Based Properties (prefix + value + unit)

These prefixes accept any numeric value with a unit:

| Prefix | Property | | Prefix | Property |
|--------|----------|-|--------|----------|
| `w`/`h` | width/height | | `m`/`mt`/`mr`/`mb`/`ml` | margin |
| `minw`/`maxw` | min/max-width | | `p`/`pt`/`pr`/`pb`/`pl` | padding |
| `minh`/`maxh` | min/max-height | | `gap`/`rg`/`cg` | gap/row-gap/column-gap |
| `t`/`r`/`b`/`l` | top/right/bottom/left | | `br` | border-radius |
| `fs` (with unit) | font-size | | `lh` | line-height |
| `ls` | letter-spacing | | `ws` | word-spacing |
| `ti` | text-indent | | `bw`/`btw`/`brw`/`bbw`/`blw` | border-width |
| `bs` | border-spacing | | `ow`/`oo` | outline-width/offset |
| `gac`/`gar` | grid-auto-columns/rows | | `gcs`/`gce`/`grs`/`gre` | grid-column/row-start/end |
| `sm`/`smt`/`smr`/`smb`/`sml` | scroll-margin | | `sp`/`spt`/`spr`/`spb`/`spl` | scroll-padding |
| `per` | perspective | | `in` | inset |
| `cw`/`crw` | column-width/rule-width | | `tuo`/`tdt`/`tbs` | text-underline-offset/decoration-thickness/tab-size |

### Axis Shortcuts
`px20px` = padding-left + padding-right, `py10px` = padding-top + bottom
`mx2rem` = margin-left + right, `my1rem` = margin-top + bottom, `mxa` = margin-left+right: auto

### Shorthand Expansion
`p`, `m`, `gap`, `bw` expand to longhand. So `p20px pl10px` works — padding-left gets overridden.
Multi-value: `p10px-20px` → top/bottom:10px, left/right:20px (2/3/4 values follow CSS rules)

---

## Color (13+ prefixes)

| Prefix | Property | Prefix | Property |
|--------|----------|--------|----------|
| `c` | color | `bg` | background-color |
| `bc` | border-color | `btc`/`brc`/`bbc`/`blc` | border-direction-color |
| `olc` | outline-color | `tdc` | text-decoration-color |
| `ac` | accent-color | `cc` | caret-color |
| `sc` | scrollbar-color | | |
| `fill` | fill (SVG) | `stroke` | stroke (SVG) |

**HEX**: `cFFFFFF`, `bg000000` (6-digit, auto-uppercase)
**RGBA**: `c255-0-0-50` → `rgba(255,0,0,0.5)` (A = 0-100)
**Opacity**: `o50` → `opacity: 0.5`

---

## Complex Patterns (non-inferrable)

These patterns have special syntax that can't be guessed from the naming rule alone.

### Grid Templates
```
gtc1fr-2fr-1fr          → grid-template-columns: 1fr 2fr 1fr
gtcr3-1fr               → repeat(3, 1fr)
gtcrfit-minmax28rem-1fr  → repeat(auto-fit, minmax(28rem, 1fr))
gtcrfill-minmax250px-1fr → repeat(auto-fill, minmax(250px, 1fr))
gtcminmax100px-1fr       → minmax(100px, 1fr)
gtccalc100p-200px-1fr    → calc(100% - 200px) 1fr
gtrauto-1fr-auto         → grid-template-rows: auto 1fr auto
```

### Grid Placement
```
gc-1_3      → grid-column: 1 / 3
gc-span-2   → grid-column: span 2
gr-2_4      → grid-row: 2 / 4
ga-header   → grid-area: header
ga-1_1_2_3  → grid-area: 1 / 1 / 2 / 3
gta-header-header_sidebar-main_footer-footer → grid-template-areas
```

### Border Shorthand
```
b1pxsolidDDDDDD        → border: 1px solid #DDDDDD
bt2pxdashed000000      → border-top: 2px dashed #000000
b1pxsolid255-0-0-50    → border: 1px solid rgba(255,0,0,0.5)
```
Styles: solid, dashed, dotted, double, groove, ridge, inset, outset

### Individual Border Radius
`btlr8px` (top-left), `btrr8px` (top-right), `bblr8px` (bottom-left), `bbrr8px` (bottom-right)

### Shadow
```
bs2px4px10px0pxFF0000       → box-shadow: 2px 4px 10px 0px #FF0000
bs0px4px12px0px0-0-0-10     → box-shadow: ... rgba(0,0,0,0.1)
bsi0px2px4px0px000000       → box-shadow: inset ...
ts2px-2px-4px-DDDDDD        → text-shadow: 2px 2px 4px #DDDDDD (hyphen-separated)
```

### Text Stroke
`ts2pxFF0000` → `-webkit-text-stroke: 2px #FF0000`

### Outline Shorthand
`ol2pxsolidFF0000` → `outline: 2px solid #FF0000` (same format as border)

### Transition
```
tall200msease              → transition: all 200ms ease
topacity500ms              → transition: opacity 500ms
tall300mscb25-10-25-100    → transition: all 300ms cubic-bezier(0.25,0.1,0.25,1)
tde300ms                   → transition-delay: 300ms
tdu500ms                   → transition-duration: 500ms
tpopacity_transform        → transition-property: opacity, transform
```

### Transform (including 3D)
```
ttx10px / tty20px / ttz30px    → translateX/Y/Z
tr45deg / trx90deg / try180deg → rotate / rotateX/Y/Z
ts1-5 / ts1-5_2                → scale(1.5) / scale(1.5, 2)
tskx10deg / tsky20deg          → skewX / skewY
neg-ttx50p                     → translateX(-50%)
```

### Animation
```
anfadeIn-500ms-ease             → animation: fadeIn 500ms ease
anfadeIn-1s-ease-infinite       → animation: fadeIn 1s ease infinite
anfadeIn-500ms-ease-3-200ms-forwards-reverse
```
**Preset keyframes**: fadeIn, fadeOut, slideUp, slideDown, slideLeft, slideRight, zoomIn, zoomOut, spinCW, spinCCW, bounce, pulse, shake, flip, swing, rubberBand, blink

### Gradient
```
bglg-to-r-FF0000-0000FF           → linear-gradient(to right, #FF0000, #0000FF)
bglg-45deg-FF0000-00FF00-0000FF   → linear-gradient(45deg, ...)
bgrg-circle-FF0000-0000FF         → radial-gradient(circle, ...)
bgcg-from-0deg-FF0000-0000FF      → conic-gradient(from 0deg, ...)
```
Color stops with position: `FF0000_50p` = `#FF0000 50%`, `transparent` keyword supported.

### Filter / Backdrop Filter
```
filterb5px     → blur(5px)         bfb10px    → backdrop-filter: blur(10px)
filterbr120p   → brightness(120%)  filterg100p → grayscale(100%)
filterhr90deg  → hue-rotate(90deg) filterds2px4px8pxFF0000 → drop-shadow(...)
```
Prefix: `filter` for filter, `bf` for backdrop-filter. Sub-functions: b(blur), br(brightness), c(contrast), g(grayscale), hr(hue-rotate), i(invert), o(opacity), sa(saturate), s(sepia), ds(drop-shadow)

### calc
```
wcalc100p-200px       → width: calc(100% - 200px)
wcalc-add50p-100px    → calc(50% + 100px)
wcalc-mul2rem-3       → calc(2rem * 3)
wcalc-div100p-3       → calc(100% / 3)
hcalc100vh-6rem       → height: calc(100vh - 6rem)
```
Works with any unit-based property prefix (w, h, m, p, t, l, maxw, etc.)

### Line Clamp
`lc3-1-5rem` → 3 lines, line-height: 1.5rem (adds overflow:hidden, display:-webkit-box, etc.)

### Aspect Ratio
`ar16-9` → 16/9, `ar4-3` → 4/3, `ar1` → 1, `ara16-9` → auto 16/9

### CSS Variables
`prefix--variable-name` → `property: var(--variable-name)`
```html
<div class="bg--primary c--text gap--spacing br--radius">
```
Works with ALL prefixes, media queries, pseudo-classes, !important, before/after.
Shorthands decompose: `gap--x` → row-gap + column-gap, `p--x` → 4 directions.

---

## Media Queries (14 Breakpoints)

Prefix any class: `{media}-{class}` (max-width)

| Prefix | Width | Prefix | Width | Prefix | Width |
|--------|-------|--------|-------|--------|-------|
| `qh-` | 2560px | `el-` | 1440px | `md-` | 1024px |
| `uh-` | 2160px | `lg-` | 1280px | `sm-` | 768px |
| `kk-` | 2048px | `rg-` | 1080px | `es-` | 640px |
| `fh-` | 1920px | | | `us-` | 420px |
| `hl-` | 1800px | | | | |
| `sl-` | 1700px | | | | |
| `ul-` | 1600px | | | | |

## Pseudo-Classes (32)

Prefix any class: `{pseudo}-{class}`

**Interaction**: `hover-`, `focus-`, `active-`, `focus-within-`, `focus-visible-`
**Input**: `disabled-`, `enabled-`, `checked-`, `indeterminate-`, `default-`
**Validation**: `valid-`, `invalid-`, `required-`, `optional-`, `in-range-`, `out-of-range-`
**Link**: `link-`, `visited-`, `any-link-`, `target-`
**Other**: `placeholder-shown-`, `empty-`, `read-only-`, `read-write-`, `fullscreen-`, `autofill-`, `modal-`, `playing-`, `paused-`, `picture-in-picture-`, `user-invalid-`, `user-valid-`

## Pseudo-Elements

`before-{class}` → `::before`, `after-{class}` → `::after`
Use with `pr` (position:relative) on parent. `contentEmpty` sets `content:''`.

## Relationship Selectors

Pattern: `{parentClass}-{pseudo}-{combinator}-{targetTag}-{atomicClass}`

| Keyword | CSS | Example |
|---------|-----|---------|
| `child-{tag}-` | `> tag` | `card-hover-child-img-ts1-1` → `.card:hover > img { transform: scale(1.1) }` |
| `children-{tag}-` | ` tag` | `nav-hover-children-a-cFFFFFF` → `.nav:hover a { color: #FFF }` |
| `next-{tag}-` | `+ tag` | `input-focus-next-span-dn` → `.input:focus + span { display: none }` |
| `siblings-{tag}-` | `~ tag` | `checkbox-checked-siblings-div-db` → `.checkbox:checked ~ div { display: block }` |

The first segment (`card`, `nav`, `input`, `checkbox`) is the **parent element's class name**.

## neg- Allowed Properties

margin(m/mt/mr/mb/ml), top, right, bottom, left, letter-spacing, word-spacing, text-indent, outline-offset, z-index, order, translateX/Y, rotate

---

## User Request

$ARGUMENTS

## Generation Guidelines

1. **Atomic CSS classes only** — no inline styles, no CSS files
2. **Grid layout first** — `dg` + `gtc` for page structure
3. **Responsive by default** — add `md-` and `sm-` variants for key breakpoints
4. **Semantic HTML** — proper tags (header, nav, main, section, article, footer)
5. **Hover/focus states** — always add interaction feedback
6. **Transitions** — `tall200msease` or `tall300ms` for smooth UX
7. **4px spacing rhythm** — 4, 8, 12, 16, 20(2rem), 24(2-4rem), 32(3-2rem), 40(4rem)
8. **Under 20px → px, 20px+ → rem**
9. **When unsure** — use MCP `lookup_class` to verify, or check [reference.md](reference.md)

## Post-Generation Verification (MUST)

After generating HTML, ALWAYS perform these checks before presenting to user:

1. **Validate all classes** — run `validate_classes` with every Atomic class used. Fix any invalid ones.
2. **No inline styles** — confirm zero `style=` attributes in output
3. **No CSS files** — confirm no `<style>` tags or external CSS references
4. **Responsive check** — verify `sm-` or `md-` variants exist for key layout properties (grid columns, display, font-size, padding)
5. **Unit convention** — under 20px uses `px`, 20px+ uses `rem`
6. **Spacing rhythm** — all spacing values are multiples of 4
7. **HEX uppercase** — all color codes are 6-digit uppercase (`FFFFFF` not `ffffff`)
8. **Interaction states** — buttons/links have `hover-` and transition classes
9. **Semantic HTML** — proper tags used (not div-soup)

If MCP is unavailable, verify against [reference.md](reference.md) manually.

### Common Patterns
```html
<!-- Center -->
<div class="df jcc aic">

<!-- Responsive card grid (auto-fit) -->
<div class="dg gtcrfit-minmax28rem-1fr gap2rem">

<!-- Full-screen layout -->
<div class="dg gtrauto-1fr-auto h100vh">
  <header></header><main></main><footer></footer>
</div>

<!-- Sidebar (responsive) -->
<div class="dg gtc25rem-1fr gap2rem md-gtc1fr">
  <aside class="md-order2">Sidebar</aside>
  <main class="md-order1">Content</main>
</div>

<!-- Responsive hide -->
<div class="db sm-dn">Desktop only</div>
<div class="dn sm-db">Mobile only</div>

<!-- Hover button -->
<button class="bg007BFF hover-bg0056B3 cFFFFFF p12px-2-4rem br8px tall200msease cp">

<!-- Glassmorphism -->
<div class="bfb10px bg255-255-255-20 br16px p2-4rem">

<!-- Text ellipsis (3 lines) -->
<p class="lc3-1-5rem">

<!-- Centered container -->
<div class="mxa maxw120rem px2rem">
```
