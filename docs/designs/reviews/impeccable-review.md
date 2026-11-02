Method: dual-agent (A: ab03ff1f10f91d698 · B: a47c151bed71d6900)

# Impeccable review: Level-Up Circle Generator

- **Target:** `levelup-circle-generator.html` (2,990 lines), branch `feature/egm-design-and-impeccable` at `626fd17`
- **Mode:** Operate (tool)
- **Context loaded:** PRODUCT.md and DESIGN.md ("The Neon Orbit"). There is no surface brief.
- **Skill:** Impeccable 4.8.0 critique and audit. The detector is 4.8.0 in both the launcher and `npx`.
- **Scope:** the app chrome, which covers the header, panels, controls, toasts, responsive layout, WCAG 2.2 AA and performance. The canvas drawing is in scope only for contrast and legibility.
- **Approach:** refine the existing design system, not replace it.
- **Constraint:** every fix fits the single-file, no-build setup.

Line numbers below refer to `levelup-circle-generator.html` unless noted.

## Scores at a glance

| Review | Score | Band |
|---|---|---|
| Critique (Nielsen heuristics) | **21/40** | Acceptable |
| Audit health | **10/20** | Acceptable |

Release-blocking (P1) issues:
1. The header overlaps itself on phones.
2. A shared link silently overwrites the recipient's saved circle.
3. Export Image text is 2.0:1 on the old pink gradient.
4. Every input on a phone is 13px, so iOS zooms when the user types.

Recent accessibility work held up when checked (see "Verified, not re-reported").

---

## 1. Critique

### Design specificity verdict

**Mostly built for this product.** The black stage sits between graphite side panels, so the glowing circle is the brightest object on screen, which is the North Star working as intended. The point rows (number, label, colour swatch, delete) are specific to this product. The app opens with a finished example circle, which makes a strong first frame. Three things make it feel more like a generic "indie generator" template:
- The Export button uses the old pink gradient (478-483), and range sliders and info toasts use the old violet.
- The share buttons use brand fills: Facebook #1877F2 (791) and ChatGPT #10a37f / #12b886 (828, 833).
- The checkboxes show the browser's default blue (no `accent-color` at 435), so a second accent hue sits in the chrome. You can see it in the desktop capture.
- Emoji icons (🎯 🎨 📋 💾 🗑️ ➕ 📸) sit beside SVG icons in the header.

DESIGN.md asks for controls that are "tactile and playful": hover lift, press and glow. The page has hover lift but no `:active` (pressed) state anywhere, so that brief is not met.

**Deterministic scan:** see section 3. The detector and the design review agree on the Export button contrast, the legacy gradient, the header occlusion at 390px and the point-index contrast. The detector added the off-system colours and font sizes. The review added the blue checkboxes, the data-loss paths and the canvas label contrast, which the detector cannot see.

**Visual overlay:** not shown. The live-server overlay was skipped because the brief asked for headless Chrome only, so no `[Human]` tab exists.

### Design health score (Nielsen)

| # | Heuristic | Score | Key issue |
|---|---|---|---|
| 1 | Visibility of system status | 3 | Export shows a busy state and then the size and MB (2724-2790). Auto-save is silent (1828), but a "Save State" button (1215) implies saving is manual. |
| 2 | Match with the real world | 2 | "Save State" and "Clear State" are developer words. The UI says "Points" where PRODUCT.md says "focus areas". The tooltip says "ROYGBIV… golden angle" (1213). |
| 3 | User control and freedom | 2 | Clear and Load Example ask before acting (verified). There is no undo, and three paths silently drop labels (Fix 2, Fix 8). |
| 4 | Consistency and standards | 2 | Four button styles: violet, pink gradient, graphite, brand fills. Emoji and SVG icons are mixed. "Number of Points" (1085) and "Add Point" (1228) do the same job. |
| 5 | Error prevention | 2 | Number inputs are clamped (good). Lowering the point count silently truncates the list (1614-1615). After a delete, focus lands on the next delete button (1686-1687), so pressing Enter twice deletes two points. |
| 6 | Recognition rather than recall | 3 | Everything is visible inline. The panel toggle's four-step cycle (both → left → right → none) can't be guessed from its icon (1054). Help is in `title` tooltips only. |
| 7 | Flexibility and efficiency | 1 | No keyboard handlers. Enter does not add the next point. You can't paste a list or reorder points. |
| 8 | Aesthetic and minimalist design | 2 | The stage is right. Export is the loudest element in the chrome. The seven glow checkboxes are always shown, while export size and format are hidden under Advanced. |
| 9 | Error recovery | 2 | Export errors are specific (2742, 2751). Every toast disappears after 3s whatever its length (1823), and none offers an action such as Undo or Restore. |
| 10 | Help and documentation | 2 | Help exists only in `title` tooltips, which touch screens can't show. What the circle means is explained only on the About page. |
| **Total** | | **21/40** | **Acceptable** |

