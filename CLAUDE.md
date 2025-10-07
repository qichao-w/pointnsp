# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static research demonstration website for MarDini (Masked Auto-Regressive Diffusion for Video Generation). The site is built with vanilla HTML/CSS/JavaScript and uses SCSS for styling. It features interactive video/image galleries with dot navigation controls and a separate "clarity" demo section.

## Build and Development Commands

### SCSS Compilation
```bash
# Compile main homepage styles
sass assets/stylesheets/main.scss assets/stylesheets/main.css --no-source-map

# Compile clarity demo styles
sass clarity/clarity.scss clarity/clarity.css --no-source-map
```

### Local Development Server
```bash
# Serve from repo root
python3 -m http.server 8000
# Then visit http://localhost:8000
```

### Code Formatting (optional, requires Node.js)
```bash
npx prettier --write index.html assets/scripts/main.js clarity/clarity.js
```

## Architecture and Code Organization

### Interactive Slide System
The core UI pattern is a **dot-navigation slide system** implemented in `assets/scripts/main.js`:

- **Pattern**: Multiple independent slide galleries (interpolation, slow, nvs, etc.)
- **Structure**: Each gallery has:
  - A container with class `slide-menu-{name}` containing dots
  - Multiple content divs with class `slide-content-{name}`
  - Click handlers that toggle active dots and show/hide corresponding content
- **Implementation**: Event delegation on parent `slide-menu-*` elements, filtering for `.dot` children by index

When adding new galleries:
1. Create HTML structure: `<div class="slide-menu {name}"><ul class="dots" id="slide-menu-{name}">` with dot elements
2. Add content divs with class `slide-content-{name}`
3. Add event handler in main.js following existing pattern (lines 1-70)

### Clarity Demo System
Located in `clarity/` directory with self-contained structure:

- **jQuery-based dropdown controls** that dynamically adjust width to match selected text
- **Image swapping logic** for Parti comparisons using external Google research URLs
- **Separate stylesheet** (`clarity.scss`) with its own compilation process
- **Video assets** stored in `clarity/videos/` subdirectory

The clarity demo uses jQuery selectors and change handlers to update image sources based on dropdown selections.

### Styling Architecture
- **Design tokens** defined in `assets/stylesheets/_master.scss`:
  - Breakpoint variables: `$extra-small-device` through `$super-super-large-device`
  - Font size variables: `$extra-extra-small-font-size` through `$extra-super-large-font-size`
  - Color palette: grayscale values (`$gray-1` to `$gray-100`), `$white`, `$black`, `$light-black`
  - Custom fonts: Charter, Poppins, Fira Code

- **Main stylesheet hierarchy**:
  - `_master.scss` contains all design tokens and font imports
  - `main.scss` and `main_free.scss` are entry points that import `_master.scss`
  - Always edit SCSS files, never edit compiled CSS directly

## Testing Guidelines

**No automated test harness exists.** Manual testing workflow:

1. **SCSS Rebuild**: Run sass compilation after any style changes
2. **Browser Refresh**: Reload local server (port 8000)
3. **Cross-browser Testing**: Test in both Chromium and WebKit

### Key Test Cases
- **Slide navigation**: Click each dot in all slide-menu sections (`interpolation`, `slow`, `nvs`) to verify correct panel toggles
- **Clarity demo**:
  - Verify dropdown width auto-adjusts to selected text
  - Confirm Parti images swap correctly based on selector combinations
  - Check all videos in `clarity/videos/` play without console errors

## Coding Standards

### SCSS
- Use 4-space indentation
- Reference design tokens from `_master.scss` (never hardcode breakpoints/colors)
- Use lowercase-hyphenated class names (e.g., `.slide-content-slow`)

### JavaScript
- Use `const`/`let` (no `var` in new code)
- camelCase for helper functions and variables
- PascalCase only for UI controller classes/constructors

### HTML
- Double-quoted attributes
- Delegate styling to SCSS classes (avoid inline styles)
- Maintain semantic structure

## Asset Management

### Organization
- **Homepage media**: `assets/figures/`, `assets/fonts/`
- **Clarity demo media**: `clarity/images/`, `clarity/videos/`
- **Mirror this pattern** for new microsites/demos

### Best Practices
- Compress images and videos before committing
- Store assets in the closest matching folder to usage location
- Regenerate source maps when shipping compressed CSS for debugging

## Commit Conventions

- Short imperative subjects (optional conventional commit prefixes: `feat:`, `fix:`)
- Include rationale or sources in commit body
- For visual changes: include before/after screenshots or screencasts
- Document manual QA steps and build commands used for verification
