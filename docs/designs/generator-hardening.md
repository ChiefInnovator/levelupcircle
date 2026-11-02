# Generator Hardening: Faithful Export, Safe Editing, Accessible Controls

*   **Author:** Richard Crane
*   **Status:** Approved (revision 2, after Goldfish review)
*   **Date:** 2026-11-02
*   **Review record:** [reviews/generator-hardening/](reviews/generator-hardening/consolidated_review_report.md)

--------------------------------------------------------------------------------

## Overview

The Level-Up Circle Generator (`levelup-circle-generator.html`) is a single
HTML file: CSS, markup, and one inline script, served by GitHub Pages from
`main`. A user types a title and up to 36 focus areas, styles them, exports an
image, or shares a compressed URL. There is no backend and no automated test.

A review of the repository found a security hole, export output that does not
match the preview, edit handlers that lose data, inconsistent state restore,
iOS export failures, false public claims, and controls that keyboard and
screen-reader users cannot operate. This design fixes those defects inside the
current single-file structure, in six phases that each leave the app working.

## The Problem

Line numbers refer to `levelup-circle-generator.html` on `develop` at commit
`9af4dd5`.

1.  **Reflected XSS through legacy share links.** The legacy decoder builds
    each point color as `#${color}` with no check (`:2003`). The point list
    then writes that value into `innerHTML` as `value="${point.color}"`
    (`:1625`). A link such as `?p=x|"><img src=x onerror=...>` runs script
    when the page opens.
2.  **The export does not match the preview.** `drawCircle()` scales the
    preview by `clamp(previewSize / 800, 0.5, 1.5)` and the export by
    `exportSize / previewSize`. These agree only when the preview is 800 CSS
    pixels wide. Further absolute floors (label font 8–32 px, title font
    16–48 px, point radius ≥ 6 px, label padding ≥ 4 px, line widths ≥ 0.5–1
    px) apply to the preview only. The exported image therefore depends on the
    window size at export time.
3.  **Number fields rewrite the value while the user types.**
    *   "Number of Points" applies every keystroke. Typing `12` over `8`
        passes through `1`, which truncates the points to one entry; labels
        2–8 are lost.
    *   "Label Font Size" and "Title Font Size" clamp each keystroke and write
        the clamped value back, so `20` cannot be typed (`2` becomes `8`).
    *   The "Label Distance" slider stores `parseInt(v) || 25`, so 0 becomes
        25 (`:2978`).
4.  **One-click destructive buttons.** "Clear State" and "Load Example"
    replace the current circle with no confirmation.
5.  **Point label edits are not live.** Label inputs use `onchange`, so the
    canvas updates only on blur, while the title updates on every keystroke.
6.  **State restore is inconsistent.** Four functions (`loadExample`,
    `clearState`, `applyStateToUI`, `loadState`) copy state into the form by
    hand and disagree:
    *   `applyStateToUI` (used by URL loads) skips the four metadata fields
        (Copyright, Author, Description, Keywords).
    *   No load path updates the "Label Distance: N px" readout.
    *   `parseInt(x) || default` turns a stored `labelOffset` of 0 into the
        default.
    *   `loadState` never defaults `showConnections`.
    *   Export format and quality are not stored at all.
