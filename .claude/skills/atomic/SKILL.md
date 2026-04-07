---
name: atomic
description: Generate HTML using Atomic CSS utility classes. Use when user asks to create UI components, layouts, pages, or any HTML structure. Atomic CSS = class names that map directly to CSS properties using first-letter abbreviation rules.
argument-hint: "[describe the component or layout to build]"
---

# Atomic CSS Component Generator

> **One rule**: combine first letters of CSS property + value = class name.
> `display: flex` → `df`, `justify-content: center` → `jcc`, `align-items: center` → `aic`

## Absolute Rules

1. **HTML only** — never create separate CSS files
2. **Classes only** — never use inline `style` attributes
3. **No CSS variables** — use direct Atomic classes instead of `var(--name)`
4. **Grid first** — use `dg` + `gtc` for layouts, not legacy float/flex grids
5. **Semantic tags** — use `header`, `nav`, `main`, `section`, `article`, `footer`
6. **Responsive** — always consider `sm-`, `md-` prefixes for mobile/tablet

## MCP Server

If available, use the `atomic-css` MCP server to look up or validate classes:
- `lookup_class` — verify class → CSS mapping
- `validate_classes` — check if classes are valid
- `css_to_classes` — convert CSS to Atomic classes
- `search_by_css` — find classes by CSS property name

MCP config (`.mcp.json`):
```json
{
    "mcpServers": {
        "atomic-css": {
            "type": "sse",
            "url": "https://mcp.atomiccss.dev/sse"
        }
    }
}
```

## Naming System

### Core Rule
**First letters of CSS property + value:**
```
display: flex           → df
justify-content: center → jcc
align-items: flex-start → aifs
flex-direction: column  → fdc
white-space: nowrap     → wsn
text-overflow: ellipsis → toe
object-fit: cover       → ofc
border-collapse: collapse → bcc
table-layout: fixed     → tlf
```

### Units
| Unit | Notation | Example |
|------|----------|---------|
| px | `px` | `w100px`, `gap8px`, `fs14px` |
| % | `p` | `w50p` → `width: 50%` |
| rem | `rem` | `fs1-5rem` → `font-size: 1.5rem` |
| em | `em` | `p1em` |
| vh/vw | `vh`, `vw` | `h100vh`, `w100vw` |
| fr | `fr` | `gtc1fr-2fr` |

> **rem base**: `html { font-size: 10px }` → `1rem = 10px`, `2rem = 20px`

### Unit Rules
| Size | Use | Example |
|------|-----|---------|
| Under 20px | `px` | `gap8px`, `p12px`, `br4px` |
| 20px and above | `rem` | `gap2rem`(20px), `p3-2rem`(32px), `w10rem`(100px) |

### Spacing (multiples of 4)
`4px`, `8px`, `12px`, `16px`, `2rem`(20px), `2-4rem`(24px), `3-2rem`(32px), `4rem`(40px)

### Hyphen (-) Uses
| Purpose | Example | Result |
|---------|---------|--------|
| Decimal | `gap1-5rem` | `1.5rem` |
| Value separator | `gtc1fr-2fr-1fr` | `1fr 2fr 1fr` |
| Media prefix | `sm-dn` | `@media(max-width:768px) display:none` |
| Pseudo-class | `hover-bg000000` | `:hover background-color:#000` |

### Special Notation
| Notation | Meaning | Example |
|----------|---------|---------|
| `i` suffix | `!important` | `w100pxi` → `width:100px !important` |
| `neg-` prefix | Negative | `neg-mt10px` → `margin-top:-10px` |

---

## Complete Class Reference

### Display
| Class | CSS |
|-------|-----|
| `df` | `display: flex` |
| `dif` | `display: inline-flex` |
| `dg` | `display: grid` |
| `db` | `display: block` |
| `dib` | `display: inline-block` |
| `di` | `display: inline` |
| `dn` | `display: none` |
| `dt` | `display: table` |
| `dtc` | `display: table-cell` |

