# Feature Specifications

Complete feature documentation for the Level-Up Circle Generator.

## Circle Visualization

The core visualization renders colored points on a radial layout with a central ring, optional connection mesh, and labeled nodes.

### Points

- 1 to 36 points arranged evenly around a circle
- Each point has a customizable label and color
- Point size is adjustable (1–60 px radius)
- Points are rendered as filled circles with optional neon glow (multiple shadow layers)

### Connection Mesh

- When enabled, draws lines between every pair of points
- Lines are colored using a gradient between the two connected points' colors
- Optional neon glow effect on connection lines
- Can be toggled on/off via the "Show Connection Lines" checkbox

### Outer Circle Ring

- A ring drawn at the radius where points sit
- Colored white (dark theme) or dark gray (light theme)
- Optional neon glow effect

### Title

- Displayed centered at the top of the canvas
- Configurable font size (16–48 px)
- Optional neon glow effect
- Color adapts to theme (white on dark, black on light)

## Themes

Two themes are supported:

| Theme | Background | Text | Circle Ring |
|-------|------------|------|-------------|
| Dark | `#000000` | White | White |
| Light | `#ffffff` | Black | Dark gray |

Theme affects both the canvas rendering and the preview container background.

## Neon Glow Effects

Five independent glow toggles, each rendering multiple `shadowBlur` / `shadowColor` layers:

1. **Neon Glow Title** — Glow behind the title text
2. **Neon Glow Points** — Glow behind each point dot
3. **Neon Glow Labels** — Glow behind label text
4. **Neon Glow Circle** — Glow behind the outer ring
5. **Neon Glow Connections** — Glow behind connection lines

Glow colors match the element's own color (point color, white for title/circle on dark theme).

## Smart Label Wrapping

Labels are automatically wrapped to fit the radial layout. The algorithm prioritizes:

1. Keep short labels on a single line (if < 80% of max width)
2. Split at semantic connectors: `with`, `and`, `of`, `for`, `in`, `at`, `on`, `to`, `by`, `from`
3. Split two-word phrases between the words
4. For three words, try both 1+2 and 2+1 splits, choose the most balanced
5. For longer phrases, find the split point that minimizes line-length difference
6. Fall back to standard width-based wrapping

Labels are positioned outside the circle with leader lines connecting them to their point. Alignment is based on angular position (left/center/right).

## Color Generation

Colors are generated in HSL space for perceptual uniformity.

### Spacing Modes

- **Equal**: `360 / N` degrees between hues — even distribution
- **Golden**: `137.507764` degrees (golden angle) — optimal visual separation

The "Random Colors" button alternates between these two modes on each click.

### Parameters

- Saturation: ~0.85–0.90 (high, for vibrant colors)
- Lightness: 0.50 (maximum vibrancy)
- Start hue: random (or seeded for reproducibility)
- Adjacent duplicate prevention via hue nudging

## PNG Export

Export generates a high-resolution PNG image using an off-screen canvas.

### Resolution Options

| Option | Dimensions | Use Case |
|--------|-----------|----------|
| 1080 | 1080 x 1080 px | Social media posts |
| 2048 | 2048 x 2048 px | Presentations, web |
| 3840 | 3840 x 3840 px | Print, high-DPI displays |

### Export Behavior

- Creates an off-screen `<canvas>` at the target resolution
- Renders the circle using the same `drawCircle()` function with `isExport = true`
- All sizes, fonts, and glow effects scale proportionally
- Downloads automatically as `level-up-circle-{size}x{size}.png`
- Shows a success/failure toast notification

## Point Management

### Adding Points

- Click "Add Point" to append a new point (up to 36 max)
- New points get a golden-angle-spaced color and a default label (`Point N`)
- The "Number of Points" slider updates automatically

### Editing Points

- Each point has an inline text input for its label
- Each point has a color picker for its color
- Changes trigger an immediate canvas redraw and auto-save

### Deleting Points

- Click the "X" button to remove a point
- Cannot delete the last remaining point (button is disabled)
- The "Number of Points" slider updates automatically

### Number of Points Slider

- Range: 1–36
- Increasing adds new points with generated colors
- Decreasing removes points from the end

## Sharing

### Copy Link

- Compresses the current state into a URL parameter using lz-string
- Copies the full URL to the clipboard
- Shows a success toast notification
- Fullscreen state is included as `&fs=1` if active

### Twitter/X

- Opens a new tab with a pre-filled tweet
- Includes the circle title and a link to the shared configuration

### Facebook

- Opens the Facebook share dialog in a new tab
- Shares the URL with the compressed state

### Ask ChatGPT

- Opens ChatGPT in a new tab with a personalized strategy prompt
- Prompt includes the circle title, number of focus areas, and all labels
- See [ask-chatgpt-feature.md](ask-chatgpt-feature.md) for full details

## Fullscreen Mode

- Hides the controls panel and shows only the circle canvas
- Toggle via the fullscreen button in the share bar
- Preserved in shared URLs with `&fs=1`
- On mobile, also hides the mobile toggle button and scroll indicator

## Responsive Design

### Desktop (> 900px)

- Two-column grid: 400px controls sidebar | flexible preview area
- All sections expanded by default
- Full share bar visible

### Tablet (600–900px)

- Stacked layout: controls above, preview below
- Controls panel has a max-height of 40vh and scrolls
- Mobile toggle button appears to show/hide controls
- Scroll indicator shown when controls are visible

### Mobile (< 600px)

- Compact padding and spacing throughout
- Button grid becomes 2-column
- Font sizes reduced
- Toast notifications span full width
- Point items use a more compact layout

## Toast Notifications

Non-intrusive notifications that appear in the top-right corner.

### Types

| Type | Icon | Color |
|------|------|-------|
| Success | Checkmark | Green |
| Error | X mark | Red |
| Info | Info icon | Blue |

### Behavior

- Slides in from the right with a 300ms animation
- Auto-dismisses after 3 seconds
- Fades out with a 300ms animation
- Multiple toasts stack vertically

## Accessibility

- Canvas has `role="img"` and `aria-label="Level-Up Circle visualization"`
- Form inputs have associated `<label>` elements
- Semantic HTML with `<details>/<summary>` for collapsible sections
- Buttons have `title` attributes for tooltips
- Delete button disabled (not hidden) when only one point remains
