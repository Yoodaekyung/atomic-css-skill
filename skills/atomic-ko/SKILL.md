---
name: atomic-ko
description: Atomic CSS 유틸리티 클래스를 사용하여 HTML을 생성합니다. UI 컴포넌트, 레이아웃, 페이지 또는 HTML 구조를 만들 때 사용하세요. Atomic CSS = CSS 속성의 앞글자 약어 규칙으로 직접 매핑되는 클래스명입니다.
argument-hint: "[구축할 컴포넌트 또는 레이아웃을 설명하세요]"
---

# Atomic CSS 컴포넌트 생성기

> **하나의 규칙**: CSS 속성 + 값의 앞글자를 조합하면 클래스명이 됩니다.
> `display: flex` → `df`, `justify-content: center` → `jcc`, `align-items: center` → `aic`

전체 클래스 레퍼런스는 [reference.md](reference.md)를 참고하세요.
**클래스명을 추측하지 마세요 — 안다고 확신하더라도.** 반드시 MCP `lookup_class`, `validate_classes`, 또는 `css_to_classes`로 검증 후 사용하세요. 예외 없음.

## 절대 규칙

1. **HTML만 사용** — 별도 CSS 파일을 생성하지 마세요
2. **클래스만 사용** — 인라인 `style` 속성을 사용하지 마세요
3. **Grid 우선** — 레이아웃에는 레거시 float/flex 그리드 대신 `dg` + `gtc` 사용
4. **시맨틱 태그** — `header`, `nav`, `main`, `section`, `article`, `footer` 사용
5. **반응형 필수** — 모바일/태블릿을 위해 `sm-`, `md-` 프리픽스 항상 추가
6. **4px 간격 리듬** — 모든 간격 값은 4의 배수로
7. **HEX 6자리 대문자** — `cFFFFFF`, `bg000000`

## MCP 서버 (필수)

**항상** `atomic-css` MCP 서버를 사용하세요 — 안다고 확신하는 클래스도 예외 없이. 확신은 검증을 대체할 수 없습니다.
- `lookup_class` — 클래스 → CSS 매핑 (사용 전 반드시 검증)
- `validate_classes` — 일괄 유효성 검사 + 무효 클래스 대안 제안
- `css_to_classes` — CSS 선언을 Atomic 클래스로 변환 (Step 1에서 사용)
- `search_by_css` — CSS 속성명으로 클래스 검색

```json
{ "mcpServers": { "atomic-css": { "type": "sse", "url": "https://mcp.atomiccss.dev/sse" } } }
```

> **왜 필수인가?** 클래스명은 코드로 생성되며 미묘한 엣지 케이스(단위 모호성, 축약형 분해, 특수 문법)가 있습니다. 사람처럼 추측하면 그럴듯하지만 틀린 클래스가 나옵니다. MCP 서버가 유일한 정답 소스입니다.

---

## 핵심 네이밍 규칙

**CSS 속성 + 값의 앞글자 = 클래스명.**

```
display: flex           → df        justify-content: center → jcc
align-items: flex-start → aifs      flex-direction: column  → fdc
white-space: nowrap     → wsn       text-overflow: ellipsis → toe
object-fit: cover       → ofc       border-collapse: collapse → bcc
table-layout: fixed     → tlf       user-select: none       → usn
pointer-events: none    → pen       background-size: cover  → bsc
```

이 규칙은 모든 CSS 속성에 적용됩니다. 추론만으로 사용하지 말고, 반드시 MCP로 검증하세요.

### 의미가 달라지는 접두사 (단위 유무에 따라)
- `fs0` = `flex-shrink: 0` (단위 없음 → flex-shrink) vs `fs16px` = `font-size: 16px` (단위 있음 → font-size)
- `fw700` = `font-weight: 700` (숫자만, 100-900 범위 100 단위)
- `o50` = `opacity: 0.5` (0-100 범위, 100으로 나눔)
- `zi10` = `z-index: 10` (숫자만)
- `order1` = `order: 1` (숫자만)

## 단위

