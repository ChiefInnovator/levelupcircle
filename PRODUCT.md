# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Three confirmed audiences (owner, 2026-11-02):

- **Self-serve individuals** who arrive from search, a shared link, or an article, map their own goals, and leave with an image or link. No account, no onboarding.
- **Coaches and consultants** who build circles with or for clients and teams, often live in a session, then export or share the result.
- **Richard Crane's own content**: talks, articles, and MILL5 material use exported circles as figures, so print-quality export matters.

## Product Purpose

Turn a list of goals into a Level-Up Circle: a radial diagram where every focus area sits on one ring and is connected to every other, showing that progress in one area feeds the rest. Success is a user who leaves with a diagram they are proud to share or print, and a clearer view of which areas reinforce each other.

## Positioning

A Level-Up Circle is Richard Crane's personal flywheel framing, drawing on Jim Collins (Flywheel, Hedgehog) and Martin Eppler (synergy mapping). The tool is the reference implementation of that concept: free, no sign-up, runs entirely in the browser, and exports up to 8192 × 8192 at 300 DPI.

## Operating Context

- Used on desktop for building and on phones for viewing shared links.
- A shared link restores the full diagram; the `fs=1` parameter opens it with both panels hidden for presenting.
- Exports go into slides, articles, printed handouts, and social posts.
- "Ask ChatGPT" hands the circle's labels to ChatGPT for a strategy prompt.

## Capabilities and Constraints

- Single HTML file, vanilla HTML/CSS/JS, no build step, served by GitHub Pages; must also work when opened from `file://`.
- Pinned CDN dependencies only: lz-string 1.5.0 (URL compression), marked 12.0.1 (article viewer).
- 1–36 points; PNG and WebP export at 1080–8192 px; metadata embedded in PNG only.
- State: URL (compressed) → localStorage → default example; auto-saves.
- Export metadata defaults to Author "Richard Crane" and "Copyright Richard Crane. All Rights Reserved."; this is deliberate (owner decision 2026-11-02).
- Branch flow: `feature/*` → `develop` → `main`; `main` is production.

## Brand Commitments

- Name: "Level-Up Circle" / "Level-Up Circle Generator".
- Author attribution: Richard Crane, Founder & Chief AI Officer at MILL5.
- Framework attribution to Jim Collins and Martin Eppler stays on the About page and structured data.
- Dark theme is the default for both the app and the exported image.

## Evidence on Hand

- `level_up_circle_blog.md`, `level_up_circle_blog_v2.md`: origin and rebuild articles.
- `preview.png`: dark-theme circle used for link previews.
- `Rich/level-up-circle-3840x3840 no lines.jpg`: Richard's own circle.
- No testimonials, usage numbers, or customer logos exist; do not invent them.

## Product Principles

1. The diagram is the product; every control exists to shape or ship it.
2. What you see is what you export and what you share.
3. No account, no server, no data leaving the browser except links the user clicks.
4. Never lose a user's labels without asking.

## Accessibility & Inclusion

Target WCAG 2.2 AA (owner decision 2026-11-02): every control named, visible focus, keyboard operable, announced status messages, and a canvas description that includes the title and focus areas.
