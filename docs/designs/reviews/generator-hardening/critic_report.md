# Stage 7 — Critic Report

*   **Target:** `docs/designs/generator-hardening.md`, FeatureRange lines 1–402
*   **Reviewers:** egm-design (completed, feature-spec-standard), egm-structural (completed, feature-spec-standard), egm-style (completed)
*   **Totals:** 1 Blocker, 21 Major (after dedupe), ~30 Nits

## Blocker

| ID | Reviewers | Scope | Finding |
|---|---|---|---|
| CR-B1 | style, design (M4), structural (4) | Q2 (141–146) vs §1 (225–230) | Minimum-font default contradicts itself (8 px promised; 320 px floor yields 6.4 px / 3.2 px). Q2 also misframes the effect: the **export** changes for most users (any window ≠ 800 px); the preview changes only < 400 or > 1200 px. 320 px canvas also fights CSS `max-width:100%` below ~360 px viewports (squeezed + blurry). |

## Major

| ID | Reviewers | Scope | Finding |
|---|---|---|---|
| CR-M1 | design M2 | Problem 5; NFR Security | **Reflected XSS (confirmed in code):** legacy decoder builds `#${color}` unvalidated (`:2003`) and `updatePointList` writes it into `innerHTML` `value="…"` (`:1625`). A crafted `?p=x|"><img onerror=…>` link runs script on open. Threat model must cover attacker URLs; ship decoder validation as a hotfix-grade first step; add a malicious-link verification case. |
| CR-M2 | design M1, structural 5 | NFR (367–381) | No Risks & Mitigations: old links rendering differently, wrap drift, iOS probe memory, large PNGs, unmeasured frame time, untested refactor. |
| CR-M3 | design M3 | Non-Goals 130; §2; Rollout | "Links keep working unchanged" conflicts with normalizer: decoders treat missing booleans as `true`; `DEFAULT_STATE` booleans unspecified; `labelOffset` 35 legacy default dropped; 80-char truncation silently loses content. Use source-specific defaults. |
| CR-M4 | design M5 | G1; CUJ 1; §1 | Unlisted absolute floors (`:2333`, `:2477`, `:2541`, `:2546`) break uniform scaling; `measureText` at very different px sizes can wrap differently. Compute wraps at 800-unit reference and reuse. |
| CR-M5 | design M6 | G2; §2–3 | Bigger data-loss paths ignored: opening a share link overwrites the recipient's saved circle via auto-save; Load Example replaces the circle with one click. |
| CR-M6 | design M7 (= ZO-1-2) | Problem 2; CUJ 3; §3 | `labelFontSize`/`titleFontSize` clamp mid-typing; `labelOffset` slider stores 0 as 25 — CUJ 3 unreachable. |
| CR-M7 | design M8 (= ZO-1-6, CO-1-2) | G4; §4 | Probe halving skips valid sizes, cache ambiguous, probe canvas not released, `toBlob` null/OOM not retried, < 2048 undefined. |
| CR-M8 | design M9 | G6; Q1 | Returning visitors already have the Richard Crane strings auto-saved; G6 wouldn't reach them. *(Superseded: owner chose to keep Richard Crane defaults — see resolution.)* |
| CR-M9 | design M10 (= ZO-1-3, CO-1-14) | §3 | `removedPoints` lifecycle undefined; can resurrect labels from another circle. |
| CR-M10 | design M11, structural 6, style 3 (= ZO-1-7, CO-1-5) | G5; §6 | Missed files (`CLAUDE.md:24`, `README.md`, hidden article `:1007`, `docs/ask-chatgpt-feature.md`); `llms.txt` mis-described; correction line contradicts G5 wording. Use a repo grep as acceptance. |
| CR-M11 | design M12, structural 2–3, style 4 | Alternatives (318–363) | Alt C is an adopted decision with a contradictory title; riskiest decisions (Clear confirm, point-count handler, iOS cap, revoke delay) have no alternatives; D's rejection isn't a reason. |
| CR-M12 | design M13 | 13–14; Rollout | No regression net for a large untested refactor deploying straight from `main`; require fixed old-URL cases and one iPad run before promotion. |
| CR-M13 | structural 1, style 5 (= CO-1-6) | 387–390 | Additional Resources links broken from `docs/designs/` (need `../`). |
| CR-M14 | structural 6 | Problems 4, 5, 7, 11 | Fixes without matching goals/acceptance criteria. |
| CR-M15 | style 2 | Problem 8 (69) | Overstated: default export is WebP (`:1096`), which carries no metadata. |
| CR-M16 | style 6 (= CO-1-7) | 128 | "Impeccable" undefined. |
| CR-M17 | style 7 | §3 heading (262), 95 | "cannot destroy data" is absolute; delete/Clear destroy by design. |
| CR-M18 | style 8 | 35 | Clamp ranges not tied to label vs title. |

## Nits (grouped)

*   **Accessibility detail (design N1–N3):** `title` is a fallback name (weak, not absent); hide toast icons from AT; consider `role="alert"` for errors; 36×80-char canvas label is long — consider `aria-describedby`; "screen" → "screen reader"; skip link / `role="form"` neither in scope nor Non-Goal.
*   **State detail (design N4–N5, CO-1-13/15):** `numPoints` from `points.length` after normalize; empty-label storage `""` vs "Point N".
*   **Perf (design N6):** set a frame budget; coalesce redraws with `requestAnimationFrame`.
*   **Rollback (design N7):** rollback needs `feature/revert-*` → `develop` → `main` per branch policy.
*   **Sources (design N8, N9; style 9–10, 24; structural 10):** cite WebKit canvas limit; drop speculative `q` claim; name Safari behavior source.
*   **Wording (style 11–23, 25–27):** "Honest" → "Accurate"; "deliberate" unmeasurable; define "owner"; "trimmed" → "truncated"; `DEFAULTS` vs `DEFAULT_STATE`; size notation; "requested side", "session-only", "two-click arm" jargon; "silently editing history"; vague "line counts", "large dependency", unsourced MB figures; "every older shape"; restated single-file/no-build.
*   **Structure (structural 7–9):** Alt D lacks lead-in; Alt D vs Future Improvements mismatch; §2 diagram labels/arrows unclear.
