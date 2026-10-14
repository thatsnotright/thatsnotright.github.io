# thatsnotright.github.io

Source for <https://thatsnotright.github.io>. GitHub Pages builds it with Jekyll on every push to `main`.

## Layout

```
_config.yml            site title, tagline, URL
_layouts/              default (page shell), home (hero + game cards), page (prose)
_includes/             head, header, footer, project-card
_data/projects.yml     the game cards on the home page, in order
assets/css/theme.css   the "Red Pen" theme: graph paper, red-pen notes, crooked index cards
assets/img/            favicon and 1200×630 card / link-preview images
index.md, 404.md       home and not-found pages
frong/                 built Frong game (generated; don't edit by hand)
```

The theme is plain CSS with custom properties (light and dark), so there is no Sass step. Fonts are
Bricolage Grotesque and Caveat from Google Fonts.

## Updating Frong

From the frong repo, which sits next to this one:

```bash
bun run publish:pages    # rebuilds ../thatsnotright.github.io/frong
```

Then commit `frong/` here and push.

## Adding a game

1. Put the built game in its own folder, e.g. `mygame/`, with relative asset paths.
2. Add a card to `_data/projects.yml` and a 1200×630 screenshot to `assets/img/`.

## Preview locally

This uses the same image GitHub Pages builds with, so what you see is what deploys:

```bash
mkdir -p _site
docker run --rm --user "$(id -u):$(id -g)" -e HOME=/tmp \
  -e GITHUB_WORKSPACE=/gh -e INPUT_SOURCE=src -e INPUT_DESTINATION=out \
  -v "$PWD":/gh/src -v "$PWD/_site":/gh/out \
  ghcr.io/actions/jekyll-build-pages:v1.0.13
python3 -m http.server -d _site 4000    # http://localhost:4000
```

Or, with Ruby and Bundler: `bundle install && bundle exec jekyll serve`.
