# AITraining — Setup & Publishing Notes

This document covers how to set up MkDocs locally, add new pages, and deploy to GitHub Pages. It also captures the issues encountered during initial setup so they're easy to avoid next time.

---

## Local Setup

### Prerequisites

- Python 3.10+
- `uv` installed (`brew install uv` or `curl -LsSf https://astral.sh/uv/install.sh | sh`)
- A GitHub account with `gh` CLI installed (`brew install gh`)

### 1. Create the project

```bash
mkdir aitraining && cd aitraining

# Initialize as a uv project (not a Python library, just a docs site)
uv init --no-package

# Add MkDocs and the Material theme
uv add mkdocs mkdocs-material
```

### 2. Create mkdocs.yml

Create `mkdocs.yml` in the project root:

```yaml
site_name: AITraining
site_description: AI-Assisted Development — hands-on curriculum for developers
site_url: https://rvij.github.io/aitraining/

theme:
  name: material
  palette:
    - scheme: default
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    - scheme: slate
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
  features:
    - navigation.top
    - toc.follow
    - search.suggest
    - search.highlight
    - content.code.copy

markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.tasklist:
      custom_checkbox: true
  - tables
  - toc:
      permalink: true

nav:
  - Getting Started: index.md
  - Setup:
    - IDE Setup: setup/ide-setup.md
```

### 3. Create the docs folder and first page

MkDocs expects all content under a `docs/` folder. The home page is always `docs/index.md`.

```bash
mkdir -p docs/setup
echo "# Getting Started" > docs/index.md
```

### 4. Preview locally

```bash
uv run mkdocs serve
```

Opens a live-reloading preview at `http://127.0.0.1:8000`. Any change you save to a `.md` file or `mkdocs.yml` reloads the browser automatically.

---

## Adding New Pages

### 1. Create the markdown file

Pages live under `docs/`. Organize them in subdirectories to match your nav sections:

```
docs/
├── index.md              # home page (required)
├── setup/
│   ├── ide-setup.md
│   └── repo-setup.md
├── ai-dev/
│   └── intro.md
└── writing-code/
    └── review.md
```

```bash
# Example: add a new page under ai-dev
touch docs/ai-dev/new-topic.md
```

### 2. Register it in mkdocs.yml

Every page must be listed under `nav:` or it won't appear in the sidebar:

```yaml
nav:
  - Getting Started: index.md
  - Setup:
    - IDE Setup: setup/ide-setup.md
    - Repository Setup: setup/repo-setup.md
  - AI-Assisted Development:
    - Introduction: ai-dev/intro.md
    - New Topic: ai-dev/new-topic.md   # ← add this line
```

### 3. Write the content

MkDocs uses standard Markdown. With the Material theme and the extensions configured above, you also get:

**Callout boxes:**
```markdown
!!! tip
    This is a tip box.

!!! warning
    This is a warning box.
```

**Checklists:**
```markdown
- [x] Done item
- [ ] Pending item
```

**Code blocks with copy button:**
````markdown
```python
def hello():
    return "world"
```
````

**Tables:**
```markdown
| Column A | Column B |
|----------|----------|
| Value 1  | Value 2  |
```

### 4. Preview and deploy

```bash
# Check it looks right locally
uv run mkdocs serve

# Deploy to GitHub Pages
uv run mkdocs gh-deploy
```

---

## Issues Encountered During Initial Setup

This document captures the issues encountered when setting up and deploying this MkDocs site to GitHub Pages, so they're easy to avoid next time.

---

## 1. You need a GitHub repo before deploying

`mkdocs gh-deploy` pushes the built site to a `gh-pages` branch in whatever git remote is configured for the directory. If the folder has no git remote, the command fails silently or errors out.

**Fix:** Create and push the repo first, then deploy.

```bash
git init
git add .
git commit -m "Initial commit"
gh repo create aitraining --public --source=. --remote=origin --push
uv run mkdocs gh-deploy
```

---

## 2. Site redirected to www.vidushika.com instead of rvij.github.io

After deploying successfully, navigating to `https://rvij.github.io/aitraining/` immediately redirected to `http://www.vidushika.com/aitraining/` with a domain configuration error.