### Position
| Class | CSS |
|-------|-----|
| `pst` | `position: static` |
| `pr` | `position: relative` |
| `pa` | `position: absolute` |
| `pf` | `position: fixed` |
| `ps` | `position: sticky` |
| `t0`/`r0`/`b0`/`l0` | `top/right/bottom/left: 0` |
| `ta`/`ra`/`ba`/`la` | `top/right/bottom/left: auto` |

### Flexbox
| Class | CSS |
|-------|-----|
| `fdr` / `fdrr` | `flex-direction: row / row-reverse` |
| `fdc` / `fdcr` | `flex-direction: column / column-reverse` |
| `fwn` / `fww` / `fwr` | `flex-wrap: nowrap / wrap / wrap-reverse` |
| `fa` / `fi` | `flex: auto / initial` |
| `fs0` / `fs1` | `flex-shrink: 0 / 1` |
| `fg1` / `fg2` | `flex-grow: 1 / 2` |
| `fbauto` / `fb200px` | `flex-basis: auto / 200px` |
| `flex1-1-auto` | `flex: 1 1 auto` |

### Justify Content
| Class | CSS |
|-------|-----|
| `jcfs` | `justify-content: flex-start` |
| `jcfe` | `justify-content: flex-end` |
| `jcc` | `justify-content: center` |
| `jcsb` | `justify-content: space-between` |
| `jcsa` | `justify-content: space-around` |
| `jcse` | `justify-content: space-evenly` |

### Align Items / Content / Self
| Class | CSS |
|-------|-----|
| `ais` / `aifs` / `aife` / `aic` / `aib` | `align-items: stretch/flex-start/flex-end/center/baseline` |
| `acs` / `acfs` / `acfe` / `acc` | `align-content: stretch/flex-start/flex-end/center` |
| `asa` / `asfs` / `asfe` / `asc` | `align-self: auto/flex-start/flex-end/center` |

### Grid Template Columns (gtc)
| Pattern | Example | CSS |
|---------|---------|-----|
| Direct | `gtc1fr-2fr-1fr` | `1fr 2fr 1fr` |
| Fixed+ratio | `gtc200px-auto-1fr` | `200px auto 1fr` |
| repeat | `gtcr3-1fr` | `repeat(3, 1fr)` |
| auto-fit | `gtcrfit-minmax28rem-1fr` | `repeat(auto-fit, minmax(28rem, 1fr))` |
| auto-fill | `gtcrfill-minmax250px-1fr` | `repeat(auto-fill, minmax(250px, 1fr))` |
| minmax | `gtcminmax100px-1fr` | `minmax(100px, 1fr)` |

### Grid Template Rows (gtr)
| Pattern | Example | CSS |
|---------|---------|-----|
| Direct | `gtrauto-1fr-auto` | `auto 1fr auto` |
| repeat | `gtrr3-1fr` | `repeat(3, 1fr)` |

### Spacing (Units required)
| Prefix | Property |
|--------|----------|
| `m`, `mt`, `mr`, `mb`, `ml` | margin |
| `p`, `pt`, `pr`, `pb`, `pl` | padding |
| `gap`, `rg`, `cg` | gap, row-gap, column-gap |
| `px`, `py`, `mx`, `my` | x-axis/y-axis shortcuts |
| `mta`, `mra`, `mba`, `mla` | margin-direction: auto |
| `mxa` | margin-left + margin-right: auto |

> **Shorthand expansion**: `p20px pl10px` works — shorthand expands to longhand, then `pl` overrides.
> Multi-value: `p10px-20px` → top/bottom:10px, left/right:20px

### Sizing
| Prefix | Property |
|--------|----------|
| `w`, `h` | width, height |
| `minw`, `maxw` | min-width, max-width |
| `minh`, `maxh` | min-height, max-height |
| `wa`, `ha` | width/height: auto |

### Color
| Pattern | Example | CSS |
|---------|---------|-----|
| `c` + HEX | `cFFFFFF` | `color: #FFFFFF` |
| `bg` + HEX | `bg000000` | `background-color: #000000` |
| `bc` + HEX | `bcDDDDDD` | `border-color: #DDDDDD` |
| RGBA | `c255-0-0-50` | `color: rgba(255,0,0,0.5)` |
| RGBA bg | `bg0-0-0-80` | `background-color: rgba(0,0,0,0.8)` |
| Opacity | `o50`, `o80` | `opacity: 0.5 / 0.8` |

