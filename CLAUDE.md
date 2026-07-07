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
- **Local preview:** `bundle exec jekyll serve` (requires Ruby + Bundler + gems from `Gemfile`)
- **Markdown engine:** kramdown (Jekyll default), GFM-like but with quirks — see `_pages/markdown.md` (rendered live at `/markdown/`), which is this theme's own syntax reference (mirrors https://academicpages.github.io/markdown/). Check it for tables, footnotes, MathJax, notices, and HTML-tag support before assuming GitHub-flavored syntax works as-is.

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
2. Preview with `bundle exec jekyll serve` at `http://localhost:4000`.
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
