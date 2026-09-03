# Property Management Stack Evaluation — September 2026

Decision brief and cost-per-door model for the PM software stack at 300 doors,
scaled to 1,000.

- **`cost-per-door.html`** — the interactive model. Every rate is editable;
  scenario tabs preset the ledger; payroll rows plug in headcount. Published as
  an Artifact for sharing.
- This file — the written summary and the pricing reference table.

## Pricing reference

Confidence: **P** = published on vendor's own pricing page · **R** = third-party
reporting, not vendor-confirmed · **Q** = no public price, placeholder estimate.

| Vendor / module | Rate | Basis | Monthly min | Conf | At 300 | At 500 | At 700 | At 1,000 |
|---|---|---|---|---|---|---|---|---|
| Rent Manager (Plus, historical) | $1.50 | door | — | Q | $450 | $750 | $1,050 | $1,500 |
| rmVoIP | ~$30 | seat | — | Q | $180 | $180 | $180 | $180 |
| Email-to-notes ("Savant" / Email2CRM) | ~$75 | flat | — | Q | $75 | $75 | $75 | $75 |
| DocuSign Business Pro | $40 | seat | — | P | $240 | $240 | $240 | $240 |
| HubSpot CRM (Sales Pro) | ~$100 | seat | — | R | $600 | $600 | $600 | $600 |
| **LeadSimple Platform** (all-in-one) | $2.99 | door | $500 | P | $897 | $1,495 | $2,093 | $2,990 |
| LeadSimple Operations only | $1.35 | door | $200 | P | $405 | $675 | $945 | $1,350 |
| LeadSimple CRM only | $99 | flat | — | P | $99 | $99 | $99 | $99 |
| LeadSimple Phone + Inbox Pro | $59 | seat | — | P | $354 | $354 | $354 | $354 |
| Aptly | ~$2.50 | door | — | Q | $750 | $1,250 | $1,750 | $2,500 |
| AppFolio — your quote (read as $3.29) | $3.29 | door | $960? | Q | $987 | $1,645 | $2,303 | $3,290 |
| AppFolio Core | $1.49 | door | $298 | R | $447 | $745 | $1,043 | $1,490 |
| AppFolio Plus | $3.20 | door | $960–1,500 | R | $960 | $1,600 | $2,240 | $3,200 |
| AppFolio Max | $5.00 | door | $1,500 | R | $1,500 | $2,500 | $3,500 | $5,000 |
| AppFolio payment processing | $1.00 | door | — | R | $300 | $500 | $700 | $1,000 |
| Haven AI | $1.00 | door | $298 | R | $300 | $500 | $700 | $1,000 |
| Property Meld (Core) | $1.60 | door | $160 | R | $480 | $800 | $1,120 | $1,600 |
| Vendoroo | $3.00 | door | — | R | $900 | $1,500 | $2,100 | $3,000 |
| Avery (AveryIQ) | ~$2.00 | door | — | Q | $600 | $1,000 | $1,400 | $2,000 |
| EliseAI | $4.50 | door | $2,083 (~$25k/yr) | R | $2,083 | $2,250 | $3,150 | $4,500 |

Notes: monthly cost is `max(rate × units, minimum)`. LeadSimple annual rates
shown; month-to-month is $3.49/door (Platform), $1.55/door (Operations).
DocuSign includes only 100 envelopes per user per year, no rollover.
Colleen AI is no longer available standalone (Entrata acquired it, June 2024).

## Scenario totals at 300 doors, 6 seats

| Scenario | Stack | Software/mo | $/door |
|---|---|---|---|
| Today (baseline) | RM + rmVoIP + email int. + DocuSign + HubSpot | $1,545 | $5.15 |
| **A — Consolidate on LeadSimple** | RM + email int. + LeadSimple Platform | $1,422 | $4.74 |
| B — Aptly as the AI layer | RM + rmVoIP + email int. + DocuSign + Aptly | $2,295 | $7.65 |
| C — AppFolio + LeadSimple | AppFolio ($3.29) + payments + LS Platform | $2,184 | $7.28 |
| D — Maximum automation | RM + email int. + LS Platform + Haven + Meld | $2,202 | $7.34 |

## Findings

1. **The PMS decision and the process-layer decision are independent.** Both
   LeadSimple and Aptly integrate with Rent Manager *and* AppFolio, so the
   process layer survives a later migration. Do not do both at once.
2. **At 300 doors nearly every vendor minimum has just been cleared** —
   LeadSimple Platform's $500 binds to ~168 doors, AppFolio Core's $298 to ~200,
   Haven's $298 to 298, Property Meld's $160 to 100. Consequence: per-door
   software cost is essentially flat from 300 to 1,000 doors. No volume discount
   is coming, and no minimum-fee drag is left.
3. **Payroll is 4–6× the whole software decision.** One $65k hire loaded at 28%
   is $23.11/door/mo at 300 doors; the entire stack is $4–7/door. The real test
   is which option lets you reach 500 doors without adding a head — worth ~$9/door,
   three times the spread between all options here.
