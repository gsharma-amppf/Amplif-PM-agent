# Amplify Product Agent

One PM agent for the whole Amplify product team. Every PO and the CPO runs the same version, so product work looks the same across areas.

Version 0.2.0 ships the continuous track, a shared product-context layer, impact analysis, and a quarterly planning flow. The rest of the project track (`/new-project` through release) comes in 0.3.0.

## Product areas

The agent works by area, not by team. Six areas: Trading (TR), Investment (INV), CRM (CRM), Integrations (INT), Client Portal (CP) and Digital Onboarding (DO). Which team and PO own each area is set on the Product Agent Config page.

## What it does

### Product context

**product-context** is the agent's knowledge of the platform. It keeps a Product Map per area (built by walking the demo platform and reading the code docs) and one platform-level Key Journeys page. It answers product questions with citations. Every other skill gets product facts from it instead of guessing.

### The continuous track

The continuous track is a loop. Each skill hands its output to the next one.

1. **discovery-synthesis** reads JSM tickets and PO notes and turns them into problem themes with counted evidence.
2. **competitive-scan** checks competitors and regulators and turns what changed into backlog implications.
3. **opportunity-mapping** keeps the living Opportunity Backlog. It links problems to outcomes, maps solution ideas and assumptions, and moves items toward Ready.
4. **strategy** writes or critiques the area strategy and owns the outcomes (OUT IDs) everything else links to.
5. **prioritization** scores Ready items with RICE, keeps must-do work in its own lane, and records the PO's calls.
6. **roadmap** turns the ranked list into Now / Next / Later against real capacity, with a change log.
7. **metrics-review** checks whether shipped work moved its outcome and runs the monthly Area Health check. What it learns goes back into the backlog.

**plan-quarter** runs steps 4 to 6 in order for the quarterly round, with a readiness check first and a PO approval gate between each step.

### Impact analysis

**impact-analysis** maps what a proposed change, an incident or an outside change (custodian, vendor) touches: modules, journeys, roles, tenants, integrations, compliance and support. It lists the engineering surface but never estimates effort.

Each skill drafts. The PO approves. Only then does anything get published to Confluence.

## Skills and when to run them

| Skill | Run it | Cadence |
|---|---|---|
| `/product-context` or `/product-context build` | To answer a product question, or build or refresh an area's Product Map and the Key Journeys page | Build once, refresh after major releases and each quarter |
| `/discovery-synthesis` | To synthesize recent customer feedback for an area | Weekly |
| `/competitive-scan` | To check competitor and regulatory moves | Monthly |
| `/opportunity-mapping` | After a synthesis, scan or review is approved | As inputs land |
| `/plan-quarter` | To run strategy, prioritization and roadmap in order | Quarterly |
| `/strategy` or `/strategy critique` | To write, refresh or test an area strategy | Quarterly, or on its own |
| `/prioritization` | To score and rank Ready items | Quarterly, or on its own |
| `/roadmap` or `/roadmap view <audience>` | To place ranked work into Now, Next, Later | Quarterly, or on its own |
| `/impact-analysis` | On a solution direction, at kickoff, or when something breaks | As needed |
| `/metrics-review <project>` or `/metrics-review health` | Post-launch at Day 30, 60, 90. Area Health monthly. | Per release, monthly |

You can also just describe what you want ("synthesize last week's Client Portal tickets") and the right skill picks it up. In Claude Code, plugin skills carry a prefix, for example `/amplify-product-agent:roadmap`.

## Setup

### 1. Connect the tools

| Tool | Used for | Access |
|---|---|---|
| Atlassian (Jira Service Management and Confluence) | Reading tickets. Reading and publishing pages. Reading code docs. | Jira read only. Confluence read and write. |
| Claude in Chrome | product-context build mode (walking the demo platform) | PO signs in. Read only. |
| Analytics tool or a PO export | metrics-review | Read only |
| Web search | competitive-scan | Public sources only |
| Miro (optional) | Drawing the opportunity tree on request | Write on request |

The plugin declares the Atlassian connector. Each user signs in with their own Atlassian account, so the agent only sees what that person can already see.

