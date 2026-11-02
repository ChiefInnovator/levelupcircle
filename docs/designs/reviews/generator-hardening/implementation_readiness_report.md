# Stage 9 — Implementation Readiness Report

*   **Target:** `docs/designs/generator-hardening.md`, DetailedRange lines 403–514
*   **Reviewers:** egm-implementation-readiness (completed), egm-codebase-details (completed), egm-style (completed)
*   **Verdict:** Not implementation-ready. 4 Blockers, ~22 Majors (deduped), ~25 Nits.

## Blockers

| ID | Reviewers | Scope | Finding |
|---|---|---|---|
| IR-B1 | readiness B1, codebase M7/M8 | 420–421, 427–428 | `DEFAULT_STATE`/`EXAMPLE_STATE` not fully specified; boolean defaults disagree across sources (clear/example `false`, loadState/decoders `true`); decoder output types, coercion, `numPoints` vs `points.length` (crash risk at `:2398/:2438`), invalid-entry replacement, point generation, deep-copy of frozen defaults, `validateState` fate all undefined. |
| IR-B2 | codebase B1 | 459 | Proposed "actual draw order" is wrong: title, ring, connections, then **per point** dot → guide line → label inside one loop (`:2437`). |
| IR-B3 | codebase B2, style M4 | 480 | `AGENTS.md` does not exist in the repo. |
| IR-B4 | codebase B3 | 446, 81 | `llms.txt` has no fullscreen wording; only fix is line 19 (AVIF + "embedded metadata"). |

## Majors

| ID | Reviewers | Scope | Finding |
|---|---|---|---|
| IR-M1 | codebase M1 | 413–415 | Existing `input[type=text]:focus,… { outline:none }` (`:419–425`, specificity 0,2,1) beats planned `input:focus-visible` (0,1,1) — focus stays invisible. |
| IR-M2 | codebase M2 | §1, 425 | `shadowBlur` ignores transform; preview under dpr transform → glow mismatch on HiDPI. |
| IR-M3 | readiness 2, codebase M3/M4 | 426; Q2 | 320 vs 400 px contradiction; CSS `max-width` squeezes it; unlisted floors (`:2333`, `:2334`, `:2477`, `:2541`, `:2546`). |
| IR-M4 | readiness 3/12, codebase M5 | 422–424, 434–436 | `updatePointList` callers, `#points` blur handler (`:3021`), `removedPoints` lifecycle, delete `data-index`, color-input event, focus after delete undefined. |
| IR-M5 | readiness 4, codebase M9 | 422 | Empty-label storage/fallback undefined across URL, metadata, ChatGPT, canvas label. |
| IR-M6 | readiness 5, codebase M10 | 424, 428 | Probe cache keyed wrong; halving skips 4096; no try/catch; probe canvas not released; filename/comment use requested size. |
| IR-M7 | readiness 6/7, codebase M6 | 425–433 | Format/quality listeners don't write `state`; `exportImage` reads DOM; `labelOffset` slider `|| 25` bug; `#points` `change` switch not listed. |
| IR-M8 | readiness 8, codebase M11/M12 | 419, 432 | Metadata edge cases (blank chunk, Author fallback, stored legacy strings). *Superseded by owner decision: keep Richard Crane defaults.* |
| IR-M9 | readiness 9, codebase M14–M16, style M1 | 405–484 | Missed claim sites: generator `:121` ("All exports include embedded metadata"), `:1007` (fullscreen), blog v2 `:95`, `CLAUDE.md:24`, `README.md`, `docs/url-sharing-spec.md`, `docs/ask-chatgpt-feature.md`, stale `architecture.md`/`features.md`/`state-management.md` items, v1 blog AVIF. |
| IR-M10 | readiness 10, codebase M17 | whole plan | No phases with checkpoints. |
| IR-M11 | readiness 11, codebase M18, style M5–M7 | 486–510 | Verification lacks pass criteria, edge cases, G5 grep, comparison method; `normalizeState({})` test not runnable. |
| IR-M12 | codebase M13 | 416–419 | Export button has no id. |
| IR-M13 | codebase M19, style N14 | 497–498 | Unpinned `impeccable detect` gate. |
| IR-M14 | codebase M20, readiness N1 | 416, 433 | Panel toggle labels inconsistent; reuse `panelModeLabels` (`:2116`); initial label. |
| IR-M15 | style M2 | 412, 447, 312 | Metadata-claim replacement copy worded three ways. |
| IR-M16 | style M3 | 420–436 | Run-on change lists; one item per bullet. |

## Nits (grouped)

EXIF orphan comment spans `:2781–2782`; `maxLength` for labels; Firefox detached-anchor download; `aria-hidden` toast icon; URL load resets recipient's format; no line anchors; self-referential "Create" entry; server command lacks URL; "CSS px" qualifier; inconsistent "size cap" terminology; `describeCanvas`/`armClear` undefined; vague roadmap items; "correction note/line"; footer acronym; iOS simulator vs real device.
