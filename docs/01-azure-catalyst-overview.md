# Azure Catalyst — Information Dossier

*Compiled 2026-11-19 from Microsoft Learn (links at bottom). All facts verified against official docs.*

## What it is

**Azure Catalyst is the new name for Azure Migrate** (announced November 2026). It is Microsoft's free, unified service to **decide on, plan, and execute** migration and modernization to Azure — now positioned as "intelligent, agent-powered modernization across infrastructure, applications, and data" and an accelerator for the path to AI.

Key facts:

- **Free service.** Discovery, assessment, business case, planning, and in-product migration tools cost nothing (partner tools may charge).
- **Nothing breaks.** Existing Azure Migrate projects, appliances, and workflows continue to work unchanged. The portal may still show "Azure Migrate" in places during the transition.
- **Workload coverage:** servers (VMware, Hyper-V, physical, other clouds), databases (SQL Server → Azure SQL DB / MI / SQL on VM), web apps (ASP.NET → App Service, AKS), virtual desktops, and bulk data (Azure Data Box). Also migration *to* Azure Local (VMware GA, Hyper-V preview) and Azure VMware Solution (AVS).
- **"Azure Migrate Classic"** is the old experience; it lacks the application and cross-workload views.

## The three phases

### 1. Decide (Discovery + Business case)

Discovery methods, in order of fidelity:

| Method | What it gives you | When to use |
|---|---|---|
| **Appliance** (recommended) | Continuous config + performance data, software inventory, agentless dependency analysis, web app + SQL discovery | Real engagements |
| **Collector** | Point-in-time snapshot without continuous Azure connectivity | Air-gapped / restricted networks |
| **CSV template import** | Up to 20,000 servers per file/project; business case + assessment only — **imported servers cannot be migrated** | Quick start, demos, no-credential environments |
| **RVTools XLSX import** (preview) | VMware inventory without an appliance | Pre-appliance quick assessment |
| **AI inventory-exports import** (preview) | Upload ServiceNow / Dynatrace / Ansible / CMDB exports as-is (.csv/.xlsx/.json, ≤500 MB, optional context file); AI maps them to the inventory schema, you review with Copilot before saving; covers servers, databases, web apps | Inventory already lives in other tools |

**Business case:** on-prem vs Azure TCO, year-over-year cash flow, utilization insights, CapEx→OpEx. Multi-strategy comparison: IaaS lift-and-shift vs PaaS modernization vs recommended mix, plus "Azure value uplift" (productivity/resiliency beyond infra savings) and Azure Hybrid Benefit modeling.

### 2. Plan (Assessment)

- **Azure readiness** for servers, SQL, web apps; blockers + remediation.
- **Right-sizing** of Azure VMs, SQL config, AVS nodes.
- **Cost estimation** per target.
- **Dependency analysis** (agentless): cross-server dependency visualization (public preview), per-server process/port view, multi-select high-affinity servers and tag them as **application groups**.

### 3. Execute (Migrate)

Two execution paths (new in Nov 2026):

- **Wave migrations** — group applications/workloads into waves for coordinated execution.
- **Direct server migration** — replicate and migrate servers without creating a wave.

Mechanics: agentless VMware migration (same appliance as discovery), agent-based via replication appliance, Hyper-V provider agents, physical/other-cloud via replication appliance, at-scale agentless ASP.NET → App Service. Test migrations to an isolated VNet before cutover.

## Azure Catalyst agent (the headline feature)

Formerly "Azure Copilot Migration Agent," **preview**. A conversational planning agent available via **Azure Copilot** (New chat → Catalyst agent) and inside the Catalyst portal (project → View summary). Planning and analysis only — **execution stays in the portal**.

Capabilities:

1. **Discovery & inventory analysis** — natural-language exploration ("show workloads on end-of-support OS"), inventory enrichment with tags (environment, migration intent).
2. **Application discovery & analysis** (preview) — auto-groups workloads into applications using metadata, software inventory, and network dependencies; classifies custom vs packaged, criticality, complexity, confidence scores.
3. **Business case & ROI analysis** (preview) — summarizes and compares strategies, explains savings drivers, Azure Hybrid Benefit impact.
4. **Migration strategy & readiness** — interprets assessments, evaluates the 6R strategies against your goals/constraints, explains sizing and cost rationale, highlights code readiness when code analysis exists.
5. **Wave planning & execution** (preview) — proposes a wave plan with grouping/sequencing rationale; you review, configure, and commit waves in the portal; during execution it summarizes replication/migration status and helps troubleshoot.
6. **Landing zone generation** — produces an Azure landing zone configuration from your region/compliance/connectivity requirements, with **downloadable deployable templates (e.g., Terraform)**.

**Prerequisites (critical for demos):** Azure subscription with Catalyst permissions **and Azure Copilot enabled for the tenant** — admins control agent access. If the agent doesn't appear in a new chat, an admin must enable it. Full end-to-end support is **VMware-first**; Hyper-V and physical get task-level help.

## November 2026 release highlights

- Azure Migrate → **Azure Catalyst** rename.
- **Wave migrations** + **direct server migrations** as the two execution paths.
- **GA: migration to Azure Confidential VMs** (hardware-enforced boundary, no code changes for many apps).
- Azure Local migrations folded into the unified Catalyst experience; Azure Local migration in **Azure Government** (preview).
- (Oct 2025) Cross-server dependency visualization preview; software-inventory scoping/filtering.

## Setup essentials

- **Roles:** creating a project requires Contributor or Owner on the subscription **and** permission to register Microsoft Entra apps. After that, Azure Migrate built-in roles govern discovery, assessment, and migration — see "Prepare Azure accounts."
- **Project:** created in a supported geography (metadata residency); one project can mix discovery sources.
- **Official CSV template:** https://go.microsoft.com/fwlink/?linkid=2109031 — 27 columns; mandatory ones are starred (`*Server name`, `*Cores`, `*Memory (In MB)`, `*OS name`). OS names must contain a string from the supported list (e.g., "Windows Server 2019", "Red Hat Enterprise Linux 8", "Ubuntu Linux"). Up to 20,000 servers per file. Imported servers can be assessed but not migrated.

## Source links

- Overview: https://learn.microsoft.com/azure/migrate/migrate-services-overview
- What's new: https://learn.microsoft.com/azure/migrate/whats-new
- Catalyst agent: https://learn.microsoft.com/azure/migrate/azure-copilot-migration-agent
- Agent VMware walkthrough: https://learn.microsoft.com/azure/migrate/how-to-plan-analyze-migration-with-agent
- Copilot side of the agent: https://learn.microsoft.com/azure/copilot/migration-agent
- Discovery methods: https://learn.microsoft.com/azure/migrate/discovery-methods-modes
- CSV import tutorial: https://learn.microsoft.com/azure/migrate/tutorial-discover-import
- RVTools import tutorial: https://learn.microsoft.com/azure/migrate/tutorial-import-vmware-using-rvtools-xlsx
- AI inventory-exports import: https://learn.microsoft.com/azure/migrate/how-to-import-inventory-exports-file-import
- Wave migrations: https://learn.microsoft.com/azure/migrate/wave-migrations-overview
- Business case concepts: https://learn.microsoft.com/azure/migrate/concepts-business-case-calculation
- Prepare accounts/roles: https://learn.microsoft.com/azure/migrate/prepare-azure-accounts
- Azure Local migration: https://learn.microsoft.com/azure/azure-local/migrate/migration-azure-migrate-vmware-overview
