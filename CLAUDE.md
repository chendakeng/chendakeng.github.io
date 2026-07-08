# CLAUDE.md — chendakeng.github.io

## Site Overview

Personal academic website for CHEN Dakeng. Built on [academicpages](https://github.com/academicpages/academicpages.github.io), a Jekyll theme designed for academic CVs and portfolios. Hosted on GitHub Pages from the `master` branch.

- **Live URL:** https://chendakeng.github.io
- **Remote:** https://github.com/chendakeng/chendakeng.github.io
- **Branch:** `master` (GitHub Pages deploys from here)

## Tech Stack

- **Static site generator:** Jekyll (Ruby)
- **Theme:** academicpages (fork of Minimal Mistakes)
- **Deployment:** GitHub Pages — pushing to `master` triggers automatic build and deploy. No CI step needed.
- **Local preview:** requires Homebrew Ruby 3.3 (system Ruby is too old; Ruby 4.x is too new for the `github-pages` gem). Run:

  ```bash
  export PATH="/opt/homebrew/opt/ruby@3.3/bin:/opt/homebrew/lib/ruby/gems/3.3.0/bin:$PATH"
  bundle exec jekyll serve --port 4000
  ```

  **Gotcha:** always preview via `jekyll serve`, not `jekyll build` + opening `_site/` pages. A bare `jekyll build` resolves asset URLs to the production domain (`https://chendakeng.github.io/...`), so locally opened pages silently load the *deployed* CSS instead of your local changes. `jekyll serve` rewrites URLs to `localhost:4000`.
- **Markdown engine:** kramdown (Jekyll default), GFM-like but with quirks — see `_pages/markdown.md` (rendered live at `/markdown/`), which is this theme's own syntax reference (mirrors https://academicpages.github.io/markdown/). Check it for tables, footnotes, MathJax, notices, and HTML-tag support before assuming GitHub-flavored syntax works as-is.

## Design System ("Ink & Cinnabar")

The site was restyled in July 2026 away from the stock academicpages look. The design language: warm paper background, near-black ink text, a single cinnabar-red accent (seal-stamp red), Fraunces display serif for headings and the brand, Source Serif 4 for body text. Keep new elements consistent with this.

| Piece | Where it lives |
|-------|----------------|
| Web fonts (Fraunces + Source Serif 4, Google Fonts) | `_includes/head.html` |
| Font family variables (`$display`, `$serif`) | `_sass/_themes.scss` |
| Light palette (CSS variables incl. `--global-accent-soft`) | `_sass/theme/_default.scss` |
| Dark palette | `_sass/theme/_dark.scss` |
| All component overrides (masthead, sidebar, publication list, pill links, footer) | `_sass/layout/_custom.scss` — imported **last** in `assets/css/main.scss` |
| Minimal footer (`© YEAR CHEN Dakeng`) | `_includes/footer.html` (year auto-updates from `site.time` at build) |

### What `_sass/layout/_custom.scss` does (section by section)

This is the single file that turns stock academicpages into the custom design. It is imported **last** in `assets/css/main.scss`, so its rules win over every stock partial without editing them. Sections, in file order:

1. **Global type** — antialiasing, body `line-height: 1.7`, all headings switched to `$header-font-family` (Fraunces) at weight 600 with slight negative letter-spacing, underlines offset 3px/1px thick, text selection tinted with `--global-accent-soft`.
2. **Masthead and navigation** — roomier padding; the brand link set in Fraunces 1.3em bold; all other nav items rendered as small (0.8em) uppercase serif with 0.14em letter-spacing.
3. **Page titles** — `#main .page__title` and the homepage's first `h1` get a `::after` pseudo-element: a 2.4rem × 3px cinnabar rule (colored by `--global-link-color`). This is the site's signature mark.
4. **Author sidebar** — avatar becomes a soft square (`border-radius: 26%`, stock circle/border removed); name in Fraunces `$type-size-4`; bio line italic, small, muted; social/academic links de-emphasized to `--global-text-color-light`, turning cinnabar on hover, no underline.
5. **Archive lists** (publications, teaching) — kills the theme's underline-everything behavior (underline on hover only); category subtitles as small uppercase letter-spaced labels; each `.list__item` separated by a hairline bottom border; item titles in Fraunces 1.15em, ink-colored, cinnabar on hover; venue/citation `<p>` lines shrunk to 0.85em muted; **action links** (`Download Paper`, `View at Publisher`, `Download Slides`) styled as bordered pill buttons via `.archive__item p > a`, border and text turning cinnabar on hover.
6. **Footer** — transparent background with a hairline top border; single centered line in small uppercase letter-spaced type.

Rules of thumb:
- Never edit vendor or stock theme partials for styling; add overrides to `_sass/layout/_custom.scss` instead, in the matching section.
- Colors always via the `--global-*` CSS variables so light/dark stay in sync; when adding a color, define it in **both** theme files (`_sass/theme/_default.scss` and `_sass/theme/_dark.scss`).
- The homepage (`_pages/about.md`) intentionally has no `title:` in front matter — this suppresses the redundant "About" `page__title` heading; the visible h1 is the markdown name header. Don't re-add a title (browser tab falls back to `site.title` automatically). This only works because `titles_from_headings: enabled: false` is set in `_config.yml`: GitHub Pages force-enables the `jekyll-titles-from-headings` plugin (local builds don't), which would otherwise infer a title from the first h1 and render the name twice **only in production**. Don't remove that setting.
- Pill-button styling on publication action links comes free from `archive-single.html`'s existing markup; don't add manual `|` separators between links there (they were deliberately removed).
- Fonts load from Google Fonts CDN (`_includes/head.html`). If mainland-China accessibility becomes a concern, self-host the WOFF2 files under `assets/fonts/` and swap the `<link>` tags for `@font-face` rules in `_custom.scss`.