### Cognitive load: 3 fails, 1 partial (moderate)

| Check | Result | Notes |
|---|---|---|
| Single focus | Pass | |
| Chunking | **Fail** | Glow Effects has 7 checkboxes (1135-1184); the header has 7 items. |
| Grouping | **Fail** | Export size and format sit in the left panel's Advanced section (1089-1111), but the Export button is in the right panel (1231). "Show Connection Lines" and "Show Guide Lines" are listed under "Glow Effects". |
| Visual hierarchy | Pass (marginal) | The pink Export button competes with the circle. |
| One thing at a time | Pass | |
| Minimal choices | **Fail** | |
| Working memory | Partial | On phones the export settings and the Export button are separated by the whole circle. |
| Progressive disclosure | Pass | It hides the wrong things: export settings are hidden, glow toggles are not. |

Decision points with more than four options: the header (7), Glow Effects (7) and Advanced (8 fields).

### Emotional journey

- **Peak:** the first frame is already a finished glowing circle, and every label edit redraws it live.
- **Valleys:**
  - On a phone the header collides with itself on load.
  - Point labels on a phone render at about 7px on a 350px canvas.
  - Long labels are cut off in the input ("Financial Freedc").
  - In the light theme, the yellow and green labels nearly disappear on white.
- **Worst valley:** opening a shared link silently replaces your own saved circle, and you only find out later.
- **High-stakes moments:**
  - Clear and Load Example use the browser's own `confirm()` dialog. It is safe, but on a projector a stock OS dialog breaks the dark control-deck look.
  - Delete has neither a confirmation nor an undo.
  - Export ends well, with a busy state and then the result.
- **End:** a 3-second grey toast. The Neon Orbit has no moment of celebration.

### What's working

1. **The circle comes first.** The 320 | 1fr | 320 grid on a #000 stage, and the option to hide both panels, give a clean, presentable circle.
2. **The export pipeline is honest** (2724-2790). It shows a busy state, falls back to a smaller size where iOS limits apply, warns when WebP quietly becomes PNG, and reports the size and MB when done.
3. **The accessibility groundwork is real:**
   - Green focus-visible outlines (426-433).
   - Named point controls.
   - A canvas description that updates with the title and every focus area (2553-2554).
   - Announced toasts (1034), with the icon hidden from screen readers (1803).
   - Focus kept in the list after a delete (1686).
   - Measured: no unnamed controls, a logical tab order and no horizontal overflow at 390px.

### Persona red flags

- **Alex (power user):**
  - No keyboard shortcuts.
  - Enter in a label does not create the next point, and a list of goals can't be pasted.
  - Points can't be reordered.
  - Export size and format sit in a collapsed section on the opposite side from Export.
  - Getting from "none" back to "left only" takes two clicks through the toggle cycle (2116).
- **Sam (screen reader or keyboard):**
  - The title is read as "direct hit Level-Up Circle Generator" (1040), and the buttons as "artist palette Random Colors" and so on (1213-1216, 1229, 1231).
  - The settings panel is a `<nav>` (1064), which adds a false landmark.
  - Glow Effects is a `div` with no group name (1135).
  - `.point-index` is 4.05:1 (503).
  - Input borders are 1.79:1 against the panel (413).
  - Errors vanish after 3s.
