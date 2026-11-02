# Consolidated Goldfish Review — generator-hardening

*   **Target:** `docs/designs/generator-hardening.md` (Draft, 2026-11-02)
*   **Mode:** Uninterrupted full flow (Stages 5→9), all reviewers completed, none "not run".
*   **Stage reports:** [zoom_out](zoom_out_report.md) · [comprehension](comprehension_report.md) · [critic](critic_report.md) · [intermediate zoom out](intermediate_zoom_out_report.md) · [implementation readiness](implementation_readiness_report.md)
*   **Owner decisions received during review:** keep "Richard Crane" metadata defaults (supersedes G6, Q1, CR-M8, IR-M8); accessibility target WCAG 2.2 AA.

## Action items

### 🔴 Blockers

| # | Action | Traces to |
|---|---|---|
| A1 | Ship legacy-URL XSS fix as a separate hotfix. **Done: PR #1.** | CR-M1, ZO-2-1 |
| A2 | Resolve preview scaling: one `ctx.scale(size/800)`, no 320 px floor, no 8 px promise; scale `shadowBlur` by the real pixel transform; remove all absolute floors; move scaling to its own phase. | CR-B1, ZO-1-4/5, ZO-2-5/6, CO-1-1, CR-M4, IR-M2/M3 |
| A3 | Fully specify the normalizer: grow `validateState` in place; one field table (default, range, coercion, missing-key → today's behavior); missing booleans default `true` (fix `showConnections`); `numPoints = points.length`; invalid color → `#39FF14`, invalid/blank label → `Point N`; no truncation; Example/Clear keep `initializePoints`. | IR-B1, ZO-1-1, ZO-2-7, CR-M3, CO-1-4/13 |
| A4 | Fix factual errors: draw order (per-point loop); no `AGENTS.md`; `llms.txt` has no fullscreen. | IR-B2/B3/B4, CO-1-20 |

### 🟡 Major

| # | Action | Traces to |
|---|---|---|
| A5 | Fix all number inputs by deletion: drop per-keystroke clamp write-backs; `#points` commits on `change` (merge blur logic); 0-safe `labelOffset` slider. Delete `removedPoints`. | ZO-1-2/3, CR-M6/M9, ZO-2-4/8, IR-M4/M7 |
| A6 | Export: step-down loop over `validExportSizes` on the real canvas; release failed canvases; filename/comment/toast use actual size; error toast if 1080 fails; button id + busy state; delayed revoke; anchor attached to DOM. | ZO-1-6, CR-M7, ZO-2-9, IR-M6/M12 |
| A7 | Confirm destructive one-click actions with `confirm()` for Clear **and** Load Example. Share-link overwrite → Non-Goal / follow-up. | ZO-1-10, CR-M5, ZO-2-10 |
| A8 | Claims: enumerate every site (grep `avif`, `fullscreen`, `embedded metadata`, `chat.openai.com`) with exact replacement text; one canonical phrase "embedded metadata (PNG only)"; exempt blog correction note. | ZO-1-7, CO-1-5, CR-M10, IR-M9, IR-M15, ZO-2-15 |
| A9 | Accessibility: remove the `outline:none` focus rule or out-specify it; reuse `panelModeLabels` for the toggle label; `aria-hidden` toast icon; labels on point inputs; canvas description. | IR-M1, IR-M14, ZO-2-11 |
| A10 | Remove G6/Q1 and metadata-default changes (owner decision); drop PNG-default change; format/quality persistence via listeners writing `state`. | ZO-2-2/3, CR-M8, IR-M7/M8 |
| A11 | Add phases with checkpoints; add a ≤4-row Risks table; group minor fixes (revoke delay, ChatGPT host) under one goal. | IR-M10, CR-M2, CR-M14, ZO-2-12 |
| A12 | Verification: per-check pass criteria, G5 grep, legacy-URL/localStorage fixtures (incl. malicious link), real iPad, drop Impeccable gate from this design. | IR-M11, CR-M12, IR-M13, ZO-2-14 |
| A13 | Doc hygiene: fix `../` links, define terms, one bullet per change, consistent naming, remove absolute wording. | CR-M13, CO-1-6/7, CR-M16–M18, IR-M16 |

### 🟢 Nits

Folded into the rewrite where cheap (see stage reports for the full list).

## Traceability matrix (raw → action)

| Raw IDs | Action |
|---|---|
| CR-M1, ZO-2-1 | A1 |
| CR-B1, ZO-1-4, ZO-1-5, ZO-2-5, ZO-2-6, CO-1-1, CO-1-11, CO-1-12, CR-M4, IR-M2, IR-M3 | A2 |
| ZO-1-1, ZO-1-12, ZO-2-7, CR-M3, CO-1-4, CO-1-13, IR-B1 | A3 |
| IR-B2, IR-B3, IR-B4, CO-1-20 | A4 |
| ZO-1-2, ZO-1-3, CR-M6, CR-M9, ZO-2-4, ZO-2-8, CO-1-14, CO-1-15, IR-M4, IR-M5, IR-M7 | A5 |
| ZO-1-6, CO-1-2, CR-M7, ZO-2-9, IR-M6, IR-M12 | A6 |
| ZO-1-10, CR-M5, ZO-2-10 | A7 |
| ZO-1-7, CO-1-5, CO-1-22, CR-M10, IR-M9, IR-M15, ZO-2-15 | A8 |
| IR-M1, IR-M14, ZO-2-11, CO-1-16 | A9 |
| ZO-2-2, ZO-2-3, ZO-1-9, CO-1-3, CO-1-17, CO-1-18, CR-M8, CR-M15, IR-M8 | A10 |
| IR-M10, CR-M2, CR-M11, CR-M14, ZO-2-12, ZO-2-13 | A11 |
| IR-M11, IR-M13, CR-M12, ZO-1-13, ZO-2-14, CO-1-21 | A12 |
| CR-M13, CO-1-6, CO-1-7, CO-1-8–10, CR-M16–M18, IR-M16, CO-1-19, CO-1-23 | A13 |
