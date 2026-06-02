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
_config.yml   site-wide settings; also controls Jekyll exclude/include
assets/       main.scss — overrides minima gem to suppress Sass deprecation warnings
about.markdown  about page; has last_modified_at: in front matter
dodo/         git submodule pointing at github.com/kpthill/dodo
```

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

## Incorporating projects from other repos

Use a **git submodule** pointing at the project's GitHub repo. GitHub Pages supports submodules natively.

Because submodules pull in the entire repo, use Jekyll's `exclude` in `_config.yml` to block files you don't want served. **Use a blacklist (not a whitelist):** GitHub Pages runs Jekyll 3.9.x, where `include:` only overrides default exclusions (dotfiles, underscore dirs) and does NOT override an explicit `exclude:` entry. A whitelist works locally on Jekyll 4.x but silently breaks on GitHub Pages.

```yaml
exclude:
  - some-project/notes_to_self.md   # block specific private files
  - some-project/todo.md
  - some-project/impl               # block whole directories
```

**Tradeoff:** any new file added to the submodule repo will be served publicly by default. When adding files to a submodule, check whether they need to be added to the blacklist in `_config.yml`.

Check that any HTML files being served are self-contained or that all their JS/asset dependencies are also reachable.

To update a submodule to its latest commit:
```bash
cd some-project && git pull && cd .. && git add some-project && git commit
```

## Git history and authorship

Keeping the git history representative of who actually wrote what is important. Follow these rules strictly:

- **Never commit work Patrick wrote.** If Patrick writes or edits something himself, leave it for him to commit.
- **Always commit your own work**, and mark it clearly as authored by Claude using the `Co-Authored-By` trailer:

```
git commit -m "Your message

Co-Authored-By: Claude <noreply@anthropic.com>"
```

- When in doubt about whether something was written by you or Patrick, ask before committing.
