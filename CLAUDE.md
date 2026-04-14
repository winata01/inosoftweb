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
- `engagement.html` — engagement model page
- `privacy.html` — privacy policy page

**Styling** — Tailwind CSS v4 via the `@tailwindcss/vite` plugin. The design system is defined entirely in [`assets/index.css`](assets/index.css) using `@theme {}`:
- Custom colors: `primary`, `primary-container`, `secondary`, `secondary-container`, `surface`, `surface-low`, `surface-lowest`, `on-surface`, `outline-variant`
- Utility classes: `.glass` (glassmorphism), `.hero-gradient`, `.btn-gradient`
- Nav scroll state toggled by adding/removing the `scrolled` class on `#mainNav` via JS

**Icons** — Lucide icons loaded via CDN (`unpkg.com/lucide`), initialized with `lucide.createIcons()` in each page's inline script.

**Assets** (`assets/`):
- `index.css` — single shared stylesheet for all pages
- `logo_white.png` / `logo_color.png` — nav logos swapped on scroll (white → color)
- `*.jpg` — images used in pages

**Design** (`design/`):
- Refer to this folder to see the UI design to recreate

**Deploy base path** — `base: '/inosoftweb/'` in `vite.config.ts` (targets GitHub Pages at `/inosoftweb/`).

**Dependencies of note**:
- `motion` — animation library (available but not yet used in pages)
- `express` + `dotenv` — present as dependencies, likely for a future/separate server component

## Constraints

- Do not introduce React, Vue, or any JS framework — pages must remain plain HTML with inline scripts.
- Do not modify `vite.config.ts` without asking first.
- Do not add new npm dependencies without asking first.

## Security Requirements

All code must follow OWASP Top 10 mitigations:
- Always use parameterized queries / Eloquent ORM, never raw SQL with user input
- Validate and sanitize all user inputs server-side
- Use Laravel's CSRF protection on all forms (never disable VerifyCsrfToken)
- Escape all output in Blade templates (use {{ }} not {!! !!} unless explicitly safe)
- Enforce authentication middleware on protected routes
- Never expose .env values in responses or logs
- Use Laravel's built-in hashing for passwords (bcrypt via Hash::make)
- Sanitize file uploads — validate MIME type, size, and store outside public/
- Set proper HTTP security headers (X-Frame-Options, CSP, X-Content-Type-Options)

When adding new features, flag any potential security concerns before implementing.

## Code Review Checklist
Before finishing any feature, verify:
- No sensitive data in logs
- All routes have appropriate middleware
- No hardcoded credentials

## Workflow

- When a change touches more than one file, outline the plan before making edits.