4. **LeadSimple is the consolidation play; Aptly is the AI-layer play.** Platform
   covers 7 of 9 job columns in one line item and retires rmVoIP, HubSpot,
   DocuSign and the task-manager gap. Aptly has **no phone system and no
   e-signature** found in its docs, so it keeps rmVoIP and DocuSign alive; it
   publishes no pricing at all. Also: on baseline assumptions LeadSimple is
   roughly **cost-neutral** at 300 doors (~$123/mo saved) — you are buying
   capability at current spend, not savings.
5. **AppFolio's free year is the highest-leverage and highest-risk item.** Confirm
   whether "$329" is $329/mo flat or $3.29/unit/mo (a $7.9k/yr gap today, $35.5k
   at 1,000 doors — almost certainly the latter, given Plus publishes at ~$3.20).
   At 300 units × $3.20 you sit *exactly* on the reported $960 Plus minimum, so
   the per-unit rate buys you nothing at current size. A free year of subscription
   is not a free migration: 100–200 internal hours ≈ $4–8k of payroll.
6. **Buy AI agents last, and only one.** LeadSimple Platform ships an AI engine
   and voice agent, AppFolio Plus gates Realm-X, Rent Manager has Orion. Layering
   Haven or Vendoroo before measuring what is still manual pays twice. EliseAI's
   ~$25k annual minimum makes it the most expensive per-door option at 300 doors
   ($6.94) — revisit past ~600.
7. **Unverified vendors, and one hidden dependency.** "Get Alvin" is not findable;
   closest match is **Avery (averyiq.com)**, a YC-backed AI contact center.
   "Savant" is not findable; closest match is **Email2CRM by Aargh Software**.
   And per `current-status.md` in this repo, the NYC developer lead-gen pipeline
   writes into HubSpot with custom properties — dropping HubSpot retires that
   pipeline's destination. Confirm API/Zapier lead creation on the replacement CRM
   first, or keep HubSpot's free tier as the lead-gen landing zone.

## Sequence

1. **Weeks 1–2 — Baseline.** Line-itemized current spend and seat counts;
   DocuSign envelope volume (12 mo); weekly call/email/text volume by category;
   confirm Alvin/Savant identities; written quotes from all four vendors.
2. **Weeks 3–4 — Pick the process layer.** Demo LeadSimple and Aptly against the
   same three real workflows (new building onboarding, maintenance intake→dispatch,
   renewal at 120 days). Score: auto-triggers off Rent Manager? editable by a
   non-technical person? carries the NYC compliance calendar (HPD, Local Law,
   boiler, elevator, lead)? how many contracts does it cancel?
3. **Weeks 5–10 — Install on Rent Manager; do not migrate.** Build only those
   three workflows. Run rmVoIP in parallel 60 days, port numbers last. Keep
   DocuSign until a real lease completes on the replacement. Measure manual
   touches per work order, speed-to-lead, renewal capture, hours per onboarding.
4. **Month 4 — Decide AppFolio with evidence.** You will know whether the pain was
   Rent Manager or the processes. These offers recur; a badly timed migration does
   not un-happen. If migrating, avoid year-end close.
5. **Month 5+ — AI agents against measured gaps.** One vendor, one workflow, one
   quarter, with the kill criterion written before the pilot starts.

## Open questions

- Is the AppFolio quote $329/mo or $3.29/unit/mo? (modeled as $3.29)
- Current office headcount and salaries? (modeled: PM $75k, ops admin $58k,
  bookkeeper $52k, all at 28% burden)
- Seat counts on rmVoIP / DocuSign / HubSpot, and the real Rent Manager invoice?
  (modeled: 6 seats, RM at $1.50/unit)
- Average management fee per door? (modeled $85/door/mo for the margin line)
- Confirm "Alvin" = Avery and "Savant" = Email2CRM; name the other AI vendors
  you couldn't recall and they can be priced in.

## Sources

LeadSimple [pricing](https://www.leadsimple.com/pricing/) ·
[e-signature announcement](https://www.leadsimple.com/blog/introducing-jot-form-e-signature-integrations-custom-property-forms) ·
Aptly [pricing](https://www.getaptly.com/pricing) · [platform](https://www.getaptly.com/platform/) ·
AppFolio [pricing research](https://costbench.com/software/property-management/appfolio/) ·
[tier minimums](https://appfoliopricing.com/) ·
Rent Manager [pricing](https://www.itqlick.com/rent-manager/pricing) ·
[Orion AI](https://aitoolsbakery.com/blog/rent-manager-review/) ·
[Haven AI](https://www.usehaven.ai/) ·
[Property Meld](https://propertymeld.com/pricing/) ·
[EliseAI](https://aitoolsbakery.com/blog/eliseai-review/) ·
[Avery](https://averyiq.com/) ·
[Email2CRM](https://aarghsoftware.com/email2crm/) ·
[DocuSign](https://ecom.docusign.com/plans-and-pricing/esignature) ·
[doors-per-manager benchmarks](https://www.getkera.com/blogs/how-many-doors-per-property-manager/) ·
[bookkeeper salary](https://www.ziprecruiter.com/Salaries/Property-Management-Bookkeeper-Salary) ·
[NARPM Financial Performance Guide](https://www.narpm.org/docs/NARPM_FinancialPerformanceGuideOverview.pdf)