| 단위 | 표기법 | 예시 |
|------|--------|------|
| px | `px` | `w100px`, `gap8px` |
| % | `p` | `w50p` → `width: 50%` |
| rem | `rem` | `fs1-5rem` → `1.5rem` |
| em | `em` | `p1em` |
| vh/vw | `vh`, `vw` | `h100vh`, `w100vw` |
| vmin/vmax | `vmin`, `vmax` | `w50vmin` |
| fr | `fr` | `gtc1fr-2fr` |

> **rem 기준**: `html { font-size: 10px }` → `1rem = 10px`

### 단위 크기 규칙
| 크기 | 단위 | 예시 |
|------|------|------|
| 20px 미만 | `px` | `gap8px`, `p12px`, `br4px` |
| 20px 이상 | `rem` | `gap2rem`(20px), `p3-2rem`(32px) |

## 하이픈 및 특수 표기

| 용도 | 예시 | 결과 |
|------|------|------|
| 소수점 | `gap1-5rem` | `1.5rem` |
| 값 구분 | `gtc1fr-2fr-1fr` | `1fr 2fr 1fr` |
| 미디어 프리픽스 | `sm-dn` | `@media(max-width:768px)` |
| 의사 클래스 | `hover-bg000000` | `:hover` |
| `i` 접미사 | `w100pxi` | `!important` |
| `neg-` 접두사 | `neg-mt10px` | `margin-top: -10px` |

---

## 단위 기반 속성 (접두사 + 값 + 단위)

이 접두사들은 단위와 함께 숫자 값을 받습니다:

| 접두사 | 속성 | | 접두사 | 속성 |
|--------|------|-|--------|------|
| `w`/`h` | width/height | | `m`/`mt`/`mr`/`mb`/`ml` | margin |
| `minw`/`maxw` | min/max-width | | `p`/`pt`/`pr`/`pb`/`pl` | padding |
| `minh`/`maxh` | min/max-height | | `gap`/`rg`/`cg` | gap/row-gap/column-gap |
| `t`/`r`/`b`/`l` | top/right/bottom/left | | `br` | border-radius |
| `fs` (단위 있을 때) | font-size | | `lh` | line-height |
| `ls` | letter-spacing | | `ws` | word-spacing |
| `ti` | text-indent | | `bw`/`btw`/`brw`/`bbw`/`blw` | border-width |
| `bs` | border-spacing | | `ow`/`oo` | outline-width/offset |
| `gac`/`gar` | grid-auto-columns/rows | | `gcs`/`gce`/`grs`/`gre` | grid-column/row-start/end |
| `sm`/`smt`/`smr`/`smb`/`sml` | scroll-margin | | `sp`/`spt`/`spr`/`spb`/`spl` | scroll-padding |
| `per` | perspective | | `in` | inset |
| `cw`/`crw` | column-width/rule-width | | `tuo`/`tdt`/`tbs` | text-underline-offset/decoration-thickness/tab-size |

### 축 단축키
`px20px` = padding-left + padding-right, `py10px` = padding-top + bottom
`mx2rem` = margin-left + right, `my1rem` = margin-top + bottom, `mxa` = margin-left+right: auto

### 축약형 분해
`p`, `m`, `gap`, `bw`는 longhand로 분해됩니다. 따라서 `p20px pl10px`이 작동합니다 — padding-left가 덮어쓰입니다.
다중 값: `p10px-20px` → top/bottom:10px, left/right:20px (2/3/4값은 CSS 규칙을 따름)

---

## 색상 (13개 이상의 접두사)

| 접두사 | 속성 | 접두사 | 속성 |
|--------|------|--------|------|
| `c` | color | `bg` | background-color |
| `bc` | border-color | `btc`/`brc`/`bbc`/`blc` | border-방향-color |
| `olc` | outline-color | `tdc` | text-decoration-color |
| `ac` | accent-color | `cc` | caret-color |
| `sc` | scrollbar-color | | |
| `fill` | fill (SVG) | `stroke` | stroke (SVG) |