- **Casey (mobile):**
  - The header occlusion, and 13px inputs that trigger iOS zoom.
  - Settings stack above the circle, so someone opening a shared link sees form fields first.
  - The 40vh panels scroll inside a page that also scrolls (899, 905).
  - Labels on the canvas are about 7px.
  - Tooltips are unreachable on touch.
- **Coach running a live session** (from PRODUCT.md):
  - With both panels hidden, the header still shows Articles, About and five icon buttons, two in brand colours.
  - The circle fills only about 60% of the stage height, because of the canvas padding (2237) plus the stage padding.
  - There is no undo when a client says "put that back".
  - The `confirm()` dialog pops up on the projector.
  - Sending the link to the client overwrites the client's own saved circle.

### Minor observations

- **Slow transitions:** `button { transition: all 0.3s }` (458), and the same in five other places. For a tool, 150-250ms on named properties is better.
- **Label column too narrow:** about 125px at desktop, so labels get cut off. The swatch column is 50px (493) and could give some of that back.
- **Default export drops the attribution:** the default format is WebP (1104, 2006), but the owner-mandated author and copyright metadata is embedded in PNG only. Owner decision needed.
- **Clear State resets the metadata:** it sets Author and Copyright to empty (1857-1858), against PRODUCT.md's deliberate defaults. Owner decision needed.
- **Off-system grey:** `.collapsible-content` uses #222 (637), which is not a DESIGN.md token.
- **Leftover logging:** `randomizeColors` calls `console.log` (1754).
- **Toast can cover Export on phones:** the toast container sits 80px from the bottom (752).
- **Missing favicon:** `/favicon.ico` returns 404 (the only console error).

### Questions to consider

1. With the panels hidden (`fs=1` or "none"), the circle is the whole deliverable. Why does any chrome stay on screen?
2. "What you see is what you export." Should phones get a full-width, circle-first view instead of a 350px preview with 7px labels?
3. Would Export feel more like part of the deck as one neon split button that carries size and format, with the four utility buttons folded into one row?

---

## 2. Audit

### Audit health score

| # | Dimension | Score | Key finding |
|---|---|---|---|
| 1 | Accessibility | 2 | Fails WCAG 1.4.3 (Export 2.04:1, `.point-index` 4.05:1), 1.4.11 (input borders 1.79:1) and 1.4.10/occlusion in the 390px header. Emoji are read aloud. |
| 2 | Performance | 3 | Lean single file with one CDN library. Redraws and auto-save are debounced. Six `transition: all` rules (458, 532, 565, 607, 707, 773). No other console errors. |
| 3 | Theming | 1 | No CSS custom properties (`var(--` occurs 0 times). DESIGN.md tokens exist only as repeated hex values, plus off-system #ccc, #555, #999, #222. |
| 4 | Responsive | 2 | No overflow at 390px, but the header occludes itself. All 20 inputs are 13px. 59 of 60 targets are under 44px. Panels scroll inside a scrolling page. |
| 5 | Implementation integrity | 2 | The legacy gradients are still on screen, checkboxes are browser blue, emoji and SVG icons are mixed, and there is dead CSS (972). |
| **Total** | | **10/20** | **Acceptable** |

**Implementation integrity verdict: Fail (narrowly).** The structure, tokens-by-convention and graphite layering do express the Neon Orbit. Three things on screen still contradict DESIGN.md's own rules: the pink Export gradient, the violet range and info accents, and the blue checkboxes. All three are the most visible coloured chrome.

**Executive summary:** 10/20. Issues by severity: P0: 0, P1: 4, P2: 7, P3: 5.

**Systemic patterns:**
- Colours are hard-coded everywhere. Retiring the legacy colours means touching about 10 rules by hand.
- Mobile CSS shrinks values (13px inputs, 24px delete buttons, 30px share buttons) instead of re-laying things out.
- Nothing in the UI can undo a destructive action.

**Positive findings:**
- Focus-visible outlines on every control.
- Logical DOM and tab order.
- Every control has an accessible name.
- The canvas description includes the title and focus areas, as PRODUCT.md requires.
- `role=status` toasts.
- Numeric inputs are clamped.
- Export handles iOS canvas limits.
- No JS exceptions.

