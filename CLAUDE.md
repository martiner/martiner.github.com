# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal homepage served by GitHub Pages at `martiner.geek.cz` (see `CNAME`). GitHub Pages runs Jekyll on every push to `master`; that push is the deploy.

There are no tests, linters or CI config. The `Gemfile` pulls in the `github-pages` gem, which pins the same Jekyll and plugin versions as production, so the site can be rendered locally:

```
bundle install               # once; gems go to vendor/bundle
bundle exec jekyll build     # output in _site/
bundle exec jekyll serve     # http://localhost:4000
```

A custom `exclude` in `_config.yaml` replaces Jekyll 3's default list instead of extending it, so anything in the repo root that must not be published (including `Gemfile` and `vendor`) has to be listed there explicitly.

## How the pages are put together

- `_config.yaml` assigns `layout: default` to every page that has front matter, so pages never name a layout themselves. It also enables `jekyll-redirect-from`.
- `_layouts/default.html` is the Bootstrap 3 "cover" template. A page supplies `title` (the `<title>`) and `brand` (the masthead heading, HTML allowed) in its front matter, and its body lands in `{{ content }}`.
- The layout links its CSS/JS by relative path (`bootstrap.min.css`), so a layout page only works from the repo root. A page in a subdirectory would lose its styles; `wishlist.md` (published as `/wishlist.html`) gets its `/wishlist/` URL through `redirect_from` instead of living in a directory.
- The nav is generated from every page that sets `nav` (the link label) in its front matter, sorted by URL, so Home (`/`) comes first; the current page gets `active`.
- `unicode/index.html` has no front matter, so Jekyll copies it verbatim: it is a standalone legacy article with its own inline styling and images, not part of the layout.
- `bootstrap.min.*`, `cover.css` and `ie10-viewport-bug-workaround.js` are vendored; site-specific styles go in `moje.css`.

## Conventions

- All files are UTF-8, and page content is Czech. `unicode/index.html` was once windows-1250 and rendered broken on GitHub Pages; keep new and converted files UTF-8.
- `wishlist.md` is the file that changes most, and the only page written in Markdown. Items that are no longer wanted are moved into an HTML comment below the list rather than deleted, so they can be brought back; a comment between list items would split the list in two. The `{: .text-left}` line has to stay directly above the first item, with no blank line between. A commented `xxx` block at the bottom is the template for a new category.
- Commit subjects use a `add:` / `chg:` / `fix:` / `del:` prefix.
