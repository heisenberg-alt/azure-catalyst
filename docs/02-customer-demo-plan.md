# Azure Catalyst — Customer Demo Runbook

Target: 45-minute customer demo telling the story *"from unknown estate to committed migration plan in one tool, with an AI agent doing the heavy lifting."*

## Demo strategy

Two options. **Option A is the recommended default** — it needs no on-prem lab and shows every headline feature.

| | Option A: Import-based (recommended) | Option B: Live replication lab |
|---|---|---|
| Setup effort | ~2 hours, 1–2 days ahead | 1–2 days (nested Hyper-V host in Azure, quota, replication pre-seeded) |
| Shows | Discovery, inventory, business case, assessment, Catalyst agent, wave plan, landing zone | Everything in A **plus** live replication + test migration/cutover |
| Limitation | CSV-imported servers **cannot be migrated** — execution is shown in the portal UI/recording, not performed | Cost, fragility, replication timing risk |

If the customer insists on seeing a live cutover, do Option A and append a 10-minute pre-recorded or pre-replicated cutover from Option B.

## Prep checklist (start 2+ business days before)

1. **Tenant admin: enable Azure Copilot** and Catalyst agent access. This is the #1 demo blocker — the agent simply won't appear in a new chat without it. Verify in the portal yourself, don't take someone's word.
2. Subscription + resource group for the demo identity. Creating a Catalyst project needs **Contributor or Owner on the subscription plus permission to register Microsoft Entra apps**; day-to-day work can then use the Azure Migrate built-in roles. The Entra app-registration permission is often locked down in enterprise tenants — check it early.
3. Create the **Catalyst project** in a supported geography matching the customer's region story.
4. Use [`assets/demo-inventory.csv`](assets/demo-inventory.csv) (15 servers modeling "Contoso Retail": web tier, two SQL servers, a Windows Server 2012 R2 straggler, Linux app servers, a physical file server). It uses the exact 27-column header of the [official CSV template](https://go.microsoft.com/fwlink/?linkid=2109031), supported OS name strings, and includes CPU/memory utilization, disk, and network data so the business case right-sizes instead of sizing as-is. Before the demo, re-download the official template and diff the header row in case Microsoft changes it.
5. **Import the CSV the day before** and generate the business case and assessments ahead of time. Generation takes minutes and feels like dead air live; pre-bake, then re-run one assessment live if you want motion.
6. Dry-run every Catalyst agent prompt you plan to use (it's preview; responses vary). Save screenshots of each step as fallback.
7. Optional wow-closer: pre-generate the landing zone Terraform download so you can open the files instantly.
8. Check the portal for rename inconsistencies — some blades may still say "Azure Migrate." Prepare the one-liner: *"You'll see both names during the transition; it's the same service, nothing you built changes."*

## 45-minute script

**0–3 min — Frame the problem.** Customer pain: unknown estate, end-of-support OS risk, no credible Azure business case, migration projects that stall in spreadsheets. Promise: one free service, one project, from discovery to executed waves.

**3–8 min — Discovery without friction.** Show the project → Start discovery options. Talking points: appliance for continuous discovery and dependencies, Collector for air-gapped sites, and the demo path — *"your CMDB export is enough to start today."* Mention the AI inventory-exports import (preview): ServiceNow/Dynatrace exports uploaded as-is, AI maps them, you review before saving. Show the already-imported 15-server inventory; filter by OS to surface the 2012 R2 box.

**8–15 min — Business case (the CFO slide).** Open the pre-baked business case. Walk: on-prem TCO vs Azure, YoY cash flow, CapEx→OpEx, utilization insights. Show the **multi-strategy comparison** — lift-and-shift vs PaaS-heavy vs recommended mix — and Azure Hybrid Benefit impact. This is where economic buyers lean in.

**15–22 min — Assessment (the architect slide).** Open the pre-baked Azure VM assessment: readiness breakdown, blockers with remediation, right-sized SKUs, monthly cost. If relevant, flash the AVS or SQL assessment for a second target. Mention dependency visualization and app grouping as what the appliance adds in the real engagement.

**22–35 min — Catalyst agent (the wow).** Open Azure Copilot → New chat → **Catalyst agent** → pick the project. Run 4–5 rehearsed prompts, e.g.:
- "Summarize my discovered inventory."
- "Show workloads running end-of-support operating systems."
- "Summarize my business case. Which strategy gives the best ROI?"
- "List the not-ready servers and their blockers."
- "Generate a wave plan for this assessment. How are applications grouped and sequenced?"
- "Generate a landing zone for West Europe with hub-spoke networking" → download the **Terraform template** and open it. *"The agent didn't just advise — it handed us deployable infrastructure."*

**35–42 min — Execution story.** Show the Server replication / wave configuration blades. Explain the two paths (waves vs direct), test migration into an isolated VNet, minimal-downtime cutover, and the Nov-2026 Confidential VM target for sensitive workloads. If you prepared Option B, show the live or recorded cutover here.

**42–45 min — Close.** Recap: estate understood, business case built, plan committed — all free, all in one project. Propose next step: a 2-week discovery pilot with the appliance on their real estate, or the hackathon (see the [hackathon plan](03-hackathon-plan.md)) to get their team hands-on.

## Known gotchas

- Catalyst agent is **preview** and VMware-first; keep prompts close to the rehearsed set.
- CSV-imported servers: no dependency data, no migration — set expectations if asked.
- Business case utilization charts classify imported servers without performance data as "unknown" and size them as-is with no right-sizing. The bundled dataset already includes utilization columns; keep them if you extend it.
- Imported servers also need a server type and hypervisor, or the business case marks virtualization as "Not applicable" and omits virtualization software cost from the on-premises total.
- Agent access is tenant-admin-gated; re-verify the morning of the demo.