### Verified, not re-reported

| Item | Status |
|---|---|
| Focus-visible outlines (426-433) | Present on every stop |
| Aria-labels on header and point controls (1042-1054) | Present |
| `role="status"` toast container (1034) | Present |
| Dynamic canvas `aria-label` (2553-2554) | Present |
| `confirm()` on Clear (1867) and Load Example (1769) | Present |

All five work. The only remaining concern is that the native `confirm()` dialog doesn't match the dark theme (P3, not listed).

---

## 3. Detector output summary

| Scan | Exit | Findings |
|---|---|---|
| `impeccable detect --json levelup-circle-generator.html` (launcher) | 2 | 21 (5 counted, 16 advisory) |
| `npx -y impeccable detect levelup-circle-generator.html` | 2 | Identical to the launcher |
| `npx -y impeccable detect http://localhost:8811/levelup-circle-generator.html` at 1280×800 | 2 | low-contrast ×9, ai-color-palette ×1 |
| URL scan at 390×844 | 2 | Same as 1280, plus text-occlusion ×1 |

**True positives:**

| Rule | Location | Detail |
|---|---|---|
| low-contrast | `#exportBtn` (479, 1231) | 2.0-2.5:1 |
| low-contrast | `.point-index` ×8 (503) | 4.05:1 |
| ai-color-palette | `#exportBtn` gradient (479) | DESIGN.md already lists it as legacy |
| text-occlusion | `h1` at 390px (267-305) | The About link covers 68% of it |
| dark-glow | `button:hover` violet shadow (465) | |
| flat-type-hierarchy | `.site-header h1` (868) | Only at ≤900px, where it matches the 16px panel heading |
| design-system-color (advisory) | #ccc (285), #fff (270), #555 (524), #999 (603), #222 (637) | Off-system greys |
| design-system-color (advisory) | #1877F2, #10a37f, #12b886 (791, 828, 833) | Brand fills, intentional but against the Diagram Owns the Rainbow rule |
| design-system-font-size (advisory) | 15px (287), 12px (504), 13px (515), 10px (616) | Off-scale sizes |

**False positives:**

| Rule | Location | Why it's false |
|---|---|---|
| low-contrast and gray-on-color | `#addPointBtn` (450) | The static scan ignores the `.add-point-btn { background: transparent }` override at 529. Measured 4.91:1. |
| design-system-radius 1px | 847 | It draws an icon bar, not a component. |
| design-system-font-size 24px / 18px | 380 / 673 | Generic `h1`/`h2`, overridden; they only reach the `<noscript>` and `.sr-only` content. |
| design-system-font-size 11px | 972 | Dead CSS: `input[type="text"]` at 13px wins. Still worth deleting. |

**What the detector missed** (found by the review):
- The blue browser-default checkboxes (435).
- The violet range `accent-color` (396).
- The violet info toast (734-740).
- 13px phone inputs.
- The data-loss paths.
- Canvas label contrast.

**Measured in the browser** (raw CDP, 1440×900, plus true 390×844 mobile emulation):

| Check | Result |
|---|---|
| Console | Only a favicon 404 |
| Overflow at 390px | None (`scrollWidth` 390) |
| Under 24px | Articles/About (18px tall) and the range sliders (16px tall). Both pass 2.5.8 through the spacing exception. |
| Placeholder contrast | Default #757575 on #2a2a2a = 3.12:1 |
| Header links | 11.76:1 |
| Labels | 7.3-8.0:1 |

Note on captures: Chrome's `--headless=new --window-size=390,…` actually renders a 500px layout. Trust the CDP captures for mobile:
- `/private/tmp/claude-501/-Users-rich--openclaw-projects-levelupcircle/e741deee-e086-449e-a835-f892cbf0ccae/scratchpad/a-mobile-390.png`
- `/private/tmp/claude-501/-Users-rich--openclaw-projects-levelupcircle/e741deee-e086-449e-a835-f892cbf0ccae/scratchpad/b-hdr-390-cdp.png`