## Key Files and Directories

| Path | Purpose |
|------|---------|
| `_config.yml` | Main site config: title, author sidebar (bio, employer, social/academic links), navigation include, publication categories |
| `_data/navigation.yml` | Top nav bar links — order and visibility |
| `_pages/` | Static single pages (about, cv, cv-json, publications, teaching, talks, markdown guide) |
| `_posts/` | Blog posts — filename: `YYYY-MM-DD-slug.md` |
| `_publications/` | One `.md` per paper — rendered on `/publications/` and (if a page pulls the collection) `/cv/` |
| `_talks/` | One `.md` per talk |
| `_teaching/` | One `.md` per teaching entry |
| `_portfolio/` | Portfolio items |
| `_includes/` | Reusable HTML partials (e.g. `archive-single.html`, `archive-single-cv.html`) |
| `_layouts/` | Page layout templates |
| `_sass/` | Sass stylesheets |
| `assets/cv.pdf` | **The** downloadable CV PDF — linked directly from `_pages/about.md`. This is the file to replace when the CV is updated. |
| `files/` | Per-paper assets: PDFs, slide decks, `.bib` files — referenced by individual `_publications/*.md` entries (`paperurl`, `slidesurl`, `bibtexurl`), NOT the CV. |
| `images/` | Site images including `images/profile.png` |

**Do not confuse `assets/cv.pdf` with `files/`.** The CV download lives in `assets/`; `files/` is only for individual publication PDFs/slides/bibtex.

## Current Navigation State (important — read before adding pages)

`_data/navigation.yml` currently only shows **Research** (`/publications/`) and **Teaching** (`/teaching/`) in the top nav. Talks, Portfolio, Blog, and both CV pages (`/cv/` markdown version and `/cv-json/` JSON version) are commented out and **not linked from anywhere on the live site**. The About page's "Curriculum Vitae" link points straight to `assets/cv.pdf`, bypassing `/cv/` entirely.

- `_pages/cv.md` still contains unedited academicpages placeholder text (fake "GitHub University" entries, joke starter content) as of the last check. Treat it as unmaintained scaffolding, not a live page, unless a session is explicitly asked to bring it in sync with the real CV.
- If asked to "add the CV to the nav" or "fix the CV page," that means editing `_pages/cv.md` content AND uncommenting the relevant line in `_data/navigation.yml`.

## How to Update Common Content

### 1. Update the About page bio (`_pages/about.md`)
Single markdown file, no collection involved. Just edit the prose directly. Keep name/title header (`# CHEN Dakeng, Devin | 陈达铿`) and the CV download link (`[Curriculum Vitae](../assets/cv.pdf)`) intact.

### 2. Replace the downloadable CV
Overwrite `assets/cv.pdf` with the new file (e.g. `cp /path/to/new_cv.pdf assets/cv.pdf`). No front matter or code changes needed — the About page link is static and always points to this path.

