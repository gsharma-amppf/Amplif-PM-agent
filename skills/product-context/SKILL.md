---
name: product-context
description: Look up how the Amplify platform works, and build or refresh the Product Map and the platform Key Journeys page that hold those facts. Reads the Product Map, Key Journeys, code documentation and Confluence pages, reconciles them and flags conflicts. Use when any skill or PO needs facts about modules, screens, roles, entitlements, data, integrations or journeys, or when a PO runs /product-context, /product-context build or /product-context build journeys.
---

# Product Context

## Purpose

Give every skill one place to get product facts: which modules exist, what each screen does, who uses it, what it depends on, what rules sit behind it, and how users move across areas to get a job done. Without this, skills guess. With it, they cite.

The skill has three modes:

- **Lookup.** Answer a product question from the sources, with citations. Every other skill uses this mode when it needs a fact about how the product works.
- **Build or refresh: Product Map.** Create or update the Product Map for one area. Walk the demo platform for the UI layer. Read the code documentation for the logic layer.
- **Build or refresh: Key Journeys.** Create or update the platform-level Key Journeys page. Current state only.

Read `${CLAUDE_PLUGIN_ROOT}/shared/standards/continuous-track-conventions.md` first. Area codes, IDs, confidence labels, the evidence scale and guardrails come from there.

## Sources

| Source | Good for | Where |
|---|---|---|
| Code documentation | Logic: business rules, calculations, validations, scheduled jobs, integrations, data objects | Confluence, through the Atlassian connector. Location on the Product Agent Config page. |
| Product Map | What users see: modules, screens, actions, roles, entitlements | Confluence, product-area space > Product Map |
| Key Journeys | How users move across modules and areas to finish a job | Confluence, platform-level page. Location on the config page. |
| PRDs, specs, release notes | Intent and history: why it works this way, when it changed | Confluence |
| PO | Anything the sources do not cover | Ask |

Which source wins depends on the question. For "what does the system do", code docs win. For "what does the user see", the Product Map wins. For "how does a user get from A to B", Key Journeys win, checked against the module pages. For "why", PRDs and the PO win. A newer source beats an older one on the same question.

Never resolve a conflict silently. Report both sides with their sources and dates, and say which one is likely current and why.

## Workflow: lookup

### Step 1. Frame the question

Restate the question and the product area in one line. Split compound questions into parts. If the question crosses areas, name each area.

### Step 2. Search the sources

- UI questions: Product Map first.
- Logic questions: code docs first.
- Journey or cross-area questions: Key Journeys first, then the module pages for each step.
- Intent or recent change: PRDs and release notes.

### Step 3. Reconcile

For each part, mark it:

- **Confirmed:** two sources agree, or code docs state it directly
- **Single source:** one source only
- **Unknown:** no source covers it

### Step 4. Answer

Return a short answer block:

- The answer, in two to five sentences
- Each fact with its confidence label and source link with date
- Journeys touched (`JRN-<AREA>-##`), when the question involves a user flow
- Conflicts between sources, if any
- Gaps marked Unknown, with who could answer them (engineering, PO, compliance, vendor)

Never fill a gap with a guess or with general knowledge of wealth platforms. Unknown is a valid answer.

When another skill calls this mode, it cites the block as returned. Gaps found during lookup are collected for the next refresh.

## Access rules for every walk

Both build modes walk the demo platform. These rules apply to both.

- **Build needs the PO present.** The PO signs in. Scheduled runs cannot build.
- **Demo or sandbox only.** If anything on screen looks like real client data, stop and tell the PO.
- **The agent never types a password and never stores credentials.**
- **Read only.** Open pages, menus, tabs and modals. Never submit a form, save, delete, send, upload or change a setting. If a screen can only be reached by submitting something, record it as "Not walked: requires a write" and move on.
- Record the environment and the build or version number if the platform shows one.
- One demo account per role: advisor, ops or admin, investor or client. One more account with a different tenant setup if one exists.

## Workflow: build or refresh the Product Map

### Step 1. Set scope

Confirm with the PO:

- Product area, and all modules or named ones
- Roles to walk
- Previous Product Map date, if any

### Step 2. Walk the platform

Go one role at a time and one module at a time, starting from the main navigation. For each screen, record:

- Name and navigation path
- Purpose, in one sentence
- Key data shown
- Actions available, and whether each one reads or writes
- Which roles see it
- States seen: empty, loading, error
- Screens it links to, including screens in other areas

Save notes to a working file after each module. Large modules are walked in parts so nothing is skipped.

If something looks broken, confusing or slow during the walk, record it as an observation. Walk observations rate Weak on the evidence scale. They are signals, not findings.

### Step 3. Compare roles and tenants

List what differs between roles and between tenant setups. These are entitlement notes. Apply the Tenant impact lens.

