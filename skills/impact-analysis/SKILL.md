---
name: impact-analysis
description: Work out what a proposed solution direction, an incident or an outside change will affect across the Amplify platform: areas, Key Journeys, modules, roles, tenants, integrations, jobs, data, compliance and support. Lists the engineering surface but never estimates effort. Use before committing to a solution direction, when a bug or sync issue is escalated, when a vendor, partner or custodian changes something, or when a PO runs /impact-analysis.
---

# Impact Analysis

## Purpose

Show everything a change touches, how badly, and how sure we are. Use it before a solution direction is chosen, at project kickoff, and when something breaks.

This skill maps impact. It does not fix anything, estimate effort or confirm root cause. Engineering does those.

Read `${CLAUDE_PLUGIN_ROOT}/shared/standards/continuous-track-conventions.md` first. Area codes, the cross-area rule, IDs, confidence labels and guardrails come from there. All product facts come through product-context.

## What it runs on

| Type | What it is | Typical source |
|---|---|---|
| Proposed change | A solution direction for an opportunity, or a project at kickoff | opportunity-mapping, the PO, the project track |
| Incident | A bug, data issue or sync problem that has been escalated | JSM tickets, the PO |
| External change | A custodian, vendor or partner changes a format, API, schedule or term | competitive-scan Impact analysis candidates, a vendor notice, the PO |

It never runs on a raw candidate or an opportunity with no solution direction. A problem has no impact surface until there is a change to trace. If asked, say so and send the item back to opportunity-mapping to pick a direction first.

It can compare two or three solution directions for the same opportunity side by side, when the PO asks.

## When it runs

- **Proposed change.** When opportunity-mapping has a preferred direction, or two to compare. At project kickoff.
- **Incident.** When a bug, data issue or sync problem is escalated.
- **External change.** When a partner or custodian change lands, often from a market scan.
- **On demand.** `/impact-analysis`.

## Inputs

| Input | Source | Required |
|---|---|---|
| The change, incident or notice | PO description, ticket keys, vendor notice, market scan entry | Yes |
| Area where the change sits | Ask the PO. Unknown is allowed for incidents. | Yes |
| Product facts: modules, screens, roles, integrations, jobs, data objects | product-context lookup | Yes |
| Key Journeys | product-context lookup | If they exist |
| JSM tickets | Atlassian connector, read only, across every area the incident may touch | Incidents: yes |
| Related opportunity, solution direction or PRD | Opportunity Backlog, project space | Proposed change: yes |

## Guardrails

- Read only in Jira. Never contact tenants, vendors or partners.
- No PII. Tenant names may stay.
- A cause stays **Suspected** until engineering confirms it.
- Every impact cites a source and carries a confidence label.
- No effort, cost or time estimates. List the engineering surface. Engineering sizes it.
- No product behavior from general knowledge. If product-context returns Unknown, the impact is Unknown.

## Workflow

### Step 1. Define the change

One paragraph. What changes, what triggers it, and what is out of scope. Classify it: Proposed change, Incident or External change.

- **Proposed change:** name the opportunity (`OPP-<AREA>-###`) and the solution direction. If comparing directions, name each one.
- **Incident:** record what is observed, since when, and where.
- **External change:** record the company, its role (partner, custodian, vendor), the effective date and the notice or source.

### Step 2. Find what it touches

Ask product-context to trace:

- **Upstream:** what feeds the changed area (data sources, syncs, user input)
- **Directly:** the modules, screens, data objects and jobs that change
- **Downstream:** what reads from it (reports, exports, client-facing views, other modules, integrations)

Note the area code, roles and tenants for each.

### Step 3. Trace the journeys

Ask product-context which Key Journeys run through the touched modules. For each journey touched:

- The steps affected, and what changes for the user at each one
- Whether an affected step is a cross-area **handoff**. Handoffs are where a change in one area breaks something in another.
- The roles on those steps

If the Key Journeys page does not exist yet, write "Key Journeys not built" and trace by module only.

### Step 4. List the engineering surface

From product-context, list what engineering would touch:

- Modules
- Integrations, and the direction data flows
- Scheduled jobs and syncs
- Data objects

Cite the code doc page for each. Mark Unknown where the code docs don't cover it. This is a list, not an estimate. Do not size it, rank it by effort, or guess hours or points.

### Step 5. Rate each impact

| Severity | Meaning |
|---|---|
| High | Wrong data shown to advisors or clients, a blocked workflow, or compliance exposure |
| Medium | A slowed workflow with a workaround |
| Low | Cosmetic, or internal only |

Confidence uses the product-context labels: Confirmed, Single source, Unknown. Never rate an Unknown impact as Low to make the list shorter.

### Step 6. Areas, tenants and entitlements

- List every area the change touches. Mark which area the change sits in, and which areas only feel it. Integrations changes often sit in INT and are felt elsewhere.
- State whether the impact hits all tenants, some, or one. Name the setups or entitlements that make the difference.

### Step 7. Apply the Amplify lenses

- **Compliance:** books and records, data shown to clients, the Marketing Rule, privacy
- **Fiduciary:** performance, holdings or advice data that clients or advisors rely on
- **Tenant impact:** different behavior across white-label tenants

A Compliance or Fiduciary flag on a High impact goes to the top of the summary.

### Step 8. Incidents only: timeline and blast radius

- First seen, tickets, tenants reporting, areas reporting, trend since
- Symptoms kept separate from cause
- Suspected cause, with the evidence for and against

Cite ticket keys. Tickets often arrive under the area where users feel the problem, not where the cause sits. Search every area the trace in Step 2 reached.

### Step 9. Compare directions (when asked)

For two or three solution directions on the same opportunity, set them side by side: areas touched, journeys and handoffs touched, engineering surface, highest severity, compliance and fiduciary flags, and Unknowns. Do not pick one. Do not compare effort. The PO decides in opportunity-mapping.

### Step 10. Support and operations

Expected ticket types, internal and tenant communication needs, and any training or documentation changes.

### Step 11. Unknowns and checks

Every Unknown gets a check: the question, and who answers it (engineering, compliance, vendor, PO).

### Step 12. Self-check

- The change is a solution direction, an incident or an external change. Not a raw candidate.
- Every impact has a source, an area code and a confidence label.
- Every journey touched lists its affected steps, and every affected handoff is marked.
- The engineering surface cites code docs or says Unknown.
- No effort, cost or time estimate anywhere.
- No Unknown written as fact or rated Low.
- No cause stated as confirmed without engineering.
- No PII.

### Step 13. Draft, review, publish

1. Fill in `templates/impact-analysis.md`.
2. Show the PO the High impacts, the handoffs touched and the Unknowns first.
3. On approval, publish to the Impact Analyses section of the area where the change sits, as `YYYY-MM-DD <Subject> Impact Analysis`. If that area is Unknown, publish in the area that raised it and say so.
4. Tell the PO which other area POs should see it. Link it from the project space when one exists, and from the opportunity in the backlog for a proposed change.

## Handoffs

- **To opportunity-mapping:** evidence for feasibility and viability assumptions, and the comparison of directions.
- **To prioritization:** risk flags, and must-do candidates when the impact is compliance or production-critical, with the source.
- **To the project track:** areas, journeys, engineering surface, dependencies and risks for the PRD (prd-writer, when it ships).
- **To discovery-synthesis:** incident patterns worth tracking as themes. Synthesis reads impact analyses published since its last run.
- **To product-context:** Unknowns that the Product Map, Key Journeys or code docs should cover.
