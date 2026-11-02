# Architecture

Technical overview of the Level-Up Circle Generator.

## Single-File Application

The entire application is contained in `levelup-circle-generator.html` (~3,000 lines). This includes:

- **HTML** — UI structure, form controls, canvas element
- **CSS** — Styling, responsive layout, animations, themes
- **JavaScript** — State management, canvas rendering, sharing, persistence

This design eliminates build tools, bundlers, and module systems. The app works by opening the HTML file directly in a browser.

## Entry Point

`index.html` serves as a redirect to `levelup-circle-generator.html` using both a `<meta http-equiv="refresh">` tag and a JavaScript fallback.

## Code Organization

Within `levelup-circle-generator.html`, the code is structured in this order:

### 1. Head / Metadata (lines 1–21)

- Open Graph and Twitter Card meta tags for social previews
- lz-string CDN script for URL compression

### 2. CSS (lines 22–900)

- Reset and base styles
- Three-panel grid layout (`.container`: 320px settings | 1fr preview | 320px point config)
- Form control styling (inputs, selects, sliders, checkboxes)
- Button grid with gradient hover effects
- Collapsible `<details>/<summary>` sections
- Toast notification system (slide-in from right)
- Floating share bar (positioned top-right of preview)
- Panel visibility classes (`hide-left`, `hide-right`, `hide-both`) for the panel toggle
- Responsive breakpoints at 900px and 600px

### 3. HTML Body (lines 900–910)

- Toast container (positioned fixed, top-right)
- Controls panel with collapsible sections
- Point list container with add button
- Canvas element with `role="img"` and `aria-label`
- Floating share bar with 5 buttons (Copy Link, Twitter, Facebook, ChatGPT, Panel Toggle)

### 4. JavaScript (lines 911–end)

- Color generation algorithm (`generateBrightColors`, `hslToHex`)
- State initialization and default labels
- Smart label wrapping (`smartWrapLabel`, `wrapText`)
- Bounding box calculation for labels
- Point management (add, delete, update label/color)
- Canvas rendering (`drawCircle`)
- Export to PNG or WebP (`exportImage`; metadata embedded in PNG only)
- State persistence (localStorage auto-save, manual save/clear)
- URL encoding/decoding for sharing (`encodeStateToURL`, `decodeStateFromCompressed`)
- Social sharing functions (clipboard, Twitter, Facebook, ChatGPT)
- Panel toggle (`cyclePanelMode`: both → left → right → none; replaces the old fullscreen button)
- ResizeObserver for responsive redraw
- Event listeners and initialization

## Rendering Pipeline

`drawCircle(ctx, size)` draws everything in an 800×800 logical space under a single `ctx.scale(size / 800, size / 800)`, using the authored values (`pointSize`, `labelOffset`, `labelFontSize`, `titleFontSize`) directly. `shadowBlur` is not affected by the transform, so each glow radius is multiplied by `ctx.getTransform().a` (current pixels per unit, including the device pixel ratio). The preview and every export are therefore uniform scalings of the same drawing; line breaks are identical at every size.

Draw order:

1. **Clear canvas** — Fill with theme-appropriate background color
2. **Draw title** — Centered title text at top with optional neon glow
3. **Draw outer circle** — Ring around the center with optional neon glow
4. **Draw connection lines** — If enabled, draw lines between all point pairs with optional neon glow
5. **For each point, in order** — Draw the colored dot, then its guide (leader) line, then its smart-wrapped label, each with optional neon glow

For export, `exportImage()` creates an off-screen canvas at the target resolution (1080/2048/3840/5120/7680/8192) and calls the same `drawCircle(ctx, exportSize)`.

## State Flow

All load paths converge on one normalizer and one form sync:

```text
URL s=… (decodeCompressed) ─┐
URL p=… (decodeLegacy) ─────┤
localStorage (JSON.parse) ──┼─► normalizeState(raw) ─► state ─► syncUIFromState() ─► updatePointList() ─► redraw()
Clear / Load Example ───────┘
```

`normalizeState` clamps and defaults every field (see [state-management.md](state-management.md)); `syncUIFromState` writes every control and readout.

## Color Generation

The `generateBrightColors(n, options)` function:

- Operates in HSL color space for perceptual uniformity
- Supports two spacing modes:
  - **Equal**: `360 / N` degrees between hues
  - **Golden**: `137.507764` degrees (golden angle) for optimal visual separation
- Uses high saturation (~0.85–0.90) and 50% lightness for vibrancy
- Supports seeded random start hue for reproducibility
- Prevents adjacent duplicate colors by nudging hue when collisions occur
- Returns both hex colors and HSL values

## Label Wrapping Algorithm

`smartWrapLabel(text, ctx, maxWidth)` uses a multi-strategy approach:

1. **Single line** — If text fits within 80% of max width, keep it on one line
2. **Semantic split** — Look for connecting words (`with`, `and`, `of`, `for`, etc.) and split there
3. **Two-word split** — Always split two-word phrases between the words
4. **Three-word balanced split** — Try both 1+2 and 2+1 splits, pick the most balanced
5. **Multi-word balanced split** — Find the split point that minimizes line-length difference
6. **Fallback** — Use standard width-based wrapping (`wrapText`)

Labels are positioned radially with alignment based on quadrant:
- **Right side** (roughly 10 o'clock to 2 o'clock): left-aligned
- **Left side** (roughly 4 o'clock to 8 o'clock): right-aligned
- **Top/bottom**: center-aligned

## Responsive Design

The layout uses CSS Grid with two responsive breakpoints:

| Breakpoint | Layout |
|------------|--------|
| > 900px (desktop) | `320px \| 1fr \| 320px` three-panel grid |
| 600–900px (tablet) | Stacked, controls collapse to 40vh max-height |
| < 600px (mobile) | Compact padding, 2-column button grid, full-width toasts |

A `ResizeObserver` on the preview container triggers `redraw()` when the viewport changes. On mobile, a toggle button and scroll indicator help manage the controls panel.

## External Dependencies

| Dependency | Load Method | Purpose |
|------------|-------------|---------|
| lz-string v1.5.0 | CDN `<script>` tag | Compress/decompress state for URL sharing |

No other external dependencies. All rendering uses native Canvas 2D API. All persistence uses native `localStorage`. All sharing uses native `window.open` and `navigator.clipboard`.

## Security

- Point labels are HTML-escaped before insertion into the DOM to prevent XSS
- Color values are validated as 6-digit hex (`/^#[0-9A-Fa-f]{6}$/`) at every input boundary (URL decoders, localStorage, color inputs)
- Toast messages are set with `textContent`, never `innerHTML`
- URL state is parsed through `JSON.parse` of decompressed lz-string data with error handling
