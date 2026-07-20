# Design system — portfolio site

Reference doc for the site's visual language. **Superseded 2026-07-21**: the
site moved from "one dark moment, then light" to the **Bold, dark-throughout**
direction prototyped in `docs/design/` (see that folder's README for the
original mockups). This file now documents the Bold system as shipped.
Update this file when the design changes — it's the source of truth, not the
CSS comments.

## Principles

1. **Dark throughout, not just the hero.** Every page — home, projects listing, project detail, about — uses the same dark background (`--bg: #07080c`). There is no light reading surface; code blocks, prose, and charts are all restyled for dark (`highlight-style: github-dark`, chart colors matched to the palette).
2. **Motion earns trust, it doesn't perform — but there's more of it.** Particle canvas, magnetic buttons, 3D-tilt cards, and live counters are all present, but every one is wrapped in `@media (prefers-reduced-motion: reduce)` and degrades to a static, fully legible state.
3. **Depth over breadth stays visible in the UI, not just the copy.** Three projects, deep rather than wide — reinforced by the homepage's tilt-card grid and the projects listing's full-width alternating rows, not a dense grid.
4. **Every header pattern is reusable, not homepage-only.** The eyebrow / large gradient title / supporting line structure is shared by the homepage hero (full 92vh) and the compact `.page-header` band (Projects listing, About).

## Design tokens

### Color

| Token | Value | Use |
|---|---|---|
| `--bg` | `#07080c` | Page background, everywhere |
| `--bg-raised` | `#0e1016` | Cards, panels, stat tiles |
| `--bg-raised-2` | `#12151d` | Nested/callout headers |
| `--fg` | `#f4f5f7` | Primary text |
| `--fg-muted` | `#9aa3b2` | Secondary text, body copy |
| `--fg-dim` | `#5c6474` | Footer, timestamps, least-emphasis text |
| `--a1` / `--a2` | `#6366f1` → `#22d3ee` | Gradient accent (indigo → cyan), used for `--grad` |
| `--border` | `rgba(255,255,255,.08)` | Hairlines, card borders |
| `--border-hi` | `rgba(255,255,255,.16)` | Hover border state |

No light theme remains — every component in `styles.scss` targets the dark tokens.

### Type

- Font: Inter (400/500/600/700/800/900 loaded via `assets/fonts.html`), JetBrains Mono for numeric/code accents (stat labels, breadcrumbs, timeline dates, code blocks).
- Hero display (`.bold-hero-inner h1`): `clamp(2.8rem, 7vw, 5.2rem)`, weight 900, `-.04em` tracking.
- Page header title (`.page-header-title`): `clamp(2.4rem, 5.5vw, 4rem)`, weight 900.
- Section heading (`.sec-title`): `clamp(1.9rem, 3.6vw, 2.8rem)`, weight 800.
- Body: 17px root / 1.6 line-height on marketing pages; project prose bumps to 17.5px / 1.75 for readability on dark.
- Eyebrow labels: `.8rem`, 700 weight, `.16em` tracking, uppercase, `--a2` (cyan) color — this is the one accent color used for labels site-wide, distinct from the gradient reserved for numbers/headlines.

### Spacing & shape

- Cards/panels: 14–20px corner radius depending on size (`.project-card` 18px, `.stats-inline .si` inherits 14px from parent grid, `.project-row` 20px).
- Full-bleed sections (`.bold-hero`, `.page-header`) use the `100vw` breakout technique (`margin-left: calc(50% - 50vw)`).
- Standard content max-width stays `820px` (`_quarto.yml` grid.body-width) for reading pages; marketing sections on the homepage use a wider `1100px` wrapper.

### Motion

- Hero load-in: staggered fade-up (`.bh-stagger`), 120ms apart, `cubic-bezier(.22,1,.36,1)`.
- Particle canvas: ~80 drifting dots + connecting lines on the homepage hero only, reacts to cursor, pauses off-screen via IntersectionObserver.
- Magnetic buttons (`.magnetic`): CTA translates toward cursor within a button-local radius, springs back on leave.
- 3D tilt (`.tilt`): project cards tilt up to ~10° toward cursor with a moving radial glare.
- Counters (`.count[data-to]`): count up from 0 once scrolled into view, cubic ease-out, respects reduced motion (jumps straight to final value).
- Scroll reveal (`.reveal`): IntersectionObserver, one-shot, threshold 0.15 — same mechanism as before, now also drives `.bar-fill` widths and the About timeline's progress line.
- Reading progress bar (`#reading-progress`): fixed gradient bar at viewport top, project pages only.
- All motion wrapped in `@media (prefers-reduced-motion: reduce)` → static fallback (verified for every animated component in `styles.scss` and `assets/reveal.html`).

