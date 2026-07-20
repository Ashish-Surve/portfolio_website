# Design system — portfolio site

Reference doc for the site's visual language. Written after the homepage dark-hero redesign, extending that pattern to the rest of the site. Update this file when the design changes — it's the source of truth, not the CSS comments.

## Principles

1. **One dark moment, then light.** The dark hero is a deliberate, singular first impression — a photo-driven "title card." Every reading surface (project pages, prose, charts, code) stays light. Recruiters skim, screenshot, and print; dark-on-dark reading surfaces work against that. Don't let dark creep past the hero.
2. **Motion earns trust, it doesn't perform.** Animation confirms things loaded correctly and guides the eye — fade-ups, gentle hover lifts, scroll reveals. Nothing bounces, spins, or calls attention to itself. If someone notices the animation before the content, it's too much.
3. **Depth over breadth stays visible in the UI, not just the copy.** The site's whole pitch is "three projects, deep rather than wide." Layouts should reinforce that: fewer, larger elements, not dense grids.
4. **Every dark-hero pattern is reusable, not homepage-only.** The eyebrow / large title / supporting line / CTA pair structure built for the homepage hero becomes the header pattern for every page — projects listing, each project page, about. Consistency across page headers is what makes the site feel designed rather than patched.

## Design tokens

### Color

| Token | Value | Use |
|---|---|---|
| `--dark-bg` | `#0b0c10` | Hero/header sections only |
| `--dark-fg` | `#f2f3f5` | Text on dark |
| `--dark-fg-muted` | `#8b93a1` | Eyebrows, captions, secondary text on dark |
| `--dark-border` | `#3a3e47` | Borders/dividers on dark |
| `body-bg` | `#ffffff` | Everything else |
| `body-color` | `#1f2328` | Body text |
| `muted` | `#57606a` | Secondary text on light |
| `primary` / `link` | `#2563eb` | Links, accents, primary actions |
| `border` | `#e6e6ec` | Card borders on light |
| `border-hover` | `#d5d5df` | Card borders on hover |

No new colors are introduced site-wide — every page reuses this palette so the "one dark moment" stays singular and intentional rather than becoming a second competing theme.

### Type

- Font: Inter (already set), weight 800 for display headings, 700 for section headings, 600 for UI/nav.
- Display scale (hero names/headlines): `clamp(2.6rem, 5vw, 4.2rem)`, tight letter-spacing (`-0.03em`), line-height `1.05`.
- Section headings (page titles on light pages): `clamp(1.8rem, 3vw, 2.4rem)`, `-0.02em`, line-height `1.15`.
- Body: existing 17px root / 1.6 line-height, unchanged.
- Eyebrow labels (small caps-style tags above a heading): `.85rem`, `600` weight, `.12em` letter-spacing, uppercase, muted color.

### Spacing & shape

- Card/section corner radius: `10px`–`12px` (already established by `.project-card`/`.project-row`).
- Full-bleed sections use `100vw` breakout (`margin-left: calc(50% - 50vw)`), consistent with the hero technique already built.
- Standard content max-width stays at the existing `body-width: 820px` from `_quarto.yml`.

### Motion

