# GitHub Pages — Publishing Plan

How all the information in this repo becomes a browsable website, and the exact steps to take it live.

## Architecture

| Layer | Choice | Why |
|---|---|---|
| Content | Markdown in `docs/` | Single source of truth; readable on GitHub even without Pages |
| Site generator | **MkDocs with the Material theme** | Sidebar navigation, full-text search, dark mode, zero HTML to maintain |
| Build & deploy | GitHub Actions (`.github/workflows/pages.yml`) | Builds on every push to `main`, deploys via the official `deploy-pages` action |
| Hosting | GitHub Pages | Free, HTTPS, no servers |

Site structure (defined in `mkdocs.yml`):

| Nav entry | Source file |
|---|---|
| Home | `docs/index.md` |
| Azure Catalyst overview | `docs/01-azure-catalyst-overview.md` |
| Customer demo runbook | `docs/02-customer-demo-plan.md` |
| Hackathon plan | `docs/03-hackathon-plan.md` |
| Publishing guide | `docs/04-github-pages-plan.md` |
| Demo dataset (download) | `docs/assets/demo-inventory.csv` |

## Current state

- Repo: `github.com/heisenberg-alt/azure-catalyst`, **private**.
- GitHub Pages: **live** at https://heisenberg-alt.github.io/azure-catalyst/ (build type: GitHub Actions, HTTPS enforced). The account plan allows Pages from a private repo, so the first push after enabling succeeded without making the repo public.
- Important: the **site is publicly readable** even though the repo is private. Anyone with the URL can view every page and download the dataset. All content derives from public Microsoft Learn docs and fictional Contoso data, so this is acceptable for the current content — keep customer-specific material out of `docs/`.
- Every push to `main` rebuilds and redeploys automatically (about one minute).

## Access options

| Goal | Action |
|---|---|
| Keep as is (private source, public site) | Nothing to do. |
| Restrict the site to people with repo access | Pages access control requires GitHub Enterprise Cloud; if available: Settings → Pages → Visibility → **Private**. Otherwise unpublish (below) and share the repo directly. |
| Open-source the whole kit | `gh repo edit heisenberg-alt/azure-catalyst --visibility public --accept-visibility-change-consequences` — the site URL does not change. |
| Unpublish the site | `gh api -X DELETE repos/heisenberg-alt/azure-catalyst/pages` (the workflow will then fail at the configure-pages step until re-enabled). |

## Manual redeploy

```bash
gh workflow run pages.yml --repo heisenberg-alt/azure-catalyst
gh run watch --repo heisenberg-alt/azure-catalyst
```

## Local preview

```bash
pip install mkdocs-material
mkdocs serve   # http://127.0.0.1:8000, live-reloads on edit
```

## Operating model

- **Edit flow:** change Markdown in `docs/` → PR or push to `main` → site updates in ~1 minute. The build runs `mkdocs build --strict`, so broken internal links fail the build instead of shipping.
- **Hackathon day:** share the Pages URL as the participant handout; the dataset CSV is downloadable straight from the site.
- **Before a customer demo:** re-check the overview page against the "What's new" link — Catalyst is evolving fast and preview features shift.

## Later enhancements (optional)

- Custom domain via a `CNAME` entry in `mkdocs.yml` + DNS.
- Versioned docs with `mike` if the kit forks per-customer.
- `git-revision-date` plugin to stamp pages with last-updated dates.
