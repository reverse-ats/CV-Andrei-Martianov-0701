# AGENT.md — tools/CV (applicant CV template)

Guidance for AI agents (Claude, Gemini, OpenCode, Codex, …) working in this directory. `CLAUDE.md`, `GEMINI.md`, and `AGENTS.md` here are committed symlinks to `AGENT.md` — don't edit them directly.

## Role in Reverse ATS

This is the **canonical per-applicant CV template**. `applicants/stages/setup-cv.sh` copies the whole directory into each client's CV dir (`cp -r tools/CV …`), overwrites `_data/data.yml` with the applicant's parsed CV YAML, then publishes to a GitHub Pages repo. The published URLs are written back into `applicants/envs/<client>.env`:

- `GITHUB_CV` — the rendered site (`https://<owner>.github.io/<repo>/`)
- `GITHUB_CV_RAW` — the raw data file (`https://raw.githubusercontent.com/<owner>/<repo>/refs/heads/main/_data/data.yml`); `rats-worker-dotnet`'s `CvService` fetches the applicant CV from this URL.

Edit this template only to change the **shape/styling** shared by every applicant — per-applicant content never lives here, it's injected as `_data/data.yml` at deploy time.

## Project Overview

This is a Jekyll-based CV/resume website deployed to GitHub Pages. It's a responsive, single-page application that displays personal information, skills, experience, and contact details. The site uses Jekyll 3.5.2 with a custom theme. In the Reverse ATS flow, `setup-cv.sh` publishes it to GitHub Pages from the **`main`** branch root (not `gh-pages`).

## Architecture

- **Jekyll Static Site Generator**: Uses Jekyll 3.5.2 with Liquid templating
- **Data-driven content**: All personal information is stored in `_data/data.yml`
- **Modular includes**: Site sections are split into reusable includes in `_includes/`
- **SCSS styling**: Styles are organized in `_sass/` with skin-based theming
- **GitHub Pages deployment**: `setup-cv.sh` force-pushes the populated site to `main` and enables Pages on `main` (`path: /`)

## Key Files and Structure

- `_data/data.yml`: Contains all CV content (contact, skills, experience, education)
- `_config.yml`: Jekyll configuration and site metadata
- `_includes/`: Modular HTML components for each CV section
- `_layouts/default.html`: Base page template
- `_sass/`: SCSS styles organized by component and skin
- `assets/css/main.scss`: Main stylesheet that imports all SCSS files
- `index.html`: Single page layout that includes all CV sections

## Development Commands

Since this is a Jekyll site, common development tasks include:

```bash
# Install dependencies
bundle install

# Serve locally for development
bundle exec jekyll serve

# Build the site
bundle exec jekyll build
```

## Content Management

To update CV content, edit `_data/data.yml` which contains structured data for:
- Personal contact information
- Technical skills (hardskills)
- Soft skills (softskills) 
- Work experience with detailed descriptions
- Education history
- Language proficiencies
- Bio/summary

## Theming

The site supports multiple color skins defined in `_sass/skins/`. Currently uses the "blue" skin as specified in `_config.yml`. Styles are modularized by component in `_sass/includes/`.

## Deployment

The site is configured for GitHub Pages deployment. In the Reverse ATS flow `applicants/stages/setup-cv.sh` owns publishing: it populates `_data/data.yml`, force-pushes to the per-applicant repo's `main` branch, and enables Pages on `main`. Any change pushed to `main` triggers a rebuild and deployment.