Desktop and light-theme captures:
- `/private/tmp/claude-501/-Users-rich--openclaw-projects-levelupcircle/e741deee-e086-449e-a835-f892cbf0ccae/scratchpad/a-desktop.png`
- `/private/tmp/claude-501/-Users-rich--openclaw-projects-levelupcircle/e741deee-e086-449e-a835-f892cbf0ccae/scratchpad/a-desktop-light.png`

---

## 4. Ordered material fixes

Every fix stays inside the single file and the existing graphite and neon system.

1. **[P1] The header occludes itself at ≤600px.**
   - **Where:** `.site-header .site-nav` and `.site-header .share-bar` are absolutely positioned (275-305) over a centred `h1` (267-273, markup 1040). At 390px the nav spans x 20-141, the h1 x 83-307 and the share bar x 204-370.
   - **Change:** in `@media (max-width: 600px)` (926), add:
     - `.site-header { height: auto; flex-wrap: wrap; justify-content: space-between; row-gap: 8px; padding: 10px 12px; }`
     - `.site-header .site-nav, .site-header .share-bar { position: static; transform: none; }`
     - `.site-header h1 { order: -1; flex-basis: 100%; text-align: center; }`

     Keep the share buttons at 36px rather than shrinking them to 30px (873-881).
   - **Serves:** WCAG 1.4.10 Reflow and 1.3.2 readable presentation; PRODUCT.md Operating Context (phones view shared links).

2. **[P1] A shared link silently overwrites the recipient's saved circle.**
   - **Where:** `loadFromURL` replaces `state` (2169). `redraw` then calls `debouncedAutoSave()` (2565), which overwrites `localStorage.levelUpCircleState`.
   - **Change:**
     - In `loadFromURL`, before line 2169, copy any existing `levelUpCircleState` to `levelUpCirclePrevious`.
     - Extend `showToast(message, type, action)` (1789) to take an optional `{label, onClick}` button.
     - Show "Opened a shared circle · Restore mine" when a previous circle existed.
   - **Serves:** PRODUCT.md Principle 4 ("Never lose a user's labels without asking").

3. **[P1] Export Image fails contrast and uses the legacy gradient.**
   - **Where:** `button.export` (478-483), `#exportBtn` (1231). White on #f093fb measures 2.04:1.
   - **Change:**
     - At rest: `background: #2a2a2a; border: 1px solid #39FF14; color: #39FF14;` (10.6:1).
     - On `:hover` and `:focus-visible`: `background: #39FF14; color: #000;` (15.5:1) with `box-shadow: 0 0 5px rgba(57,255,20,0.3)`.

     It stays the primary action, quietly at rest.
   - **Serves:** WCAG 1.4.3; DESIGN.md One Voice Rule, "Don't add new gradients", Flat-at-Rest.

4. **[P1] Inputs on phones are 13px, so iOS zooms when you type.**
   - **Where:** `input[type="text"], input[type="number"], select { font-size: 13px }` (945-951). There is also a dead `.point-label-input { font-size: 11px }` (970-973).
   - **Change:**
     - In `@media (max-width: 900px)` (862), set `input[type="text"], input[type="number"], select { font-size: 16px; }`.
     - Delete the 13px and 11px declarations.
   - **Serves:** DESIGN.md "The 16px Input Rule".

5. **[P2] Retire the remaining legacy accents and the browser-blue checkboxes.**
   - **Where and change:**

     | Selector | Line | Change |
     |---|---|---|
     | `input[type="range"]` | 396 | `accent-color: #39FF14` |
     | `input[type="checkbox"]` | 435 | Add `accent-color: #39FF14` |
     | `button` | 450, 465 | Graphite base `#2a2a2a` / `#444` border; hover shadow `0 3px 10px rgba(255,255,255,0.1)` per the DESIGN.md hover lift |
     | `.toast.info` and `.toast.info .toast-icon` | 734-740 | Neutral `#888` border and icon |
   - **Serves:** One Voice Rule; Diagram Owns the Rainbow Rule.

6. **[P2] Point index text fails contrast.**
   - **Where:** `.point-index { color: #888 }` (502-503) on `.point-item` #2a2a2a measures 4.05:1 at 12px.
   - **Change:** use `#b0b0b0` (the text-label token, about 7:1).
   - **Serves:** WCAG 1.4.3.

