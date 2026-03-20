# Level-Up Circle Generator

## Project Overview

A single-file HTML5 Canvas web app that creates radial synergy diagrams (Level-Up Circles) for goal visualization. Hosted on GitHub Pages at `chiefinnovator.github.io/levelupcircle/`.

## Architecture

- **Single-file app**: All HTML, CSS, and JS live in `levelup-circle-generator.html` (~3,015 lines)
- **index.html**: Redirect page that sends users to the generator
- **about.html**: About page explaining the strategic frameworks behind the tool
- **articles.html**: Article listing page
- **article.html**: Article viewer — fetches markdown files and renders with marked.js
- **No build step**: Pure vanilla HTML/CSS/JS, no frameworks or bundlers
- **CDN dependencies** (pin these versions):
  - lz-string 1.5.0 — URL state compression
  - marked.js 12.0.1 — Markdown rendering for article viewer

## Key Technical Details

- HTML5 Canvas with HiDPI/Retina support (`devicePixelRatio`)
- CSS Grid 3-column layout: left settings panel, center preview, right point config
- Panel visibility cycles through 4 modes: both/left/right/none via CSS class toggling
- Export supports PNG, WebP, AVIF up to 8192x8192 at 300 DPI with embedded metadata

### State Management

- All state lives in a single `state` object
- **Load priority**: URL params (compressed) → localStorage → default example
- Auto-saves to localStorage on every redraw (debounced 500ms)
- URL sharing compresses state with abbreviated keys via lz-string

### Canvas Rendering Pipeline (draw order matters)

1. Clear canvas (theme background color)
2. Draw connection lines (optional neon glow)
3. Draw outer circle ring (optional neon glow)
4. Draw point dots (optional neon glow)
5. Draw smart-wrapped labels with leader lines (optional neon glow)
6. Draw title (optional neon glow)

## File Structure

```text
levelup-circle-generator.html  — Main application (single file)
index.html                     — SEO redirect to generator
about.html                     — About page with framework origins
articles.html                  — Article listing page
article.html                   — Markdown article viewer (uses marked.js)
level_up_circle_blog.md        — Original blog article
level_up_circle_blog_v2.md     — V2 blog article (tool rebuild)
preview.png                    — OG/social preview image
Rich/                          — Reference images
docs/
  architecture.md              — Code organization & rendering pipeline
  features.md                  — Feature specifications (1-36 points, themes, glow toggles)
  state-management.md          — State schema & persistence details
  url-sharing-spec.md          — URL encoding/compression spec
  ask-chatgpt-feature.md       — ChatGPT integration design
  roadmap.md                   — Completed & planned features
```

## Local Development

No build step required. Serve files with any static server:

```bash
python3 -m http.server 8000
# or
npx serve .
```

## Conventions

- Dark theme by default (#0a0a0a background)
- UI changes call `redraw()` which triggers `debouncedAutoSave()`
- Toast notifications for user actions (save, export, share)
- No toast for panel visibility cycling or auto-save
- 5 independent neon glow toggles: title, points, labels, circle, connections
- Color generation uses HSL space with equal or golden-angle spacing modes

## Git Rules

- **NEVER rebase** without explicit user permission
- If branches diverge, STOP and ask the user how to handle it
- Commit messages should be descriptive with bullet points for multiple changes

## SEO/GEO

- All pages have Open Graph, Twitter Card, and JSON-LD structured data
- FAQ structured data on generator and about pages for AI answer extraction
- Author attribution: Richard Crane, Founder & Chief AI Officer at MILL5
- Framework attribution: Jim Collins (Flywheel, Hedgehog), Martin Eppler (Synergy Mapping)

## Roadmap (Planned Features)

- Drag-and-drop point reordering
- Keyboard shortcuts & undo/redo (Cmd/Ctrl+Z)
- Accessibility improvements (ARIA, focus styles, skip links)
- Future ideas: SVG export, preset templates, alternative AI services, animation preview
