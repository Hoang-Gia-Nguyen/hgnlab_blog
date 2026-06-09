# Zola Theme Management & Customization Guide

> **HgN Lab** — Zola v0.22.1 · Active theme: **Linkita** · Alternate theme: **Nivis**

---

## Table of Contents

1. [Zola Theme Architecture](#1-zola-theme-architecture)
2. [Managing Themes](#2-managing-themes)
3. [Theme Comparison: Linkita vs Nivis](#3-theme-comparison-linkita-vs-nivis)
4. [Customization Layers (by priority)](#4-customization-layers-by-priority)
5. [Linkita Theme Deep Dive](#5-linkita-theme-deep-dive)
6. [How This Blog Is Customized](#6-how-this-blog-is-customized)
7. [Workflow & Best Practices](#7-workflow--best-practices)
8. [Troubleshooting](#8-troubleshooting)

---

## 1. Zola Theme Architecture

### How themes work in Zola

Zola looks for templates in this order of priority:

1. `<site>/templates/` — **Site-level overrides** (highest priority)
2. `<site>/themes/<theme-name>/templates/` — **Theme templates**
3. Zola built-in fallbacks

This means **any file you place in `templates/` with the same name as a theme template will override it**. Partial files (`partials/*.html`) and shortcodes follow the same rule. You only need to copy the specific files you want to customize — the rest fall through to the theme.

### Static & Sass override rules

The same priority applies to **static files** and **Sass**:

- `<site>/static/` overrides `<theme>/static/` (same relative path)
- `<site>/sass/` overrides `<theme>/sass/`

### Theme structure (Linkita)

```
themes/linkita/
├── src/                      # Source files (Tailwind CSS, JS)
│   ├── app.css               # Tailwind input CSS
│   ├── main.css              # Expanded CSS (dev output)
│   ├── linkita.js            # Main JS bundle
│   ├── linkita-search.js     # Search JS
│   └── gc.js                 # GoatCounter analytics JS
├── static/                   # Compiled assets served by Zola
│   ├── main.min.css          # Minified Tailwind output
│   ├── js/                   # Minified JS bundles
│   ├── katex/                # Self-hosted KaTeX (math rendering)
│   └── icons/                # Social icons (SVG files)
├── templates/
│   ├── index.html            # Base layout (extends variables.html)
│   ├── page.html             # Single article page
│   ├── section.html          # Section listing
│   ├── pages.html            # Custom pages template
│   ├── archive.html          # Archive view
│   ├── taxonomy_list.html    # Category/Tag listing
│   ├── taxonomy_single.html  # Posts in a category/tag
│   ├── 404.html              # Error page
│   ├── sitemap.xml           # SEO sitemap
│   ├── split_sitemap_index.xml
│   ├── variables.html        # Global variables & macros
│   ├── partials/             # 15 reusable template fragments
│   ├── macros/               # Tera macros (i18n, url, profiles)
│   └── shortcodes/           # Zola shortcodes (admonition, gallery, mermaid, projects)
├── theme.toml                # Theme metadata
├── theme.just                # Justfile (dev/build commands)
├── tailwind.config.js        # Tailwind configuration
└── package.json              # Node dependencies
```

### Key concepts

- **Tera templating**: Zola uses the Tera template engine (inspired by Jinja2/Django)
- **Template inheritance**: Templates use `{% extends "base.html" %}` and `{% block name %}...{% endblock %}`
- **Inheritance chain**: `variables.html` → `index.html` → `page.html` / `section.html` (blocks: `meta`, `html`, `main`)
- **Macros**: Reusable functions in `templates/macros/` (imported via `{% import "macros/url.html" as m_url %}`)
- **Shortcodes**: Template files in `templates/shortcodes/` usable from Markdown content via `{{/* shortcode_name() */}}`

---

## 2. Managing Themes

### Installing a new theme

```bash
# As a git submodule (recommended for version tracking)
git submodule add <repo-url> themes/<theme-name>

# Examples used in this blog:
git submodule add https://codeberg.org/salif/linkita.git themes/linkita
git submodule add -b master https://github.com/Resorie/zola-theme-nivis.git themes/nivis
```

### Activating a theme

In `config.toml`, set at the top level (not under `[extra]`):

```toml
theme = "linkita"
```

### Switching themes

1. Change `theme = "nivis"` in `config.toml`
2. Check which templates/partials exist in the new theme
3. Audit your site-level overrides (`templates/`) — they may need updating for the new theme's template API
4. Update `[extra]` config variables to match the new theme's expectations
5. Run `zola build` to verify

### Updating a theme

```bash
git submodule update --remote themes/linkita
```

After updating, check the theme's `CHANGELOG.md` for breaking changes. If the theme uses Tailwind and you have overridden templates that use Tailwind classes, rebuild the CSS:

```bash
cd themes/linkita
pnpm tailwindcss -i ./src/app.css -o ../../static/main.min.css --minify
```

### Removing a theme

```bash
# Remove submodule entry
git submodule deinit themes/<theme-name>
git rm themes/<theme-name>
rm -rf .git/modules/themes/<theme-name>
# Then remove theme = "..." from config.toml
```

---

## 3. Theme Comparison: Linkita vs Nivis

This blog has both themes installed. Here's a comparison to help decide which to use.

| Feature | Linkita (active) | Nivis |
|---|---|---|
| **Style** | Clean, elegant, Paper-inspired | Minimalist, focus on typography |
| **CSS framework** | Tailwind CSS (v3) via `@tailwindcss/typography` | Custom CSS + Sass |
| **Dark mode** | ✅ Class-based toggle, localStorage | ✅ Supported |
| **Multilingual** | ✅ Full i18n with per-language `[extra]` | Limited |
| **Search** | ✅ elasticlunr (configurable) | ❌ Not built-in |
| **KaTeX math** | ✅ Self-hosted or CDN | ❌ Uses MathJax instead |
| **Mermaid diagrams** | ✅ | ❌ |
| **Comments** | ✅ Giscus integration | ❌ |
| **SEO / Open Graph** | ✅ Comprehensive | Basic |
| **Taxonomies** | ✅ Categories + Tags + Authors | Tags only |
| **Author profiles** | ✅ Rich profile system | ❌ Simple avatar only |
| **Archive page** | ✅ Built-in | ✅ Built-in |
| **Shortcodes** | Admonition, Gallery, Mermaid, Projects | None |
| **Sitemap** | ✅ Split sitemap support | ❌ |
| **Customization** | Inject points, partial overrides, config-driven | Config-driven |
| **Maintenance** | Active development (2025) | Less active |
| **Zola min version** | 0.19.0 | 0.21.0 |

**When to use Linkita**: You need multilingual support, search, comments, rich SEO, math rendering, or diagrams.

**When to use Nivis**: You want a simpler, lighter theme with focus on reading experience and don't need multilingual or search.

---

## 4. Customization Layers (by priority)

Customizations should be applied at the **highest layer that meets your needs** to minimize maintenance burden when updating themes.

### Layer 1: `config.toml` (easiest, no template changes)

The most common customizations are done through `config.toml` under `[extra]`:

```toml
[extra]
# Colors
style.bg_color = "#f4f4f5"
style.bg_dark_color = "#18181b"
style.header_color = "#e4e4e7"
style.header_dark_color = "#27272a"
style.header_blur = true

# Content
header_menu_name = "menu_name"
post_navigation = "reversed"
toc = { open = true }
page_info = [
    { when = "date" },
    { when = "reading_time" },
    { when = "authors" },
]

# Features
comment = true
math = false
mermaid = false
disable_javascript = false
disable_default_favicon = false
relative_urls = false
```

See [Linkita configuration reference](#linkita-configuration-reference) for all available options.

### Layer 2: Inject files (moderate, survives theme updates)

The Linkita theme provides **inject points** — optional files placed in `<site>/templates/injects/` that are included if they exist:

| Inject file | Location in page | Typical use |
|---|---|---|
| `templates/injects/head.html` | Inside `<head>`, before CSS/JS | Custom fonts, preconnects |
| `templates/injects/head_end.html` | End of `<head>`, after CSS/JS | Analytics, extra meta tags |
| `templates/injects/body_start.html` | After `<body>` opens | Loading spinners, banners |
| `templates/injects/body_end.html` | Before `</body>` closes | Chat widgets, opt-in scripts |
| `templates/injects/page_start.html` | Top of article content | Alert banners, ad placement |
| `templates/injects/page_end.html` | Bottom of article content | Related posts, newsletter signup |
| `templates/injects/header_nav.html` | Inside header nav | Extra nav items (dynamic) |
| `templates/injects/footer.html` | Inside footer | Custom credits, badges |

**Example** — adding Google Fonts:

```html
<!-- templates/injects/head_end.html -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
```

### Layer 3: Override partials (targeted changes)

Copy a partial from `themes/linkita/templates/partials/` to `<site>/templates/partials/` and edit it.

**When to use this**: When you need to change the structure of a specific component (footer, header, profile card, page list).

**⚠️ Maintenance risk**: If the theme updates the partial, your override won't get those updates. Review periodically by diffing.

### Layer 4: Override full templates (major changes)

Copy a template from `themes/linkita/templates/` to `<site>/templates/` and edit it.

**When to use this**: When you need to change page layout fundamentally — for example, adding a custom section to the home page, changing the HTML structure of articles, or adding a custom footer element outside the theme's footer partial.

**Current example in this blog**: `templates/index.html` overrides the theme's `index.html` to add a donation CTA after the page list.

### Layer 5: Custom CSS / Sass (visual changes)

The `sass/` directory can contain Sass files that Zola compiles to CSS automatically (`compile_sass = true` in `config.toml`):

```bash
# Create a custom stylesheet
touch sass/custom.scss
```

Then load it in `<site>/templates/injects/head_end.html`:

```html
<link rel="stylesheet" href="{{ get_url(path='custom.css', cachebust=true) }}">
```

Alternatively, you can modify the Tailwind build output (see [Rebuilding Tailwind CSS](#rebuilding-tailwind-css)).

### Layer 6: Modify theme source directly (last resort)

Directly editing files inside `themes/linkita/` makes your theme diverge from upstream — `git submodule update` will overwrite your changes. Only do this if you plan to maintain your own fork.

---

## 5. Linkita Theme Deep Dive

### Configuration reference

All variables go under `[extra]` in `config.toml` unless noted.

#### Appearance

| Variable | Type | Default | Description |
|---|---|---|---|
| `style.bg_color` | string | `"#f4f4f5"` | Light mode background color |
| `style.bg_dark_color` | string | `"#18181b"` | Dark mode background color |
| `style.header_color` | string | `"#e4e4e7"` | Light mode header color |
| `style.header_dark_color` | string | `"#27272a"` | Dark mode header color |
| `style.header_blur` | bool | `false` | Use `backdrop-filter: blur()` on header |

These can also be overridden per-page via frontmatter: `[extra.style]` with the same keys.

#### Navigation & Layout

| Variable | Type | Default | Description |
|---|---|---|---|
| `header_menu_name` | string | — | Key into `[extra.menus]` dict for nav items |
| `menus.<name>` | array | — | Named navigation menus (items with `url`, `name`) |
| `post_navigation` | string | — | Set `"reversed"` to reverse chronological prev/next |
| `invert_page_navigation` | bool | — | Alternative way to invert nav direction |
| `relative_urls` | bool | `false` | Use relative URLs (useful for offline/`file://` browsing) |
| `urls_to_index_html` | bool | `false` | Append `index.html` to URLs for static hosting |
| `taxonomy_sorting` | string | `"posts_count"` | Sort taxonomy terms by number of posts |

#### Content & Metadata

| Variable | Type | Default | Description |
|---|---|---|---|
| `page_info` | array | date, updated, reading_time, authors | What metadata to show on post pages |
| `page_info_on_paginator` | array | date, reading_time | Metadata shown on listing/paginator pages |
| `page_summary_on_paginator` | bool | `false` | Show summary instead of description on listings |
| `toc` | bool/object | `true` | Table of contents; set `{ open = true }` for expanded by default |
| `date_format` | string | `"%F"` | Date format string, per language |
| `title_separator` | string | `" \| "` | Separator in `<title>` tag |

The `page_info` array supports these `when` values: `date`, `date_updated`, `reading_time`, `word_count`, `authors`, `tags`. Each item can have `prepend` and `append` strings.

#### Features

| Variable | Type | Default | Description |
|---|---|---|---|
| `comment` | bool | `false` | Enable Giscus comments globally |
| `comment.repo` | string | — | GitHub repo for Giscus (`user/repo`) |
| `comment.repo_id` | string | — | Giscus repo ID |
| `comment.category` | string | — | Giscus discussion category |
| `comment.category_id` | string | — | Giscus category ID |
| `math` | bool | `false` | Enable KaTeX math rendering globally |
| `mermaid` | bool | `false` | Enable Mermaid diagrams globally |
| `disable_javascript` | bool | `false` | Disable all JavaScript |
| `disable_default_favicon` | bool | `false` | Don't include default favicon links |
| `use_cdn` | bool | `false` | Load KaTeX from CDN instead of self-hosted |
| `goatcounter.endpoint` | string | — | GoatCounter analytics endpoint URL |
| `vercel_analytics.src` | string | — | Vercel Web Analytics script source |
| `webmanifest` | string | — | Path to PWA webmanifest file |
| `translations` | dict | — | Custom translation strings for UI elements |

#### Footer

| Variable | Type | Default | Description |
|---|---|---|---|
| `footer.copyright` | string | — | Custom copyright text. Supports `$YEAR`, `$BASE_URL`, `$LICENSE_URL` placeholders |
| `footer.since` | int | — | Starting year for copyright range (e.g., `2024`) |
| `footer.license_url` | string | — | Link to content license |
| `footer.privacy_policy_url` | string | — | Link to privacy policy |
| `footer.terms_of_service_url` | string | — | Link to terms of service |
| `footer.search_page_url` | string | — | Link to search page |

#### Profiles

Author profiles go under `[extra.profiles.<username>]`:

```toml
[extra.profiles.hgn]
avatar_url = "icons/favico.svg"
avatar_alt = ""
avatar_invert = true     # Invert in dark mode
name = "Hoàng Gia Nguyên"
bio = "Description text (supports **markdown**)"
email = "user@example.com"
social = [
    { name = "github", url = "https://github.com/..." },
    { name = "rss", url = "$BASE_URL/atom.xml" },
]
```

Social icon names correspond to:
1. SVG files in `static/icons/` (project-specific)
2. Icon names from [Simple Icons](https://simpleicons.org/) (included via `node_modules/simple-icons/`)

#### Languages

Each language has its own `[languages.<code>]` section:

```toml
[languages.en]
title = "My Blog"
description = "Blog description"
generate_feeds = true
feed_filenames = ["atom.xml"]
taxonomies = [
    { name = "categories", feed = true, paginate_by = 5 },
    { name = "tags", feed = true, paginate_by = 5 },
]

[extra.languages.en]
i18n_code = "en"                    # For i18n.json lookups
language_code = "en"                # For HTML lang attribute
locale = "en_US"                    # For date formatting
header_buttons = ["site_title", "theme_button", "search_button"]
header_menu_name = "menu_name"
date_format = "%B %d, %Y"
# Taxonomy descriptions for SEO
taxonomy_descriptions.categories = "Browse articles by category"
term_descriptions.tags = "Articles tagged with $NAME"
```

### Template blocks

The main template chain defines these blocks you can override:

| Block | Defined in | Used by | Purpose |
|---|---|---|---|
| `meta` | `variables.html` | All pages | Sets `g_is_article`, page metadata variables |
| `html` | `variables.html` | `index.html`, `404.html` | Entire `<html>` element |
| `main` | `index.html` | `page.html`, `section.html`, etc. | Main content area |

### Shortcodes

Available shortcodes — use them in Markdown content:

```
{{/* admonition(type="note", title="Heads up") */}}
This is a note callout box.
{{/* end */}}
```

```
{{/* gallery(path="gallery/") */}}
```

```
{{/* mermaid() */}}
graph TD;
    A-->B;
    B-->C;
{{/* end */}}
```

```
{{/* projects(path="data/projects.toml", format="toml") */}}
```

### Inject points summary

| File | Location |
|---|---|
| `injects/head.html` | `<head>`, before resource loading |
| `injects/head_end.html` | `<head>`, after JS/CSS links |
| `injects/body_start.html` | After `<body>` open |
| `injects/body_end.html` | Before `</body>` close |
| `injects/page_start.html` | Top of article content |
| `injects/page_end.html` | Bottom of article content |
| `injects/header_nav.html` | Inside header navigation |
| `injects/footer.html` | Inside footer |

---

## 6. How This Blog Is Customized

### Customizations in use

| File | What it does | Diff from theme |
|---|---|---|
| `templates/index.html` | Overrides theme's `index.html` — adds a donation CTA section after the blog post list | +12 lines (donation partial + styled `<hr>`) |
| `templates/partials/head.html` | Overrides theme's `head.html` — adds custom SVG favicon links | +3 lines (SVG favicon `<link>` tags) |
| `templates/partials/page_list.html` | Overrides theme's page list — **removed** `truncate` class on description div | Changed `truncate` → no truncation |
| `templates/partials/donation_info.html` | **Custom partial** — a buy-me-a-coffee styled CTA card with multilingual text | Entirely new file |
| `static/icons/favico.svg` | Custom SVG favicon icon | Unique to this blog |
| `config.toml` | Full theme config: author profile, social links, bilingual setup, menus, translations | — |

### Bilingual setup details

- **English posts**: `content/post-name.md`
- **Vietnamese posts**: `content/post-name.vi.md`
- Both languages share `[extra.translations]` in `config.toml` for multilingual UI strings (donation messages)
- Per-language config under `[languages.en]` and `[languages.vi]`
- Each language has its own taxonomy config (categories + tags with feeds)
- Each language has a separate search index (`search_index.js`, `search_index.vi.js`)

### What's in `static/`

```
static/
├── icons/
│   └── favico.svg            # Custom SVG favicon (HgN Lab logo)
├── images/                   # Blog post cover images (39 files)
└── (other theme assets are served from themes/linkita/static/)
```

### Donation integration architecture

The donation CTA at the bottom of the home page:

1. **Template override** (`templates/index.html`): extends `variables.html`, overrides `main` block, calls `donation_info.html` partial
2. **Partial** (`templates/partials/donation_info.html`): renders a styled card with heading, text, and CTA button
3. **i18n** (`config.toml` `[extra.translations]`): English + Vietnamese text for heading, body, and button
4. **Macro call**: uses `m_i18n::tr(key=..., lk=..., d=config.extra.translations)` to look up the correct language string

---

## 7. Workflow & Best Practices

### Development workflow

```bash
# Live-reload dev server (default port 1111)
zola serve

# Dev server with specific interface/port
zola serve --interface 0.0.0.0 --port 8080

# Build for production to public/
zola build

# Check all internal links (run after build)
zola check --skip-external-links

# Build with base URL for production
zola build --base-url https://blog.hgnlab.org
```

### Customization checklist

When making changes, follow this order to minimize maintenance:

1. **`config.toml` first** — exhaust theme configuration options before touching templates
2. **Injects** — for analytics, custom fonts, extra scripts (no template upgrade risk)
3. **Partials** — to change specific components like footer or header
4. **Full templates** — for layout changes (highest maintenance cost)
5. **Custom CSS** — for visual polish via `sass/` or injects
6. **Modify theme source** — only as last resort; maintain a fork if necessary

### Keeping overrides in sync

After updating a theme, check what changed:

```bash
# See what commits the submodule moved through
git submodule update --remote themes/linkita
git diff --submodule themes/linkita

# Compare your overrides with the new theme versions
diff templates/partials/head.html themes/linkita/templates/partials/head.html
diff templates/index.html themes/linkita/templates/index.html
diff templates/partials/page_list.html themes/linkita/templates/partials/page_list.html
```

### Rebuilding Tailwind CSS

If you've customized templates that use Tailwind utility classes, rebuild:

```bash
cd themes/linkita
pnpm install
pnpm tailwindcss -i ./src/app.css -o ../../static/main.min.css --minify
```

The `tailwind.config.js` content paths already include `../../templates/**/*.html`, so site-level templates are scanned for class usage.

### Rebuilding JavaScript

```bash
cd themes/linkita
pnpm install
just build_js
```

This minifies `src/linkita.js`, `src/linkita-search.js`, and `src/gc.js` into `static/js/`.

### Git submodule tips

```bash
# Update all submodules to latest tracked commit
git submodule update --remote --merge

# See what commit each submodule is pinned to
git submodule status

# Clone the repo WITH submodules (for new contributors)
git clone --recurse-submodules <repo-url>

# Add a new submodule
git submodule add <repo-url> themes/<name>

# Change a submodule's remote URL
git submodule set-url themes/linkita <new-url>
```

---

## 8. Troubleshooting

### "Template not found" errors

Zola searches site templates first, then theme templates. If a template is referenced but missing from both, Zola errors.

- Ensure `theme = "linkita"` is set at the **top level** of `config.toml` (not nested under `[extra]`)
- For frontmatter with `template = "archive.html"`, the file must exist in either site or theme

### CSS not reflecting changes

- Hard refresh the browser (`Cmd+Shift+R`)
- Use `cachebust=true` in your `get_url()` calls to bust Zola's cache
- If using Tailwind rebuild, ensure your classes exist in scanned `content` paths
- Check that `compile_sass = true` is set in `config.toml`

### Search not working

- Ensure `build_search_index = true` is in `config.toml`
- The search index is only generated during `zola build`, not `zola serve` (the dev server serves whatever was last built)
- Run `zola build` explicitly after content changes

### JavaScript not loading

```bash
# Rebuild JS bundles
cd themes/linkita
pnpm install
just build_js
```

Check that `disable_javascript` is not set to `true` in `config.toml`.

### Multilingual issues

- Vietnamese files must use the `.vi.md` suffix
- Category names in frontmatter should always be in English (e.g., `categories = ["Technology"]`)
- Language codes in URLs follow from the file suffix and `default_language` setting
- Each language needs its own taxonomies config in `[languages.<code>]`

### Theme update broke something

1. Check the theme's `CHANGELOG.md` for breaking changes
2. Compare your override files with the updated originals using `diff`
3. Check `config.toml` for deprecated config variables
4. Run `zola build` and fix errors one by one
5. If the issue is in a template override, temporarily rename it to confirm the fallback works

### Build performance

If `zola build` is slow:

- Reduce `build_search_index` scope (disable `include_content` or reduce taxonomies)
- Check for very large images in `static/`
- Use `zola serve` for development iteration (incremental rebuilds)

---

## Quick Reference: Useful Zola Commands

```bash
zola init <name>                    # Create new Zola site
zola serve                          # Dev server with live reload
zola serve --drafts                 # Include draft posts
zola build                          # Build static site to public/
zola build --base-url <url>         # Build with custom base URL
zola check                          # Check all internal links
zola check --skip-external-links    # Skip external URL checks
```

---

*Last updated: 2026-06-09 · Zola v0.22.1 · Linkita theme v3.2025_04_22*
