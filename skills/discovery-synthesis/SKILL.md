---
name: discovery-synthesis
description: Turn customer feedback from JSM tickets (plus any notes the PO provides) into evidence-backed themes, problem statements and candidate opportunities for one product area. Tags each theme with its module and Key Journey, and routes themes to the area where the cause sits. Use for the weekly feedback synthesis, a ticket spike on a module, or when a PO runs /discovery-synthesis.
---

# Discovery Synthesis

## Purpose

Turn raw customer feedback into a short list of user problems, each backed by counted evidence and tied to where it happens in the product. The output feeds the opportunity backlog. The next skill, opportunity-mapping, picks up from here.

This skill finds problems. It does not design solutions.

Read `${CLAUDE_PLUGIN_ROOT}/shared/standards/continuous-track-conventions.md` first. The area codes, cross-area rule, evidence scale, candidate table, lenses, guardrails and page locations come from there. If this file and the conventions file differ, the conventions file wins.

## When it runs

- **Weekly cadence.** Scheduled run per product area. Default window is the last 7 days.
- **Event.** A spike in tickets on one module, or a large client escalation. Scope is the module or tenant involved.
- **On demand.** A PO or the CPO runs `/discovery-synthesis`.

## Inputs

| Input | Source | Required |
|---|---|---|
| Product area | Ask the PO. Codes and JSM mapping are on the Product Agent Config page. | Yes |
| Time window | Default 7 days for weekly runs. Ask for other runs. | Yes |
| JSM tickets | Atlassian connector, read only | Yes |
| Module list, JSM mapping per module, Key Journeys | product-context lookup | If they exist |
| Previous synthesis for this area | Confluence, latest page under Feedback Synthesis | If one exists |
| Themes routed to this area | Other areas' latest syntheses, "Routed to other areas" section | If any |
| Incident patterns | Impact analyses for this area since the last synthesis | If any |
| Walk observations | Product Map and Key Journeys pages, through product-context | If any |
| Interview notes, CS or sales notes, other docs | Pasted or uploaded by the PO | Optional |

## Guardrails

- **Read only in Jira.** Never create, edit, transition or comment on a JSM ticket.
- **No client PII.** Remove names, emails, phone numbers, account numbers and any account data before analysis. Never copy them into the output. Tenant (firm) names may stay, because tenant impact matters.
- **Cite everything.** Every theme lists the ticket keys or note references behind it. No uncited claims.
- **Problems, not solutions.** Feature requests get reframed as the underlying need.
- **Product facts come from product-context.** Module, journey and cause area tags cite what product-context returns. If it returns Unknown, write Unknown.
- **Humans approve writes.** Nothing goes to Confluence until the PO approves the draft.

## Workflow

### Step 1. Set scope

Confirm with the PO before pulling anything:

- Product area
- Time window
- Run type: weekly, event, or on demand
- Any extra notes to include

For scheduled runs with no one present, use the defaults and state them at the top of the draft.

### Step 2. Load product context

Ask product-context for the area's module list, the JSM mapping per module, and the Key Journeys that pass through the area. If the Product Map or Key Journeys page does not exist yet, carry on. Tag modules and journeys as Unknown and say so in the Notes for the PO.

### Step 3. Pull the evidence

Query JSM for tickets in the product area and window. Use the area's JSM mapping from the config page. Example:

```
project = <JSM project key> AND <mapping field> = "<mapping value>" AND created >= -7d ORDER BY created DESC
```

Pull these fields: key, summary, description, request type, components, labels, priority, organization (tenant), status, created.

Exclude tickets carrying the test or spam label set in JSM. Count them under Noise.

Save the raw pull to a working file before analysis. Process large pulls in batches of about 50 so nothing is dropped. Record the total count. Every ticket must be accounted for in Step 10.

Then add the other inputs that exist: themes routed to this area by other areas, incident patterns from impact analyses, walk observations, and PO notes. Keep each one tagged with its source type: Ticket, Routed theme, Incident, Walk observation, or PO note.

### Step 4. Clean and anonymize

- Strip PII from every ticket and note.
- Merge duplicates (same tenant, same issue, same week). Keep all keys on the merged record.
- Tag each ticket with one type, using the JSM request type where it is set:
  - Bug
  - Usability or how-to
  - Feature request
  - Data or integration issue (Addepar, Wealthbox, custodians, etc.)
  - Access or entitlement
  - Noise (password reset with no product cause, spam, test tickets)
- Tag each ticket with its module, from the JSM mapping per module. Unknown if no mapping fits.

