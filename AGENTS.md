# Repository Guidelines

## Project Structure & Module Organization
- `index.html` renders the site shell and links the compiled CSS/JS bundles.
- `assets/stylesheets` stores SCSS sources (`main.scss`, `_master.scss`) plus generated CSS; always edit SCSS then rebuild.
- `assets/scripts/main.js` plus media folders under `assets/` (`figures`, `fonts`) drive homepage interactions; keep binaries lean.
- `clarity/` contains the clarity demo with its own `clarity.scss`, `clarity.js`, `images/`, and `videos/`; mirror this structure for new microsites.

## Build, Test, and Development Commands
- `sass assets/stylesheets/main.scss assets/stylesheets/main.css --no-source-map` updates homepage styles; run the same pattern for `clarity/clarity.scss`.
- `python3 -m http.server 8000` (from repo root) serves the static site for local QA.
- `npx prettier --write index.html assets/scripts/main.js clarity/clarity.js` keeps markup and scripts tidy when Node is available.

## Coding Style & Naming Conventions
- Use 4-space indentation in SCSS and reuse tokens from `_master.scss`; keep class names lowercase-hyphenated (e.g., `.slide-content-slow`).
- Favor `const`/`let` in JavaScript, camelCase for helpers, and PascalCase only for UI controllers such as `ToggleVideo`.
- HTML should prefer double-quoted attributes and delegated styling via SCSS rather than inline styles.

## Testing Guidelines
- There is no automated harness; manually rebuild SCSS, refresh the local server, and retest in Chromium and WebKit.
- Click every dot selector managed by `assets/scripts/main.js` to confirm the matching `slide-content-*` panel toggles.
- In the clarity demo, verify dropdown resizing, Parti image swaps, and that each asset in `clarity/videos` plays without console warnings.

## Commit & Pull Request Guidelines
- With no existing history, use short imperative subjects (optionally `feat:`, `fix:`) and note rationale or sources in the body.
- Keep PRs focused, link tracking issues, and include before/after screenshots or screencasts for visual updates.
- List any manual QA or build commands executed so reviewers can reproduce the checks.

## Asset Management Tips
- Compress new imagery or video before committing and store it beside the closest matching folder (`assets/figures`, `clarity/videos`, etc.).
- Regenerate source maps when shipping compressed CSS so browser debugging continues to map back to SCSS.