7. **[P2] Control borders are invisible against the panel.**
   - **Where:** the input and select border `#444` (413) and the `.add-point-btn` dashed border (560) on #1a1a1a measure 1.79:1. The input fill against the panel is 1.21:1.
   - **Change:** raise the border to `#6e6e6e` (about 3.5:1) and update DESIGN.md `control-edge` to match.
   - **Serves:** WCAG 1.4.11 Non-text Contrast.

8. **[P2] Labels can be lost without asking.**
   - **Where:**
     - Lowering Number of Points runs `state.points.slice(0, targetCount)` (1614-1615) without checking.
     - `deletePoint` (1672) has no undo, and focus moves to the next delete button (1686-1687).
   - **Change:**
     - Ask before the truncation when any removed label differs from `Point N`.
     - After a delete, show the action toast from Fix 2: "Deleted 'Career Growth' · Undo".
     - Move focus to the next row's `.point-label-input` instead of its delete button.
   - **Serves:** PRODUCT.md Principle 4; WCAG 3.3.4-style error prevention.

9. **[P2] Toasts disappear after a fixed 3 seconds.**
   - **Where:** `showToast` (1820-1824) closes every toast after 3000ms, including error messages of about 110 characters (2066, 2077).
   - **Change:**
     - Set the duration to `Math.max(4000, message.length * 60)`.
     - Pause the timer on `mouseenter` and `focusin`.
     - Keep `error` toasts until the user closes them with a × button.
     - On phones, move `.toast-container` up so it doesn't cover Export (752).
   - **Serves:** WCAG 2.2.1 Timing Adjustable; Nielsen H9.

10. **[P2] Default label colours are illegible in the exported image.**
    - **Where:** `ctx.fillStyle = point.color` (2511, 2529). The outline is drawn only when label glow is on (2496-2503).
    - **Measured:** Community #131cec on black is 2.41:1. In the light theme, Relationships is 1.35:1 and Learning & Skills 1.57:1.
    - **Change:** in `drawCircle`, fill labels with the point's hue at clamped lightness (L ≥ 62% on dark, L ≤ 38% on light), or always draw the `labelOutlineColor` stroke. Leave the dots in the raw colour.
    - **Serves:** WCAG 1.4.3 for text in images; PRODUCT.md Principles 1-2 (export is the product).
    - **Owner check:** this changes the diagram's look.

11. **[P2] On phones, the circle is buried between two scrolling panels.**
    - **Where:** at ≤900px, settings come first and the panels are capped at 40vh with their own scroll (885-922).
    - **Change:** in that block:
      - `.preview { order: -1; }`
      - Rows `minmax(50vh, 1fr) auto auto`.
      - `.preview { padding: 4px }` at ≤600px.
      - Remove the `max-height: 40vh` caps so the page has one scroll.
    - **Serves:** PRODUCT.md Operating Context (phones view shared links) and Principle 1.

12. **[P3] Emoji in the title and buttons are read aloud.**
    - **Where:** 1040, 1213-1216, 1229, 1231.
    - **Change:** wrap each emoji in `<span aria-hidden="true">…</span>`. If you also want one icon style, replace them with inline SVGs matching the header icons.
    - **Serves:** WCAG 1.1.1 and 2.5.3 Label in Name; DESIGN.md consistency.

13. **[P3] Landmark and grouping semantics.**
    - **Where:** `<nav class="panel-left">` (1064) and the closing `</nav>` (1218); the `.section-label` "Glow Effects" `div` (1135).
    - **Change:**
      - Make the settings panel a `<section aria-label="Circle settings">`.
      - Wrap the checkboxes in a `<fieldset>` with a `<legend>` styled as `.section-label`.
      - Split "Show Connection Lines" and "Show Guide Lines" into their own group. Rename it only with owner approval, because that is a copy change.
    - **Serves:** WCAG 1.3.1 Info and Relationships.