Keep how-to tickets. A repeated how-to question is a usability signal.

### Step 5. Extract observations

Write one plain observation per ticket or note. Use this shape:

> [Persona] tried to [task] and [what happened]. Source: [key]

Apply The Mom Test:

- Facts about past behavior and real events count as evidence.
- Opinions, predictions and "it would be nice if" count as weak evidence.
- A feature request becomes the need behind it. "Add a CSV export" becomes "Advisor needs holdings data outside the portal for a client review."

### Step 6. Cluster into themes

Group observations by the user problem they share (affinity mapping). Then for each theme:

- **Name it as a problem.** "Advisors can't confirm a sync finished," not "Sync status indicator."
- **Count the evidence.** Tickets, distinct tenants, personas affected. Count tickets and tenants only from tickets, routed themes and notes that name a tenant. Walk observations and PO notes with no tenant add context, not count.
- **Rate evidence strength** on the shared scale. Walk observations alone rate Weak.
- **Note severity.** Highest ticket priority, and whether a workflow was blocked or only slowed.
- **Note the trend** against the previous synthesis: New, Growing, Steady or Fading. Skip if there is no previous page.

A theme seen at only one tenant may be a configuration issue, not a product gap. Say so.

### Step 7. Tag module, journey and cause area

For each theme:

- **Module.** The module or modules the tickets point to.
- **Journey.** The Key Journey the problem sits on, and the step if it is clear (`JRN-<AREA>-##`, step <n>). Ask product-context to confirm. None if it fits no journey. A theme at a cross-area handoff step says so. Handoffs are where cross-area problems usually sit.
- **Cause area.** Where the cause sits, from product-context and the ticket content. It is often a different area from where users feel it, most of all for Integrations. Use Unknown if the evidence does not show it.

Then apply the cross-area rule from the conventions file:

- **Cause sits in this area, or is Unknown:** the theme stays here. If the cause is Unknown, say so on the theme.
- **Cause sits in another area:** the theme moves to the "Routed to other areas" section, with its evidence and the cause area code. It stays visible here as a flag, because users in this area feel it. It does not go in this area's candidate table. The cause area's next synthesis picks it up.

Cause area is a product fact, not a root cause. A suspected cause stays Suspected until engineering confirms it.

### Step 8. Apply the Amplify lenses

Flag a theme when any of these apply:

- **Compliance.** Touches Reg S-P, FINRA, books and records, the Marketing Rule or privacy.
- **Fiduciary.** Affects advice, suitability or client disclosures.
- **Tenant impact.** Behaves differently across white-label tenants or entitlements.

### Step 9. Write problem statements and candidates

For each Strong or Medium theme that stays in this area, write one JTBD problem statement:

> When [situation], [persona] wants to [motivation], so they can [outcome]. Today they [current pain or workaround].

Record the **value signal**: cited evidence of value, such as churn risk, support load, escalations, or a tenant at risk. Write "None found" when there is none. Do not estimate numbers the evidence does not support.

Then fill the candidate opportunities table, using the format in the conventions file. Keep each candidate in problem form. Candidates carry no cost or effort. Weak themes go to the Watch list, not the table.

### Step 10. Self-check

Before showing the PO anything:

- Every ticket from Step 3 is in a theme, the Watch list, Routed to other areas, or Noise. The counts add up to the total.
- Every theme cites its sources and its source types.
- Evidence counts are recounted, not estimated.
- Every theme has a module, journey and cause area tag, or Unknown.
- The candidate table matches the conventions file format exactly.
- Every value signal is cited, or reads "None found".
- No PII anywhere in the draft.
- No solutions written as themes or candidates.

### Step 11. Draft, review, publish

1. Fill in `templates/synthesis-report.md`.
2. Show the PO the draft. Ask for corrections, merges or splits. Ask the PO to confirm each routed theme.
3. On approval, publish to Confluence:
   - Space: the product area's space
   - Parent page: Discovery Hub > Feedback Synthesis
   - Title: `YYYY-MM-DD <Product Area> Feedback Synthesis`
4. Fill in the audit footer with the approver's name and date.
5. If any themes were routed, tell the PO which area POs to let know.

For scheduled runs with no one present, save the draft and notify the PO. Do not publish.

## Handoffs

- **To opportunity-mapping:** the candidate opportunities table. Its format lives in the conventions file. Do not change it here.
- **To other areas' discovery-synthesis:** the "Routed to other areas" section. The cause area reads it on its next run.
- **To product-context:** module or journey tags that came back Unknown, and ticket clusters that suggest the Product Map or Key Journeys are out of date.