## Component patterns

### 1. Homepage hero (`.bold-hero`)

Full 92vh, gradient mesh blobs + particle canvas, eyebrow pill with live-status pulse dot, huge two-line headline with gradient second line, tagline, three CTAs (primary gradient pill + two ghost pills), scroll cue. Immediately followed by a floating stats band (`.bold-stats`) that overlaps the hero's bottom edge.

### 2. Page header (`.page-header`) — Projects listing, About

Compact ~40vh dark band, same eyebrow/title/sub structure as the hero minus the photo and particle canvas, with two static gradient mesh blobs (`.page-header-mesh`) instead. Quarto's auto-rendered `#title-block-header` is suppressed via `main:has(.page-header) #title-block-header { display: none }` so there's no duplicate title.

### 3. Individual project pages — now dark, not light

Unlike the previous (light-with-breadcrumb) direction, project pages are fully dark: breadcrumb + tags at top, gradient-text result banner, a `.stats-inline` three-tile metric band, then prose sections with `.reveal`. Matplotlib chart chunks are recolored per-figure (background `#0e1016`, muted axis text `#9aa3b2`, accent lines from the gradient palette) so charts don't render as light rectangles on a dark page. A `.next-project` card closes every page, chaining project → project → back to project 1.

### 4. Project cards / rows

- Homepage: `.project-cards` grid, 3D-tilt (`.tilt`), gradient glare on hover, no thumbnail images (text-forward, per the Bold mockup — thumbnails are hidden via `.project-card img { display:none }`).
- Projects listing: `.project-row`, full-width alternating rows with a gradient-radial visual panel using the project's `hero.png`.

### 5. Navbar

**Changed from the previous "always light" decision.** The navbar is now always dark (`background: "#07080c"` in `_quarto.yml`), matching the rest of the site — there is no light surface left for a light navbar to contrast against. Still no scroll-triggered transparency/blur-in; it's a fixed, consistently-styled dark bar throughout, avoiding the flicker/jank risk flagged in the previous version of this doc.

### 6. Footer

Dark background, muted footer links (`--fg-dim` default, `--fg` on hover), matches the rest of the page — no visual seam at the bottom of the site anymore.

## Known content notes (carried over, still accurate)

- Trade promotion / OOS: 300,000 models, 2TB dataset, automated backtesting + hyperparameter tuning, $2.4M in avoided stockout/expiry losses — real, traceable via `about.qmd`.
- Russia engagement: 50,000 models, 84% accuracy, plus a Temporal Fusion Transformer for cross-learning adding +17% uplift — now included on the About timeline.
- RAG marketing assistant: 2nd place in Tiger Analytics' internal hackathon (corrected from an earlier "top 4 teams" framing) — reflected on the homepage card, projects listing, and project detail page.

## Decisions (confirmed, this pass)

- Full Bold dark-throughout direction adopted site-wide (not cherry-picked) — confirmed by user.
- `about.qmd` dropped the `trestles` Quarto about-template in favor of plain markup + a custom timeline component, because the template's built-in photo/sidebar layout assumptions collided with the dark page-header + timeline pattern (this was flagged as an open risk in the pre-Bold version of this doc, and became a real conflict once attempted).
- Navbar and footer both moved to always-dark, replacing the previous "navbar stays light" decision — there's no light theme left for a light navbar to anchor against.
- Chart color palettes in all three project-page Python chunks were updated in place (dark figure/axes background, light-legible text and gridlines) rather than left light-on-dark.

## Open items / follow-ups

- **Render verification**: this port was built and statically validated (YAML front matter, Python chunk syntax, SCSS brace-balance, JS id/class cross-references) in an environment without network access to install the Quarto CLI. Run `quarto render` locally before merging to confirm no Pandoc/Quarto-specific build errors and to eyeball the rendered output.
- Homepage project cards intentionally drop thumbnail images per the Bold mockup (text + gradient only) — confirm this reads well against the previous image-forward cards, or restore thumbnails with a dark treatment if the recruiter-facing test says otherwise.
- `docs/design/*.html` mockups can be deleted or kept as a historical reference now that the direction has shipped — not deleted in this pass in case of rollback.