**HEX**: `cFFFFFF`, `bg000000` (6자리, 자동 대문자)
**RGBA**: `c255-0-0-50` → `rgba(255,0,0,0.5)` (A = 0-100)
**Opacity**: `o50` → `opacity: 0.5`

---

## 복합 패턴 (추론 불가능한 패턴)

네이밍 규칙만으로는 추측할 수 없는 특수 문법을 가진 패턴들입니다.

### Grid 템플릿
```
gtc1fr-2fr-1fr          → grid-template-columns: 1fr 2fr 1fr
gtcr3-1fr               → repeat(3, 1fr)
gtcrfit-minmax28rem-1fr  → repeat(auto-fit, minmax(28rem, 1fr))
gtcrfill-minmax250px-1fr → repeat(auto-fill, minmax(250px, 1fr))
gtcminmax100px-1fr       → minmax(100px, 1fr)
gtccalc100p-200px-1fr    → calc(100% - 200px) 1fr
gtrauto-1fr-auto         → grid-template-rows: auto 1fr auto
```

### Grid 배치
```
gc-1_3      → grid-column: 1 / 3
gc-span-2   → grid-column: span 2
gr-2_4      → grid-row: 2 / 4
ga-header   → grid-area: header
ga-1_1_2_3  → grid-area: 1 / 1 / 2 / 3
gta-header-header_sidebar-main_footer-footer → grid-template-areas
```

### Border 축약형
```
b1pxsolidDDDDDD        → border: 1px solid #DDDDDD
bt2pxdashed000000      → border-top: 2px dashed #000000
b1pxsolid255-0-0-50    → border: 1px solid rgba(255,0,0,0.5)
```
스타일: solid, dashed, dotted, double, groove, ridge, inset, outset

### 개별 Border Radius
`btlr8px` (좌상단), `btrr8px` (우상단), `bblr8px` (좌하단), `bbrr8px` (우하단)

### 그림자
```
bs2px4px10px0pxFF0000       → box-shadow: 2px 4px 10px 0px #FF0000
bs0px4px12px0px0-0-0-10     → box-shadow: ... rgba(0,0,0,0.1)
bsi0px2px4px0px000000       → box-shadow: inset ...
ts2px-2px-4px-DDDDDD        → text-shadow: 2px 2px 4px #DDDDDD (하이픈 구분)
```

### Text Stroke
`ts2pxFF0000` → `-webkit-text-stroke: 2px #FF0000`

### Outline 축약형
`ol2pxsolidFF0000` → `outline: 2px solid #FF0000` (border와 동일한 형식)

### Transition
```
tall200msease              → transition: all 200ms ease
topacity500ms              → transition: opacity 500ms
tall300mscb25-10-25-100    → transition: all 300ms cubic-bezier(0.25,0.1,0.25,1)
tde300ms                   → transition-delay: 300ms
tdu500ms                   → transition-duration: 500ms
tpopacity_transform        → transition-property: opacity, transform
```

### Transform (3D 포함)
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
**프리셋 키프레임**: fadeIn, fadeOut, slideUp, slideDown, slideLeft, slideRight, zoomIn, zoomOut, spinCW, spinCCW, bounce, pulse, shake, flip, swing, rubberBand, blink

### 그라디언트
```
bglg-to-r-FF0000-0000FF           → linear-gradient(to right, #FF0000, #0000FF)
bglg-45deg-FF0000-00FF00-0000FF   → linear-gradient(45deg, ...)
bgrg-circle-FF0000-0000FF         → radial-gradient(circle, ...)
bgcg-from-0deg-FF0000-0000FF      → conic-gradient(from 0deg, ...)
```
위치가 포함된 색상 정지점: `FF0000_50p` = `#FF0000 50%`, `transparent` 키워드 지원.

### Filter / Backdrop Filter
```
filterb5px     → blur(5px)         bfb10px    → backdrop-filter: blur(10px)
filterbr120p   → brightness(120%)  filterg100p → grayscale(100%)
filterhr90deg  → hue-rotate(90deg) filterds2px4px8pxFF0000 → drop-shadow(...)
```
접두사: filter는 `filter`, backdrop-filter는 `bf`. 하위 함수: b(blur), br(brightness), c(contrast), g(grayscale), hr(hue-rotate), i(invert), o(opacity), sa(saturate), s(sepia), ds(drop-shadow)

