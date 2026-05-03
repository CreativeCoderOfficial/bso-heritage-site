# BSO Heritage Website

A static digital archive for British Scouting Overseas, built on [CollectionBuilder-Sheets](https://github.com/CollectionBuilder/collectionbuilder-sheets) (CB-Sheets). The site loads collection metadata from a published Google Sheet CSV at runtime — no build-time database, no backend.

**Live site:** https://CreativeCoderOfficial.github.io/bso-heritage-site (to be set up)

---

## Table of Contents

- [Architecture overview](#architecture-overview)
- [Prerequisites](#prerequisites)
- [Local setup](#local-setup)
- [Project structure](#project-structure)
- [Configuration files](#configuration-files)
- [Metadata & Google Sheets](#metadata--google-sheets)
- [Branding & theming](#branding--theming)
- [Custom pages & layouts](#custom-pages--layouts)

---

## Architecture overview

CB-Sheets is a Jekyll static site generator template. At build time, Jekyll compiles Liquid templates into plain HTML/CSS/JS. There is no server-side logic.

**How metadata works at runtime:**

1. A visitor loads any page.
2. `pageinit-js.html` fires — it checks `sessionStorage` for a cached copy of the metadata.
3. If none exists, [PapaParse](https://www.papaparse.com/) fetches the Google Sheet CSV URL (configured in `_config.yml` as `metadata-csv`) and parses it client-side.
4. The parsed array of item objects is stored in `sessionStorage` for the rest of the session.
5. Every visualization (browse cards, map markers, timeline rows, search index) is built from that array via JS functions collected in `includeFunctions[]`.

This means **the site can be rebuilt without touching the metadata** — volunteers update the Google Sheet, a nightly GitHub Action rebuilds the static files, and the new CSV is fetched fresh on next visit.

---

## Prerequisites

| Dependency | Version | Purpose |
|---|---|---|
| Ruby | >= 3.1 | Jekyll runtime |
| Bundler | >= 2.x | Gem management |
| Jekyll | via `github-pages` gem | Static site generator |
| Git | any | Version control |
| WSL (if you're on Windows) | any Ubuntu LTS | Recommended shell environment |

---

## Local setup

```bash
# 1. Clone the repo
git clone https://github.com/CreativeCoderOfficial/bso-heritage-site-codespace.git
cd bso-heritage-site-codespace

# 2. Install gems (first run only, or after Gemfile changes)
bundle install

# 3. Serve locally
bundle exec jekyll serve

# Site is now at http://localhost:4000
# Jekyll watches for file changes and rebuilds automatically
```


## Project structure

```
bso-heritage-site-codespace/
│
├── _config.yml                 # Site-wide settings: title, metadata CSV URL, baseurl
├── _data/                      # YAML/CSV config files read by Jekyll at build time
│   ├── config-nav.csv          # Navigation bar items and dropdown structure
│   ├── config-browse.csv       # Fields shown on Browse cards
│   ├── config-metadata.csv     # Fields shown on individual item pages
│   ├── config-map.csv          # Fields shown in map popups
│   ├── config-search.csv       # Fields indexed by Lunr.js search
│   ├── config-table.csv        # Columns shown on the Data table page
│   ├── config-theme-colors.csv # Bootstrap color overrides (primary = BSO purple)
│   ├── theme.yml               # Visual settings: featured image, map defaults, fonts
│   ├── footer.yml              # Footer column content (links, social, legal)
│   ├── home-stats.csv          # Stat cards on the homepage
│   └── home-features.csv       # Feature cards on the homepage
│
├── _includes/                  # Reusable Liquid partials
│   ├── collection-nav.html     # Top navbar
│   ├── footer.html             # Site footer
│   ├── hero.html               # Homepage hero section
│   ├── stats-grid.html         # Homepage stats section
│   ├── features-grid.html      # Homepage features section
│   ├── feature/                # Embeddable content blocks (image, video, card, etc.)
│   ├── index/                  # Homepage-specific includes (carousel, time span, etc.)
│   └── js/                     # JS partials injected at foot of pages
│       ├── pageinit-js.html    # Core: loads CSV, runs includeFunctions[]
│       ├── browse-js.html      # Browse page filtering and card rendering
│       ├── item-js.html        # Item page rendering
│       ├── map-js.html         # Leaflet map setup
│       ├── timeline-js.html    # Timeline rendering
│       └── lunr-js.html        # Full-text search index
│
├── _layouts/                   # Page shell templates
│   ├── default.html            # Base layout (head + nav + footer)
│   ├── home.html               # Homepage (hero + stats + features)
│   ├── browse.html             # Browse page with filter UI
│   ├── item.html               # Individual item page
│   ├── heritage-page.html      # Heritage period pages with sidebar nav
│   ├── category-page.html      # Type-filtered browse (Groups, Badges, etc.)
│   └── ...                     # map, timeline, search, data, about, cloud
│
├── _sass/                      # SCSS source files, compiled into assets/css/cb.css
│   ├── cb.scss                 # Entry point — imports everything, sets CB variables
│   ├── _base.scss              # Body, links, nav, footer base overrides
│   ├── _custom.scss            # BSO-specific styles (hero, cards, browse filters)
│   ├── _pages.scss             # Page-specific styles (map height, timeline, about)
│   ├── _theme-colors.scss      # Bootstrap color system override (btn-primary etc.)
│   └── _theme-utilities.scss   # Bootstrap utility class patches
│
├── pages/                      # One .md file per page (sets layout, permalink, front matter)
│   ├── browse.md
│   ├── map.md
│   ├── timeline.md
│   ├── groups.md               # category-page layout, filter: Group
│   ├── badges.md               # category-page layout, filter: Badge
│   ├── documents.md            # category-page layout, filter: Document
│   ├── far-and-wide.md         # category-page layout, filter: Far & Wide
│   ├── british-groups-abroad.md
│   ├── bswe-germany-europe.md
│   ├── bso-2013-present.md
│   └── ...
│
├── objects/                    # Static media files (images, PDFs, audio)
│   └── README.md               # File naming and size guidelines
│
├── assets/
│   ├── css/cb.css              # Compiled SCSS output (do not edit directly)
│   ├── img/                    # Site images (logos, featured banner)
│   ├── lib/                    # Vendored JS/CSS: Bootstrap, Leaflet, Lunr, PapaParse
│   └── metadata-template.csv  # Blank template for new metadata sheets
│
├── index.md                    # Homepage (layout: home)
├── Gemfile                     # Ruby gem dependencies
└── _config.yml
```

---

## Configuration files

### `_config.yml`

The primary site configuration. Key values:

```yaml
url: https://CreativeCoderOfficial.github.io
baseurl: /bso-heritage-site-codespace

title: BSO Heritage Website
tagline: British Scouting Overseas Digital Archive
description: "..."

# The CSV that drives the entire collection:
metadata-csv: https://docs.google.com/spreadsheets/d/e/...

development-mode: true   # Shows "Change Metadata" dev modal; set false for production
noindex: true            # Prevents Google indexing; remove for launch
```

> **`baseurl`** is critical. Jekyll prepends it to all relative URLs via the `relative_url` Liquid filter. If you fork and host elsewhere, update both `url` and `baseurl`.

---

## Metadata & Google Sheets

### Sheet structure

The collection is driven by a single published Google Sheet. The current sheet has multiple tabs; CB-Sheets reads only the tab referenced in the published CSV URL (the `gid` parameter in the URL identifies the tab).

The metadata CSV URL in `_config.yml` currently points to the main items tab:

```
https://docs.google.com/spreadsheets/d/e/2PACX-.../pub?gid=1179782383&single=true&output=csv
```

### Required fields

| Field | Notes |
|---|---|
| `objectid` | Unique, lowercase, no spaces or slashes. Used as the URL `?id=` parameter. |
| `title` | Displayed everywhere. |
| `format` | MIME type string. Controls how the item is rendered (`image/jpeg`, `application/pdf`, `audio/mpeg`, `video/mp4`). |
| `filename` | Filename in `objects/` folder, or a full HTTPS URL to an external file. Leave blank for YouTube items. |
| `youtubeid` | YouTube video ID only (e.g. `dQw4w9WgXcQ`). Leave blank for everything else. |

### BSO-specific fields

These are not part of the CB-Sheets default template but have been added for this project:

```
type, era,	organisation,	district,	country,	group_number,	scarf_colour,	date_founded,	date_closed,	subject,	latitude,	longitude,	badge_tier, badge_type,	colour_variant,	document_type,	date,	issue_number,	season
```

These are configured in `_data/config-metadata.csv`, `_data/config-browse.csv`, and `_data/config-search.csv`.

---

## Branding & theming

BSO colours are defined in `_data/config-theme-colors.csv`:

| Class | Hex |
|---|---|
| `primary` | `#5C2D91` (BSO purple) |
| `dark` | `#3B1A5E` (purple dark) |
| `secondary` | `#EDE7F6` (purple light) |
| `light` | `#FFFFFF` |


---

## Custom pages & layouts

### `layout: home`

`index.md` uses this layout. It composes three includes: `hero.html`, `stats-grid.html`, `features-grid.html`. Stats and features are driven by `_data/home-stats.csv` and `_data/home-features.csv` respectively — no layout edits needed to change content.

### `layout: heritage-page`

Used by the three period pages (`british-groups-abroad.md`, `bswe-germany-europe.md`, `bso-2013-present.md`). Renders a two-column layout: main content on the left, a sidebar with period navigation and explore links on the right. The sidebar links are hardcoded in the layout — update `_layouts/heritage-page.html` if pages are added or renamed.

### `layout: category-page`

Used by Groups, Badges, Documents, Far & Wide. Accepts a `filter` front matter value that auto-applies a `type` field filter to the browse results on page load. The filtering is done client-side via a `setInterval` polling loop in the layout that waits for `cb_items` to be available, then calls `filterItems()`.

### Adding a new static page

1. Create `pages/your-page.md` with appropriate front matter (`layout`, `permalink`, `title`).
2. Add a row to `_data/config-nav.csv` if it needs a nav link.
3. If it needs a dropdown, set `dropdown_parent` to an existing nav item's `display_name`.

---




## All credits to CollectionBuilder for their great template:

<https://collectionbuilder.github.io/>

CollectionBuilder is a project of University of Idaho Library's [Digital Initiatives](https://www.lib.uidaho.edu/digital/) and the [Center for Digital Inquiry and Learning](https://cdil.lib.uidaho.edu) (CDIL) following the [Lib-Static](https://lib-static.github.io/) methodology. 
Powered by the open source static site generator [Jekyll](https://jekyllrb.com/) and a modern static web stack, it puts collection metadata to work building beautiful sites.

The basic theme is created using [Bootstrap](https://getbootstrap.com/).
Metadata visualizations are built using open source libraries such as [DataTables](https://datatables.net/), [Leafletjs](http://leafletjs.com/), [Spotlight gallery](https://github.com/nextapps-de/spotlight), [lazysizes](https://github.com/aFarkas/lazysizes), and [Lunr.js](https://lunrjs.com/).
Object metadata is exposed using [Schema.org](http://schema.org) and [Open Graph protocol](http://ogp.me/) standards.

Questions can be directed to **collectionbuilder.team@gmail.com**

## License

CollectionBuilder documentation and general web content is licensed [Creative Commons Attribution-ShareAlike 4.0 International](http://creativecommons.org/licenses/by-sa/4.0/). 
This license does *NOT* include any objects or images used in digital collections, which may have individually applied licenses described by a "rights" field.
CollectionBuilder code is licensed [MIT](https://github.com/CollectionBuilder/collectionbuilder-gh/blob/main/LICENSE). 
This license does not include external dependencies included in the `assets/lib` directory, which are covered by their individual licenses.
