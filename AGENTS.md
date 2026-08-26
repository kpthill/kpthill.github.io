# Agent Notes — kpthill.github.io

This is Patrick's personal Jekyll blog, hosted at thill.me via GitHub Pages (pushing to `master` deploys automatically).

## Running locally

```bash
bundle exec jekyll serve --drafts   # includes _drafts/; omit flag to preview live site only
```

Requires rbenv with Ruby 3.3+ and `bundle install`. Config changes require a server restart.

## Repo structure

```
_posts/       published posts (YYYY-MM-DD-title.md)
_drafts/      unpublished posts (no date in filename)
_layouts/     HTML templates: default, home, page, post
_includes/    partials (header, footer, social links, etc.)
_data/projects.yml  drives the /projects/ card grid
projects.html       the projects page (renders _data/projects.yml)
_config.yml   site-wide settings; also controls Jekyll exclude/include
assets/       main.scss (minima overrides, project-card grid, header dropdown)
              images/projects/ — screenshots for the projects page
about.markdown  about page; has last_modified_at: in front matter
build-ten-thousand-things/  RC end-of-batch talk + poem.html, static export
                            served verbatim (files have no front matter)
```

## Header nav

`_includes/header.html` overrides minima's header with an explicit dropdown
(About, Projects, and RC webring links). It does NOT auto-list pages, on
purpose: GitHub Pages' default plugins (`jekyll-optional-front-matter`,
`jekyll-titles-from-headings`) turn stray markdown files into pages, which
minima's auto-nav would then display. The webring links render only when
`recurse_webring_id` is set in `_config.yml`.

## Adding a project to /projects/

Add an entry to `_data/projects.yml` and a screenshot (~900px wide) under
`assets/images/projects/`. Cards crop images to a 3:2 top-aligned box.

## Writing a post

Create `_posts/YYYY-MM-DD-your-title.md` with front matter:

```yaml
---
layout: post
title: "Your Title"
tags: whatever
---
```

MathJax is loaded in `_layouts/post.html`, so LaTeX works with `$$ ... $$`.

Drafts go in `_drafts/` with no date in the filename; they only appear when serving with `--drafts`.

## Hosting projects (the standard pattern)

Every JS/web project is hosted from **its own repo** with a GitHub Actions →
GitHub Pages workflow, which serves it at `thill.me/<repo-name>/` (project
sites appear as subpaths of this user site, and they take precedence over
same-named paths here). Current examples: `dodo`, `live-dangerously`,
`voronoi-game`, `transit-fantasy`, `rhythm-game`, `just-one-bot`, `llm-game`.

To host a new project:

1. In the project repo: make sure the build works from a subdirectory
   (for Vite, set `base: './'`; avoid root-absolute asset paths like
   `/audio/x.mp3` in code). Add a `pages.yml` workflow — copy one from
   `just-one-bot` (plain Vite app), `rhythm-game` (wrapper page + build
   under `game/`), or `dodo` (no build; stages a whitelist of files,
   manual-trigger only).
2. Enable Pages once: `gh api repos/kpthill/<repo>/pages -X POST -f build_type=workflow`
   (the workflow's GITHUB_TOKEN cannot do this itself).
3. Add the project to `_data/projects.yml` here with a screenshot.

Two exceptions live inside this repo instead: the dodo *blog post* (links to
the project site) and `build-ten-thousand-things/` (a finished static talk
export, not an evolving app — served verbatim from this repo).

## Deploying dodo updates

The dodo pages (`/dodo/repl.html`, `/dodo/spec.html`) are served by the dodo
repo's own Pages site, NOT this repo. Its workflow publishes a **whitelist**
(repl.html, spec.html, repl-shims.js, impl/*.js, dodo-spec.md), so new files
in the dodo repo stay private unless added to the staging step in
`dodo/.github/workflows/pages.yml`.

Deploys stay **manual by design**: push to dodo `main`, then trigger the
workflow (`gh workflow run pages.yml -R kpthill/dodo`, or the Actions tab).
Nothing in this blog repo needs to change for a dodo update.

The rcade games (`rhythm-game`) get a browser-playable demo via
`web/index.html` in their repo — a game-agnostic port of the cabinet's input
plugins (same key map as `rcade dev`). Reuse it for future rcade games.

## Git history and authorship

Keeping the git history representative of who actually wrote what is important. Follow these rules strictly:

- **Never commit work Patrick wrote.** If Patrick writes or edits something himself, leave it for him to commit.
- **Always commit your own work**, and mark it clearly as authored by Claude using the `Co-Authored-By` trailer:

```
git commit -m "Your message

Co-Authored-By: Claude <noreply@anthropic.com>"
```

- When in doubt about whether something was written by you or Patrick, ask before committing.
