# Level-Up Circle Generator

Interactive single-page tool for designing and exporting a "Level-Up Circle" — a visual map of interconnected goals or initiatives. Built as a self‑contained HTML Canvas app (`levelup-circle-generator.html`) with minimal external dependencies.

**🚀 Try it now:** [https://chiefinnovator.github.io/levelupcircle/levelup-circle-generator.html](https://chiefinnovator.github.io/levelupcircle/levelup-circle-generator.html)

## Features
- Up to 36 labeled points arranged radially
- Smart color generation (equal or golden-angle spacing) for vivid, distinct colors
- Dark and light theme support with theme-aware preview background
- Independent glow effects for title, points, labels, circle, and connections
- Automatic multi-line smart label wrapping
- Adjustable point size, label offset spacing, label font size, and title font size
- Complete connection mesh toggle (show/hide)
- High‑resolution PNG export (1080, 2048, 3840 square)
- **Deep linking** with compressed URL state sharing (lz-string)
- **Fullscreen mode** for presentations and embedding
- **Social sharing** buttons (Copy Link, Twitter/X, Facebook)
- Auto-save to `localStorage` on every change
- Responsive resizing with `ResizeObserver`
- Mobile-friendly with collapsible controls panel

## Quick Start
1. Open `levelup-circle-generator.html` in any modern browser (Chrome, Edge, Safari, Firefox). No build step required.
2. Adjust the controls in the left panel.
3. Click Export PNG to download a high‑resolution image.
4. Use the share buttons to copy a link or share to social media.

## Sharing & Deep Links
- **Copy Share Link**: Copies a URL containing your entire configuration. Recipients see your exact circle.
- **Fullscreen Mode**: Toggle fullscreen to hide controls and show only the circle. Share links preserve this mode.
- **Twitter/X & Facebook**: Share directly to social platforms (requires deployed URL, not localhost).

URLs use lz-string compression for compact sharing. Legacy URL format is still supported for backwards compatibility.

## Core Controls
| Control | Description |
|---------|-------------|
| Title | Text at top (glow optional) |
| Theme | Dark or Light background |
| Number of Points | 1–36 nodes around the circle |
| Export Size | Output canvas dimension (square) |
| Point Size | Radius of each goal node |
| Label Spacing | Distance of label boxes from point edge |
| Label Font Size | Size of point label text (Advanced) |
| Title Font Size | Size of title text (Advanced) |
| Neon Glow Title | Toggle glow for title text |
| Neon Glow Points | Toggle glow for point dots |
| Neon Glow Labels | Toggle glow for label text |
| Neon Glow Circle | Toggle glow for the ring |
| Neon Glow Connections | Toggle glow for connection lines |
| Show Connection Lines | Toggle full interconnection mesh |
| 🎨 Random Colors | Re‑generate colors (alternates spacing mode) |
| 📋 Load Example | Reset to seeded demo state |
| 💾 Save State | Persist current configuration to `localStorage` |
| 🗑️ Clear State | Clear saved state and reload example |
| 🔄 Redraw | Force redraw the canvas |
| 📸 Export PNG | Generate and download image |

## Share Bar Buttons
| Button | Description |
|--------|-------------|
| 🔗 Copy Link | Copy shareable URL to clipboard (includes fullscreen state) |
| 𝕏 Twitter | Share to Twitter/X with pre-filled text |
| f Facebook | Share to Facebook |
| ⛶ Fullscreen | Toggle fullscreen presentation mode |

## Color Algorithm
Colors are generated in HSL space:
- Start hue: random or seeded
- Spacing mode: `equal` (360/N) or `golden` (≈137.507°)
- High saturation (≈0.85–0.90) & balanced lightness (0.50) for vibrancy
- Adjacent duplicate prevention with hue nudging

## Label Wrapping
Custom smart wrapping tries semantic split points (connector words, balanced halves) before falling back to a traditional width-based wrapper. Ensures readable two-line labels without overflowing the radial layout.

## Export Notes
- Export uses an off-screen canvas at target resolution
- Glow and stroke effects scale proportionally
- Filenames: `level-up-circle-<size>x<size>.png`

## State Persistence
- **Auto-save**: Configuration automatically saves to `localStorage` on every change
- **Manual save**: Use 💾 Save State for explicit save with confirmation
- **Deep links**: Share configuration via compressed URL parameters
- Backward compatibility logic normalizes older glow property names

## Dependencies
- [lz-string](https://github.com/pieroxy/lz-string) v1.5.0 (CDN) - URL compression for deep linking

## Project Structure
```
levelup-circle-generator.html  # Main application
level_up_circle_blog.md        # Long-form article explaining the concept
Rich/                          # Image assets (reference examples)
```

## Concept Article
See `level_up_circle_blog.md` for the philosophy behind interconnected goal design ("Level-Up Circle" / synergy mapping / flywheel thinking).

## Roadmap Ideas
- Drag-and-drop manual reordering or angular offset per point
- Selective connection modes (adjacent, strategic, custom)
- Import/export JSON configuration files
- SVG export for print / vector workflows
- Accessibility: keyboard navigation & reduced motion mode
- Open Graph image generation for social previews

## License
No license is granted. All rights reserved. The source code, images, article content, and generated outputs may not be copied, modified, redistributed, published, or used (commercially or non‑commercially) without the explicit prior written permission of the author.

Permitted use: You may view and run the HTML locally for personal evaluation only. Any other usage requires written authorization.

**Copyright © 2025 Richard Crane. All Rights Reserved.**

## Attribution
Inspired by synergy / flywheel diagrams (Disney, Amazon) and goal compounding principles.

## Contributing
Open an issue or submit a PR with focused changes (feature, refactor, or doc update).

---
**Copyright © 2025 Richard Crane – Unauthorized use prohibited.**