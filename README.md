# Portfolio site

Personal data-science portfolio. **Quarto** website, Python code chunks managed with **uv**, deployed to **GitHub Pages** on a custom domain.

## Layout

```
_quarto.yml              site config, nav, theme
styles.scss              light theme
index.qmd                landing page (HR-facing, one scroll)
about.qmd
resume.pdf               ← replace with your real one-pager
projects/
  index.qmd              grid listing
  _template.qmd          copy this to start a new project
  churn-uplift/index.qmd fully-worked example (the pattern to follow)
  demand-forecast/…      placeholder — fill in
  pricing-experiment/…   placeholder — fill in
blog/                    scaffolded but NOT in nav yet (see below)
  index.qmd
  <slug>/index.qmd       3 draft posts, each cross-linked to a project
assets/                  thumbnails + profile image
CNAME                    custom domain
.github/workflows/       auto-publish on push to main
```

## One-time setup

Two commands need to run on a machine with network access (this repo was
scaffolded in a sandbox that couldn't reach pypi/github):

```bash
# 1. Generate and commit the lockfile
uv lock
git add uv.lock && git commit -m "Add uv lockfile"

# 2. Install Quarto locally: https://quarto.org/docs/get-started/
```

## Local preview

```bash
uv sync                  # create .venv from the lockfile
quarto preview           # live-reloading local server
```

Quarto runs the `.qmd` Python chunks with the uv-managed environment. If it
doesn't pick it up automatically, point it there:

```bash
QUARTO_PYTHON=.venv/bin/python quarto preview
```

## Deploy

**Automatic (recommended).** Push to `main`; the workflow in
`.github/workflows/publish.yml` renders and publishes to the `gh-pages` branch.
In GitHub → Settings → Pages, set the source to the `gh-pages` branch.

**Manual, one-off:**

```bash
uv sync
quarto publish gh-pages
```

## Custom domain (~$12/yr)

1. `CNAME` already contains `yourname.com` — change it to your domain.
2. At your registrar, point DNS at GitHub Pages:
   - Apex (`yourname.com`): four `A` records → `185.199.108.153`,
     `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
     (and the matching `AAAA` records if you want IPv6).
   - `www`: a `CNAME` record → `yourhandle.github.io`.
3. GitHub → Settings → Pages → set the custom domain, tick **Enforce HTTPS**.

## Before you publish — content checklist

- [ ] `_quarto.yml`: title, `site-url`, GitHub/LinkedIn/email links
- [ ] `index.qmd`: name, one-line positioning, three card results, bio
- [ ] Replace `resume.pdf`
- [ ] Fill `demand-forecast` and `pricing-experiment` (use `_template.qmd`)
- [ ] Swap a real photo into `assets/profile.png`
- [ ] `CNAME`: your domain

## Shipping the blog later

The blog is built but intentionally **not linked in the nav** — don't ship
`/blog` until 3 posts are ready. When they are:

1. Remove `draft: true` from each post's front matter.
2. Uncomment the **Writing** nav item in `_quarto.yml`.
3. Optionally uncomment the "Latest writing" block at the bottom of `index.qmd`.