### Step 4. Add the logic layer

For each module, pull from the code docs:

- Business rules and calculations
- Integrations (Addepar, Wealthbox, custodians, others) and the direction data flows
- Scheduled jobs and syncs
- Key data objects

Cite the code doc page for each item. Where the walk and the code docs disagree, record a conflict.

### Step 5. Map to JSM

Record the JSM component, request type or label for each module, from the config page or the PO. If none is known, write Unknown. This mapping lets discovery-synthesis tie themes to modules.

### Step 6. Link to journeys

If the Key Journeys page exists, list the journeys each module appears in. If a module's screens changed and a journey runs through them, flag that journey for refresh.

### Step 7. Diff against the last map

If a previous Product Map exists, list what changed: new screens, removed screens, changed actions, changed role access. This list goes in each module page's change log.

### Step 8. Self-check

- Every screen has a role and a walk date.
- No PII. No screenshots with client data.
- Every logic claim cites a code doc page.
- Every conflict is listed.
- The Not walked list is complete.
- Walk observations are rated Weak.
- No credentials anywhere.

### Step 9. Draft, review, publish

1. Fill in `templates/product-map.md`, one page per module.
2. Show the PO the module summaries, conflicts, Not walked list and observations first.
3. On approval, publish under the area's Product Map parent page as `<Product Area> Product Map: <Module>`. Update existing pages in place with a change log line.

## Workflow: build or refresh Key Journeys

Key Journeys is one platform-level page. The journeys that matter cross areas, so the page does not belong to one area. It records the current state only. Future-state journeys belong to journey-map in the project track.

### Step 1. Set scope

Confirm with the PO:

- Which journeys to build or refresh. For a first build, propose a short list from the Product Map: flows that link modules across areas, or that carry the most tickets. The PO picks. Aim for the handful of journeys that matter most to advisors, ops and investors, not every path.
- The areas each journey crosses, and the roles involved
- The previous Key Journeys date, if any

### Step 2. Assign IDs

Each journey gets `JRN-<AREA>-##`. `<AREA>` is the area where the journey starts. A refreshed journey keeps its ID. A retired journey keeps its ID and is never reused.

### Step 3. Define each journey

For each journey, record:

- Name, written as the job the user is doing ("Onboard a new household from the CRM")
- Starting role and trigger
- End state: what "done" looks like for the user
- Areas crossed, in order

### Step 4. Map the steps

Build the steps from the Product Map module pages first. Walk only the gaps, under the access rules. For each step, record:

- Step number and what the user does
- Area code and module
- Screen, from the Product Map
- Role
- System action behind it, if any: sync, job, integration call (from the code docs)
- Source and confidence label

Where a step hands off between areas, mark it as a **handoff**. Handoffs are where cross-area problems usually sit. Record what passes across (data, status, user) and what triggers it.

If a step can only be reached by a write, record it as Not walked and fill it from the code docs or PRDs, labeled Single source.

### Step 5. Note differences

List where the journey changes by role, tenant setup or entitlement. Apply the Tenant impact lens. Note any step that touches compliance-relevant data or client disclosures.

### Step 6. Record observations

Friction seen during the walk goes in an observations list for each journey. Walk observations rate Weak. Do not add ticket counts or opportunity IDs here. That is discovery's job.

### Step 7. Diff against the last version

List new, changed and retired journeys and steps. This goes in the page's change log.

### Step 8. Self-check

- Every journey has an ID, a starting role, a trigger and an end state.
- Every step has an area code, a module, a source and a confidence label.
- Every cross-area handoff is marked.
- No future-state steps.
- Walk observations are rated Weak.
- No PII and no credentials.

### Step 9. Draft, review, publish

1. Fill in `templates/key-journeys.md`.
2. Show the PO the journey list, the handoffs, Not walked steps and conflicts first.
3. List the areas each journey crosses, so the PO can check those steps with the other area POs before approving.
4. On approval, publish to the Key Journeys location on the config page. Update the page in place with a change log line.

## Cadence

- Product Map: build once per area. Refresh after a major release, and before each quarterly strategy refresh.
- Key Journeys: build once after the first Product Maps exist. Refresh each quarter, and when a module refresh flags a journey.
- Lookup runs any time, including inside scheduled runs.

## Handoffs

- **To every skill:** answer blocks with citations and confidence labels.
- **To discovery-synthesis:** the module list, the JSM mapping and the journey list, so themes tie to modules and journeys. Walk observations, as Weak signals.
- **To opportunity-mapping:** journeys, for the Journey column and for matching candidates. Context for feasibility and viability assumptions.
- **To impact-analysis:** modules, roles, data objects, integrations, jobs, dependencies and the journeys a change touches.
- **To strategy:** facts for the Product line of the vision board.
- **To journey-map (project track):** the current-state journey as the baseline for any future-state design.
