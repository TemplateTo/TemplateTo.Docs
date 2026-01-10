# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the documentation website for TemplateTo, a web-based PDF template builder service. The site is built using MkDocs with the Material theme and is automatically deployed to GitHub Pages.

## Essential Commands

### Local Development
- **Install dependencies**: 
  ```bash
  pip install mkdocs-material mkdocs-glightbox
  ```
- **Run local server**: `mkdocs serve` (serves at http://localhost:8000)
- **Build static site**: `mkdocs build` (outputs to `site/` directory)

### Testing and Validation
- **Check for broken links**: Run local server and manually verify links
- **Validate markdown**: MkDocs will report errors during `mkdocs serve` or `mkdocs build`

## Architecture and Structure

### Documentation Organization
The documentation follows a hierarchical structure:
- **Home** (`docs/index.md`) - Landing page
- **Getting Started** (`docs/getting-started/`) - User guides and tutorials
  - Editor overview, creating templates, settings, themes, elements, variables, user management, API keys
- **Integrations** (`docs/integrations/`) - API and third-party platform integrations
  - REST API, Zapier, N8N

### Key Configuration
- **`mkdocs.yml`** - Defines site structure, theme settings, navigation, and plugins
  - Uses Material theme with custom primary color
  - Enables glightbox plugin for image lightboxes
  - Configures multiple pymdownx markdown extensions for enhanced formatting

### Deployment Pipeline
- **GitHub Actions** (`.github/workflows/ci.yml`) automatically deploys to GitHub Pages on push to main branch
- The workflow installs dependencies and runs `mkdocs gh-deploy --force`

## Development Guidelines

### Adding New Documentation
1. Create markdown files in appropriate subdirectory under `docs/`
2. Update `nav` section in `mkdocs.yml` to include new pages in navigation
3. Place screenshots/images in `docs/images/` directory
4. Use relative paths for internal links and images

### Image Handling
- Store screenshots and visual content in `docs/images/`
- Images automatically get lightbox functionality via glightbox plugin
- Use markdown image syntax: `![alt text](../images/filename.png)`

### Markdown Features
The following enhanced markdown features are available:
- Admonitions for callout boxes
- Code blocks with syntax highlighting and copy button
- Collapsible details sections
- Inline code highlighting
- Content tabs for alternative content

### Custom Styling
- Custom CSS can be added to `docs/stylesheets/extra.css`
- Primary theme color is customized via the Material theme configuration