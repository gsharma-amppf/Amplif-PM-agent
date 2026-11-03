---
name: strategy
description: Write, refresh or critique a product area's strategy. Produces a one-page vision, a Rumelt strategy kernel, a North Star with input metrics, the outcomes (OUT IDs) that opportunities and priorities link to, cross-area dependencies, and the questions the next discovery round should answer. Use for the quarterly strategy refresh, when company OKRs change, when a PO asks to test a strategy doc, or when a PO runs /strategy.
---

# Strategy

## Purpose

Give each product area one clear strategy page. It says where the area is going, why, what it will and won't do, and which outcomes count. Those outcomes (`OUT-<AREA>-##`) sit at the top of the Opportunity Solution Tree and drive strategic fit in prioritization.

The strategy also points discovery. Each refresh ends with the questions the next round of discovery should answer. That closes the loop between the two.

The skill has two modes:

- **Write or refresh.** Build or update the Strategy page.
- **Critique.** Test any strategy doc against the kernel and report gaps. No rewrite unless asked.

Read `${CLAUDE_PLUGIN_ROOT}/shared/standards/continuous-track-conventions.md` first. Area codes, IDs, the cross-area rule, the evidence scale and guardrails come from there.

## When it runs

- **Quarterly cadence.** Refresh per product area, ahead of the quarterly prioritization and roadmap round. Usually through plan-quarter.
- **Event.** Company OKRs change, a major competitor move, a big client shift, a reorg.
- **On demand.** `/strategy`, or `/strategy critique` with a doc attached or linked.

## Inputs

| Input | Source | Current if newer than | Required |
|---|---|---|---|
| Product area | Ask the PO | | Yes |
| Company OKRs and goals | Confluence, or pasted by the PO | | Yes. Ask if not found. |
| Current Strategy page | product-area space > Strategy | | If it exists |
| Opportunity Backlog, with Journey, Value signal, Unlinked and Provisional lists | opportunity-mapping | Updated since the last synthesis | If it exists |
| Feedback syntheses since the last refresh | discovery-synthesis | 2 weeks for the latest | If they exist |
| Market scans and Competitor Matrix | competitive-scan | 30 days | If they exist |
| Area Health and post-launch reviews | metrics-review | 30 days | If they exist |
| Impact analyses since the last refresh | impact-analysis | | If they exist |
| Product facts and Key Journeys | product-context lookup | Product Map: 1 quarter | Yes |
| Other areas' Strategy pages | Their product-area spaces | | For the cross-area check |
| PO and leadership input | Interview the PO | | Yes |

List each input with its date at the top of the draft. Mark any that are stale. Do not block on a stale input. Say what it means for confidence.

## Default frameworks

- **Product Vision Board** (Pichler): vision, target group, needs, product, business goals. One page.
- **Rumelt strategy kernel:** diagnosis, guiding policy, coherent actions. This is the test for whether the page is a strategy or a wish list.
- **North Star metric with input metrics.** Outcomes come from the input metrics.
- **OKR alignment.** Every outcome traces to a company objective.

On request or when the situation calls for it:

- **Build / buy / partner** for any big bet that overlaps a vendor or partner (Addepar, custodians, CRMs).
- **TAM / SAM / SOM** when the area is entering a new segment or persona.

## Workflow: write or refresh

### Step 1. Gather the evidence

Read every input that exists. Summarize what changed since the last refresh in five bullets or fewer: user problems, market, regulation, metrics, company goals.

Then look at where the evidence clusters:

- **By journey.** Which Key Journeys and steps carry the most opportunities and the strongest evidence, from the backlog's Journey column. A cluster at a cross-area handoff is worth naming.
- **By value signal.** Which opportunities carry cited value signals (churn risk, escalations, tenant at risk).
- **By outcome.** Which current outcomes have strong opportunities behind them, and which have none.

### Step 2. Interview the PO

Ask, in this order, and stop when you have enough:

1. Who is the area for, and who is it not for?
2. What is the single biggest obstacle to the area's success right now?
3. Which company objectives does this area serve?
4. What are you sure you won't do this year?
5. What would make this quarter a clear win?

Show the PO the evidence clusters from Step 1 before question 2. Do not fill gaps with your own guesses. Mark them **Open** for the PO.

### Step 3. Draft the Vision Board

One sentence of vision. Then target group, top needs (from the backlog evidence, with OPP IDs and journeys), product, business goals.

The Product line states what the area is and what makes it different. Every claim about what Amplify does today comes from product-context, cited. Differentiation claims cite the Competitor Matrix.

### Step 4. Draft the kernel

