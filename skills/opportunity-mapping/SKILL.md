---
name: opportunity-mapping
description: Maintain a product area's Opportunity Solution Tree and living Opportunity Backlog. Takes candidate opportunities from discovery-synthesis, competitive-scan, metrics-review or a PO, matches them by problem and Key Journey, links them to outcomes, maps solution ideas and assumptions, and moves opportunities toward Ready. Use after a synthesis or scan is approved, or when a PO runs /opportunity-mapping.
---

# Opportunity Mapping

## Purpose

Keep one living Opportunity Backlog per product area. Each opportunity is a user problem linked to an outcome and a Key Journey, with evidence, solution ideas and the assumptions that must hold. The skill decides which assumptions are riskiest and what to test. It moves opportunities from New to Ready. Prioritization takes it from there.

Read `${CLAUDE_PLUGIN_ROOT}/shared/standards/continuous-track-conventions.md` first. IDs, statuses, the candidate table, the evidence scale, the cross-area rule and page locations come from there.

## When it runs

- **After a new input lands.** A synthesis, a market scan or a metrics review is approved.
- **On demand.** A PO runs `/opportunity-mapping`, adds an idea, or asks to work one opportunity deeper.
- **Before a prioritization round.** To make sure Ready items are actually ready.

## Inputs

| Input | Source | Required |
|---|---|---|
| Product area | Ask the PO | Yes |
| Current Opportunity Backlog | Confluence, Discovery Hub > Opportunity Backlog | If it exists |
| Candidate opportunities | Candidate table on the latest approved synthesis, market scan or metrics review | At least one input |
| PO ideas | Pasted by the PO | Optional |
| Outcomes | Strategy page (`OUT-<AREA>-##`) | If it exists |
| Key Journeys and product facts | product-context lookup | If they exist |
| Impact analyses on solution directions | Impact Analyses section | If they exist |

## Default framework

**Opportunity Solution Tree** (Teresa Torres): outcome, then opportunities, then solution ideas, then assumption tests.

Supporting tools:

- **Four Risks** (Cagan) for assumptions: value, usability, feasibility, viability.
- **Assumption mapping** to rank them: importance against evidence.
- **Kano** on request only, to tag a solution as basic, performance or delighter.

## Workflow

### Step 1. Load the current state

Read the Opportunity Backlog page and the Strategy page for the area. If there is no backlog yet, start a new one from the template. If there is no strategy yet, see Step 4.

### Step 2. Check the inputs

Every candidate table must match the format in the conventions file. If a table is missing a column, stop and tell the PO which page is out of date. Do not guess the missing values.

Only take candidates that belong to this area. Themes another area routed here arrive through that area's synthesis and this area's own next synthesis, not directly.

### Step 3. Match candidates to the backlog

For each candidate, look for an existing opportunity that describes the same user problem. Check in this order:

1. **Same journey step.** The candidate and an opportunity sit on the same `JRN-<AREA>-##` step. Strong sign of a match. Confirm the problem is the same, not just the place.
2. **Same journey, different step.** Possible match. Check whether one problem causes the other.
3. **Same problem, no journey.** Compare the problem statements and the modules.

Then:

- **Match.** Add the new sources to the existing opportunity. Update evidence strength and trend. Carry over any new value signal. Do not create a duplicate.
- **New.** No match. Create it with the next `OPP-<AREA>-###` ID and status New.
- **Weak.** Weak candidates stay on the source page's Watch list. They do not enter the backlog unless the PO asks.

Two opportunities on the same journey step with different problems stay separate. Note the shared step on both.

List every merge for the PO to check, with the reason. A wrong merge hides evidence.

### Step 4. Link each opportunity to an outcome

Every opportunity sits under one outcome from the Strategy page.

- If it fits an existing `OUT-<AREA>-##`, link it.
- If it fits none, mark it **Unlinked**. That is a signal for the strategy skill, not an error.
- If there is no Strategy page yet, ask the PO for the working outcome. Record it as **Provisional outcome: <text>**. The strategy skill replaces it later.

