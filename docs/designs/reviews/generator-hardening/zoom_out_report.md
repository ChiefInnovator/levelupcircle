# Stage 5 — Initial Zoom Out Report

*   **Target:** `docs/designs/generator-hardening.md` (full file, lines 1–514)
*   **Reviewer:** egm-zoom-out (completed)
*   **Standard:** zoom-out-standard
*   **Verdict:** No Blockers. 7 Major, 6 Nit.

Context sufficiency passes. Alternative A's rejection is sound. Single-file / no-build fits the project.

## Major

| ID | Scope | Finding |
|---|---|---|
| ZO-1-1 | Tech Plan §2; Detailed "new/removed" | `validateState()` (generator `:2018`) already exists; design adds `normalizeState` without retiring it. Grow `validateState` into `normalizeState`; list it as removed/renamed. |
| ZO-1-2 | Problem #2; Tech Plan §3 | Root cause is every `input` handler treating keystrokes as final. `labelFontSize` (`:2984`) and `titleFontSize` (`:3002`) clamp mid-typing (can't type 20/30); `labelOffset` (`:2978`) turns 0 into 25, so CUJ 3's `labelOffset: 0` can't be authored. Need one inbound form→state path; clamp write-back only on `change`/blur. |
| ZO-1-3 | Tech Plan §3; Detailed line 423 | `removedPoints` stack is partial undo; undo is a Non-Goal. `change` alone meets G2. Drop it. |
| ZO-1-4 | Tech Plan §1 | `shadowBlur` ignores the canvas transform; preview is dpr-pre-scaled, so glow differs on HiDPI and G1's 1% target fails. Simpler: `ctx.scale(size/800)` once, draw in 800-unit space, scale only `shadowBlur` by `ctx.getTransform().a`. |
| ZO-1-5 | Q2 vs Tech Plan §1 | Q2 promises an 8 CSS px font floor; §1's 320 px minimum yields 6.4 px (3.2 px at font 8). Forcing a canvas wider than the phone container overflows. Pick one rule. |
| ZO-1-6 | Tech Plan §4; NFR; CUJ 4 | Probe allocates a second full-size canvas (≈256 MB at 8192²) on memory-limited devices; halving yields sizes outside `validExportSizes` (`:2877`); session cache unneeded. Check the export canvas itself and step down through `validExportSizes`; release failed canvases (`width = 0`). |
| ZO-1-7 | G5; Tech Plan §6; Target Files | Stale claims missed: `README.md:18,81,85,90`; `docs/url-sharing-spec.md:10,18,93`; `docs/ask-chatgpt-feature.md:22,52,72,114,145`; generator `:61`; hidden article `:1007` ("Fullscreen presentation mode"). `fs=1` is still a live param — document it, don't drop it. Make G5's test a repo grep. |

## Nit

| ID | Scope | Finding |
|---|---|---|
| ZO-1-8 | Problem #4; §3 | "Every point edit rebuilds the list" is false — label/color edits only `redraw()`. Labels already escaped; DOM rewrite is for ARIA, not security. |
| ZO-1-9 | Alternative C | Adopting PNG default is scope creep hidden in Alternatives; move to Open Questions. Persisting format/quality is a feature — mark optional. |
| ZO-1-10 | §3 Clear; CUJ 6 | Two-click arm adds state; `confirm()` gives the same protection (codebase already uses `prompt()`). |
| ZO-1-11 | §2, §3, §4 | `toInt`, `data-armed`, `setTimeout(…,1000)`, regex literals belong in Detailed Plan; architecture should state contracts. |
| ZO-1-12 | Detailed line 420 | `EXAMPLE_STATE` frozen object duplicates `initializePoints()` (`:1563`); reuse it. |
| ZO-1-13 | Detailed lines 479–480, 497–498 | No `AGENTS.md` exists in repo; `npx impeccable detect` is an unpinned external gate. |
