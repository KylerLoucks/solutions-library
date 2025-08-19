# Solutions Library

Curated, well-structured solution guides focused on AWS (and related domains), built with MkDocs + Material.

## Live Docs

- GitHub Pages (once enabled): `https://kylerloucks.github.io/solutions-library/`

## Requirements

- Python 3.9+
- mkdocs + plugins (installed via `requirements.txt`)

## Quick Start (Local)

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Open http://127.0.0.1:8000. MkDocs auto-reloads on changes.

To validate a one-off build:

```bash
mkdocs build --strict
```

## Project Structure

```text
docs/
  index.md
  _templates/
    solution-template.md
  aws/
    landing-zone-accelerator/
      _assets/
        diagrams/
        snippets/
      index.md
      prerequisites.md
      architecture.md
      deployment.md
      customization-examples.md
      validation.md
      operations.md
      troubleshooting.md
  networking/
    transit-gateway-hub-spoke/
      _assets/
        diagrams/
        snippets/
      index.md
```

MkDocs config and CI/CD:

- `mkdocs.yml`
- `.github/workflows/deploy.yml`
- `requirements.txt`

## Navigation (Auto-generated with .pages)

This repo uses `mkdocs-awesome-pages-plugin` to avoid manually editing `nav:` in `mkdocs.yml`.

- Root `.pages` controls top-level sections:

```yaml
# docs/.pages
nav:
  - Home: index.md
  - AWS: aws
  - Networking: networking
```

- Section-level `.pages` sets order/titles within a folder:

```yaml
# docs/aws/.pages
title: AWS
nav:
  - Landing Zone Accelerator: landing-zone-accelerator
```

- Solution-level `.pages` defines each page’s order/title:

```yaml
# docs/aws/landing-zone-accelerator/.pages
title: Landing Zone Accelerator
nav:
  - Overview: index.md
  - Prerequisites: prerequisites.md
  - Architecture: architecture.md
  - Deployment: deployment.md
  - Customization Examples: customization-examples.md
  - Validation: validation.md
  - Operations: operations.md
  - Troubleshooting: troubleshooting.md
```

## Adding a New Solution

1. Create a folder under the appropriate domain, e.g. `docs/aws/control-tower-baseline/`.
2. Add your markdown files (follow the template in `docs/_templates/solution-template.md`).
3. Add a `.pages` file in the solution folder to set page order/titles.
4. Create a per-solution `_assets/diagrams/` and `_assets/snippets/` folder next to your pages.

## Assets

- Prefer per-solution asset folders (short paths, portable):
  - Example: `docs/aws/landing-zone-accelerator/_assets/diagrams/arch.svg`
  - Reference from `architecture.md`:
    ```markdown
    ![Architecture](_assets/diagrams/arch.svg)
    ```
- If you keep shared assets, namespace them by solution (longer paths):
  - Example: `docs/_assets/diagrams/aws/landing-zone-accelerator/arch.svg`
  - Reference from `architecture.md`:
    ```markdown
    ![Architecture](../../_assets/diagrams/aws/landing-zone-accelerator/arch.svg)
    ```
- Prefer SVG for diagrams; use PNG/JPG when necessary.
- Don’t use leading slashes in paths so it works on GitHub Pages under `/solutions-library/`.

## Deployment (GitHub Pages)

- Workflow: `.github/workflows/deploy.yml` builds on pushes to `main`.
- One-time setup in GitHub: Settings → Pages → Source: “GitHub Actions”.
- Push to `main`; the site is built and deployed automatically to GitHub Pages.

## Troubleshooting

- If pages don’t appear in the sidebar, ensure the folder has a `.pages` file and entries reference existing files/folders.
- Run `mkdocs build --strict` to surface configuration or link issues.