> Auto-uppercase: `cffffff` → `cFFFFFF`

### CSS Variables
Pattern: `prefix--variable-name` → `property: var(--variable-name)`

```html
<div class="bg--primary c--text gap--spacing br--radius">
```

Works with all prefixes, media queries, pseudo-classes, !important, before/after.

### Typography
| Class | CSS |
|-------|-----|
| `fs16px` / `fs1-6rem` | `font-size` |
| `fw400` / `fw700` | `font-weight` |
| `lh1-5` / `lh24px` | `line-height` |
| `ls1px` | `letter-spacing: 1px` |
| `tac` / `tal` / `tar` / `taj` | `text-align` |
| `tdn` / `tdu` / `tdlt` | `text-decoration: none/underline/line-through` |
| `ttu` / `ttl` / `ttc` | `text-transform: uppercase/lowercase/capitalize` |
| `wsn` / `wsp` / `wspw` | `white-space: nowrap/pre/pre-wrap` |
| `toe` / `toc` | `text-overflow: ellipsis/clip` |
| `wbba` / `wbka` | `word-break: break-all/keep-all` |

### Border
| Pattern | Example | CSS |
|---------|---------|-----|
| Shorthand | `b1pxsolidDDDDDD` | `border: 1px solid #DDDDDD` |
| Direction | `bt2pxdashed000000` | `border-top: 2px dashed #000` |
| Radius | `br8px`, `br50p` | `border-radius` |
| None | `bn` | `border: none` |
| Direction 0 | `bt0`, `bb0`, `bl0` | `border-top/bottom/left: 0` |
| Styles | solid, dashed, dotted, double, groove, ridge, inset, outset |

### Shadow
| Pattern | Example | CSS |
|---------|---------|-----|
| Box (HEX) | `bs2px4px10px0pxFF0000` | `box-shadow: 2px 4px 10px 0px #FF0000` |
| Box (RGBA) | `bs0px4px12px0px0-0-0-10` | `box-shadow: 0px 4px 12px rgba(0,0,0,0.1)` |
| Inset | `bsi0px2px4px0px000000` | `box-shadow: inset ...` |
| Text | `ts2px-2px-4px-DDDDDD` | `text-shadow: 2px 2px 4px #DDD` |
| None | `bsn` | `box-shadow: none` |

### Transform
| Class | CSS |
|-------|-----|
| `ttx10px` / `tty20px` | `translateX(10px)` / `translateY(20px)` |
| `neg-ttx50p` | `translateX(-50%)` |
| `tr45deg` | `rotate(45deg)` |
| `ts1-5` / `ts1-5_2` | `scale(1.5)` / `scale(1.5, 2)` |

### Transition
| Class | CSS |
|-------|-----|
| `tall200msease` | `transition: all 200ms ease` |
| `tall300ms` | `transition: all 300ms` |
| `topacity500ms` | `transition: opacity 500ms` |
| `tnone` | `transition: none` |

### Filter / Backdrop Filter
| Class | CSS |
|-------|-----|
| `filterb5px` | `filter: blur(5px)` |
| `filterbr120p` | `filter: brightness(120%)` |
| `filterc80p` | `filter: contrast(80%)` |
| `filterg100p` | `filter: grayscale(100%)` |
| `filtern` | `filter: none` |
| `bfb10px` | `backdrop-filter: blur(10px)` |

### calc (Arithmetic)
| Pattern | Example | CSS |
|---------|---------|-----|
| Subtract | `wcalc100p-200px` | `width: calc(100% - 200px)` |
| Add | `wcalc-add50p-100px` | `width: calc(50% + 100px)` |
| Multiply | `wcalc-mul2rem-3` | `width: calc(2rem * 3)` |
| Divide | `wcalc-div100p-3` | `width: calc(100% / 3)` |

### Overflow / Cursor / Visibility
| Class | CSS |
|-------|-----|
| `oh` / `oa` / `os` | `overflow: hidden/auto/scroll` |
| `oxh` / `oyh` | `overflow-x/y: hidden` |
| `cp` / `cd` / `cm` | `cursor: pointer/default/move` |
| `vh` / `vv` | `visibility: hidden/visible` |
| `usn` | `user-select: none` |
| `pen` / `pea` | `pointer-events: none/auto` |