**Root cause:** The `rvij.github.io` repository (the special user-level GitHub Pages repo) had a `CNAME` file containing `www.vidushika.com`. GitHub applies this CNAME to *all* `rvij.github.io/*` URLs, not just the root — so every project page under the account was being redirected.

**Things that looked like the cause but weren't:**
- The Custom Domain field in `github.com/rvij/aitraining/settings/pages` — it appeared empty
- The account-level Pages settings at `github.com/settings/pages` — no domain listed there either
- A `CNAME` file in the `aitraining` repo's `gh-pages` branch — there wasn't one

**Fix:** Delete the `CNAME` file from the `rvij.github.io` repo's default branch and commit. GitHub Pages picks up the change within a few minutes. Browser cache can hold onto the redirect — test in an incognito window or hard-refresh (`⌘⇧R`) to confirm it's resolved.

---

## 3. Navigation appeared as top tabs, not a left sidebar

The deployed site showed section names (Setup, AI-Assisted Development, Writing Code with AI) as horizontal tabs in the top bar rather than a collapsible tree in the left sidebar.

**Root cause:** The `navigation.tabs` feature was enabled in `mkdocs.yml`. This moves top-level nav sections into a tab bar.

**Fix:** Remove `navigation.tabs` from the `features` list in `mkdocs.yml`.

```yaml
# Before
features:
  - navigation.tabs
  - navigation.sections
  - navigation.expand
  - navigation.top

# After
features:
  - navigation.top
```

---

## 4. Left sidebar showed all sections expanded, not collapsible

After removing `navigation.tabs`, the left sidebar appeared but all sections were open and static — nothing collapsed when clicked.

**Root cause:** Two features were responsible:
- `navigation.expand` — forces all sections open on every page load
- `navigation.sections` — renders section headers as non-interactive bold labels instead of collapsible items

**Fix:** Remove both. The default MkDocs Material behavior (no `expand`, no `sections`) collapses sections and expands only the one containing the current page.

---

## 5. uv vs pip

The project was initially set up with a `requirements.txt` and `pip`. To use `uv` instead:

```bash
# Initialize as a uv project (--no-package since this is a docs site, not a library)
uv init --no-package

# Add dependencies (creates pyproject.toml + uv.lock, installs into .venv)
uv add mkdocs mkdocs-material

# Remove requirements.txt
rm requirements.txt

# All mkdocs commands run via uv run
uv run mkdocs serve       # local preview at http://127.0.0.1:8000
uv run mkdocs gh-deploy   # build and push to gh-pages branch
```

---

## Quick Reference

```bash
# Local preview
uv run mkdocs serve

# Deploy to GitHub Pages (rvij.github.io/aitraining)
uv run mkdocs gh-deploy

# Force redeploy (overwrites gh-pages branch cleanly)
uv run mkdocs gh-deploy --force
```

Live site: https://rvij.github.io/aitraining/

---

## Deploying to www.vidushika.com/aitraining

The AITraining docs are also served at `www.vidushika.com/aitraining/`. This works by building the site and copying it as a subfolder into the `rvij/vidushika.com` repo, which is the repo behind `www.vidushika.com`.

### Why this approach

GitHub Pages serves each repo at its own path. To serve the AITraining docs at a subpath of an existing custom domain (`www.vidushika.com/aitraining/`), the built HTML must live as a subfolder inside the repo that owns that domain — there is no native GitHub Pages way to route subpaths across repos.

### Deploy steps

```bash
# 1. Build the AITraining site
cd /Users/rvij/Documents/projects/aiskills/aitraining
uv run mkdocs build

# 2. Copy into the vidushika.com repo (replace previous version)
rm -rf /Users/rvij/Documents/projects/aiskills/vidushika.com/aitraining
cp -r site /Users/rvij/Documents/projects/aiskills/vidushika.com/aitraining

# 3. Commit and push
cd /Users/rvij/Documents/projects/aiskills/vidushika.com
git add aitraining/
git commit -m "Update AITraining docs"
git push
```

### Important: set site_url correctly

`mkdocs.yml` must have the correct `site_url` so internal links and assets resolve properly under the `/aitraining/` subpath:

```yaml
site_url: https://www.vidushika.com/aitraining/
```

Live site: https://www.vidushika.com/aitraining/
