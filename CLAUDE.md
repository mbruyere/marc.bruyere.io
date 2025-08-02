# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About This Repository

This is Marc Bruyere's academic website built with Hugo and the Wowchemy Academic theme. It's a static site showcasing research publications, projects, and professional information in the field of computer networking.

## Development Commands

### Building and Serving
- `hugo` - Build the static site
- `hugo server` - Start development server with live reload
- `hugo --gc --minify` - Build with garbage collection and minification for production

### Deployment
The site is deployed to Netlify with automatic builds from git commits. The Netlify configuration is in `netlify.toml`.

## Architecture and Structure

### Content Organization
- `content/authors/admin/` - Main author profile (Marc Bruyere)
- `content/publication/` - Academic publications (each in separate folder with `index.md` and `cite.bib`)
- `content/project/` - Research projects
- `content/post/` - Blog posts
- `content/event/` - Events and talks

### Configuration
- `config/_default/` - Hugo configuration files
  - `config.yaml` - Main site configuration
  - `params.yaml` - Theme parameters and site settings
  - `menus.yaml` - Navigation menus
  - `languages.yaml` - Language settings

### Content Management
- Publications use BibTeX format (`cite.bib` files) and markdown (`index.md`)
- Images and assets are stored in respective content folders
- Static files (like PDFs) go in `static/uploads/`

### Theme and Styling
- Uses Wowchemy v5 theme via Hugo modules
- Theme configuration in `config/_default/params.yaml`
- Custom styling possible through theme parameters

### Hugo Modules
The site uses Hugo modules for theme management:
- `github.com/wowchemy/wowchemy-hugo-themes/modules/wowchemy/v5`
- `github.com/wowchemy/wowchemy-hugo-themes/modules/wowchemy-plugin-netlify`
- `github.com/wowchemy/wowchemy-hugo-themes/modules/wowchemy-plugin-reveal`

### Publication Workflow
Each publication follows a standard structure:
1. Create folder under `content/publication/`
2. Add `index.md` with frontmatter (title, authors, abstract, etc.)
3. Add `cite.bib` with BibTeX citation
4. Optional: Add `featured.jpg/png` for publication image

## Key Files to Understand
- `go.mod` - Hugo module dependencies
- `netlify.toml` - Deployment configuration
- `config/_default/config.yaml` - Core Hugo settings
- `content/authors/admin/_index.md` - Main author profile