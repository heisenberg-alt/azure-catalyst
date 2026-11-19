# Azure Catalyst — Demo & Hackathon Kit

Research dossier, customer demo runbook, and hackathon plan for **Azure Catalyst** (formerly Azure Migrate, renamed November 2026) — Microsoft's free service to discover, assess, plan, and execute migration and modernization to Azure.

📖 **Docs site:** https://heisenberg-alt.github.io/azure-catalyst/ — rebuilt on every push to `main` (see the [publishing guide](docs/04-github-pages-plan.md)).

## Contents

| Document | Purpose |
|---|---|
| [Overview](docs/01-azure-catalyst-overview.md) | What Azure Catalyst is: lifecycle, Catalyst agent, Nov 2026 highlights, Learn links |
| [Customer demo runbook](docs/02-customer-demo-plan.md) | Scripted 45-minute demo via CSV import, prep checklist, agent prompts, gotchas |
| [Hackathon plan](docs/03-hackathon-plan.md) | "Catalyst Cup": 1-day, 5-challenge event with schedule, rubric, and logistics |
| [Demo dataset](docs/assets/demo-inventory.csv) | 15-server Contoso Retail inventory for the CSV import path |
| [Publishing guide](docs/04-github-pages-plan.md) | How the GitHub Pages site is built and how to take it live |

## Preview the site locally

```bash
pip install mkdocs-material
mkdocs serve
```

All product facts are sourced from official Microsoft Learn documentation (linked throughout). Contoso data is fictional.
