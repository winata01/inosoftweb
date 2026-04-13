# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm install          # Install dependencies
npm run dev          # Start dev server at http://localhost:3000 (0.0.0.0)
npm run build        # Production build to dist/
npm run preview      # Preview production build
npm run lint         # Type-check with tsc --noEmit
npm run clean        # Remove dist/
```

Set `GEMINI_API_KEY` in `.env.local` before running (used by the `@google/genai` dependency).

## Architecture

This is a **multi-page static site** built with Vite + Tailwind CSS v4. There is no JavaScript framework — pages are plain HTML files with inline `<script>` blocks.

**Pages** — each is a separate Vite entry point configured in `vite.config.ts`:
- `index.html` — home (hero, history, services, impact, testimonials, contact footer)
- `about.html` — company history timeline, culture values diagram, contact form
- `services.html` — services detail page

**Styling** — Tailwind CSS v4 via the `@tailwindcss/vite` plugin. The design system is defined entirely in [`assets/index.css`](assets/index.css) using `@theme {}`:
- Custom colors: `primary`, `primary-container`, `secondary`, `secondary-container`, `surface`, `surface-low`, `surface-lowest`, `on-surface`, `outline-variant`
- Utility classes: `.glass` (glassmorphism), `.hero-gradient`, `.btn-gradient`
- Nav scroll state toggled by adding/removing the `scrolled` class on `#mainNav` via JS

**Icons** — Lucide icons loaded via CDN (`unpkg.com/lucide`), initialized with `lucide.createIcons()` in each page's inline script.

**Assets** (`assets/`):
- `index.css` — single shared stylesheet for all pages
- `logo_white.png` / `logo_color.png` — nav logos swapped on scroll (white → color)
- `hero-pattern.svg` — honeycomb decorative SVG in hero section

**Deploy base path** — `base: '/inosoftweb/'` in `vite.config.ts` (targets GitHub Pages at `/inosoftweb/`).

**Dependencies of note**:
- `@google/genai` — Gemini AI SDK (API key injected at build time via `process.env.GEMINI_API_KEY`)
- `motion` — animation library (available but not yet used in pages)
- `express` + `dotenv` — present as dependencies, likely for a future/separate server component

## Constraints

- Do not introduce React, Vue, or any JS framework — pages must remain plain HTML with inline scripts.
- Do not modify `vite.config.ts` without asking first.
- Do not add new npm dependencies without asking first.

## Workflow

- When a change touches more than one file, outline the plan before making edits.

