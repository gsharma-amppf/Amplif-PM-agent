---
name: competitive-scan
description: Scan competitors, partners and regulators for moves that affect Amplify. Tracks Orion, Envestnet, AssetMark, Black Diamond, Addepar, Altruist, Advyzon and Nitrogen, plus SEC, FINRA, privacy and custodian changes, and turns findings into backlog implications for each product area. Use for the monthly scan, when a competitor launches something, when a regulatory or custodian change lands, or when a PO runs /competitive-scan.
---

# Competitive Scan

## Purpose

Tell each product area what changed in the market and in regulation, and what it means for the backlog. Keep a living Competitor Matrix so anyone can see where Amplify stands on the capabilities that matter.

This skill is the market lens of discovery. It finds problems and gaps. It does not design solutions.

Read `${CLAUDE_PLUGIN_ROOT}/shared/standards/continuous-track-conventions.md` first. Area codes, the cross-area rule, the evidence scale, the candidate table, lenses and guardrails come from there.

## When it runs

- **Monthly cadence.** Window is since the last scan. A scan older than 30 days is stale for quarterly planning.
- **Event.** A competitor launch or acquisition, a partner API or data change, a new SEC or FINRA rule, notice or exam priority, a custodian change.
- **On demand.** `/competitive-scan`, optionally naming one area, one competitor or one capability.

## Scope: one area or the whole platform

- **One area.** Search for what matters to that area. Publish one Market Scan page for it.
- **All areas.** Search each competitor and regulator once, across the platform. Tag every finding with the area codes it touches. Then publish one Market Scan page per area that has findings, each holding only its own findings. This avoids running the same searches six times.

Monthly scheduled runs use all areas by default.

## Inputs

| Input | Source | Required |
|---|---|---|
| Product area, or all areas | Ask the PO. Default for scheduled runs: all areas. | Yes |
| Competitor list and roles | Product Agent Config page | Yes |
| Capability rows per area | Competitor Matrix page for each area | If it exists |
| Amplify's own capabilities | product-context lookup | Yes, for matrix cells and parity calls |
| Key Journeys | product-context lookup | If they exist |
| Last scan date | Latest Market Scans page per area | If it exists |
| Current Opportunity Backlog | Opportunity Backlog per area | If it exists, for matching |
| Specific question or event | PO | Optional |

**Competitor list.** The config page sets it. The current list is Orion, Envestnet, AssetMark, Black Diamond (SS&C), Addepar, Altruist, Advyzon and Nitrogen. Add others when the PO names them.

**Addepar is both a competitor and an integration partner.** Label every Addepar finding with its role:

- **Competitor:** Addepar ships something that competes with an Amplify capability.
- **Partner:** Addepar changes something Amplify depends on: an API, a data format, a sync schedule, a partnership term.

The same label applies to any other company the config page marks as both.

## Guardrails

- Public sources only. No logins, no gated reports, no content a PO pasted from a paid source unless they confirm the license allows it.
- Never contact competitors or partners, or pose as a customer.
- Summarize in your own words and link the source. Do not copy press releases or articles.
- Separate fact from inference. "Orion announced X" is a fact. "Orion will target our tenants" is an inference and must be labeled.
- Market signals never rate above Medium evidence. A competitor having a feature is not proof our users need it.
- Amplify's side of any comparison comes from product-context, never from general knowledge or marketing pages. If product-context returns Unknown, the matrix cell reads Unknown.
- Regulatory findings are not legal advice. They flag work for compliance to confirm.

## Workflow

### Step 1. Set scope

Confirm area or all areas, competitors, window and any specific question. For a scheduled run, use the defaults and state them at the top.

### Step 2. Search each competitor separately

One competitor at a time. Combined searches give shallow results. For each, look for:

