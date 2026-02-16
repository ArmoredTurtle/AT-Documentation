# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

MkDocs documentation site for the Armored Turtle project (3D printer filament control systems). Built with MkDocs and Material for MkDocs theme. Content is Markdown in `docs/`, with `AFC-Klipper-Add-On` pulled in as a git submodule.

### Subdirectory Overview for docs/

- `afc-klipper-add-on/` - Documentation for the AFC Klipper Add-On plugin, generated from the plugin's Python source code using mkdocstrings.
- `afc-accessories/` - Documentation for Armored Turtle accessories (filament guides, mounts, etc.)
- `boxturtle/` - Documentation for the Boxturtle filament control system
- `troubleshooting/` - Common issues and solutions for Armored Turtle products

## Key Characteristics

Documentation projects differ from code projects in several ways:
- **Content focus**: Text, diagrams, and media rather than code
- **Review emphasis**: Editorial review alongside technical review
- **Accessibility**: Documentation must be clear for target audience
- **Structure**: Organizational consistency is critical
- **Versioning**: May track software versions or standalone revisions
- **Formats**: Markdown, RST, AsciiDoc, or custom formats
- **Tooling**: Static site generators, doc frameworks, or wikis


## Development Commands

### Setup
```bash
uv venv && source .venv/bin/activate
uv sync
git submodule init && git pull --recurse-submodules && git submodule update --recursive --remote
```

Or with mise: `mise install` handles venv and dependencies automatically.

### Common Tasks
| Command | Description |
|---------|-------------|
| `mkdocs serve` | Local dev server with live reload |
| `mkdocs build` | Build static site to `site/` |
| `ruff check .` | Lint Python code |

Mise equivalents: `mise serve`, `mise lint`, `mise install`

## Architecture

- **`docs/`** - All documentation content (Markdown). Organized by project: `afc-klipper-add-on/`, `boxturtle/`, `afc-accessories/`, `troubleshooting/`
- **`AFC-Klipper-Add-On/`** - Git submodule (main branch of the AFC plugin repo). Must be initialized before building. Used by mkdocstrings to generate Python API docs from `extras/`
- **`mkdocs.yml`** - Site configuration: navigation structure, theme settings, plugins, markdown extensions
- **`mkdoc_hooks.py`** - Custom MkDocs hook that strips `cmd_` prefix from output HTML (normalizes command names from the Python source)
- **`overrides/`** - MkDocs Material theme overrides (custom icons, templates)

## Key Details

- Python >= 3.11 required (targets 3.13 via `.python-version`)
- Package manager: `uv` with `pyproject.toml` and `uv.lock`
- Navigation is explicitly defined in `mkdocs.yml` `nav:` section (not auto-discovered)
- `use_directory_urls: False` in mkdocs config - pages are `page.html` not `page/index.html`
- The mkdocstrings plugin generates API docs from Python source in `AFC-Klipper-Add-On/extras/`

## Branch Workflow

- PRs go against `DEV` branch
- Only hot-fixes for the current deployed version go against `main`

## Common Tasks


### Development Common

| Command | Description |
|---------|-------------|
| `mkdocs serve` | Local dev server with live reload |
| `mkdocs build` | Build static site to `site/` |
| `ruff check .` | Lint Python code |

Mise equivalents: `mise serve`, `mise lint`, `mise install`

### Quality Assurance
- Check for broken links
- Verify code examples work correctly
- Ensure consistent formatting
- Review for grammar and spelling
- Test documentation builds
- Validate technical accuracy
- Check accessibility compliance

## Documentation Standards

### Style and Consistency
- Follow project style guide (voice, tone, terminology)
- Use consistent formatting and structure
- Apply appropriate heading hierarchy
- Use clear, concise language
- Define technical terms
- Maintain consistent examples
- Follow naming conventions

### Technical Accuracy
- Verify all technical information
- Test code examples and commands
- Update for version changes
- Cross-check with source code
- Validate API references
- Ensure example outputs are current

### Organization
- Use logical content hierarchy
- Maintain clear navigation
- Provide appropriate cross-references
- Include table of contents where needed
- Group related topics together
- Use consistent file and folder naming

## Common Documentation Tools

### Formats
- Markdown (most common)

### Diagramming
- Mermaid (diagrams as code)

### Build Tools
- MkDocs (Markdown documentation)
