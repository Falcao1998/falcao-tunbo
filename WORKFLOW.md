# TUNBO — Workflow

TUNBO theme for **tunbo.shop**, forked from Shopify Horizon 4.3.0.
Technical conventions for the theme code live in `AGENTS.md`. This file covers the Git and release process.

## Environment

| Item | Value |
|---|---|
| Repo | github.com/Falcao1998/falcao-tunbo (private) |
| Branch `main` | the **live** theme on the store (Shopify GitHub integration, theme "falcao-tunbo/main") |
| Branch `dev` | staging; connected as the unpublished theme "falcao-tunbo/dev" |
| Local folder | ~/Desktop/falcao-tunbo |
| Store | tunbo.shop (permanent domain: falca.myshopify.com — use this with the CLI) |
| Shopify CLI | `shopify theme dev --store falca.myshopify.com` to preview locally, `shopify theme check` to lint |

Never run `shopify theme push --live`. Deploying = `git push` to `main`.

## Day-to-day

```bash
git checkout dev
git pull                                 # pick up Theme Editor commits and other changes
shopify theme dev --store falca.myshopify.com     # edit with live preview
shopify theme check                      # lint before committing
git add -A
git commit -m "feat(home): add lookbook section"
git push                                 # falcao-tunbo/dev updates itself → preview in Admin
```

## Releasing

```bash
git checkout main && git pull
git merge dev
# 1. bump "theme_version" in config/settings_schema.json (e.g. 1.1.0)
# 2. add an entry at the top of release-notes.md
git commit -am "chore(release): v1.1.0"
git tag -a v1.1.0 -m "Short description"
git push && git push --tags
```

The store updates within seconds. Re-check home, product, collection and cart pages on desktop and mobile.

## Rolling back

| Situation | Command |
|---|---|
| Uncommitted edits you want to drop | `git restore <file>` or `git restore .` |
| Undo a commit that is already live | `git revert <commit>` then `git push` |
| Restore one file from an older version | `git checkout v1.0.0 -- sections/header.liquid` |
| Inspect the code at a tag | `git checkout v1.0.0` … `git checkout main` |
| Emergency | Admin → Themes → Publish the backup theme |

## Conventions

- Commit messages: `feat:` `fix:` `style:` `chore:` `docs:` with a scope, e.g. `fix(cart): …`. One purpose per commit. English only.
- Everything in the repo and on the storefront is written in English.
- Version the whole theme, including `templates/*.json` and `config/settings_data.json`. `.gitignore` only excludes `.shopify/`, OS files and archives.
- Images referenced from templates must exist in Admin → Content → Files, referenced as `shopify://shop_images/<file>`.
- Edits made in the Theme Editor on the live theme are committed to `main` by Shopify; always `git pull` before working locally.
- Keep section/block IDs in `templates/*.json` stable — merchant settings depend on them.
- Theme check: the 2 `UnknownFilter` errors and a few warnings come from upstream Horizon; ignore them.
- Every release: bump `theme_version`, update `release-notes.md`, add a tag.
