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

## Key Files and Directories

| Path | Purpose |
|------|---------|
| `_config.yml` | Main site config: title, author profile, social links, navigation |
| `_pages/` | Static pages (about, CV, publications list, teaching, talks) |
| `_posts/` | Blog posts — filename: `YYYY-MM-DD-slug.md` |
| `_publications/` | One `.md` per paper — linked from publications page |
| `_talks/` | One `.md` per talk |
| `_teaching/` | One `.md` per teaching entry |
| `_portfolio/` | Portfolio items |
| `_data/` | YAML data files (navigation, author bio, UI text) |
| `_includes/` | Reusable HTML partials |
| `_layouts/` | Page layout templates |
| `_sass/` | Sass stylesheets |
| `assets/` | Static assets (CSS, JS, images) |
| `images/` | Site images including `profile.png` |
| `files/` | Downloadable files (CV PDF, etc.) |

## Content Conventions

### Publications (`_publications/`)
Filename: `YYYY-MM-DD-short-slug.md`. Front matter fields: `title`, `collection: publications`, `permalink`, `excerpt`, `date`, `venue`, `paperurl`, `citation`.

### Blog posts (`_posts/`)
Filename: `YYYY-MM-DD-slug.md`. Front matter: `title`, `date`, `permalink`, `tags`, `category` (optional).

### Pages (`_pages/`)
Front matter: `permalink`, `title`, `layout` (usually `archive` or `single`), `author_profile: true`.

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
