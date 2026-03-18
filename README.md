# Level-Up Circle Generator

Interactive single-page tool for designing and exporting a "Level-Up Circle" — a visual map of interconnected goals, projects, or focus areas that reinforce each other through synergy and compounding growth. Built as a self-contained HTML Canvas app with minimal external dependencies.

**Try it now:** [https://chiefinnovator.github.io/levelupcircle/levelup-circle-generator.html](https://chiefinnovator.github.io/levelupcircle/levelup-circle-generator.html)

## Features

- Up to 36 labeled points arranged radially around a central circle
- Smart color generation (equal or golden-angle spacing) for vivid, distinct colors
- Dark and light theme support with theme-aware preview background
- Independent neon glow effects for title, points, labels, circle, and connections
- Automatic multi-line smart label wrapping with semantic split points
- Adjustable point size, label offset spacing, label font size, and title font size
- Complete connection mesh toggle (show/hide lines between all points)
- High-resolution PNG export (1080, 2048, 3840 square)
- Deep linking with compressed URL state sharing (lz-string)
- Fullscreen mode for presentations and embedding
- Social sharing buttons (Copy Link, Twitter/X, Facebook)
- Ask ChatGPT for personalized strategic advice on your focus areas
- Auto-save to `localStorage` on every change
- Responsive resizing with `ResizeObserver`
- Mobile-friendly with collapsible controls panel and scroll indicator
- Toast notification system for user feedback
- XSS-safe label rendering with HTML entity escaping

## Quick Start

1. Open `levelup-circle-generator.html` in any modern browser (Chrome, Edge, Safari, Firefox). No build step required.
2. Adjust the controls in the left panel — title, theme, number of points, glow effects, etc.
3. Edit point labels and colors directly in the point list.
4. Click **Export PNG** to download a high-resolution image.
5. Use the share buttons to copy a link or share to social media.

## Controls Reference

### Basic Settings

| Control | Description |
|---------|-------------|
| Title | Text displayed at top of the circle (glow optional) |
| Theme | Dark (black) or Light (white) background |

### Advanced Settings (collapsible)

| Control | Range | Description |
|---------|-------|-------------|
| Number of Points | 1–36 | Nodes arranged around the circle |
| Export Size | 1080 / 2048 / 3840 | Output canvas dimension (square, in pixels) |
| Point Size | 1–60 | Radius of each goal node |
| Label Spacing | 0–100 | Distance of labels from point edge |
| Label Font Size | 8–32 | Size of point label text |
| Title Font Size | 16–48 | Size of title text |

### Glow Effects

| Control | Description |
|---------|-------------|
| Neon Glow Title | Toggle glow for title text |
| Neon Glow Points | Toggle glow for point dots |
| Neon Glow Labels | Toggle glow for label text |
| Neon Glow Circle | Toggle glow for the outer ring |
| Neon Glow Connections | Toggle glow for connection lines |
| Show Connection Lines | Toggle full interconnection mesh |

### Action Buttons

| Button | Description |
|--------|-------------|
| Random Colors | Re-generate colors (alternates spacing mode) |
| Load Example | Reset to seeded demo state |
| Save State | Persist configuration to `localStorage` with confirmation |
| Clear State | Clear saved state and reload example |
| Redraw | Force redraw the canvas |
| Export PNG | Generate and download image |

### Share Bar (floating, top-right)

| Button | Description |
|--------|-------------|
| Copy Link | Copy shareable URL to clipboard (includes fullscreen state) |
| Twitter/X | Share to Twitter/X with pre-filled text |
| Facebook | Share to Facebook |
| ChatGPT | Ask ChatGPT for strategic advice on your focus areas |
| Fullscreen | Toggle fullscreen presentation mode (hides controls) |

## Sharing & Deep Links

- **Copy Share Link**: Copies a URL containing your entire circle configuration. Recipients see your exact circle.
- **Fullscreen Mode**: Toggle fullscreen to hide controls and show only the circle. Share links preserve this mode with `&fs=1`.
- **Twitter/X & Facebook**: Share directly to social platforms (requires deployed URL, not localhost).
- **Ask ChatGPT**: Opens a new browser tab with a personalized strategy prompt based on your circle's title and focus areas.

URLs use lz-string compression for compact sharing. Legacy URL format is supported for backwards compatibility.

## Project Structure

```
levelupcircle/
├── levelup-circle-generator.html   # Main application (single-file app)
├── index.html                      # Redirect page to main app
├── README.md                       # This file
├── level_up_circle_blog.md         # Philosophy article on synergy mapping
├── preview.png                     # Open Graph / social media preview image
├── docs/
│   ├── architecture.md             # Technical architecture & rendering pipeline
│   ├── features.md                 # Complete feature specifications
│   ├── url-sharing-spec.md         # URL sharing & deep linking specification
│   ├── state-management.md         # State schema & persistence details
│   ├── ask-chatgpt-feature.md      # ChatGPT integration documentation
│   └── roadmap.md                  # UI enhancement plan & roadmap
├── Rich/                           # Reference example images
│   ├── level-up-circle-3840x3840 no lines.jpg
│   ├── level-up-circle-3840x3840 no lines.png
│   ├── level-up-circle-3840x3840 w lines.jpg
│   └── level-up-circle-3840x3840 w lines.png
└── .gitignore
```

## Dependencies

| Dependency | Version | Purpose |
|------------|---------|---------|
| [lz-string](https://github.com/pieroxy/lz-string) | 1.5.0 (CDN) | URL compression for deep linking |

No build tools, package managers, or frameworks required. Works standalone in any modern browser.

## Documentation

Detailed specifications are in the [docs/](docs/) folder:

- **[Architecture](docs/architecture.md)** — Technical overview, rendering pipeline, and code organization
- **[Features](docs/features.md)** — Complete feature specifications and behavior details
- **[URL Sharing](docs/url-sharing-spec.md)** — Deep linking format, compression, and legacy support
- **[State Management](docs/state-management.md)** — State schema, localStorage persistence, auto-save
- **[Ask ChatGPT](docs/ask-chatgpt-feature.md)** — ChatGPT integration and prompt template
- **[Roadmap](docs/roadmap.md)** — Planned enhancements and completion status

## Concept & Philosophy

See [level_up_circle_blog.md](level_up_circle_blog.md) for the philosophy behind interconnected goal design. The Level-Up Circle represents a shift from isolated goal-setting to synergistic systems thinking, inspired by flywheel diagrams (Disney, Amazon) and goal compounding principles.

## Browser Support

Modern browsers supporting HTML5 Canvas, localStorage, and ResizeObserver:
- Chrome / Edge (Chromium)
- Safari
- Firefox

## License

No license is granted. All rights reserved. The source code, images, article content, and generated outputs may not be copied, modified, redistributed, published, or used (commercially or non-commercially) without the explicit prior written permission of the author.

Permitted use: You may view and run the HTML locally for personal evaluation only. Any other usage requires written authorization.

**Copyright &copy; 2025 Richard Crane. All Rights Reserved.**