### 3. Add a new publication
Create `_publications/YYYY-MM-DD-short-slug.md` (filename date is just a sort-friendly identifier; the *displayed* date comes from the `date:` front-matter field, they don't need to match). Front matter fields, following the existing entries — or the format reference in `_publications/sample_publications/` (see §9) — as the template:

```yaml
---
title: 'Author, A., & Author, B. (YEAR). Full Paper Title.'
collection: publications
category: published        # or: under_review  (must match a key in _config.yml's publication_category)
date: YYYY-MM-DD
venue: 'Journal Name'                      # for category: under_review, use the venue field to say how far along it is
paperurl: 'https://chendakeng.github.io/files/paper_slug.pdf'   # omit/comment out if no PDF yet
bibtexurl: 'https://doi.org/...'                                 # or a files/*.bib path
citation: 'Author, A., &amp; Author, B. (YEAR). &quot;Full Paper Title.&quot; <i>Journal Name</i>, vol(issue), pages. https://doi.org/...'
---

Abstract / summary paragraph goes here as the markdown body.
```

`category` must be one of the keys under `publication_category` in `_config.yml`. There are two keys: `published` (heading "Published") and `under_review` (heading **"Working Papers"** — this single section covers both papers genuinely under journal review and papers still in progress; use the `venue` field to distinguish them, e.g. `'Journal Name (under review)'` vs `'Working Paper'`). To add a new top-level category, add a key under `publication_category` in `_config.yml` first.

### 4. Add a new teaching entry
Create `_teaching/short-slug.md`:

```yaml
---
title: "YYYY-YYYY: COURSECODE Course Title"
collection: teaching
type: "Undergraduate Course"   # or "Postgraduate Course", "Teaching Assistant", etc.
venue: "Institution, Department"
date: YYYY-MM-DD
---

Course description paragraph.
```

### 5. Add a new talk
Create `_talks/YYYY-MM-DD-slug.md` (see `_talks/sample_talks/` for the template, §9): needs `title`, `collection: talks`, `type`, `permalink: /talks/YYYY-MM-DD-slug`, `venue`, `date`, `location`. Talks are currently **not** linked in nav — uncomment the "Talks" line in `_data/navigation.yml` to expose `/talks/`.

### 6. Edit the top navigation
Edit `_data/navigation.yml`. Order in the file = order on the site. Uncomment a `- title / url` pair to add it back; comment out (`#`) to hide without deleting.

### 7. Edit sidebar author info (name, employer, bio blurb, social/academic links)
Edit the `author:` block in `_config.yml`. Fields like `googlescholar`, `orcid`, `researchgate`, `email`, `employer`, `location` populate the left sidebar automatically via `_includes/author-profile.html`. Leave a field blank/commented to hide that icon.

### 8. Keep cross-references in sync
When a paper's status changes (e.g. moves from working paper → under review → published), update **all** of these together, since they're not derived from one source of truth:
- The relevant `_publications/*.md` file (`category`, `venue`, `date`, and `paperurl`/`bibtexurl` once available)
- `_pages/about.md`'s narrative paragraph, if it name-checks the paper's venue or status
- The CV PDF in `assets/cv.pdf`, if regenerating from an external CV document

### 9. Sample/template files (`sample_publications/`, `sample_talks/`, `sample_portfolio/`)
Each of `_publications/`, `_talks/`, and `_portfolio/` has a `sample_xxx/` subfolder holding the original academicpages demo entries ("Paper Title Number 1," "Talk 1 on Relevant Topic," "Portfolio item number 1," etc.). These are kept purely as **field-format references** — copy their front matter structure when creating a new real entry.

They are inert by design: every file in a `sample_xxx/` folder has `published: false` in its front matter, which tells Jekyll to exclude it from the collection entirely — it won't build a page, won't appear in `site.publications`/`site.talks`/`site.portfolio` loops, and won't show up on `/publications/`, `/talks/`, `/portfolio/`, or the CV pages. When adding a real entry, create it as a sibling at the collection root (e.g. `_publications/2027-01-01-new-paper.md`), not inside `sample_xxx/`.

## Workflow: Updating the Site

1. Edit content files locally.
   - **Content-only change** (new publication, bio edit, CV replacement): follow §"How to Update Common Content"; no styling work needed.
   - **Styling change**: touch only `_sass/layout/_custom.scss` and, for colors, both `_sass/theme/*.scss` files. See §"Design System".
2. Preview at `http://localhost:4000` (Homebrew Ruby 3.3 required — see Tech Stack for the exact commands and the `jekyll serve` vs `jekyll build` URL gotcha):

   ```bash
   export PATH="/opt/homebrew/opt/ruby@3.3/bin:/opt/homebrew/lib/ruby/gems/3.3.0/bin:$PATH"
   bundle exec jekyll serve --port 4000
   ```

   Check both color modes (sun/moon toggle in the masthead) — every color change must look right in light *and* dark.
3. Commit changes.
4. Push to `master` — GitHub Pages deploys automatically (takes ~1 min).

```bash
git add <files>
git commit -m "short description"
git push origin master
```

## Git Notes

- Default branch is `master` (not `main`).
- Never commit large binary files or data files.
- Repo is **private** — keep it that way.
- No co-author lines in commits.
