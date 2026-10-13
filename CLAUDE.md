# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal homepage served by GitHub Pages at `martiner.geek.cz` (see `CNAME`). GitHub Pages runs Jekyll on every push to `master`; that push is the deploy.

There is no build, lint or test tooling in the repo: no `Gemfile`, no `package.json`, no CI config. Jekyll is not set up locally, so a change can only be checked by reading the HTML or by looking at the published site.

## How the pages are put together

- `_config.yaml` assigns `layout: default` to every page that has front matter, so pages never name a layout themselves. It also enables `jekyll-redirect-from`.
- `_layouts/default.html` is the Bootstrap 3 "cover" template. A page supplies two front matter values: `title` (the `<title>`) and `brand` (the masthead heading, HTML allowed), and its body lands in `{{ content }}`.
- The layout links its CSS/JS and the nav by relative path (`bootstrap.min.css`, `wishlist.html`), so a layout page only works from the repo root. A page in a subdirectory would lose its styles; `wishlist.html` gets its `/wishlist/` URL through `redirect_from` instead of living in a directory.
- The nav is hard-coded in the layout, including which item is `active`. A new page needs its link added there by hand.
- `unicode/index.html` has no front matter, so Jekyll copies it verbatim: it is a standalone legacy article with its own inline styling and images, not part of the layout.
- `bootstrap.min.*`, `cover.css` and `ie10-viewport-bug-workaround.js` are vendored; site-specific styles go in `moje.css`.

## Conventions

- All files are UTF-8, and page content is Czech. `unicode/index.html` was once windows-1250 and rendered broken on GitHub Pages; keep new and converted files UTF-8.
- `wishlist.html` is the file that changes most. Items that are no longer wanted are commented out rather than deleted, so they can be brought back, and a commented `xxx` block at the bottom is the template for a new category.
- Commit subjects use a `add:` / `chg:` / `fix:` / `del:` prefix.