For JSM, each PO needs Browse Projects on the JSM project. Customer portal access is not enough. For a hard read-only guarantee, set the Jira write tools to Blocked in the connector's tool permissions.

### 2. Create the Product Agent Config page

Every skill reads one Confluence page called **Product Agent Config**. It holds the product areas with their codes, teams and POs, the JSM mapping, the Key Journeys page location, the Product Map sources (code docs location, demo environment, demo roles), the competitor list, and per-area settings.

A blank version ships with the plugin at `shared/templates/product-agent-config.md`. Create the page once, fill it in, and change values there. Do not edit the plugin to change a setting. Never put credentials on it.

### 3. Set up Confluence

One platform-level page, **Key Journeys**, at the location set on the config page.

Each product area space needs this layout. The skills create pages under it.

```
<Product Area> space
├── Product Map
├── Discovery Hub
│   ├── Feedback Synthesis
│   └── Opportunity Backlog
├── Competitive Hub
│   ├── Market Scans
│   └── Competitor Matrix
├── Strategy
├── Prioritization
├── Roadmap
├── Impact Analyses
└── Metrics
    ├── Post-launch Reviews
    └── Area Health
```

### 4. Build the Product Map

Run `/product-context build` once per area, with the PO signed in to the demo platform. Use one demo account per role. Do this before the first synthesis or strategy run, so the other skills have product facts to cite.

### 5. Schedule the cadence runs (optional)

Set up scheduled tasks per area for the weekly synthesis, monthly scan and monthly Area Health. Scheduled runs never publish. They save a draft and notify the PO. Product Map builds and quarterly planning need the PO present and are not scheduled.

## Guardrails

- Humans approve every write. Nothing is published until the PO says so.
- Read only in Jira and analytics tools. The agent never edits, transitions or comments on a ticket.
- Platform walks are demo or sandbox only, read only, and the PO signs in. The agent never types or stores a password.
- No client PII. Names, emails, phone numbers, account numbers and account data are stripped. Tenant (firm) names stay, because tenant impact matters.
- Every claim cites its source: ticket keys, page links, code doc pages or URLs. Product facts carry a confidence label.
- Every page ends with an audit footer: which skill generated it, when, and who approved it.

Hooks only run in Claude Code. POs use both the Claude app and Claude Code, so approval inside each skill is the real guardrail.

## Standards

All shared rules live in `shared/standards/continuous-track-conventions.md`: product areas, Confluence layout, product facts rules, ID formats, the candidate opportunities table, opportunity statuses, the evidence scale, the Amplify lenses (Compliance, Fiduciary, Tenant impact), guardrails and the audit footer. It applies to every skill. Change a convention there, then check every skill.

## What's in the plugin

```
amplify-product-agent/
├── .claude-plugin/plugin.json
├── .mcp.json                         Atlassian connector
├── README.md
├── shared/
│   ├── standards/continuous-track-conventions.md
│   └── templates/product-agent-config.md
└── skills/
    ├── product-context/              SKILL.md, templates/product-map.md, key-journeys.md
    ├── discovery-synthesis/          SKILL.md, templates/synthesis-report.md
    ├── opportunity-mapping/          SKILL.md, templates/opportunity-backlog.md
    ├── competitive-scan/             SKILL.md, templates/market-scan.md, competitor-matrix.md
    ├── strategy/                     SKILL.md, templates/strategy-page.md
    ├── prioritization/               SKILL.md, templates/prioritization-record.md
    ├── roadmap/                      SKILL.md, templates/roadmap-page.md
    ├── plan-quarter/                 SKILL.md
    ├── impact-analysis/              SKILL.md, templates/impact-analysis.md
    └── metrics-review/               SKILL.md, templates/post-launch-review.md, area-health.md
```

## Not in this version

- Project-track skills: new-project, prd-writer, journey-map, prototype-setup, story-breakdown, release-notes, uat-scripts.
- Review skills: prd-review, story-review.

The roadmap skill already defines the handoff the project track will use: Now items carry their OPP ID, evidence, problem statement and outcome into `/new-project`.
