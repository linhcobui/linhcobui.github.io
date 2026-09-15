---
name: site
description: Work on linhcobui.github.io, Linh-Co Bui's Jekyll personal site. Use when editing the homepage, bio, updates, posts, styling, social/profile links, or deploying changes to this repo.
---

# linhcobui.github.io

Jekyll site based on the jekyll-swiss theme, with the theme's layouts and Sass vendored into this repo. Deployed to GitHub Pages by GitHub Actions.

## Deploying

- Push to `master`. `.github/workflows/build_and_deploy.yml` builds with Ruby 2.7 + Bundler 1.17.3 and publishes `_site` via `peaceiris/actions-gh-pages`. The change is live a few minutes later.
- **Do not build locally.** The toolchain is pinned to old Ruby/Bundler versions that are awkward to install, and the owner prefers to push and check the live site. Don't run `bundle install` or `jekyll build`/`serve`.
- The remote uses SSH: `git@github.com:linhcobui/linhcobui.github.io.git`.
- To force a rebuild with no content change, update the timestamp in `.publish-trigger.txt` and push.

## Verifying a change

Sass only compiles in CI, so check the deployed output rather than guessing:

```sh
curl -s https://linhcobui.github.io/ | grep -A4 'PATTERN'
curl -s https://linhcobui.github.io/assets/style.css | tr '}' '\n' | grep 'SELECTOR'
```

If the served HTML and CSS are both correct but the page still looks wrong, suspect browser cache (see Gotchas).

## Where things live

| What | File |
|---|---|
| Name, description, profile URLs, theme, WIP note | `_config.yml`: `title`, `linkedin_url`, `scholar_url`, `theme_color`, `wip` |
| Bio paragraphs | `index.html`, rendered into `{{ content }}` in the home layout |
| Homepage structure | `_layouts/home.html` |
| Dated updates list | `_data/updates.yml`, newest first, markdown body |
| Posts | `_posts/` |
| Social icons, footer, head | `_includes/` |
| Palette | `_sass/_theme-<color>.scss`; the active one is picked by `theme_color` |
| Grid, spacing and colour utility classes | `_sass/_utilities.scss` |
| Type scale, breakpoint, spacer | `_sass/_variables.scss` |

## Homepage header

- `h1#site-name.header-title` shows `site.title`. Hovering swaps the text instantly: 75% of the time to `data-name-vi` (Bùi Linh Cơ), 25% to `data-name-han` (裴灵機). Mouse-out restores `data-name-latin`. The script is inline at the bottom of `home.html`.
- Under the name is a two-column grid: `col-7` holds the social icon row and bio; `col-4` holds a dot and the "Website in progress" note.

## Adding a social or profile icon

1. Fetch the logo from simple-icons: `curl -s https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/NAME.svg`
2. Create `_includes/NAME.html` in the same shape as the existing icons:
   ```html
   <a href="URL" title="Linh-Co Bui on NAME" class="link-social block">
   <svg height="32" class="header-social" viewBox="0 0 24 24" version="1.1" width="32" aria-hidden="true"><path d="..."/></svg>
   </a>
   ```
3. Add `{% include NAME.html %}` to the icon row in `_layouts/home.html`, wrapped in `<div class="inline-block mt-3 mr-1">`. The last icon in the row drops `mr-1`.

Icons fill with `$color-foreground` and switch to `$color-nav-link` on hover through `.link-social:hover .header-social`, so a new icon needs no CSS. Unused includes for twitter, instagram, dribbble and medium already exist.

## Conventions

- **Colours:** use the theme variables (`$color-foreground`, `$color-title`, `$color-nav-link`, …), never hex values, so every theme keeps working.
- **Design:** Swiss typography. Align to the 12-column grid (`.col-N`, `.left`, `.sm-width-full`), keep edges hard and type restrained.
- **Copy:** Australian/British spelling, e.g. "modelling".

## Gotchas

- `.h0` is shared with post and page titles. To resize only the homepage name, change `.header-title`, not `.h0`.
- If you replace an asset at the same URL, browsers keep serving the cached old file. Give the new file a new name.
- Jekyll skips dot-directories, so `.claude/` never gets published.

## Already decided: don't reintroduce without asking

- **The name swap is instant.** A 3D signboard flip was tried and removed.
- **No photo in the header.** A portrait block and a circular headshot in a row of dots were both tried and removed.
