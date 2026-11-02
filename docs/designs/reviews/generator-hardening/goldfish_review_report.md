# Goldfish Review Resolution (egm-resolve)

*   **Target:** `docs/designs/generator-hardening.md`
*   **Input:** [consolidated_review_report.md](consolidated_review_report.md) action items A1–A13
*   **Mode:** Uninterrupted (owner asked for the review feedback to be implemented); all PROPOSE FIX items applied in revision 2 of the design.

## PROPOSE FIX (applied)

| Action | Resolution in revision 2 |
|---|---|
| A1 | Phase 0 records the hotfix shipped as PR #1 (validated in headless Chrome). |
| A2 | §1 rewritten: one `ctx.scale(size/800)`; `shadowBlur` multiplied by `ctx.getTransform().a`; every absolute floor listed for removal; no minimum preview size; scaling isolated in Phase 5 as its own commit. Q2 replaced by a stated consequence in Risks. |
| A3 | §2 has a full field table; `validateState` grows in place into `normalizeState`; missing booleans → `true`; `numPoints = points.length`; no truncation; Example/Clear keep `initializePoints`. |
| A4 | Draw order stated as per-point loop; `AGENTS.md` reference removed; `llms.txt` change limited to line 19. |
| A5 | §3: number inputs stop writing back on keystrokes; `#points` commits on `change`; slider 0-safe; `removedPoints` deleted. |
| A6 | §4: step-down loop over `validExportSizes`; failed canvases released; actual size used in filename/metadata/toast; `#exportBtn` busy state; delayed revoke; anchor attached to DOM. |
| A7 | `confirm()` guards the Clear and Load Example buttons only (programmatic first-run example load stays silent). Share-link overwrite → Non-Goal; URL loads keep the recipient's metadata and export preferences. |
| A8 | §6 lists every site from a repository grep with exact replacement text; canonical phrase "with embedded metadata (PNG)"; blog correction note exempt. |
| A9 | §5: replace the `outline:none` rule with a `:focus-visible` rule; toggle label built from `panelModeLabels`; toast icon `aria-hidden`; named point controls; focus kept after delete. |
| A10 | G6/Q1 removed (owner kept Richard Crane defaults); WebP stays the default format; format/quality listeners write `state`. |
| A11 | Phases 0–5 with checkpoints; Risks table (4 rows); minor fixes grouped under G7. |
| A12 | Verification has pass criteria per check, a G5 grep, legacy-URL fixtures including the malicious link, and a real-device note. Impeccable lint removed from this design. |
| A13 | Links use `../`; terms defined; one change per bullet; neutral wording. |

## CONTEST

| Finding | Evidence |
|---|---|
| style M1 (v1 blog mentions AVIF) | The `level_up_circle_blog.md:20` match is inside a base64 JPEG data URI, not prose. No change. |
| CR-M3 per-source default tables | All external loaders already default missing booleans to `true` (`levelup-circle-generator.html:1944–1950`, `:1985–1991`, `:2245–2250`); one rule suffices. Only the legacy `lo` fallback of 35 is kept in the legacy decoders. |
| design N2 `aria-describedby` for long canvas labels | WCAG 2.2 AA has no length limit on accessible names; a single label keeps one source of truth. Accepted risk. |

## Owner decisions recorded

*   Metadata defaults stay "Richard Crane" / "Copyright Richard Crane. All Rights Reserved." (asked 2026-11-02).
*   Accessibility target: WCAG 2.2 AA.
*   Scaling consequence (exports now match the preview at every window size, so exports taken from windows other than 800 px change) is accepted as the purpose of G1; flagged in the PR for owner veto.
