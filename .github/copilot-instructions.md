<!-- .github/copilot-instructions.md - guidance for AI coding agents working in this repo -->

# Quick Context

This repository is a Hugo-based static site (Russian language) using the `PaperMod` theme.
Key work happens in the inner site folder `imperial-herald/imperial-herald` (this is where `hugo.toml` lives).

# Big picture

- Site generator: Hugo; configuration in `imperial-herald/imperial-herald/hugo.toml`.
- Theme: `themes/PaperMod` (overrides exist in the local `layouts/` directory).
- Content: Markdown under `content/` (section folders like `00-Posts`, `military-reports`, `ImpG`, `Archives`, `worlds`).
- Archetypes: default front matter templates live under `archetypes/` (TOML front matter is used).
- Templates & shortcodes: local `layouts/_default/` and `layouts/shortcodes/` override theme behavior and provide project-specific UI (examples: `better-header`, `music`, `crossword`).
- Static assets: `static/` is served as site root; `public/` is the generated site output (don't edit `public/` manually if you mean to change content).

# Developer workflows (explicit commands)

- Run local dev server (from the inner site folder):

  cd to `imperial-herald/imperial-herald` then:

  ```bash
  hugo server -D
  # open http://localhost:1313
  ```

  `-D` shows drafts (archetype default sets `draft = true`).

- Build static site (production):

  ```bash
  cd imperial-herald/imperial-herald
  hugo --gc --minify
  # output written to ./public/
  ```

- Debugging templates: run `hugo server --verbose --disableFastRender` (inside inner folder) to get more template logs.

# Project-specific conventions & gotchas

- Front matter: archetypes are TOML-style (see `archetypes/default.md` and `archetypes/mir.md`). New posts created with `hugo new` will contain `draft = true` by default.
- Section index files are named `_index.md` (e.g., `content/map/_index.md`, `content/ImpG/_index.md`).
- Directory-case matters on Linux — the repo contains both `archives/` and `Archives/` as distinct sections; be careful when linking or moving files.
- Raw HTML is allowed (Goldmark `unsafe = true` in `hugo.toml`), but prefer shortcodes for reusable interactive elements.

# Shortcodes — concrete examples

- `music` shortcode (see `layouts/shortcodes/music.html`):

  ```md
  {{< music name="Symphony" authors="A. Composer" link="/music/symphony.mp3" text="Lyrics..." >}}
  ```

  It renders an audio player and optional lyrics (the shortcode reads parameters via `.Get`).

- `better-header` shortcode (see `layouts/shortcodes/better-header.html`):

  ```md
  {{< better-header title="Раздел" >}}
  ```

- `crossword` shortcode expects JSON inside the shortcode body (see `layouts/shortcodes/crossword.html`):

  ```md
  {{< crossword >}}
  { "across": [...], "down": [...] }
  {{< /crossword >}}
  ```

# Key files to review when making changes

- `imperial-herald/imperial-herald/hugo.toml` — site config, languageCode is `ru-ru`.
- `archetypes/` — new-content templates (TOML front matter, `draft = true`).
- `layouts/_default/` — main page templates (index, map, council) override the theme.
- `layouts/shortcodes/` — project shortcodes (music, better-header, crossword) — prefer shortcodes over inline HTML.
- `themes/PaperMod` — upstream theme; overrides live in `layouts/`.

# Integration points and CI notes

- The theme contains `.github` workflow files (PaperMod). Verify whether CI/Pages uses the theme workflow or repo-level workflows.
- `static/` provides site-root assets (JS, images, music) — reference these with absolute URLs like `/js/...` or `/music/...`.

# What I couldn't discover automatically / questions for you

- Is `public/` intended to be committed to this repo (GitHub Pages), or is deployment handled from CI? I did not find a repo-level workflow; PaperMod includes example workflow files in `themes/PaperMod/.github/`.
- Any special deploy steps (rsync/Netlify/GitHub Pages) or secret/env requirements? Please supply CI/deploy docs if they exist.

# If you want me to iterate

- Tell me how you deploy (commit `public/` vs. CI build), and I will add exact CI/deploy instructions.
- If there are more project-specific shortcodes or templating patterns to surface, point me at files and I'll add concise examples.

-- End of guidance — please review and tell me what to expand or correct.