- Entrance: `fade-up` (translateY 14px → 0, opacity 0 → 1), `.6–.7s ease`, staggered `~120ms` per element.
- Scroll reveal: IntersectionObserver-driven `.reveal` / `.is-visible` pair (already built), threshold `0.15`, one-shot (unobserve after firing — never re-hides on scroll-up, which would feel gimmicky).
- Hover: `translateY(-2px)` + shadow/border color shift, `.15s ease`. No scale-up on hover (reserved for the hero photo's load-in only).
- Always wrapped in `@media (prefers-reduced-motion: reduce)` fallback to instant/static.

## Component patterns

### 1. Page header (new — generalizes the homepage dark hero)

Every top-level page (Projects listing, About, and optionally each Project page) gets a **compact dark header band** instead of the full 88vh homepage treatment. Same visual language, shorter:

```
┌─────────────────────────────────────────────┐
│  DARK BAND (~30–38vh, not 88vh)              │
│                                               │
│   EYEBROW LABEL                              │
│   Page Title (large, animated fade-up)       │
│   One-line supporting description            │
│                                               │
└─────────────────────────────────────────────┘
        ↓ soft gradient seam into light body
```

- No photo on these secondary headers — photo is a homepage-only device, reused elsewhere it dilutes the "one moment" principle.
- Same fade-up stagger animation as the homepage hero, just simpler (title + one supporting line, no CTA row needed on most).
- Projects listing header: eyebrow "Selected work", title "Projects", supporting line reuses existing intro copy ("Three projects, deep rather than wide...").
- About header: eyebrow "About", title "About Ashish", supporting line: one-sentence positioning pulled from the existing about copy's first sentence.
- Individual project pages: **do not** get the dark band — see below.

### 2. Individual project pages — light, unchanged structural pattern, refreshed banner

Project pages stay fully light (they're the reading/reference surface: prose, charts, code). Rather than a dark header, the existing `.result-banner` (blue-tinted left-border callout) is upgraded to feel more like a natural extension of the hero language without going dark:

- Keep `.result-banner` as the "result in one line" element — it already does this job well.
- Add a slim breadcrumb/eyebrow above the page's H1-equivalent title area: "← All projects" link + category tags, styled like the light-theme eyebrow (small, uppercase-ish, muted), so navigating back doesn't require the browser back button.
- `.reveal` scroll-fade applies to each major section (`## The problem`, `## The finding`, etc.) for consistency with the homepage motion, applied lightly — sections should feel alive without feeling like a slideshow.

### 3. Project cards / rows — refine existing hover, no structural change

The homepage `.project-card` grid and the `/projects` `.project-row` stacked layout are both good and shouldn't be redesigned structurally (recent work). Bring their hover/motion timing in line with the token table above (already close — just confirm consistent `.15s` timing and shadow values across both).

### 4. Navbar

Currently light-only, unaffected by the dark hero (Quarto renders navbar above the page content). Options considered:

- **Recommended: leave as-is (light navbar, always).** A navbar that flips dark-on-hero/light-on-scroll adds real implementation complexity (scroll-position JS, flicker risk) for a cosmetic gain, and a consistently light navbar is a safe, stable anchor — the user always knows where nav is and how it behaves. This matches principle 2 (motion earns trust, doesn't perform).
- Alternative (not recommended for this pass): transparent-over-hero navbar that solidifies on scroll. Nicer on paper, meaningfully more fragile to implement well in a static Quarto site, and a common source of jank/flash-of-unstyled-navbar bugs. Revisit only if the recommended approach feels flat once live.

### 5. Footer

No change — existing `page-footer` config is fine and consistent with the light theme throughout.

## Page-by-page wireframes (text form)

### Homepage (`index.qmd`) — done, reference pattern

```
[DARK HERO — full 88vh]
  Eyebrow · Name (huge) · Tagline · [View projects] [Résumé] [Get in touch]
  → photo, right side
  ↓ Scroll cue
[LIGHT — reveal on scroll]
  Project cards (3-across grid, image + result + context)
[LIGHT — reveal on scroll]
  Bio row (photo + 3-line bio)
[LIGHT — reveal on scroll]
  Contact links
```

### Projects listing (`projects/index.qmd`) — to build

```
[DARK HEADER — compact ~32vh]
  Eyebrow: "Selected work"
  Title: "Projects"
  Supporting line: "Three projects, deep rather than wide..."
  ↓ soft seam
[LIGHT — reveal on scroll, staggered per row]
  Project row 1 (image left, title/description right)
  Project row 2
  Project row 3
```

### Individual project page (e.g. `projects/trade-promotion-forecasting/index.qmd`) — to build

```
[LIGHT — no dark band]
  ← All projects   ·   category tags        (new: light eyebrow/breadcrumb)
  Title (existing H1 via Quarto)
  [Result banner — existing, unchanged]
[reveal] ## The problem
[reveal] ## The finding (chart)
[reveal] ## Decisions and tradeoffs
[reveal] ## Measurement and limitations
[reveal] ## How it works (code)
[reveal] ## Code (repo link)
```

### About (`about.qmd`) — to build

```
[DARK HEADER — compact ~32vh]
  Eyebrow: "About"
  Title: "About Ashish"
  Supporting line: one-sentence positioning
  ↓ soft seam
[LIGHT]
  Existing `trestles` about template content (photo + bio + links)
  Note: trestles template has its own layout assumptions — verify the
  dark header seam doesn't visually collide with the template's own
  photo/sidebar before shipping.
```

## Known content fix (found during audit, unrelated to visual design but worth fixing in the same pass)

`about.qmd` already contains the real, specific numbers that the project pages' `[[TODO]]` placeholders were waiting on:
- Trade promotion / OOS: **300,000 models, 2TB dataset, automated backtesting + hyperparameter tuning, $2.4M in avoided stockout/expiry losses** — this confirms the homepage/project-page headline numbers are real and traceable, contrary to the earlier caution flag.
- Russia engagement: 50,000 models, 84% accuracy, **plus a Temporal Fusion Transformer for cross-learning adding +17% uplift** (not yet on the project page).
- RAG marketing assistant: **took 2nd place** in Tiger Analytics' internal hackathon (project page currently says "top 4 teams" — should be corrected to 2nd place, which is a stronger and more specific claim).

Recommend a follow-up content pass to pull these into the relevant project pages' TODO slots once the visual redesign ships — flagging here so it doesn't get lost.

## Decisions (confirmed)

- Individual project pages stay fully light, no dark band — light breadcrumb only. Confirmed.
- Navbar stays always-light, no scroll-triggered transparency. Confirmed.
- Content accuracy fixes (2nd place not top-4, TFT +17% uplift, $2.4M/300k confirmed via about.qmd) ship in the same pass as the visual work. Confirmed.

## Implementation order (proposed)

1. Extract the dark-header pattern into a reusable partial (`_dark-header.html` via Quarto include, or a documented copy-paste block) so Projects listing and About don't duplicate hand-written HTML three times.
2. Projects listing dark header + `.reveal` on each row.
3. About page dark header (verify `trestles` template compatibility first — flagged above as a risk).
4. Individual project pages: light breadcrumb/eyebrow + `.reveal` per section.
5. Cross-page QA: click through every nav path, confirm no dark-on-dark or light-on-light contrast failures, confirm animations don't double-fire on back/forward navigation.