7.  **Exports above 4096 px fail on iOS and iPadOS.** WebKit limits one canvas
    to 16,777,216 pixels, which is 4096 × 4096
    ([WebKit canvas size limit](https://pqina.nl/blog/canvas-area-exceeds-the-maximum-limit/)).
    The default export size is 8192, so the export canvas is unusable there.
8.  **The download can be cancelled.** `downloadBlob()` revokes the object
    URL in the same task as `a.click()`, on a link that is not attached to the
    document.
9.  **Public claims are false.**
    *   AVIF export is claimed in structured data, the FAQ, the hidden SEO
        article inside the generator, `index.html`, `llms.txt`,
        `level_up_circle_blog_v2.md`, and `CLAUDE.md`. The format menu offers
        PNG and WebP only.
    *   "Embedded metadata" is claimed for all exports. Only PNG gets metadata.
    *   `README.md` and four files in `docs/` describe a Fullscreen button.
        The app replaced it with a four-state panel toggle; the `fs=1` URL
        parameter still exists and now hides both panels.
10. **Controls are not accessible.**
    *   A CSS rule sets `outline: none` on focused text inputs, number inputs,
        and selects (`:419–425`), and no other rule shows focus.
    *   Icon-only header buttons rely on `title` alone.
    *   Point label inputs, color inputs, and delete buttons have no names.
    *   Toasts are not announced.
    *   The panel toggle does not expose its current mode.
    *   The canvas label is a fixed sentence that never names the user's
        title or focus areas.
11. **The ChatGPT link uses the legacy host** `chat.openai.com`, which now
    redirects to `chatgpt.com`.

## Goals

*   **G0 — No script injection from links:** No URL parameter can place markup
    into the page.
*   **G1 — Faithful export:** For any window size and device pixel ratio, the
    exported image is a uniform scaling of the preview. Measured: label
    height, dot diameter, and ring line width as a fraction of image width
    differ by less than 1% between a preview screenshot and the export.
*   **G2 — No unintended data loss while editing:** Typing any value into a
    number field never changes other data. "Clear State" and "Load Example"
    act only after the user confirms.
*   **G3 — One restore path:** Every load passes through `normalizeState` and
    `syncUIFromState`. After any load, every control and readout matches
    `state`.
*   **G4 — Export works on iOS/iPadOS:** When the requested size cannot be
    allocated, the app exports at the largest size in the size menu that
    works, and the toast names both sizes.
*   **G5 — Accurate claims:** A case-insensitive search of the repository for
    `avif`, `fullscreen`, `chat.openai.com`, and "embedded metadata" not
    followed by "(PNG)" finds no claim about the current app. Exempt: this
    design, its review reports, the dated correction note in the v2 blog,
    base64 data, and code that handles the legacy `fs` parameter.
*   **G6 — Keyboard and screen-reader operable (WCAG 2.2 AA for the items
    below):** every control has an accessible name, focus is visible on every
    focusable element, toasts are announced, and the canvas name includes the
    title and focus areas.
*   **G7 — Minor fixes:** Label edits redraw on every keystroke; downloads are
    not cancelled by early revocation; the ChatGPT button opens `chatgpt.com`.

## Non-Goals

*   Drag-and-drop reordering and undo/redo (roadmap items 8 and 9).
*   Splitting the file into modules or adding a build step.
*   AVIF export or WebP metadata. The claims are corrected instead.
*   Changing metadata defaults. The owner keeps "Richard Crane" defaults.
*   Changing the default export format (WebP stays the default).
*   Changing the URL share format. Old links must still load.
*   Protecting a recipient's saved circle from being replaced when they open a
    share link. This is a product decision for a later design.
*   Visual redesign. A separate Impeccable design pass (the `impeccable` agent
    skill, run after this work) owns visual changes.
*   Skip links and `role="form"` (roadmap item 10 remainder).

## Critical User Journeys (CUJs)

### CUJ 1: Export matches what I see

*   **Given**: 8 labeled points in a 600 CSS px preview on a 2× display.
*   **When**: The user exports at 8192 × 8192 PNG.
*   **Then**: The PNG scaled to 600 px matches the preview's label sizes, line
    breaks, dot sizes, glow width, and spacing. The PNG has `tEXt` metadata
    and a 300 DPI `pHYs` chunk.

### CUJ 2: Changing a number keeps my data

*   **Given**: 8 points with custom labels.
*   **When**: The user selects "Number of Points", types `12`, presses Tab.
*   **Then**: Points 1–8 keep labels and colors; points 9–12 are added; the
    canvas never shows 1 point. Typing `20` into Label Font Size results in 20.

### CUJ 3: A shared link restores every setting

*   **Given**: The sender sets Label Distance to 0, glow on labels only,
    connections hidden, and copies the share link.
*   **When**: A recipient opens it.
*   **Then**: Canvas, checkboxes, number fields, and "Label Distance: 0 px"
    match. The recipient's own metadata fields and export format are kept.

### CUJ 4: Export on an iPad

*   **Given**: Safari on iPadOS, export size 8192.
*   **When**: The user taps Export Image.
*   **Then**: A 3840 × 3840 file (the largest menu size at or under the
    4096 limit) downloads, named `level-up-circle-3840x3840.webp`, and the
    toast reads "Exported 3840×3840 WEBP (… MB). 8192 is too large for this
    device."

### CUJ 5: Screen-reader user

*   **Given**: VoiceOver with Safari on macOS.
*   **When**: The user tabs through header, settings, and point list.
*   **Then**: Each stop has a visible focus ring and a name: "Copy share link",
    "Share to X", "Share to Facebook", "Ask ChatGPT for strategy", "Toggle
    panels (current: Both panels)", "Point 3 label", "Point 3 color", "Delete
    point 3". Export announces the result toast. The canvas reads "Level-Up
    Circle diagram: <title>. 8 focus areas: <label 1>, <label 2>, …".

### CUJ 6: Destructive buttons ask first

*   **Given**: A saved circle.
*   **When**: The user clicks "Clear State" or "Load Example" and cancels the
    browser confirmation.
*   **Then**: Nothing changes.

## Technical Plan (Architecture)

### 1. One drawing space (Phase 5)

`drawCircle(ctx, size)` replaces `drawCircle(ctx, size, isExport)`. It calls
`ctx.scale(size / 800, size / 800)` once and draws everything in an 800 × 800
logical space using the authored values directly (`pointSize`,
`labelOffset`, `labelFontSize`, `titleFontSize`). Because `measureText`
ignores the transform, line breaks are identical at every output size.

`shadowBlur` is the one value the transform does not scale. Each blur radius
is multiplied by `ctx.getTransform().a`, the current pixels-per-unit, which
already includes the preview's device pixel ratio.

The preview is whatever size fits its container (unchanged rule:
`min(width, height) − 40`). There is no minimum size and no per-element
floor. Every absolute floor in `drawCircle` is removed (list in the Detailed
Plan).

Draw order is unchanged: background, title, ring, connection lines, then for
each point in order: dot, guide line, label.

### 2. One normalizer and one form sync (Phase 1)

```text
 URL s=…       URL legacy p=…     localStorage       Clear / Example
    │                │                 │                    │
 decodeCompressed  decodeLegacy     JSON.parse        initializePoints
    └───────┬────────┘                 │                    │
            ▼                          ▼                    ▼
   raw object (+ recipient prefs) ─► normalizeState(raw) ◄──┘
                                         │
                                         ▼
                        state ─► syncUIFromState() ─► updatePointList() ─► redraw()
```

*   The existing `validateState` is renamed `normalizeState` and becomes the
    only validator. `loadState`'s inline clamps move into it.
*   Decoders keep their key maps and produce booleans as today (`gt !== 0`).
*   Field rules (missing means `undefined`; numbers parsed with `toInt`, which
    keeps 0):

| Field | Valid | Missing or invalid |
|---|---|---|
| `title` | non-empty string | `'Level-Up Circle'` |
| `theme` | `dark`, `light` | `dark` |
| `exportSize` | 1080, 2048, 3840, 5120, 7680, 8192 | 8192 |
| `exportFormat` | `png`, `webp` | `webp` |
| `exportQuality` | 50–100 (clamped) | 100 |
| `pointSize` | 1–60 (clamped) | 14 |
| `labelOffset` | 0–150 (clamped) | 25 (legacy URL decoder passes 35 when `lo` is absent, as today) |
| `labelFontSize` | 8–32 (clamped) | 16 |
| `titleFontSize` | 16–48 (clamped) | 28 |
| `neonGlow*`, `showConnections`, `showGuideLines` | boolean | `true` |
| `metaCopyright`, `metaAuthor` | string (empty allowed) | today's Richard Crane defaults |
| `metaDescription`, `metaKeywords` | string | `''` |
| `points` | array of 1–36 `{label, color}` | `initializePoints(numPoints)` where `numPoints = toInt(n, 8)` clamped 1–36 |
| `points[i].label` | non-empty string | `Point i+1` |
| `points[i].color` | `#` + 6 hex digits | `#39FF14` |
| `numPoints` | always `points.length` | — |

*   Arrays longer than 36 keep the first 36. Labels are not truncated.
*   `normalizeState` returns new objects; it never returns shared references.
*   URL loads first copy the recipient's saved `meta*`, `exportFormat`, and
    `exportQuality` into the raw object, so a link does not erase them.
*   `syncUIFromState()` writes every control and readout (`labelOffsetValue`,
    `qualityValue`, quality-group visibility), then calls `updatePointList()`.
    It replaces the four hand-written copies.

### 3. Edit handlers that keep data (Phase 1)

*   **Number of Points:** its `input` listener is removed. A `change` listener
    (fires on Enter, blur, and spinner clicks) clamps, writes back, and calls
    `updatePointList()`. The separate `blur` listener is folded into it.
*   **Point Size, Label Font Size, Title Font Size:** the `input` listener
    updates `state` and redraws only when the typed value is a number inside
    the range; it never writes back. The existing `blur` listeners keep
    clamping and writing back.
*   **Label Distance slider:** uses `toInt(v, 25)`.
*   **Point label:** `oninput` stores `value || 'Point N'` and redraws; the
    input keeps showing what the user typed. `onchange` writes `Point N` into
    an empty input.
*   **Clear State / Load Example buttons:** call `confirm()` first. The
    first-visit call to `loadExample()` from `loadState()` does not confirm.
*   **Export Format / Quality:** listeners write `state` and auto-save;
    `exportImage()` reads `state`.

### 4. Export (Phase 2)

*   `exportImage()` tries each size from `validExportSizes` that is at or
    below the requested size, largest first. For each: allocate the canvas,
    draw, read pixel (0,0) with `getImageData` (the background is opaque, so
    alpha 0 or an exception means the canvas is unusable), then `toBlob`.
    Failure at any step sets the canvas `width` and `height` to 0 and moves
    to the next size. If every size fails, an error toast says so.
*   The filename, PNG `Comment` metadata, and toast use the size actually
    exported.
*   `#exportBtn` is disabled and reads "Exporting…" until the export settles.
*   `downloadBlob()` attaches the link, clicks, removes it, and revokes the
    URL one second later.

### 5. Accessibility (Phase 3)

*   The `outline: none` focus rule is replaced by
    `:focus-visible { outline: 2px solid #39FF14; outline-offset: 2px; }`
    covering buttons, links, inputs, selects, and `summary`. The existing
    border-color focus cue stays.
*   Header buttons get `aria-label`s (CUJ 5 strings). The panel toggle's label
    is `Toggle panels (current: <panelModeLabels[mode]>)`, set in markup and
    in `applyPanelMode()`.
*   `#toastContainer` gets `role="status"` and `aria-live="polite"`; the toast
    icon gets `aria-hidden="true"`; the message is set with `textContent`.
*   Point rows: label input `aria-label="Point N label"`, color input
    `aria-label="Point N color"`, delete button `aria-label="Delete point N"`.
    After a delete, focus moves to the delete button now at that index, or to
    "Add Point" when none remains.
*   `redraw()` sets the canvas `aria-label` (CUJ 5 text).

### 6. Claims and docs (Phase 4)

Canonical phrase for metadata: "with embedded metadata (PNG)". Every site is
listed in the Detailed Plan. The v2 blog gets a dated correction note instead
of a silent edit. The `fs=1` parameter is documented as "hides both panels".

## Alternatives Considered

### Alternative A: Make the export follow the preview's clamped scale

Keep the preview math and compute the export factor from it.

*   **Pros**: The preview looks the same as today at every window size.
*   **Cons**: The export still depends on window size at export time, so two
    people exporting the same shared link get different images.
*   **Reason for Rejection**: It keeps the non-determinism G1 removes.

### Alternative B: Implement AVIF export and WebP metadata

*   **Pros**: Makes the old claims true.
*   **Cons**: Safari and Firefox cannot encode AVIF from a canvas, so a WASM
    encoder would be needed in a no-build file; WebP metadata needs byte-level
    rewriting of the RIFF container with no tests to protect it.
*   **Reason for Rejection**: Higher cost and risk than correcting the text.

### Alternative C: Probe the device canvas limit once and cache it

*   **Pros**: Known limit before the export starts.
*   **Cons**: Allocates a second full-size canvas on memory-limited devices;
    the cache can be keyed wrongly; halving skips valid sizes.
*   **Reason for Rejection**: Trying the real export canvas and stepping down
    covers the same failures with less code.

### Alternative D: Two-click "arm" or undo toast for destructive buttons

*   **Pros**: No native dialog.
*   **Cons**: Needs timers and extra button state, or a history mechanism
    (roadmap item 9).
*   **Reason for Rejection**: `confirm()` gives the same protection; the file
    already uses `prompt()` as a fallback.

### Alternative E: Extract the script into ES modules for unit tests

*   **Pros**: Enables unit tests of the normalizer and PNG chunk code.
*   **Cons**: Module scripts do not load from `file://`, which breaks opening
    the file directly, a documented project workflow.
*   **Reason for Rejection**: The cost to the local workflow is higher than
    the benefit for this change; see Future Improvements.

--------------------------------------------------------------------------------

## Non-Functional Requirements

*   **Security & Privacy**: Colors are validated at every input boundary;
    labels stay HTML-escaped; toast text uses `textContent`. No data leaves the
    browser except through links the user clicks.
*   **Performance**: Live label typing redraws per keystroke, as the title
    already does. Budget: median `redraw()` under 16 ms at 36 points with all
    glows on in desktop Chrome; if exceeded, route label input through the
    existing `debouncedRedraw`.
*   **Rollout & Rollback**: `feature/*` → `develop` → `main`, enforced by the
    branch-policy workflow. Each phase is a separate commit; a rollback is a
    `feature/revert-*` branch with `git revert` of the phase commit, merged to
    `develop` then `main`.

### Risks

| Risk | Mitigation |
|---|---|
| Phase 5 changes every export taken from a window that is not 800 px wide (it now matches the preview). | Isolated commit; called out in the PR for owner veto. |
| Glow width on HiDPI screens depends on the `getTransform().a` multiplier. | CUJ 1 check on a 2× display. |
| iOS memory pressure from large canvases. | Failed canvases are released before the next attempt; real-device check. |
| Large refactor of an untested file. | Fixture URLs and console checks below, run before promotion to `main`. |

--------------------------------------------------------------------------------

### Additional Resources

*   [../architecture.md](../architecture.md),
    [../state-management.md](../state-management.md),
    [../url-sharing-spec.md](../url-sharing-spec.md),
    [../roadmap.md](../roadmap.md) — current docs; several statements in them
    are stale and are corrected by Phase 4.

### Future Improvements

*   **Headless-browser test harness** for `normalizeState`, `smartWrapLabel`,
    and PNG chunks, loading the HTML file as-is.
*   **Undo/redo:** `normalizeState` and `syncUIFromState` are the restore half
    a history stack needs.

--------------------------------------------------------------------------------

## Detailed Implementation Plan

All changes on branch `feature/egm-design-and-impeccable`, one commit per
phase. Line numbers refer to `develop` at `9af4dd5`.

### Phase 0 — XSS hotfix (shipped separately)

*   `levelup-circle-generator.html:2003`: legacy decoder validates the color
    with `/^[0-9A-Fa-f]{6}$/`, falling back to `39FF14`. Shipped as PR #1.
*   **Checkpoint:** malicious link fixture (Verification V1) is inert.

### Phase 1 — State and edit safety (`levelup-circle-generator.html`)

*   Add `toInt(v, fallback)`: `Number.parseInt(v, 10)`; returns `fallback`
    only for `NaN`.
*   Rename `validateState` (`:2018`) to `normalizeState` and implement the §2
    field table. Remove the inline clamps from `loadState` (`:2236–2254`).
*   Keep the legacy migrations (`neonGlow`, `neonGlowPointsLabels`,
    `neonGlowLines`) in `loadState`, before `normalizeState`.
*   Legacy URL decoder: pass `lo` as `35` when the parameter is absent.
*   Add `syncUIFromState()`; replace the hand-written copies in `loadExample`,
    `clearState`, `loadState`, and remove `applyStateToUI`.
*   `loadExample()` and `clearState()` build raw state and call
    `normalizeState` then `syncUIFromState`.
*   Add `confirmLoadExample()` and `confirmClearState()`; the two buttons
    (`:1206`, `:1208`) call them.
*   `loadFromURL()`: merge the recipient's saved `meta*`, `exportFormat`,
    `exportQuality` (from localStorage, if valid JSON) into the decoded raw
    object before `normalizeState`.
*   `#points`: remove the `input` listener (`:2859`) and `blur` listener
    (`:3021`); add one `change` listener with the blur logic.
*   `pointSize`, `labelFontSize`, `titleFontSize` `input` listeners: apply
    only in-range values; no write-back.
*   `labelOffset` listener: `toInt(e.target.value, 25)`.
*   `exportFormat` and `exportQuality` listeners: write `state`, call
    `debouncedAutoSave()`.
*   Point label input: `oninput` replaces `onchange` for the live update;
    `onchange` restores `Point N` in an empty input.
*   **Checkpoint:** V2, V3, V4, V5.

### Phase 2 — Export (`levelup-circle-generator.html`)

*   `#exportBtn` id on the Export button (`:1223`).
*   `exportImage()`: step-down loop per §4; reads `state.exportFormat` and
    `state.exportQuality`; busy state in `try/finally`.
*   `getImageMetadata(actualSize)`: `comment` uses the exported size.
*   `downloadBlob()`: attach, click, remove, delayed revoke.
*   **Checkpoint:** V6, V7.

### Phase 3 — Accessibility (`levelup-circle-generator.html`)

*   CSS: replace the `outline: none` declaration at `:419–425` with the §5
    `:focus-visible` rule; keep `border-color`.
*   Markup: `aria-label` on the four share buttons and the panel toggle;
    `role="status" aria-live="polite"` on `#toastContainer`.
*   `showToast()`: build icon and message with `textContent`; icon
    `aria-hidden="true"`.
*   `updatePointList()` template: `aria-label`s on the three controls;
    `data-index` on the delete button.
*   `deletePoint()`: restore focus per §5.
*   `applyPanelMode()`: set the toggle `aria-label`.
*   `redraw()`: set the canvas `aria-label`.
*   **Checkpoint:** V8.

### Phase 4 — Claims, minor fixes, and docs

*   `levelup-circle-generator.html`:
    *   `:55` → "High-resolution export up to 8192×8192 in PNG and WebP".
    *   `:61` → "300 DPI print-quality export with embedded metadata (PNG)".
    *   `:113` → "Export as a high-resolution PNG or WebP image up to 8K".
    *   `:121` → "The Level-Up Circle Generator exports in PNG (lossless) and
        WebP (smaller files) at up to 8192×8192 pixels at 300 DPI. PNG exports
        include embedded metadata (title, description, author, keywords,
        copyright)." (keep the remainder of the answer if it adds facts).
    *   `:161`, `:220`, `:1004`: remove AVIF; metadata phrase per canonical.
    *   `:1007` → "Panel toggle to show only the circle".
    *   `:2108` → `https://chatgpt.com/?q=`.
    *   `:2781–2782`: delete the orphan EXIF comment.
*   `index.html:49` → "High-resolution export up to 8K in PNG and WebP".
*   `llms.txt:19` → replace "PNG/WebP/AVIF export with embedded metadata" with
    "PNG/WebP export with embedded metadata (PNG)".
*   `level_up_circle_blog_v2.md:95` → "PNG or WebP"; metadata sentence per
    canonical; append "*Correction (2026-11-02): an earlier version of this
    post listed AVIF export and metadata in every format; the tool exports PNG
    and WebP, with metadata in PNG.*"
*   `CLAUDE.md:24` → "Export supports PNG and WebP up to 8192x8192 at 300 DPI
    with embedded metadata (PNG)"; correct the draw-order list to the
    per-point order in §1.
*   `README.md:18, 81, 85, 90` → describe the panel toggle and `fs=1`
    ("hides both panels").
*   `docs/architecture.md`: line count, three-panel grid, panel toggle,
    `exportImage`, §1 drawing model and draw order, §2 data flow, security
    note.
*   `docs/features.md`: replace `isExport` (`:105`) with the drawing model;
    Fullscreen section (`:161`) → panel toggle; `fs=1` (`:143`) semantics.
*   `docs/roadmap.md`: item 6 marked as replaced by the panel toggle; item 9
    `syncUIFromState` and item 10 `:focus-visible` / `aria-label` checkboxes
    marked done.
*   `docs/url-sharing-spec.md:10, 18, 93`: `fs` hides both panels; `lo`
    default; ranges per §2.
*   `docs/state-management.md`: schema per §2 table.
*   `docs/ask-chatgpt-feature.md:22, 52, 72, 114, 145`: `chatgpt.com`; panel
    toggle.
*   **Checkpoint:** V9.

### Phase 5 — Drawing space (`levelup-circle-generator.html`)

*   `drawCircle(ctx, size)` per §1; remove `isExport`, `dpr`,
    `previewLogicalSize`, `displayScaleFactor`, `exportScaleFactor`, and
    `scaleFactor`.
*   Remove these floors and clamps: `minPadding = 60` (`:2303`); font clamps
    (`:2326–2331`); `Math.max(4, …)` label padding (`:2333`);
    `Math.max(6, pointRadius)` (`:2441`); `Math.max(1, 1.5 * …)` (`:2477`);
    `Math.max(0.5, …)` and `Math.max(1, …)` guide line (`:2541`, `:2546`);
    `Math.max(15 * …, …)` label offset floor (`:2505`) becomes `Math.max(15,
    labelOffset)` in units.
*   Multiply every `shadowBlur` by `ctx.getTransform().a`.
*   `exportImage()` calls `drawCircle(exportCtx, size)`.
*   **Checkpoint:** V10.

### Verification Plan

Serve from the repository root with `python3 -m http.server 8000` and load
`http://localhost:8000/levelup-circle-generator.html`. No console errors on
load is a precondition for every check.

*   **V1 (G0):** Open
    `?p=x%7C%22%3E%3Cimg%20src%3Dx%20onerror%3D%22document.body.dataset.pwned%3D1%22%3E`.
    Pass: `document.body.dataset.pwned` is `undefined`.
*   **V2 (G3):** In the console,
    `normalizeState({}).numPoints === 8`,
    `normalizeState({labelOffset: 0}).labelOffset === 0`,
    `normalizeState({points: [{label: '', color: 'red'}]}).points[0]` deep
    equals `{label: 'Point 1', color: '#39FF14'}` (compare with
    `JSON.stringify`), and `normalizeState({showConnections: undefined})
    .showConnections === true`.
*   **V3 (CUJ 2):** Type `12` over `8` and Tab: 12 points, labels 1–8 intact.
    Type `20` in Label Font Size: field and state are 20.
*   **V4 (CUJ 3):** Create a link with Label Distance 0; open it in a second
    profile. Pass: all controls match and the readout says 0.
*   **V5 (CUJ 6):** With `window.confirm = () => false`, click Clear and Load
    Example. Pass: state unchanged.
*   **V6 (G4):** In the console, temporarily make allocation fail above 4096
    (monkey-patch `HTMLCanvasElement.prototype.getContext` to return `null`
    when `this.width > 4096`), export at 8192. Pass: 3840 file and the
    reduced-size toast. Confirm once on a real iPad before promotion to
    `main`.
*   **V7 (G7):** Export PNG; the file downloads in Chrome and Safari and has
    the `tEXt` and `pHYs` chunks (`pngcheck -t` or a hex dump).
*   **V8 (G6):** Tab through the page: every stop shows the outline. Accessible
    names (Chrome DevTools Accessibility pane) match CUJ 5.
*   **V9 (G5):** `git grep -n -i -E 'avif|fullscreen|chat\.openai\.com'` lists
    only exempt sites.
*   **V10 (G1):** At preview widths 600 and 1000 on a 2× display, export at
    2048; downscale to the preview width and compare to a preview screenshot.
    Pass: label cap height, dot diameter, and ring width within 1%. Also
    record median `redraw()` time at 36 points (budget 16 ms).
*   **Regression:** A compressed link and a legacy link created on `develop`
    before this change load with the same labels, colors, and toggles.

--------------------------------------------------------------------------------

*Drafted with the Elephant-Goldfish Model (EGM) design-review workflow.*
