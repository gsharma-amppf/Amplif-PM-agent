# Product Agent Conventions

Every skill in the plugin follows this file. Change a convention here, not inside a skill.

## Product areas

Everything is keyed by area, not by team. Teams get reshuffled. Areas do not.

| Area | Code |
|---|---|
| Trading | TR |
| Investment | INV |
| CRM | CRM |
| Integrations (external integrations, platform utility) | INT |
| Client Portal | CP |
| Digital Onboarding | DO |

Which team and PO own each area lives on the Product Agent Config page, not here. A PO who owns two areas appears on two rows there.

### Cross-area problems

Integrations is a supplier area. Its problems often show up as tickets in other areas. A theme or opportunity lands in the area where the cause sits. It is flagged in the area where users feel it. If the cause is not known yet, it lands where users feel it and says the cause area is Unknown.

## Product Agent Config

The area list, team and PO per area, JSM mapping, Product Map sources, competitor list and per-area settings live on one Confluence page outside the plugin: **Product Agent Config**. Skills read it. If it is missing or the area is not on it, ask the PO.

A blank version of that page ships with the plugin at `shared/templates/product-agent-config.md`, in the same plugin root as this file. Offer it to the PO when the config page does not exist yet. Never publish it without PO approval.

## Confluence layout

### Platform level

One page covers the whole platform:

```
Key Journeys                      living page     product-context
```

Key Journeys holds current-state journeys only. Each journey step is tagged with its area. Future-state journeys belong to journey-map in the project track. Where the page lives is set on the config page.

### Per product-area space

```
<Product Area> space
├── Product Map                   parent page     product-context
│   └── <Module>                  living pages    product-context
├── Discovery Hub
│   ├── Feedback Synthesis        dated pages     discovery-synthesis
│   └── Opportunity Backlog       living page     opportunity-mapping
├── Competitive Hub
│   ├── Market Scans              dated pages     competitive-scan
│   └── Competitor Matrix         living page     competitive-scan
├── Strategy                      living page     strategy
├── Prioritization                dated pages     prioritization
├── Roadmap                       living page     roadmap
├── Impact Analyses               dated pages     impact-analysis
└── Metrics
    ├── Post-launch Reviews       dated pages     metrics-review
    └── Area Health               dated pages     metrics-review
```

Dated page titles: `YYYY-MM-DD <Product Area> <Page Type>`. Impact analyses use the subject instead of the area: `YYYY-MM-DD <Subject> Impact Analysis`.
Product Map page titles: `<Product Area> Product Map: <Module>`.
Living pages are updated in place. Each update adds a line to the page's change log section.

## Product facts

- When a skill needs a fact about how the product works (modules, screens, roles, entitlements, data, integrations, business rules, journeys), it gets it through product-context and cites what product-context returns.
- Never state product behavior from general knowledge.
- Confidence labels for product facts: **Confirmed**, **Single source**, **Unknown**.
- If product-context returns Unknown, the skill says Unknown. It does not guess.

## IDs

| Item | Format | Created by |
|---|---|---|
| Journey | `JRN-<AREA>-##` | product-context |
| Opportunity | `OPP-<AREA>-###` | opportunity-mapping |
| Assumption | `OPP-<AREA>-###-A#` | opportunity-mapping |
| Outcome | `OUT-<AREA>-##` | strategy |

IDs never get reused. A merged, parked or retired item keeps its ID.

## Candidate opportunities table

discovery-synthesis, competitive-scan and metrics-review all hand candidates to opportunity-mapping in this one format. Change it here only, then check all four skills.

| # | Opportunity (problem form) | Theme | Strength | Tenants | Flags | Journey | Value signal |
|---|---|---|---|---|---|---|---|
| 1 | | | Strong / Medium | <list or "Market"> | Compliance / Fiduciary / Tenant impact / None | JRN-<AREA>-## or None | <cited signal> or None found |

- **Opportunity** is a user problem, never a solution.
- **Journey** names the Key Journey the problem sits on, from product-context. None if it fits no journey.
- **Value signal** is cited evidence of value: churn risk, support load, escalations, a tenant at risk. Write "None found" when there is none. No estimates.
- Candidates carry no cost or effort. Cost belongs to a solution, not a problem.

## Opportunity status

| Status | Meaning | Set by |
|---|---|---|
| New | Came in from synthesis, scan, metrics or a PO | opportunity-mapping |
| Exploring | Solutions and assumptions being worked | opportunity-mapping |
| Ready | Riskiest assumptions tested or accepted, ready to score | opportunity-mapping |
| Prioritized | Scored and ranked | prioritization |
| On roadmap | Placed in Now, Next or Later | roadmap |
| In project | A project started from it via `/new-project` | new-project (project track) |
| Shipped | Released, awaiting or under metrics review | metrics-review |
| Parked | Not now. Reason recorded | any skill, PO approval |
| Invalidated | Evidence says the problem is not real or not worth solving | any skill, PO approval |

## Evidence strength

Same scale everywhere.

- **Strong:** 3 or more tenants and 5 or more tickets, or a validated test
- **Medium:** 2 tenants, or 3 or more tickets, or a credible market signal
- **Weak:** 1 tenant, 1 to 2 tickets, or opinion only

Market signals (competitor moves) alone never rate above Medium. Observations from an agent walk of the demo platform rate Weak. Regulatory requirements are a separate lane. See prioritization.

## Amplify lenses

Applied as flags on any theme, opportunity, impact or roadmap item:

- **Compliance:** Reg S-P, FINRA, books and records, the Marketing Rule, privacy
- **Fiduciary:** affects advice, suitability or client disclosures
- **Tenant impact:** behaves differently across white-label tenants or entitlements

## Guardrails (all skills)

- Read only in Jira and analytics tools.
- No client PII. Strip names, emails, phone numbers, account numbers and account data. Tenant (firm) names may stay.
- Cite sources for every claim: ticket keys, page links, code doc pages, URLs.
- Draft first. The PO approves before anything is published or updated in Confluence.
- Scheduled runs with no one present: save the draft, notify the PO, never publish.
- Platform walks (product-context build mode): demo or sandbox only, the PO signs in, read only. Never submit, save, delete, send, upload or change a setting.
- Never store passwords or credentials in any page, file or note.
- Every page ends with the audit footer below.

## Audit footer

```
Generated by: Amplify Product Agent, <skill name>
Generated on: <date>
Approved by: <PO name> on <date>
```

## Writing style

Plain, direct language. Short declarative sentences. No em dashes.
