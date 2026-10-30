# Website migration — October 30, 2026

The site was synchronized with `origin/master` at `683332b9` and migrated to al-folio v1.x at upstream commit
`929fbf71c5a45b13e9b5a20dfa425ce9e9de7935` on `update/al-folio-v1`. The upstream history is merged into this branch so existing
website commits are retained.

## Preserved content

- Biography, publications, coauthor and venue data, content collections, photos, and downloadable assets come from the existing website.
- `_bibliography/papers.bib` is unchanged from `origin/master`.
- The uncommitted local `assets/pdf/Yikai_Wu_CV.pdf` is included. Its SHA256 is
  `9420df7bc5fcb60ec6e9eb9bcf7c6f90d22db12a635b3bf33a3b26db7179b495`.
- The original checkout remains recoverable through `backup/pre-al-folio-sync-20261030`; the original local CV change also remains in a named Git stash.

## Compatibility changes

- The starter now loads the pinned `al_folio_core` theme and versioned feature gems from the current upstream Gemfile and lockfile.
- Old copied layouts, includes, Sass partials, plugins, scripts, icon fonts, and runtime assets were removed in favor of their gem-owned equivalents.
- Personal social links moved from `_config.yml` into `_data/socials.yml`, replacing the inherited demo identity. The profile address uses `more_info`.
- The only runtime override is `assets/css/main.scss`, which retains Princeton orange accents and imports the current gem's Sass modules.
  `.al-folio-overrides.yml` records its reviewed upstream and local checksums.
- Bootstrap compatibility remains enabled for existing project and sample-post markup.
- The CV route links to the maintained personal PDF. Legacy example CV data is retained but is not rendered by that route.
- The site retains its root URL, existing navigation, and content routes. Home-page sample announcements and recent-post widgets are disabled.
- A sample post's broken reference to an already-deleted Distill post now links to the upstream example.
- Newly imported demo courses were removed. Citation refresh and RenderCV workflows are manual; automatic RenderCV use needs a data conversion first.
- GitHub Pages uses the current upstream deployment workflow with Ruby 3.3.5. The configuration keeps `baseurl: ""` for this personal GitHub Pages site.

## Validation

The following checks passed using Ruby 3.3.5 in a disposable Docker container:

```bash
bundle install
npm ci --ignore-scripts
npm run lint:prettier
npm run lint:style-contract
bundle exec al-folio upgrade audit
bundle exec al-folio upgrade overrides audit
JEKYLL_ENV=production bundle exec jekyll build
purgecss -c purgecss.config.js
```

Rendered home, publications, CV, repositories, projects, and blog routes were checked for local script, stylesheet, and image assets.
The home-page identity, selected publications, personal social links, and generated CV checksum were also verified.

The migration audit has no blocking findings. Four non-blocking notices identify legacy `data-toggle` attributes in the existing sample tables post;
the enabled Bootstrap compatibility plugin supports them. The color override is acknowledged with no stale overrides.

Visual browser inspection could not be completed because the browser access host was unavailable. The biography and inherited sample content were
preserved; their factual currency was not independently updated as part of this theme migration.

## Future theme updates

Update the pinned plugin versions in `Gemfile`, regenerate `Gemfile.lock`, and run the validation commands above. When updating `al_folio_core`, also run:

```bash
bundle exec al-folio upgrade overrides diff assets/css/main.scss
bundle exec al-folio upgrade overrides accept assets/css/main.scss
```

Review the diff before accepting so the local stylesheet continues importing all required modules. Keep the two Princeton color declarations when
reconciling future upstream changes. Bootstrap compatibility is supported through v1.2 and should be migrated before v2.0.