- Release notes and product update pages
- Press releases and newsroom posts
- Partnership and integration announcements (custodians, CRMs, data vendors)
- Developer and API change notes, for companies marked as partners
- Acquisitions
- Conference launches (T3, Future Proof, the competitor's own events)
- Pricing or packaging changes when public

Keep only items inside the window. Tag each with the area codes it touches. Drop items that touch no area.

### Step 3. Search regulation and industry changes

Check for new or changed items that touch any area in scope:

- SEC rules, proposals, risk alerts and exam priorities
- FINRA notices and rule changes
- Privacy changes (Reg S-P, state privacy laws)
- Custodian or industry changes that force work (data formats, account transitions)

For each, note the effective date, any compliance deadline, and the area codes it touches.

### Step 4. Verify

- Confirm each finding against a primary source when one exists (the company's own page, the regulator's site).
- Drop anything you cannot verify, or mark it **Unconfirmed** with the reason.
- Note the date of each source.

### Step 5. Classify each finding

| Class | Meaning |
|---|---|
| Parity gap | Competitor now does something we don't, and it matters to our users |
| Differentiator at risk | Competitor caught up on something we lead on |
| Opportunity | A gap in the market, or a competitor weakness we can use |
| Regulatory work | A rule change that creates product work |
| Partner change | A partner changed something Amplify depends on |
| Watch | Relevant but no action yet |
| No impact | Logged for completeness |

Parity gap and Differentiator at risk need Amplify's current state. Ask product-context. If it returns Unknown, classify as Watch and list the question for the PO.

Add the Amplify lenses as flags.

### Step 6. Apply the cross-area rule

A finding lands in the area where the work would sit. It is flagged in the areas where users would feel it. Partner changes to an integration usually land in Integrations (INT) and are flagged in the areas that read the data.

### Step 7. Update the Competitor Matrix

For each capability row in each area in scope, mark each competitor and Amplify:

- **Yes:** available and verified
- **Partial:** limited, beta, or only for some segments
- **No:** not available as far as public sources show
- **Unknown:** no evidence either way

Competitor cells cite a public source and date. Amplify cells cite the product-context answer. Change a cell only with a new source.

### Step 8. Write implications

For each Parity gap, Differentiator at risk, Opportunity, Regulatory work and Partner change finding:

- What it means for Amplify in one or two sentences
- Whether it matches an existing opportunity (`OPP-<AREA>-###`) or is new
- For new ones, write a candidate opportunity in problem form. Ask what user problem the competitor is solving. Do not just copy their feature.
- The Key Journey it touches, from product-context, or None
- The value signal: cited evidence of value, such as a tenant asking about the competitor's feature or a lost deal named by the PO. Write "None found" when there is none. A competitor having the feature is not a value signal.

Fill the candidate opportunities table using the format in the conventions file. Tenants reads "Market". Strength is Medium at most. Candidates carry no cost or effort.

Regulatory work with a deadline goes in the Must-do candidates section. Prioritization treats it as a must-do lane.

Partner changes and custodian changes go in the Impact analysis candidates section. They need an impact analysis (External change), not a backlog entry, unless they reveal a user problem.

### Step 9. Self-check

- Every finding has a source link, a date and area codes.
- Every Addepar finding, and any other dual-role company, is labeled Competitor or Partner.
- Facts and inferences are labeled.
- Every Amplify matrix cell and parity call cites product-context, or reads Unknown.
- No copied text beyond a short quote.
- No market signal rated above Medium.
- The candidate table matches the conventions file format exactly.
- Candidate opportunities are in problem form.

### Step 10. Draft, review, publish

1. Fill in `templates/market-scan.md` for each area in scope. Update each area's matrix using `templates/competitor-matrix.md`.
2. Show the PO the draft with the top three implications first. For an all-areas run, show one summary across areas, then each area's page.
3. On approval, publish each scan to that area's Competitive Hub > Market Scans as `YYYY-MM-DD <Product Area> Market Scan`. Update each Competitor Matrix in place with a change log line.
4. For an all-areas run, tell the running PO which area POs have findings to review. Each area PO approves their own area's page.

For scheduled runs with no one present, save the drafts and notify the POs. Do not publish.

## Handoffs

- **To opportunity-mapping:** the candidate opportunities table. Its format lives in the conventions file.
- **To prioritization:** Must-do candidates, regulatory work with deadlines, sourced.
- **To impact-analysis:** Partner changes and custodian changes, as External change candidates.
- **To strategy:** the Competitor Matrix and any Differentiator at risk findings, for the quarterly refresh.
- **To product-context:** Amplify capability questions that came back Unknown.