### calc
```
wcalc100p-200px       → width: calc(100% - 200px)
wcalc-add50p-100px    → calc(50% + 100px)
wcalc-mul2rem-3       → calc(2rem * 3)
wcalc-div100p-3       → calc(100% / 3)
hcalc100vh-6rem       → height: calc(100vh - 6rem)
```
모든 단위 기반 속성 접두사와 함께 사용 가능 (w, h, m, p, t, l, maxw 등)

### Line Clamp
`lc3-1-5rem` → 3줄, line-height: 1.5rem (overflow:hidden, display:-webkit-box 등 자동 추가)

### Aspect Ratio
`ar16-9` → 16/9, `ar4-3` → 4/3, `ar1` → 1, `ara16-9` → auto 16/9

### CSS 변수
`prefix--variable-name` → `property: var(--variable-name)`
```html
<div class="bg--primary c--text gap--spacing br--radius">
```
모든 접두사, 미디어 쿼리, 의사 클래스, !important, before/after와 함께 사용 가능.
축약형 분해: `gap--x` → row-gap + column-gap, `p--x` → 4방향.

---

## 미디어 쿼리 (14개 브레이크포인트)

모든 클래스에 프리픽스로 적용: `{미디어}-{클래스}` (max-width)

| 프리픽스 | 너비 | 프리픽스 | 너비 | 프리픽스 | 너비 |
|---------|------|---------|------|---------|------|
| `qh-` | 2560px | `el-` | 1440px | `md-` | 1024px |
| `uh-` | 2160px | `lg-` | 1280px | `sm-` | 768px |
| `kk-` | 2048px | `rg-` | 1080px | `es-` | 640px |
| `fh-` | 1920px | | | `us-` | 420px |
| `hl-` | 1800px | | | | |
| `sl-` | 1700px | | | | |
| `ul-` | 1600px | | | | |

## 의사 클래스 (32개)

모든 클래스에 프리픽스로 적용: `{의사클래스}-{클래스}`

**인터랙션**: `hover-`, `focus-`, `active-`, `focus-within-`, `focus-visible-`
**입력 요소**: `disabled-`, `enabled-`, `checked-`, `indeterminate-`, `default-`
**폼 검증**: `valid-`, `invalid-`, `required-`, `optional-`, `in-range-`, `out-of-range-`
**링크**: `link-`, `visited-`, `any-link-`, `target-`
**기타**: `placeholder-shown-`, `empty-`, `read-only-`, `read-write-`, `fullscreen-`, `autofill-`, `modal-`, `playing-`, `paused-`, `picture-in-picture-`, `user-invalid-`, `user-valid-`

## 의사 요소

`before-{클래스}` → `::before`, `after-{클래스}` → `::after`
부모에 `pr` (position:relative) 사용. `contentEmpty`로 `content:''` 설정.

## 관계 선택자

패턴: `{부모클래스}-{의사}-{결합자}-{대상태그}-{Atomic클래스}`

| 키워드 | CSS | 예시 |
|--------|-----|------|
| `child-{태그}-` | `> tag` | `card-hover-child-img-ts1-1` → `.card:hover > img { transform: scale(1.1) }` |
| `children-{태그}-` | ` tag` | `nav-hover-children-a-cFFFFFF` → `.nav:hover a { color: #FFF }` |
| `next-{태그}-` | `+ tag` | `input-focus-next-span-dn` → `.input:focus + span { display: none }` |
| `siblings-{태그}-` | `~ tag` | `checkbox-checked-siblings-div-db` → `.checkbox:checked ~ div { display: block }` |

첫 번째 세그먼트(`card`, `nav`, `input`, `checkbox`)는 **부모 요소의 클래스명**입니다.

## neg- 허용 속성

margin(m/mt/mr/mb/ml), top, right, bottom, left, letter-spacing, word-spacing, text-indent, outline-offset, z-index, order, translateX/Y, rotate

---

## 사용자 요청