14. **[P3] There is no pressed state and motion is unrestrained.**
    - **Where:** `button` (458, 463-466) and `.share-btn` hover scale. There is no `:active` rule and no `prefers-reduced-motion` rule.
    - **Change:**
      - Add `button:active { transform: translateY(0) scale(0.98); }`.
      - Replace `transition: all 0.3s` with `transition: transform .18s, box-shadow .18s, background-color .18s` (458, 532, 565, 607, 707, 773).
      - Add `@media (prefers-reduced-motion: reduce) { button:hover, .share-btn:hover { transform: none; } .toast { transition: opacity .2s; } }`.
    - **Serves:** DESIGN.md "Tactile feedback: hover lift, press"; operate.md motion timing of 150-250ms.

15. **[P3] Brand-filled share buttons.**
    - **Where:** `.share-btn.facebook` (790-793) and `.share-btn.chatgpt` (827-834).
    - **Change:** use `#2a2a2a` fill with `#e0e0e0` icons, matching `.copy-link` and `.panel-toggle`, plus the neon glow on hover.
    - **Serves:** Diagram Owns the Rainbow Rule.
    - **Owner check:** this is a taste call.

16. **[P3] Placeholder contrast.**
    - **Where:** the `#meta*` inputs use the default #757575 on #2a2a2a, which is 3.12:1.
    - **Change:** add `::placeholder { color: #999; }` (5.0:1) near 412-418.
    - **Serves:** WCAG 1.4.3 (best practice for hint text).

**Need an owner decision, so not ranked:**
- Default export format WebP versus PNG, given that the metadata is PNG-only (1104, 2006).
- Clear State blanking Author and Copyright (1857-1858).
- Renaming "Save State" and "Clear State".

**Suggested command order:**
1. `/impeccable adapt` (Fixes 1, 4, 11)
2. `/impeccable harden` (2, 8, 9)
3. `/impeccable quieter` (3, 5, 15)
4. `/impeccable audit` re-check (6, 7, 12, 13, 16)
5. `/impeccable colorize` (10)
6. `/impeccable animate` (14)
7. `/impeccable polish`

---

## Run notes

- **No questions asked:** the review ran as a subagent with no way to ask the user, so the critique's closing questions were not asked. *Questions skipped: subagent run with no user-question channel (16 priority fixes listed).*
- **No snapshot saved:** the critique snapshot was not written to `.impeccable/critique/`, because the brief named this file as the only output. No source files were edited.
- **No overlay:** the live-server overlay was skipped because the brief asked for headless Chrome only.
- **Server:** the static server on :8811 was already running and was left running.

---

## Resolution (2026-11-02)

Applied on `feature/egm-design-and-impeccable`:

| Fix | Status |
|---|---|
| 1 Header reflow ≤600px | Applied |
| 2 Shared link "Restore mine" toast | Applied (`showToast` takes an optional action) |
| 3 Export button contrast / gradient | Applied (graphite + neon border; neon fill on hover/focus) |
| 4 16px inputs on phones | Applied (≤900px; 13px/11px rules removed) |
| 5 Legacy accents, checkbox/range accent | Applied |
| 6 Point index contrast | Applied (#b0b0b0) |
| 7 Control borders | Applied (#6e6e6e; DESIGN.md `control-edge` updated) |
| 8 Label loss: truncation confirm, delete Undo, focus to label input | Applied |
| 9 Toast timing, pause on hover/focus, sticky errors with close | Applied; toasts move to top on phones |
| 10 Label colours in export | **Not applied — owner decision** (changes the diagram's look) |
| 11 Circle first on phones, one page scroll | Applied |
| 12 Emoji hidden from screen readers | Applied |
| 13 Settings `<section>` landmark, Glow Effects `<fieldset>` | Applied (no copy changes) |
| 14 Pressed state, targeted transitions, reduced motion | Applied |
| 15 Brand-filled share buttons | **Not applied — owner decision** (taste call) |
| 16 Placeholder contrast | Applied; also themed `::selection` and caret |
| Detector `dark-glow` (#f5576c halo on delete hover) | Removed |

Detector after fixes: static scan 2 counted findings (was 5; remaining `flat-type-hierarchy` is driven by the hidden SEO article); live URL scan exit 0 (was 10 findings).