Opportunities stay in problem form. If one is really a solution ("build a sync dashboard"), rewrite it as the need behind it and keep the original as a solution idea.

Opportunities carry no cost or effort. Cost belongs to a solution.

### Step 5. Map solution ideas

Only for opportunities the PO wants to explore, or that have Strong evidence. For each:

- Generate or collect at least three distinct solution ideas. Include one small or non-build option (process change, configuration, documentation, partner).
- Note any build, buy or partner angle (Addepar, custodians, CRM vendors).
- Check each idea against product-context. Name the modules and journey steps it would change. Do not state product behavior from general knowledge.
- Do not pick a winner. The PO does.

Set status to Exploring.

### Step 6. Map assumptions

For the solution ideas under consideration, list the assumptions that must be true. Tag each one:

- **Value:** will advisors, ops staff or investors want it and use it?
- **Usability:** can they figure it out?
- **Feasibility:** can engineering build it with our stack, integrations and data?
- **Viability:** does it work for the business, compliance, tenants and support?

Give each assumption an ID (`OPP-<AREA>-###-A#`). Rate it:

- **Importance:** High or Low. If this is wrong, does the solution fail?
- **Evidence:** Strong, Medium or Weak, on the shared scale.

High importance with Weak evidence is the riskiest. Those go first.

### Step 7. Suggest tests

For the top one to three risky assumptions, suggest the cheapest test that would change the decision:

| Risk | Typical tests |
|---|---|
| Value | Advisor interviews (Mom Test), fake-door in portal, ask top tenants |
| Usability | Clickable prototype walkthrough, 5-user usability test |
| Feasibility | product-context lookup, engineering spike, integration vendor docs |
| Viability | impact-analysis on the solution direction, compliance review, tenant config check |

impact-analysis runs on a solution direction, never on a raw candidate or opportunity. Suggest it once the PO has a preferred direction, or two directions to compare.

For each test, state the hypothesis, the signal that would confirm or kill it, and who runs it. Do not run tests. Do not contact clients.

### Step 8. Move status

Recommend status changes. The PO confirms each one.

- **Ready:** value and viability assumptions are Strong, or the PO accepts the risk in writing. The opportunity has an outcome link and at least one preferred solution direction.
- **Parked:** real but not now. Record the reason.
- **Invalidated:** tests or evidence show the problem is not real or not worth solving. Record the evidence.

### Step 9. Apply the Amplify lenses

Carry flags forward from the source. Add any new ones the solution ideas raise. A Compliance flag adds a Viability assumption that compliance review must clear.

Record the areas where users feel the problem, when they differ from this area. That comes from the source page's flags.

### Step 10. Self-check

- Every input table matched the conventions file format.
- No duplicate opportunities. Every merge is listed with its reason.
- Every opportunity has sources, a journey (or None), a value signal (or None found), an outcome link (or Unlinked or Provisional), and a status.
- Every opportunity is phrased as a problem. No cost or effort on any opportunity.
- Every solution idea's product claims cite product-context.
- Every Ready item meets the Ready rule.
- No PII.

### Step 11. Draft, review, publish

1. Update the backlog using `templates/opportunity-backlog.md`. Show the PO a summary of changes: new items, merges with reasons, status changes, unlinked items.
2. On approval, update the Confluence page in place and add a change log line.
3. Optional on request: draw the tree in Miro for a team walkthrough. Confluence stays the source of truth.

## Handoffs

- **To prioritization:** opportunities with status Ready, plus their preferred solution direction, evidence strength, value signal, outcome link, journey and flags.
- **To strategy:** the Unlinked list and any Provisional outcomes.
- **To impact-analysis:** preferred or competing solution directions, when the PO asks for one.
- **From metrics-review:** learnings that validate or invalidate an opportunity come back here.