### Background
| Class | CSS |
|-------|-----|
| `bsc` / `bsct` | `background-size: cover/contain` |
| `bgrn` / `bgrr` | `background-repeat: no-repeat/repeat` |
| `bgpc` / `bgpt` / `bgpb` | `background-position: center/top/bottom` |
| `bgn` | `background: none` |

### Other
| Class | CSS |
|-------|-----|
| `ar16-9` / `ar4-3` / `ar1` | `aspect-ratio` |
| `lc3-1-5rem` | Line clamp (3 lines, lh: 1.5rem) |
| `zi10` / `zi999` | `z-index` |
| `order1` / `neg-order1` | `order: 1 / -1` |
| `bxzbb` | `box-sizing: border-box` |
| `appn` | `appearance: none` |

---

## Media Queries (14 Breakpoints)

Prefix any class: `{prefix}-{class}`

| Prefix | Width | | Prefix | Width |
|--------|-------|-|--------|-------|
| `qh-` | 2560px | | `rg-` | 1080px |
| `uh-` | 2160px | | `md-` | 1024px |
| `kk-` | 2048px | | `sm-` | 768px |
| `fh-` | 1920px | | `es-` | 640px |
| `hl-` | 1800px | | `us-` | 420px |
| `sl-` | 1700px | | | |
| `ul-` | 1600px | | | |
| `el-` | 1440px | | | |
| `lg-` | 1280px | | | |

```html
<div class="dg gtcr3-1fr md-gtcr2-1fr sm-gtc1fr gap2rem">
```

## Pseudo-Classes (32)

Prefix any class: `{pseudo}-{class}`

| Category | Prefixes |
|----------|----------|
| Interaction | `hover-`, `focus-`, `active-`, `focus-within-`, `focus-visible-` |
| Input | `disabled-`, `enabled-`, `checked-`, `indeterminate-` |
| Validation | `valid-`, `invalid-`, `required-`, `optional-`, `in-range-`, `out-of-range-` |
| Link | `link-`, `visited-`, `any-link-`, `target-` |
| Other | `placeholder-shown-`, `empty-`, `read-only-`, `fullscreen-`, `autofill-`, `modal-` |

## Pseudo-Elements

| Keyword | CSS | Example |
|---------|-----|---------|
| `before-` | `::before` | `before-bg000000` |
| `after-` | `::after` | `after-cFFFFFF` |

## Relationship Selectors

| Combinator | Keyword | CSS |
|------------|---------|-----|
| `>` direct child | `child` | `.parent > child` |
| ` ` descendant | `children` | `.parent child` |
| `+` adjacent sibling | `next` | `.el + sibling` |
| `~` general sibling | `siblings` | `.el ~ siblings` |

```html
<!-- On card hover, scale child img -->
<div class="card-hover-child-img-ts1-1">
```

---

## Output Rules

$ARGUMENTS

---

### Generation Guidelines

1. **Always use Atomic CSS classes** — no inline styles, no CSS files
2. **Grid layout first** — `dg` + `gtc` for page structure
3. **Responsive by default** — add `md-` and `sm-` variants
4. **Semantic HTML** — proper tags (header, nav, main, section, footer)
5. **Hover/focus states** — add interaction feedback with pseudo prefixes
6. **Transitions** — `tall200msease` or `tall300ms` for smooth interactions
7. **4px spacing rhythm** — use multiples of 4 for all spacing values
8. **Colors as HEX 6-digit** — `cFFFFFF`, `bg000000`, uppercase
9. **Under 20px → px, 20px+ → rem** — follow unit convention

### Common Patterns
```html
<!-- Center align -->
<div class="df jcc aic">

<!-- Responsive card grid -->
<div class="dg gtcrfit-minmax28rem-1fr gap2rem">

<!-- Full-screen layout -->
<div class="dg gtrauto-1fr-auto h100vh">
  <header></header>
  <main></main>
  <footer></footer>
</div>

<!-- Sidebar layout -->
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
