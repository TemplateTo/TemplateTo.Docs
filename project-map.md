# TemplateTo.Docs — Project Map

MkDocs Material documentation site for TemplateTo. Covers template design, API integration, and account management. Deployed to [docs.templateto.com](https://docs.templateto.com) via GitHub Pages.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Generator | MkDocs |
| Theme | Material for MkDocs |
| Plugins | search, glightbox |
| Hosting | GitHub Pages (custom domain) |
| CI/CD | GitHub Actions |

## Directory Structure

```
TemplateTo.Docs/
├── mkdocs.yml                      # Site config, navigation, theme, extensions
├── .github/workflows/ci.yml        # Deploy on push to main
└── docs/
    ├── index.md                    # Landing page
    ├── CNAME                       # docs.templateto.com
    ├── assets/                     # Logos and branding (5 files)
    ├── stylesheets/extra.css       # Custom branding (primary: #0B0B5F)
    ├── images/                     # Screenshots and diagrams (73 files)
    ├── getting-started/            # 3 pages
    ├── template-guide/             # 21 pages
    │   ├── creating/               # First template, elements, settings
    │   ├── dynamic-content/        # Variables, Liquid syntax, filters, themes
    │   │   ├── functions/          # repeat(), content blocks, QR/barcodes, utilities
    │   │   └── recipes/            # Invoice, iteration, conditional examples
    │   └── output-formats/         # PDF, image, plain text
    ├── developer-guide/            # 8 pages
    │   └── no-code/                # Zapier, N8N integration guides
    └── account/                    # 3 pages — users, API keys
```

## Navigation Structure (42 pages)

| Section | Pages | Audience |
|---------|-------|----------|
| Getting Started | 3 | All users — quick start, how templates work |
| Template Builder Guide | 21 | Designers — editor, elements, dynamic content, functions, recipes, output formats |
| Developer Guide | 8 | Developers — REST API, auth, async rendering, code builder, Zapier/N8N |
| Account & Settings | 3 | Admins — user management, API keys |

## Markdown Extensions (9 enabled)

- `admonition` — callout boxes (note, warning, tip)
- `pymdownx.details` — collapsible sections
- `pymdownx.highlight` + `pymdownx.inlinehilite` — syntax highlighting with line numbers
- `pymdownx.superfences` — fenced code blocks with Mermaid diagram support
- `pymdownx.emoji` — Twemoji
- `pymdownx.snippets` — code snippet inclusion
- `attr_list` + `md_in_html` — HTML attributes and Markdown-in-HTML

## Theme Features

- Instant navigation with prefetch
- Sticky tabs and section navigation
- Code copy buttons and annotations
- Full-text search with suggestions and highlighting
- Back-to-top button, sticky TOC

## Deployment

GitHub Actions on push to `main`:
1. Setup Python 3.x with weekly dependency cache
2. Install `mkdocs-material` and `mkdocs-glightbox`
3. `mkdocs gh-deploy --force` to GitHub Pages

## Local Development

```bash
pip install mkdocs-material mkdocs-glightbox
mkdocs serve        # Dev server on port 8000
mkdocs build        # Build to site/
```

---

[Back to root project map](../project-map.md)
