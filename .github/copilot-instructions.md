# Pelican Static Site - AI Agent Instructions

## Project Overview
This is a bilingual (English/Russian) static website for "iRLS" (Russian Language Services) built with [Pelican](https://getpelican.com/). Content is deployed automatically to GitHub Pages on pushes to `main`.

## Architecture & Key Files

- **Content Source**: All Markdown content lives in `content/` with bilingual pages using `-ru.md` suffix
- **Config Split**: `pelicanconf.py` (dev) vs `publishconf.py` (prod with SITEURL=https://irlsonline.com)
- **Build Output**: Generated static files go to `output/` directory (git-ignored)
- **Dual Tooling**: Both `make` and `invoke` (Python) task runners are maintained in parallel

## Critical Developer Workflows

### Local Development
```bash
# Option 1: Using Make (recommended for quick dev)
make devserver          # Build + serve + auto-regenerate at localhost:8000

# Option 2: Using Python invoke
invoke livereload       # Alternative with Python-based live reload

# Clean build
make clean && make html
```

### Content Authoring
- **Bilingual Pattern**: Create pairs like `about.md` / `about-ru.md` with matching `Slug:` in frontmatter
- **Frontmatter Required**: Each page needs `Title:`, `Slug:`, and `lang:` (en/ru)
- **Case Sensitivity**: Note inconsistent naming (e.g., `Experience.md` vs `experience-ru.md`)

### Deployment
- **Automatic**: Push to `main` triggers `.github/workflows/publish.yml` → `github_pages.yml`
- **Manual**: `make github` or `invoke gh-pages` to deploy immediately
- **SITEURL Injection**: CI dynamically sets SITEURL via `--extra-settings` flag in workflow

## Project-Specific Conventions

1. **No Plugins**: i18n_subsites config is commented out; bilingual support via file suffixes only
2. **Python Version**: Target is 3.11 (see `pyproject.toml` and CI workflow)
3. **Formatting**: Black with 88 char line length for Python code
4. **Dependencies**: Managed via `pyproject.toml` (pelican[markdown], typogrify, ghp-import, invoke, livereload)

## Integration Points

- **GitHub Pages**: Deployed to `gh-pages` branch using `ghp-import` tool
- **CI/CD**: Reusable workflow pattern (`.github/workflows/github_pages.yml` called by `publish.yml`)
- **Dev Dependencies**: Optional `[dev]` group includes black, flake8, mypy

## Common Pitfalls

- Don't edit files in `output/` - they're regenerated on every build
- `publishconf.py` imports from `pelicanconf.py` - changes in base config affect both
- CI uses `pelicanconf.py` (NOT publishconf) but overrides SITEURL dynamically
- Russian content pages must have `lang: ru` frontmatter to be processed correctly

## Quick Reference

- **Add New Page**: Create `content/pages/newpage.md` with proper frontmatter
- **Test Locally**: `make devserver` opens browser automatically at http://localhost:8000
- **Check Output**: Build generates to `output/` with same structure as `content/`