- **Diagnosis.** What is really going on. Name the critical obstacle. Back it with evidence from syntheses, scans, metrics, impact analyses and the journey clusters. Cite them.
- **Guiding policy.** The overall approach to the obstacle. It must rule things out.
- **Coherent actions.** Three to five actions that follow from the policy and support each other. Each one names the opportunities (`OPP-<AREA>-###`) or bets it covers.
- **What we won't do.** Explicit list. A policy that rules nothing out is not a policy.

### Step 5. Set the North Star and outcomes

- **North Star:** one metric that captures value delivered to users in this area.
- **Input metrics:** two to four levers that move the North Star.
- **Outcomes:** one per input metric, written as a change in behavior or result. Assign each an ID `OUT-<AREA>-##`. For each, record the metric, the baseline (or "Baseline needed"), the target, the timeframe and the company objective it serves.

On a refresh, keep existing OUT IDs when the outcome still stands. Retire outcomes that no longer apply. Never reuse a retired ID.

### Step 6. Check across areas

Journeys cross areas and Integrations supplies the others, so no area's strategy stands alone. Read the Strategy pages of the areas this area shares Key Journeys with, and of Integrations.

- **Dependencies.** Coherent actions or outcomes here that need work in another area. Name the area, the work and the journey step.
- **Conflicts.** Anything here that another area's won't-do list rules out, or the reverse.
- **Shared handoffs.** Journey handoffs both areas touch. Note who owns which side.

List these for the PO. Do not change another area's page. The PO raises them with the other area's PO.

### Step 7. Resolve the backlog links

- Map each **Provisional outcome** in the backlog to a real OUT ID, or flag it.
- For each **Unlinked** opportunity, decide with the PO: it reveals a missing outcome, it belongs to another area, or it should be Parked.
- An opportunity that belongs to another area moves under the cross-area rule. The other area creates it under its own code. This area's entry is Parked with the reason "Moved to <new ID>". IDs are never changed or reused.
- List the changes so opportunity-mapping can apply them.

### Step 8. Big bets (if any)

For any bet that is large, uncertain or vendor-adjacent, add a short build / buy / partner note: options, strategic control, risks, and any cost or time signals with their source. Use impact analyses for what each option touches. Note when a partner on the Competitor Matrix is also a competitor.

Size the market with TAM / SAM / SOM only when the bet opens a new segment. Show the math and sources. Mark estimates as estimates.

### Step 9. Apply the Amplify lenses

Call out compliance, fiduciary and tenant implications of the guiding policy and the big bets.

### Step 10. Write questions for discovery

Write two to four questions the next round of discovery should answer. Each one comes from a gap in this strategy: an Open item, a weak spot in the diagnosis, an outcome with no baseline, a bet that rests on Weak evidence, or a journey with little evidence either way.

For each question, name the lens best placed to answer it: customer (discovery-synthesis, interviews), market (competitive-scan), product (product-context, Key Journeys) or metrics (metrics-review). These are prompts for the PO's next discovery runs, not tasks the agent runs on its own.

### Step 11. Run the critique on your own draft

Use the checklist below. Fix what fails. List what the PO must decide.

### Step 12. Draft, review, publish

1. Fill in `templates/strategy-page.md`.
2. Show the PO the kernel, outcomes and cross-area check first. Those are the parts that drive everything else.
3. On approval, update the Strategy page in place and add a change log line.
4. Tell the PO which other area POs need to see the cross-area items.

## Workflow: critique

Run the checklist against the doc. Return a short report: what passes, what fails, and the one or two changes that would help most. Quote the doc's own words when pointing out a problem. Do not rewrite the doc unless asked.

## Critique checklist

- Is there a diagnosis that names one critical obstacle, backed by evidence?
- Does the guiding policy rule anything out?
- Do the actions follow from the policy and support each other?
- Is every action tied to opportunities or bets, not just themes?
- Is there an explicit "won't do" list?
- Is there one North Star with input metrics?
- Does every outcome have a metric, baseline, target and timeframe?
- Does every outcome trace to a company objective?
- Are goals ("grow 20%") presented as strategy? That is a fail.
- Are claims about the product cited to product-context, and claims about competitors cited to the Competitor Matrix?
- Are cross-area dependencies and conflicts named?
- Are there questions for the next discovery round?

## Handoffs

- **To opportunity-mapping:** OUT IDs, Provisional-to-OUT mappings, Unlinked decisions and moves to other areas.
- **To prioritization:** outcomes and their weight for strategic fit.
- **To roadmap:** coherent actions, the won't-do list and cross-area dependencies.
- **To metrics-review:** North Star, input metrics and baselines to track.
- **To discovery (synthesis, scan, product-context):** the questions for discovery.
- **To other areas' POs:** cross-area dependencies and conflicts, raised by the PO.
