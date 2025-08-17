# Level-Up Circle Generator

Interactive single-page tool for designing and exporting a "Level-Up Circle" — a visual map of interconnected goals or initiatives. Built as a self‑contained HTML Canvas app (`levelup-circle-generator.html`) with no external dependencies.

## Features
- Up to 36 labeled points arranged radially
- Smart color generation (equal or golden-angle spacing) for vivid, distinct colors
- Independent glow effects for points/labels and lines/circle
- Automatic multi-line smart label wrapping
- Adjustable point size & label offset spacing
- Complete connection mesh toggle (show/hide)
- High‑resolution PNG export (1080, 2048, 3840 square)
- Local state save/restore via `localStorage`
- Responsive resizing with `ResizeObserver`

## Quick Start
1. Open `levelup-circle-generator.html` in any modern browser (Chrome, Edge, Safari, Firefox). No build step.
2. Adjust the controls in the left panel.
3. Click Export PNG to download a high‑resolution image.

## Core Controls
| Control | Description |
|---------|-------------|
| Title | Text at top (glow optional) |
| Number of Points | 1–36 nodes around the circle |
| Export Size | Output canvas dimension (square) |
| Point Size | Radius of each goal node |
| Label Spacing | Distance of label boxes from point edge |
| Neon Glow Points & Labels | Toggle glow for points + text |
| Neon Glow Circle & Lines | Toggle glow for ring + connections |
| Show Connection Lines | Toggle full interconnection mesh |
| 🎨 Random Colors | Re‑generate colors (alternates spacing mode) |
| 📋 Load Example | Reset to seeded demo state |
| 💾 Save State | Persist current configuration to `localStorage` |
| 🗑️ Clear State | Clear saved state and reload example |
| 📸 Export PNG | Generate and download image |

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
Configuration object is serialized to `localStorage` under key `levelUpCircleState`. Backward compatibility logic normalizes older glow property names.

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
- Dark/light theme toggle
- SVG export for print / vector workflows
- Accessibility: keyboard navigation & reduced motion mode

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