$ARGUMENTS

## 필수 3단계 워크플로우

> **3단계를 반드시 순서대로 수행하세요. 어떤 단계도 건너뛰면 안 됩니다.**

### Step 1: MCP 조회 (HTML 작성 전에 반드시)

필요한 CSS 속성을 파악한 뒤, MCP로 올바른 클래스를 조회합니다:

1. 컴포넌트에 필요한 CSS 속성 목록 작성 (레이아웃, 간격, 색상, 타이포그래피 등)
2. `css_to_classes`로 CSS 선언 → Atomic 클래스 변환
3. 특정 클래스가 확실하지 않으면 `lookup_class`로 검증
4. CSS 속성의 접두사를 모르면 `search_by_css`로 검색

**MCP로 검증된 클래스를 확보하기 전까지 HTML을 작성하지 마세요.**

### Step 2: HTML 생성

Step 1에서 MCP로 검증된 클래스만 사용하여:

1. **Atomic CSS 클래스만 사용** — 인라인 스타일, CSS 파일 금지
2. **Grid 레이아웃 우선** — 페이지 구조에 `dg` + `gtc` 사용
3. **기본적으로 반응형** — 주요 브레이크포인트에 `md-`, `sm-` 변형 추가
4. **시맨틱 HTML** — 적절한 태그 사용 (header, nav, main, section, article, footer)
5. **Hover/Focus 상태** — 항상 인터랙션 피드백 추가
6. **트랜지션** — 부드러운 UX를 위해 `tall200msease` 또는 `tall300ms`
7. **4px 간격 리듬** — 4, 8, 12, 16, 20(2rem), 24(2-4rem), 32(3-2rem), 40(4rem)
8. **20px 미만 → px, 20px 이상 → rem**

### Step 3: MCP 검증 (사용자에게 제시하기 전에 반드시)

1. **생성된 HTML에서 모든 Atomic 클래스를 수집**
2. **`validate_classes` 실행** — 전체 클래스 목록으로 일괄 검증. 무효한 클래스는 MCP 제안으로 수정
3. **자체 점검** (MCP 불필요):
   - `style=` 속성이 0개인지 확인
   - `<style>` 태그나 외부 CSS 없는지 확인
   - 주요 레이아웃 속성에 `sm-` 또는 `md-` 변형 있는지 확인
   - 단위 규칙 (20px 미만 → px, 20px 이상 → rem)
   - 간격 값이 4의 배수인지 확인
   - HEX 색상이 6자리 대문자인지 확인
   - 버튼/링크에 `hover-`와 transition 클래스 있는지 확인
   - 시맨틱 HTML 태그 사용 여부

MCP를 사용할 수 없는 경우, [reference.md](reference.md)로 수동 검증하세요 — 단, 이는 폴백이지 기본이 아닙니다.

### 자주 사용하는 패턴
```html
<!-- 센터 정렬 -->
<div class="df jcc aic">

<!-- 반응형 카드 그리드 (auto-fit) -->
<div class="dg gtcrfit-minmax28rem-1fr gap2rem">

<!-- 풀스크린 레이아웃 -->
<div class="dg gtrauto-1fr-auto h100vh">
  <header></header><main></main><footer></footer>
</div>

<!-- 사이드바 (반응형) -->
<div class="dg gtc25rem-1fr gap2rem md-gtc1fr">
  <aside class="md-order2">사이드바</aside>
  <main class="md-order1">콘텐츠</main>
</div>

<!-- 반응형 숨김 -->
<div class="db sm-dn">데스크탑만 표시</div>
<div class="dn sm-db">모바일만 표시</div>

<!-- 호버 버튼 -->
<button class="bg007BFF hover-bg0056B3 cFFFFFF p12px-2-4rem br8px tall200msease cp">

<!-- 글래스모피즘 -->
<div class="bfb10px bg255-255-255-20 br16px p2-4rem">

<!-- 텍스트 말줄임 (3줄) -->
<p class="lc3-1-5rem">

<!-- 가운데 정렬 컨테이너 -->
<div class="mxa maxw120rem px2rem">
```
