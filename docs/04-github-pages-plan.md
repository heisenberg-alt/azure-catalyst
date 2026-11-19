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

- Repo pushed to `github.com/heisenberg-alt/azure-catalyst` as **private**.
- Site scaffold and deploy workflow are committed and build-verified locally.
- Pages is **not yet enabled** — that is the go-live decision below.

## Go-live steps

1. **Decide visibility.** GitHub Pages on a Free personal account requires a **public** repo (Pro/Team/Enterprise can publish from private repos). Content review: everything here derives from public Microsoft Learn documentation and fictional Contoso data — nothing customer-specific or confidential. When ready:

   ```bash
   gh repo edit heisenberg-alt/azure-catalyst --visibility public --accept-visibility-change-consequences
   ```

2. **Enable Pages with the Actions source.** Either in the UI (Settings → Pages → Source: *GitHub Actions*) or:

   ```bash
   gh api repos/heisenberg-alt/azure-catalyst/pages -X POST -f build_type=workflow
   ```

   The workflow also passes `enablement: true` to `actions/configure-pages`, so a manual run can self-enable Pages once the repo is public.

3. **Run the deployment.**

   ```bash
   gh workflow run pages.yml --repo heisenberg-alt/azure-catalyst
   gh run watch --repo heisenberg-alt/azure-catalyst
   ```

   After this, every push to `main` redeploys automatically.

4. **Verify.** The site lands at:

   ```text
   https://heisenberg-alt.github.io/azure-catalyst/
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
