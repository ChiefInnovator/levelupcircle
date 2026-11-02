# Stage 6 — Comprehension Report

*   **Target:** `docs/designs/generator-hardening.md` — FeatureRange 1–402, DetailedRange 403–514
*   **Reviewer:** egm-comprehension (completed; did not read the Stage 5 report)

## Feature Range (lines 1–402)

**What this proposes**

Fix defects in the single-file generator while keeping its no-build shape. Preview and export share one `drawCircle(ctx, size)` that scales every authored size by `unit = size / 800`, dropping per-element clamps so export is a uniform scaling of the preview. All loads (example, clear, URL, localStorage) pass through `normalizeState(raw)` built on a frozen `DEFAULT_STATE`, then `syncUIFromState()` writes every control and readout; export format/quality become persisted. Editing is made safer: point count commits on `change` (with a session `removedPoints` stack), label edits redraw live without rebuilding the list, the list is DOM-built, and Clear needs a second click within 3 s. Export pre-flights the device canvas limit and reports a cap, delays URL revocation, and disables the button while running. Accessibility gaps get names, live toasts, focus rings, and a descriptive canvas label. Public claims are corrected (no AVIF, PNG-only metadata, panel toggle not fullscreen); metadata defaults become neutral; ChatGPT moves to `chatgpt.com`; default format becomes PNG.

**How the existing system works, per this document**

One HTML file on GitHub Pages, no tests. Users enter a title and up to 36 focus areas, export or share an lz-string URL; state auto-saves to localStorage. `drawCircle` scales the preview by clamped `size/800` with font clamps and the export by `exportSize/previewSize`, so export depends on window width. Four load functions copy state by hand and disagree. "Number of Points" reacts per keystroke and truncates. Export defaults to 8192 WebP; only PNG gets metadata, defaulting to Richard Crane's name. Public pages advertise AVIF and fullscreen.

## Findings — Feature Range

| ID | Sev | Scope | Finding |
|---|---|---|---|
| CO-1-1 | Major | Q2 (141–146) vs §1 (227–230) | Q2 promises 8 px min font; §1's 320 px min gives 6.4 px (3.2 px at font 8). Reader can't tell what the owner approves. |
| CO-1-2 | Major | G4; §4; NFR | Halving probe can't deliver "largest supported size" (5120 → 2560 on a 4096 device); behavior if 2048 fails, cache semantics, and side-vs-area limit unexplained. |
| CO-1-3 | Major | Problem 8; G6; Q1; detailed 432 | Metadata defaults described three ways; existing Author fallback "Level-Up Circle Generator" (`:2664`) unmentioned; blank vs omitted chunk unclear. |
| CO-1-4 | Major | §2 diagram; 245–247; 420; 493 | `DEFAULT_STATE` vs `EXAMPLE_STATE` undefined: full vs overrides, which a first visit gets, which Clear restores, whether defaults hold 8 points. |
| CO-1-5 | Major | G5; §6; file list | Files with stale claims missing from plan: `README.md` (18,81,85,90), `docs/url-sharing-spec.md` (10,18,93), `docs/ask-chatgpt-feature.md` (22,145; `chat.openai.com` 52,72,114). Is `fs=1` covered? |
| CO-1-6 | Major | Additional Resources (387–390) | Links are relative to repo root, broken from `docs/designs/`. Linked docs contradict the design (labelOffset 35, 3 export sizes, 2-column, fullscreen) with no staleness warning. |
| CO-1-7 | Major | 128; 497–498 | "Impeccable" undefined; how to run and compare "versus develop" unstated. |
| CO-1-8 | Nit | 51 | Four metadata fields never named; not stated they live in `state`. |
| CO-1-9 | Nit | 76, 309, 411 | "Hidden SEO article" not located (it's in the generator HTML). |
| CO-1-10 | Nit | 235, 54, 257, 253, 381 | Legacy URL and `neonGlow*` formats undefined; rollback ignores old code reading new localStorage. |
| CO-1-11 | Nit | 209 vs 226 | "Padding floors scaled" vs "`minPadding = 60` removed" conflict. |
| CO-1-12 | Nit | 227 | Behavior when container < 320 px unstated. |
| CO-1-13 | Nit | 248–253 | Replacement for invalid points/colors, `numPoints` consistency, and 80-char trim vs "links keep working unchanged" unclear. |
| CO-1-14 | Nit | 266–268 | `removedPoints` interactions with delete/example/clear/URL and restore order undefined. |
| CO-1-15 | Nit | 271 | Empty label after blur: stored `""` or "Point N"? |
| CO-1-16 | Nit | 299, 82 | Panel toggle labels elided; left/right → settings/points mapping unstated. |
| CO-1-17 | Nit | Alt C (346–354) | PNG default change buried in Alternatives; original WebP rationale missing. |
| CO-1-18 | Nit | 259 vs 429 | Format/quality saved on change or on export? |
| CO-1-19 | Nit | 62–63, 391 | WebKit canvas limit unsourced. |

## Detailed Range (lines 403–514)

**What this proposes**

Concrete edits to `levelup-circle-generator.html` (claims, one focus-visible rule, ARIA on header/toasts, neutral metadata values, PNG default, a set of new/changed/removed functions), claim fixes in `index.html`, `llms.txt`, the v2 blog (dated correction), `CLAUDE.md`, and four `docs/` files. Verification: local static server, three console checks of `normalizeState`, an Impeccable lint, manual CUJ checks in Chrome and the iOS simulator, a 36-point `redraw()` timing, and an old-URL regression.

**How the existing system works, per this document**

Static server, no build or unit tests; checks run in the console against globals. `develop` is the integration branch and lint baseline. `CLAUDE.md` holds project notes; rules said to live in `AGENTS.md`.

## Findings — Detailed Range

| ID | Sev | Scope | Finding |
|---|---|---|---|
| CO-1-20 | Nit | 480 | `AGENTS.md` does not exist in the repo. |
| CO-1-21 | Nit | 488–510 | Server command lacks cwd/URL; iOS simulator canvas limit unconfirmed; CUJ 1 says 8192 but verification exports 1080; perf check has no threshold. |
| CO-1-22 | Nit | 455–480 | File list omits `README.md`, `docs/url-sharing-spec.md`, `docs/ask-chatgpt-feature.md` (see CO-1-5). |
| CO-1-23 | Nit | 514 | "EGM" undefined. |
