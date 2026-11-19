# Azure Catalyst Hackathon — "Catalyst Cup: Migrate & Modernize"

A one-day (8-hour) hands-on hackathon where teams act as a migration consultancy winning a fictional customer, **Contoso Retail**, using Azure Catalyst end to end. Scales to 2 days with the stretch challenge.

## Format

- **Audience:** customer engineers / partner architects / internal field. 12–40 people.
- **Teams:** 3–5 people, each team gets its own resource group and its own Catalyst project in a shared subscription (or one subscription per team if available).
- **Scenario:** Contoso Retail has ~15 discovered servers (provided dataset), end-of-support OS risk, a board mandate to exit the datacenter in 12 months, and no business case yet. Teams must take Contoso from raw inventory to a committed, defensible migration plan.

## Prerequisites (organizer, 1–2 weeks ahead)

1. Subscription(s) with quota; one resource group + Azure Migrate built-in roles (or Contributor) per team.
2. **Tenant admin enables Azure Copilot and Catalyst agent access for all participants.** Longest-lead-time item; test with a non-admin account.
3. Distribute the dataset: `assets/demo-inventory.csv` (or a richer 50-server variant you generate from it), plus the official CSV template link.
4. Verify Catalyst project creation works in your chosen geography.
5. Dry-run challenges 1–4 yourself end to end; preview features (agent, wave planning) can shift.
6. Prepare judge briefing + rubric printouts; recruit 2–3 judges (one plays the Contoso CIO).
7. Stretch-challenge teams only: pre-provision one nested Hyper-V host VM in Azure per stretch team with 2 tiny guest VMs, since replication takes hours. Skip entirely if budget or time is tight.

## Challenges

**Challenge 1 — Know the estate (60 min, 15 pts).** Create the Catalyst project, import the inventory via CSV (or the AI inventory-exports import for bonus points), explore and enrich: tag environments (prod/dev), flag end-of-support OS, identify the riskiest servers. Deliverable: inventory summary with 3 risk findings.

**Challenge 2 — Make the CFO say yes (75 min, 20 pts).** Build a multi-strategy business case. Compare lift-and-shift vs PaaS-heavy vs recommended mix; quantify Azure Hybrid Benefit and CapEx→OpEx. Deliverable: 3-slide ROI pitch delivered to the "CIO" judge in 3 minutes.

**Challenge 3 — Prove it will work (75 min, 20 pts).** Create assessments for at least two targets (e.g., Azure VM + AVS or Azure SQL). Surface blockers, remediation steps, right-sized SKUs, monthly cost. Deliverable: readiness report naming every not-ready server and its fix.

**Challenge 4 — Agent mastery (60 min, 20 pts).** Using only the Catalyst agent in Azure Copilot: summarize inventory, interpret the business case, compare 6R strategies for two named workloads, and **generate + download a landing zone Terraform template** for a stated region/compliance requirement. Deliverable: prompt log + the template, with a 2-minute explanation of what the generated architecture contains. Bonus: critique one thing you'd change in the template and why.

**Challenge 5 — Commit the plan (45 min, 15 pts).** Generate a wave plan with the agent, then review/configure waves in the portal. Defend the grouping and sequencing (dependencies, risk, business criticality) to the judges. Deliverable: committed wave plan + sequencing rationale.

**Presentation (10 pts).** 5 minutes per team: the Contoso story arc from unknown estate to committed plan.

**Stretch (2-day format only, +15 pts).** Live replication: discover the nested Hyper-V guests with the appliance, replicate one VM, run a test migration into an isolated VNet, and demo the migrated VM booting in Azure.

## Schedule (1-day)

| Time | Item |
|---|---|
| 08:30 | Arrival, access checks (portal + Copilot agent visible for every participant) |
| 09:00 | Kickoff: Contoso scenario + 20-min Catalyst walkthrough (reuse `02-customer-demo-plan.md` flow) |
| 09:30 | Challenge 1 |
| 10:30 | Challenge 2 |
| 11:45 | Lunch + CIO pitch sessions (Challenge 2 deliverable) |
| 12:30 | Challenge 3 |
| 13:45 | Challenge 4 |
| 14:45 | Challenge 5 |
| 15:30 | Team presentations |
| 16:15 | Judging + awards |
| 16:45 | Retro: what maps to participants' real estates; next-step pilots |

## Judging rubric (100 pts)

- Challenges 1–5 as scored above (90).
- Presentation storytelling (10).
- Tiebreaker: best single agent prompt of the day (judges' pick).

## Risks and mitigations

- **Agent not enabled / throttled (preview):** test with real participant accounts beforehand; keep portal-only fallback paths for Challenges 4–5 (assessment blades + manual wave configuration).
- **Business case/assessment generation latency:** build wait time into Challenge 2; teams start Challenge 3 setup while waiting.
- **Shared-subscription collisions:** strict one-RG-per-team naming (`rg-catalyst-team0X`), projects named `catalyst-team0X`.
- **Rename confusion:** brief participants that "Azure Migrate" strings still appear in places; same service.
- **Cost:** import-based challenges are near-zero cost (Catalyst is free; only trivial storage). Stretch lab is the only material spend — size the nested host small and delete same-day.

## Participant handout checklist

- Azure portal access verified, RG + roles assigned
- Catalyst agent visible in Azure Copilot new-chat menu
- Dataset CSV + official template link
- Docs quick links (see `01-azure-catalyst-overview.md` source list)
