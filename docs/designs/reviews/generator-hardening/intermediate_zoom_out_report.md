# Stage 8 — Intermediate Zoom Out Report

*   **Target:** `docs/designs/generator-hardening.md` (full file, unrevised) + Stage 5–7 reports
*   **Reviewer:** egm-zoom-out (completed)
*   **Verdict:** 1 Blocker, 9 Major, 5 Nit. Context sufficiency passes.

## Recommended scope split

*   **Hotfix now:** reflected XSS only (apply the existing `/^[0-9A-Fa-f]{6}$/` check from `:1959` to the legacy decoder at `:2003`).
*   **Phase 1:** state/edit safety, iOS export cap, revoke delay, claims, accessibility, ChatGPT host.
*   **Phase 2:** 800-unit scaling model alone (changes every existing user's export; holds Q2; revertable on its own).

## Essential vs scope creep

*   **Adopt (minimum size):** CR-M1 (hotfix), CR-B1 (delete floor), ZO-1-2/CR-M6 (reuse blur handlers), CR-M4 (one `ctx.scale`), CR-M3 (keep defaults where they live), ZO-1-6/CR-M7 (step-down loop), ZO-1-7/CR-M10 (grep acceptance), CR-M12 (fixed URL list + one iPad run).
*   **Reject / defer:** `removedPoints` and its lifecycle, per-source default tables, `EXAMPLE_STATE`, 80-char truncation, PNG default (Alt C), wrap-cache pass, rAF coalescing, skip link / `role="form"` / `role="alert"` / `aria-describedby`, new goals for Problems 7 & 11, alternatives for one-line choices, Impeccable lint gate, G6 polishing (owner kept Richard Crane defaults).

## Findings

| ID | Sev | Scope | Finding |
|---|---|---|---|
| ZO-2-1 | Blocker | CR-M1; Problem 5; NFR Security; Rollout | Ship XSS fix as a separate hotfix; localStorage path is not attacker-controlled and stays out of it. |
| ZO-2-2 | Major | Problem 8; G6; Q1; 418–419, 432, 466–467 | Delete G6 and metadata-default changes (owner rejected); don't polish them (CO-1-3, CR-M15). |
| ZO-2-3 | Major | Alt C; 246; 419 | Cut the PNG default; §6 claim fix already solves it; worsens iOS memory. Format/quality persistence optional. |
| ZO-2-4 | Major | ZO-1-2/CR-M6; Problem 2; §3 | Fix handlers by deletion: drop `input` write-backs (`:2859–2875`, `:2959–3018`), 0-safe `labelOffset` (`:2978`), Points on `change` reusing blur logic (`:3021–3066`). No new inbound layer. |
| ZO-2-5 | Major | §1; CR-M4; ZO-1-4 | One `ctx.scale(size/800)`; `measureText` is transform-agnostic so wraps match; floors and `isExport` become dead code; only `shadowBlur` needs a multiplier. State the contract, not field lists. |
| ZO-2-6 | Major | Q2; §1; CO-1-1/12; ZO-1-5 | Delete the 320 px minimum and 8 px promise; Q2 becomes yes/no, reframed: exports from non-800 px windows change. |
| ZO-2-7 | Major | §2; 420; 493; CR-M3; CO-1-4/13 | Grow `validateState` (`:2018`) into `normalizeState`; all loaders already default missing booleans to `true` (fix `showConnections` outlier); `DEFAULT_STATE` seeds only Clear/first visit; Example keeps `initializePoints(8)`; drop truncation; fix line 493 test. |
| ZO-2-8 | Major | §3; 423; CR-M9 | Delete `removedPoints`. |
| ZO-2-9 | Major | §4; NFR; CR-M7 | Step-down loop over `validExportSizes` on the real export canvas; release failed canvases; error toast if 1080 fails. No probe/cache/UA sniff. |
| ZO-2-10 | Major | G2; CUJ 6; §3; CR-M5 | Share-link overwrite is a product question → Non-Goal/follow-up; narrow G2 wording. Use `confirm()` for Clear and Load Example. |
| ZO-2-11 | Nit | §5; CO-1-16 | Reuse unused `panelModeLabels` (`:2116`) for toggle `aria-label`; `aria-hidden` toast icon OK; reject other a11y extras. |
| ZO-2-12 | Nit | CR-M2/M11/M14; Problem 11 | Risks table ≤ 4 rows; no alternatives for trivial choices; group Problems 7 & 11 as minor fixes; drop speculative `q` claim. |
| ZO-2-13 | Nit | NFR Perf; 507 | No rAF coalescing until measured; add pass/fail threshold; `debouncedRedraw` exists if needed. |
| ZO-2-14 | Nit | 497–498; CO-1-7; CR-M12 | Drop Impeccable gate; regression net = fixed old URLs (incl. malicious link) + one iPad run. |
| ZO-2-15 | Nit | G5; §6; 453 | Exempt the blog correction note from G5's grep. |
