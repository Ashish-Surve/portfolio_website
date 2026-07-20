# Design mockups — "Bold" direction

Interactive HTML prototypes for the portfolio redesign. Open `index.html` in a browser to browse all mockups. Each file is self-contained (no build step, no dependencies).

Goal: make a recruiter stop scrolling. Dark, cinematic, motion-rich — but every number shown is real and traceable to the résumé.

## Files

| File | Page |
|---|---|
| `index.html` | Design hub — links to every mockup |
| `home.html` | Homepage: animated gradient-mesh + particle hero, magnetic CTAs, live stat counters, 3D-tilt project cards |
| `projects.html` | Projects listing: staggered scroll reveals, alternating rows |
| `project-detail.html` | Project page: reading progress bar, animated stats, section reveals |
| `about.html` | About: timeline with scroll-draw line, skill marquee |

## Design tokens

| Token | Value | Use |
|---|---|---|
| `--bg` | `#07080c` | Page background |
| `--bg-raised` | `#0e1016` | Cards, panels |
| `--fg` | `#f4f5f7` | Primary text |
| `--fg-muted` | `#9aa3b2` | Secondary text |
| `--accent` | `#6366f1` → `#22d3ee` | Gradient accent (indigo → cyan) |
| `--border` | `rgba(255,255,255,.08)` | Hairlines, card borders |
| Radius | `16px` cards, `999px` pills | |
| Font | Inter (800 display / 600 UI / 400 body) | loaded from Google Fonts |

## Motion vocabulary

- **Hero load-in:** staggered fade-up, 120ms apart, `cubic-bezier(.22,1,.36,1)`.
- **Particle canvas:** ~70 drifting dots + connecting lines, reacts to cursor. Pauses off-screen.
- **Magnetic buttons:** CTA translates toward cursor within a 60px radius, springs back on leave.
- **Counters:** stats count up from 0 when scrolled into view (once).
- **3D tilt:** project cards tilt up to 8° toward cursor with a moving glare highlight.
- **Scroll reveals:** IntersectionObserver, one-shot, threshold 0.15.
- **Progress bar:** gradient bar at viewport top on long reading pages.
- All motion is wrapped in `@media (prefers-reduced-motion: reduce)` → static fallback.

## Porting notes (if this direction wins)

- The mockups are plain HTML/CSS/JS — everything ports to Quarto via `styles.scss` + `include-after-body` snippets (same mechanism as the current `assets/reveal.html`).
- This direction replaces the current "one dark moment, then light" principle in `/DESIGN.md` with dark-throughout. If adopted, update `/DESIGN.md`; if not, cherry-pick components (counters, tilt cards, magnetic CTAs work fine on the light theme too).
- Reading surfaces (project prose, code blocks) keep high contrast and larger type; syntax highlighting would need a dark theme (`highlight-style: github-dark`).
