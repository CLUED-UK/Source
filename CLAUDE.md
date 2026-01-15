# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Source is the default theme for Ghost (a publishing platform). It uses Handlebars templating with PostCSS for styling and vanilla JavaScript for interactivity.

## Common Commands

```bash
yarn install          # Install dependencies
yarn dev              # Run dev server with live reload (watches CSS/JS changes)
yarn zip              # Create distributable .zip file in dist/
yarn test             # Validate theme against Ghost standards (gscan)
yarn test:ci          # CI testing with verbose output
```

## Architecture

### Template Hierarchy

All templates extend `default.hbs` via `{{!< default}}` syntax:
- `default.hbs` - Parent template with global header/footer/viewport
- `home.hbs` - Homepage with configurable hero header
- `index.hbs` - Post archive/listing page
- `post.hbs` - Individual post view
- `page.hbs` - Static pages
- `tag.hbs` / `author.hbs` - Archive pages

**Custom templates**: Create `page-{slug}.hbs`, `tag-{slug}.hbs`, or `author-{slug}.hbs` for one-off designs.

### Partials Organization

```
/partials/
  /components/     # Major UI components (header, footer, navigation, post-list, post-card)
  /icons/          # SVG icons as HBS partials - use via {{> "icons/name"}}
  /typography/     # Font loading variants
```

Key components:
- `header-content.hbs` - Multi-layout hero section (Landing/Highlight/Magazine/Search)
- `post-list.hbs` - Feed rendering with list/grid styles
- `post-card.hbs` - Reusable post preview card

### Assets

```
/assets/
  /css/screen.css      # Source CSS (PostCSS) - edit this
  /js/                 # Source JS modules (main, dropdown, lightbox, pagination)
  /js/lib/             # Third-party (PhotoSwipe, imagesLoaded, reframe)
  /built/              # Compiled output (auto-generated, don't edit)
```

### Build Pipeline

Gulp handles:
- CSS: PostCSS → Autoprefixer → CSSNano (minification) → `/assets/built/screen.css`
- JS: Concatenate → Uglify → `/assets/built/source.js`
- Source maps generated for both

## Theme Customization

Settings in `package.json` under `config.custom` are accessible in templates via `@custom`:
- `navigation_layout` - Logo position (middle/left/stacked)
- `header_style` - Landing, Highlight, Magazine, Search, Off
- `post_feed_style` - List or Grid
- `site_background_color` - Hex color with auto text contrast
- `title_font` / `body_font` - Typography options

## Key Patterns

- CSS variables defined in `:root` of `screen.css` for theming
- Responsive design uses `clamp()` for fluid spacing
- Ghost Handlebars helpers: `{{#foreach}}`, `{{#is}}`, `{{#has}}`, `{{content}}`
- Template conditionals check `@custom` settings for layout switching
