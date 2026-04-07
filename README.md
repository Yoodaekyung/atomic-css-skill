# Atomic CSS Skill for Claude Code

A Claude Code skill that enables AI to generate HTML using [Atomic CSS](https://atomiccss.dev) utility classes with 100% accuracy.

## What it does

When you type `/atomic`, Claude generates HTML components using only Atomic CSS classes — no inline styles, no separate CSS files. Class names directly map to CSS properties using a first-letter abbreviation system.

```
/atomic Create a responsive card grid with hover effects
```

Claude will output:
```html
<div class="dg gtcrfit-minmax28rem-1fr gap2rem p2rem">
    <div class="bgFFFFFF br8px p2rem bs0px4px12px0px0-0-0-10 hover-bs0px8px24px0px0-0-0-20 tall200msease">
        <h3 class="fs2rem fw700 mb8px c333333">Card Title</h3>
        <p class="fs1-4rem lh1-6 c666666">Card content</p>
    </div>
</div>
```

## Installation

### Option 1: Plugin Marketplace

```
/plugin marketplace add Yoodaekyung/atomic-css-skill
/plugin install atomic@Yoodaekyung-atomic-css-skill
```

### Option 2: Clone directly

```bash
git clone https://github.com/Yoodaekyung/atomic-css-skill.git
cd atomic-css-skill
# Skills are immediately available
```

### Option 3: Copy to your project

Copy `.claude/skills/atomic/` to your project's `.claude/skills/` directory.

## MCP Server (Optional, Recommended)

For real-time class validation and lookup, add the MCP server to your project's `.mcp.json`:

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

This gives Claude access to:
- `lookup_class` — verify any class → CSS mapping
- `validate_classes` — check validity + suggest fixes
- `css_to_classes` — convert CSS declarations to Atomic classes
- `search_by_css` — find classes by CSS property name

## Usage Examples

```
/atomic Login page with email/password form
/atomic Responsive navbar with logo and hamburger menu
/atomic Pricing table with 3 tiers
/atomic Dashboard layout with sidebar and header
/atomic Blog post card with image, title, excerpt, and author
```

## Key Features

- **Complete class reference** — 300+ fixed classes + unlimited dynamic patterns
- **14 responsive breakpoints** — 420px to 2560px
- **32 pseudo-classes** — hover, focus, disabled, valid, etc.
- **Grid-first layouts** — modern CSS Grid over legacy float/flex
- **MCP integration** — real-time class validation via atomiccss.dev

## Links

- [Official Docs](https://atomiccss.dev)
- [VS Code Extension](https://marketplace.visualstudio.com/items?itemName=Drangon-Knight.atomicss)
- [npm CLI](https://www.npmjs.com/package/atomic-css-cli)
- [MCP Server](https://mcp.atomiccss.dev)

## License

MIT
