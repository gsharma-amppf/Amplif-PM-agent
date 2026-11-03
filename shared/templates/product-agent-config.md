# Product Agent Config

Every Amplify Product Agent skill reads this page. Keep it in one place. Change values here, not in the plugin.

**Owner:** <CPO or product ops>
**Plugin version owner:** <PO name>
**Last updated:** <date>

## Product areas

Keyed by area. A PO who owns two areas appears on two rows.

| Area | Code | Team | PO | Confluence space key | JSM project key | JSM mapping field | JSM mapping value |
|---|---|---|---|---|---|---|---|
| Trading | TR | Team 1 | <name> | <space key> | <key> | Component / Request type / Label | <value> |
| Investment | INV | Team 1 | <name> | <space key> | <key> | Component / Request type / Label | <value> |
| CRM | CRM | Team 2 | <name> | <space key> | <key> | Component / Request type / Label | <value> |
| Integrations | INT | Team 3 | <name> | <space key> | <key> | Component / Request type / Label | <value> |
| Client Portal | CP | Team 3 | <name> | <space key> | <key> | Component / Request type / Label | <value> |
| Digital Onboarding | DO | Team 4 | <name> | <space key> | <key> | Component / Request type / Label | <value> |

The JSM mapping is how discovery-synthesis finds an area's tickets. Example query it builds:

```
project = <JSM project key> AND <mapping field> = "<mapping value>" AND created >= -7d
```

Module-level JSM mapping lives on each Product Map module page.

## Platform pages

| Page | Location |
|---|---|
| Key Journeys | <space and link> |

## Product Map sources

| Area code | Product Map parent page | Code documentation location | Demo environment | Demo roles available |
|---|---|---|---|---|
| TR | <link> | <Confluence link> | <URL> | <advisor, ops, investor, second tenant> |
| INV | <link> | <Confluence link> | <URL> | |
| CRM | <link> | <Confluence link> | <URL> | |
| INT | <link> | <Confluence link> | <URL> | |
| CP | <link> | <Confluence link> | <URL> | |
| DO | <link> | <Confluence link> | <URL> | |

Never put passwords or credentials on this page. The PO signs in during a platform walk.

## Per-area settings

Prioritization uses RICE in every area.

| Area code | Reach unit | Unplanned work buffer | Analytics source |
|---|---|---|---|
| TR | Active advisor users per quarter | 20% | <tool or "PO export"> |
| INV | Active advisor users per quarter | 20% | <tool or "PO export"> |
| CRM | Active advisor users per quarter | 20% | <tool or "PO export"> |
| INT | Active advisor users per quarter | 20% | <tool or "PO export"> |
| CP | Active advisor users per quarter | 20% | <tool or "PO export"> |
| DO | Active advisor users per quarter | 20% | <tool or "PO export"> |

- **Reach unit:** one unit across all areas, so the CPO can compare them side by side.
- **Unplanned work buffer:** share of team capacity held back for bugs, support and interrupts.

## Competitor list

| Competitor | Areas it applies to | Notes |
|---|---|---|
| Orion | All | |
| Envestnet | All | |
| AssetMark | All | |
| Black Diamond (SS&C) | All | |
| Addepar | All | Also an integration partner. Label each finding as competitor or partner. |
| Altruist | All | |
| Advyzon | All | |
| Nitrogen | All | |

## Evidence thresholds

Defaults from the plugin's shared conventions. Change only with CPO agreement.

| Level | Rule |
|---|---|
| Strong | 3 or more tenants and 5 or more tickets, or a validated test |
| Medium | 2 tenants, or 3 or more tickets, or a credible market signal |
| Weak | 1 tenant, 1 to 2 tickets, opinion only, or an agent walk observation |

## Change log

| Date | Change | Approved by |
|---|---|---|
