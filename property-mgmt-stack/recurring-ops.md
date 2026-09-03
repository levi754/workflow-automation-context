# Building Cadence — recurring ops, building intelligence, AI vendors

Companion to `cost-per-door.html`. See `building-cadence.html` for the full
version (published as an Artifact).

## Confirmed portfolio facts (Sept 2026)

- **25 buildings, ~300 units** — ~12 units/building. Small multifamily, not
  scattered-site: the 150–250 doors-per-manager band is realistic; the industry
  54-per-employee average is not the target.
- **bcompliant is already in place** for city compliance.
- **Alvin.ai is a BigQuery cost-optimization platform** — not property management.

## Corrections to the previous version

1. **RegWatch withdrawn as a compliance recommendation.** bcompliant covers
   violations, complaints and compliance across every NYC agency (FDNY, DOB, DOT,
   ECB, DSNY, HPD) with push/email alerts, *plus* managed resolution — OATH
   hearing representation, stipulation negotiation, correction certification.
   RegWatch explicitly does not file. bcompliant is the stronger tool; don't add a
   second. RegWatch retains one narrow use: a **one-time $15 report** to diagnose
   how much of the stalled lead-gen spec its Prospector already assembles.
2. **The "Alvin can migrate our data" premise collapses.** alvin.ai rewrites SQL
   and manages BigQuery reservations; it has no PM product and no PMS
   integrations. Either the domain is off and the company is Avery
   (averyiq.com), or it's a new company whose real URL is on the original
   outreach. Regardless: no AI agent vendor in this category does PMS migration.
3. **Stack total drops to $897/mo = $2.99/door** for Steps 0–2, because two of
   the three steps are already owned.

## Headline finding: the engine is already paid for

Rent Manager has three features that together are exactly the system needed for
building- and unit-specific recurring reminders, at **$0 incremental cost**:

1. **User Defined Fields (UDFs)** — arbitrary custom fields on property and unit
   records. This is the asset registry (`hose_bib_count`, `hvac_filter_size`,
   `drain_type`).
2. **Task Automation** — posts recurring service issues *without any user
   intervention*; scans the database, finds what's due, posts it.
3. **rmAppSuite Pro** — surfaces selected UDFs to maintenance techs in the field.

**No reminder can target a hose bib until a field says which units have one.**
This is a data problem before it is a tool problem. Build order: registry →
triggers → tools.

## Architectural split — do not get this backwards

| Work type | Where it goes | Why |
|---|---|---|
| High-frequency, mechanical, calendar-driven (filters, hose bibs, drains) | **Rent Manager Task Automation** | Posts on schedule regardless of completion state |
| Low-frequency, judgment-heavy, multi-step (collections, renewals, move-in/out, onboarding) | **LeadSimple Operations** | Branching logic, escalation, SLA |

LeadSimple's recurring tasks only generate the next instance **once the current
one is marked complete** — a missed weekly filter task stalls the series. Across
300 units that is a guaranteed pile-up. Keep high-frequency work out of it.

## Cadence spec

| Item | Frequency | Scope | Needs field |
|---|---|---|---|
| HVAC filter clean/swap | Weekly (see caution) | Unit | `hvac_filter_size`, `hvac_unit_count` |
| HVAC professional clean | Quarterly | Building | `hvac_system_type`, `hvac_vendor` |
| HVAC deep clean / duct | Annual | Building | `hvac_system_type`, last service |
| Hose bib shutoff + drain down | Oct 1–15 | Unit | `hose_bib_count`, `hose_bib_location` |
| Hose bib turn-on + leak check | Apr 1–15 | Unit | same, inverse season |
| Main / floor drain jetting | Semi-annual | Building | `drain_type`, `drain_count`, `has_commercial_tenant` |
| Roof drain / leader clearing | Mar & Oct | Building | `roof_drain_count`, `roof_access` |
| Heat season start check | Sep (pre-Oct 1) | Building | `boiler_type`, `boiler_location` |
| Monthly newsletter | Monthly | Portfolio | contact preferences |

NYC statutory (indicative only — applicability and dates vary by building
characteristics; compute per building and confirm with counsel): HPD registration
(annual, early Sept), boiler inspection (annual), elevator inspection (annual),
backflow test (annual), window guard notice (annual), lead paint/LL31 (annual),
LL97 emissions report (annual), LL152 gas piping (4-yr cycle by community
district, **$5,000/building/BIN penalty**), FISP/LL11 facade (5-yr cycle, 6+
stories).

**Caution on weekly filters:** manufacturers generally call for monthly rinsing
(washable) or 1–3 months (disposable). Weekly is ~52 touches for a 12-touch job
and invites residents to mute the channel — which mutes the messages that matter.
Suggested: weekly for the first month post-move-in, monthly thereafter, weekly
only for units with a documented coil freeze or airflow complaint. The
`filter_reminder_cadence` field lets both run and be compared.

## Asset registry — UDFs to create

Unit level: `hose_bib_count`, `hose_bib_location`, `hvac_filter_size`,
`hvac_unit_count`, `filter_reminder_cadence`.

Building level: `hvac_system_type`, `drain_type`, `drain_count`,
`roof_drain_count`, `has_commercial_tenant`, `boiler_type`, `boiler_location`,
`water_main_shutoff`, `gas_shutoff`, `access_notes`, `lockbox_code`,
`super_contact`, `roof_access`, `built_pre_1960`, `has_elevator`,
`has_backflow_device`, `stories`, `sq_ft`, `community_district`.

The access/shutoff/super/roof fields solve the "building codes and accesses"
problem as a byproduct — rendered to techs in rmAppSuite Pro, no wiki to go stale.

## AI vendor deep dive

Almost all of these are the same product: an AI that answers the phone, triages
maintenance, dispatches a vendor. They differ on modality and depth, not category.
**You need exactly one.**

| Vendor | What it actually does | Rent Manager | Price | Verdict |
|---|---|---|---|---|
| **Vendoroo** | Reactive maintenance lifecycle: triage → expert review → vendor scheduler → job verification | **Native** (joined RM Integrations Program, Apr 2026) | ~$3/door | Strongest if staying on RM |
| **Haven AI** | Two agents only: Maintenance AI + Leasing AI. Voice-first. Vendors/collections "coming soon" | Not listed | $1/door, $298 min | Cheapest credible entry |
| **Avery (AveryIQ)** | AI contact center — calls/texts/emails, tours, work orders, vendor coordination; 88% no-human | Not confirmed | Quote only | Very likely your "Alvin" |
| **Super** | Voice-first receptionist, answers *and places* calls, triages, routes | Not confirmed | Quote only | Overlaps Avery — pick one |
| **Zuma (Kelsey)** | Full resident lifecycle incl. **rent collections**, human-in-the-loop, multifamily-native | Multifamily PMS focus | Free pilots | Best collections story |
| **EliseAI** | Most established ($2.2B, ~$200M ARR): leasing, renewals, maintenance, delinquency | Enterprise | $3–6/unit, ~$25k/yr min | Skip until ~600 doors ($6.94/door now) |
| **Property Meld** | Not a chatbot — AI intake **plus preventive maintenance scheduling with asset tracking** | Yes | $1.60/door, $160 min | Only one serving the recurring ambition |
| Colleen AI | Was AI collections | — | Unavailable | Acquired by Entrata Jun 2024, folded into ELI+ |

**Is Vendoroo redundant?** Against Avery/Haven/Super — yes, heavily; all four
take an inbound request, triage, dispatch. Against Property Meld — no; Vendoroo is
reactive, Meld also does *scheduled* preventive maintenance. Note Vendoroo's own
product pages say nothing about preventive/recurring scheduling, so **Vendoroo
will not fire the HVAC filter reminders.**

**Can "Alvin" migrate the data?** Almost certainly not. No AI agent vendor in this
category shows evidence of PMS migration, and it would be odd for a contact-center
product. Migration means chart of accounts, historical transactions, security
deposits, prepaids, leases, owner statements and tax history, reconciled at both
ends — an accounting project with legal exposure. It comes from the destination
vendor's team (AppFolio migrates from Rent Manager with dedicated onboarding). If
told otherwise, ask who reconciles the trial balance at cutover and who is liable
if a deposit lands in the wrong ledger.

## Recommended stack — fewest tools, at 300 doors

| Step | Tool | Covers | Cost |
|---|---|---|---|
| **0 (free)** | Rent Manager UDFs + Task Automation + rmAppSuite Pro | Asset registry, all recurring triggers, building info, newsletter via mass comms | **$0** |
| **1 (owned)** | bcompliant | All-NYC-agency violations, complaints, compliance, alerts, plus managed resolution/OATH representation | **$0** — already paid |
| 2 | LeadSimple Platform | Collections, renewals, move-in/out, onboarding; retires rmVoIP + HubSpot + DocuSign | $897/mo ($2.99/door) |
| 3 | One front-door AI, after 90 days of data | Vendoroo (RM native) / Haven (price) / Zuma (collections, free pilot) | $300–900/mo |

Steps 0–2: **$897/mo = $2.99/door** — one new line item; the rest already owned.

## Other tools worth knowing (RM / AppFolio integrations)

**Sorting rule:** some of these are software and some are outsourced labor
wearing software's per-unit pricing. Software runs $1–3/unit. Managed services
run ~$25/unit — 10× — because a human answers. Latchel at ~$25/unit is
$7,500/mo across 300 units, i.e. roughly a full-time hire. Compare those to a
headcount decision, not to Haven or Vendoroo.

| Tool | Job | RM | AF | Price | Verdict |
|---|---|---|---|---|---|
| **RentCheck** | Residents self-complete move-in/out + periodic inspections | Yes | Yes | $1.25/unit | Best value for the move-in/out consistency ask |
| **zInspector** | Inspections, photo/video, reports | Yes | Yes | Free ≤5 → $110/mo Max | Flat $110 may beat per-unit at 300 doors |
| **AppWork** | Work orders, turnovers, inspections; mobile-first. bcompliant partner | Yes | — | One flat per-unit fee, all features | Possibly your "Appfull" — check if already owned |
| **AvidXchange** | AP automation | Built-in | — | Quote | The lever to pull *instead of* a bookkeeper hire |
| **LeaseTrack** | Renters insurance tracking | Built-in | — | Quote | Only if insurance chase is manual today |
| **Zego / AmRent** | Payments; screening | Built-in | Native equiv | Quote | Check what you already use |
| **ButterflyMX** | Smartphone access control, video intercom, self-guided tours | — | Stack | Hardware + per-door | Attacks the "access codes" problem, but it's capex |
| **Conservice** | Utility allocation/billing | — | Stack | Quote | Only if you bill back utilities |
| **HappyCo** | Inspections + unit/resident data | Yes | Stack | Per unit, **500-unit min** | Don't qualify yet |
| **Latchel** | AI front office + staffed 24/7 emergency maintenance; claims 48% fewer after-hours emergencies | Check | Check | **~$25/unit** | $7,500/mo ≈ one hire |
| **Lula** | After-hours/24-7 maintenance coordination | Check | Check | **Billed only when work completes** | Best cost structure — no fixed monthly |
| **Anequim** | Remote staffing / call center | Built-in | — | Per seat | A hiring decision, not software |
| **Second Nature** | Resident Benefits Package | Check | Check | $25–75/mo per resident; ~$156/home/yr GP | **See NYC caution** |
| **Apartment List** | Pay-per-lease listings | ILS | Stack (Jul 2026) | Per signed lease | Zero fixed cost — easy to test |

### ⚠️ NYC caution on Resident Benefits Packages

The RBP model was built for **single-family rentals in lightly regulated states.**
You operate 25 small multifamily buildings in NYC. If any units are
rent-stabilized, a mandatory recurring charge on top of the legal regulated rent
is a rent-overcharge exposure with treble-damage potential — it does not become
safe by being called a benefits package. NY has also been tightening on mandatory
tenant-side fees. **Do not implement any RBP, resident-billed filter program, or
mandatory fee bundle without counsel reviewing it against each building's
regulatory status.** A genuinely opt-in offering is a different question, but
"voluntary" has a specific legal meaning here.

### Three to shortlist

- **RentCheck ($375/mo at 300 units)** — makes move-in/move-out inspections
  resident-completed and consistent. Compare head-to-head with zInspector's flat
  $110/mo, which may simply win at your size.
- **Lula** — the only after-hours option costing nothing until work completes.
  The right way to discover your true after-hours volume.
- **AvidXchange** — quote it specifically against the bookkeeper hire (~$5,500/mo
  loaded). It won't do everything a bookkeeper does; model both.

### The AI marketplace point

Rent Manager's marketplace has quietly filled with AI entrants — **Domos,
1edge.ai, Brickwise AI, aiNgo, Insured AI, Rent Butter, Blanket** — alongside the
Vendoroo integration. None are established enough to plan around today, but it
means staying on Rent Manager is not the "no AI future" choice it looked like a
year ago. **That materially weakens the AppFolio migration case,** which was
substantially an argument about access to AI.

## Ambition ledger — from 225 turns of prior Claude history

Read from `project-context.txt` (NYC developer lead-gen project, last touched
7 Aug 2025).

**Done:** Supabase foundation (then reset Aug 2025 for a 20-year rebuild) ·
HubSpot custom properties · all six NYC permit/registration APIs identified.

**Partial:** BIN cross-referencing (real wall — DOB permits and DOB NOW use
different permit-number types, only BIN crosses, and filers use one or the other)
· flexible lead scoring with manual override · 20-year all-borough load.

**Not built:** management-pattern detection (self vs. third-party) · contact
enrichment via Seamless · **daily/weekly API auto-updates (requested at least 4×
— the most-requested unbuilt item)** · address → owner → portfolio → contacts
lookup · **"same owner, different managers" signal (the sharpest commercial idea
in the transcript)** · the CRM buttons · the Lovable front end · "replicable for
others."

**The pattern:** everything that landed is infrastructure; everything that didn't
is the part that makes money. The project never got past data loading — the
transcript cycles through rebuilds, clean-slate SQL, column-mapping failures,
oversized files and duplicates. Fourteen months in, the database is ready and no
email has been sent by it. That is a sequencing problem, not a discipline one:
the first milestone required a pipeline from six government APIs before producing
one lead, with no cheap intermediate win to sustain it.

**Why ops will succeed where lead-gen stalled:** both projects are the same shape
— recurring, data-driven actions fired off a building registry. Lead-gen had to
*build* the registry. Ops does not: Rent Manager already holds every building,
unit and resident. The first milestone is two weeks away and free.

**Worth buying instead of building:** RegWatch Max includes a Prospector lead
finder, AI Analyst mode and CRM/CSV export alongside compliance — meaningful
overlap with the pipeline hand-built over fourteen months, at $50–100/mo. It will
not replicate BIN-level permit cross-referencing, the owner-portfolio graph, or
the "same owner, different managers" signal (still the edge). Spend $15 on one
report first and see how much of the spec arrives pre-built; buy the overlap,
build only the differentiated remainder.

## Two-week plan

- **Days 1–3:** create the UDFs in Rent Manager (configuration, not a project).
- **Days 4–10:** walk two buildings end to end, fill every field including
  shutoffs, access codes, roof access. Time it, extrapolate, template repeats.
- **Days 8–10:** configure Task Automation for four cadences only — hose bib
  shutoff (Oct is weeks away), roof drains, quarterly HVAC, heat-season check.
- **Day 11:** buy one $15 RegWatch report on the largest building; compare to the
  lead-gen spec and to what is tracked manually today.
- **Days 12–14:** send one newsletter through Rent Manager mass communication.
  Prove the channel before designing a content calendar.

No vendor demos, no migration, no AI pilots — those belong after the registry
exists, because the registry is what makes them evaluable.

## Open questions

- **What was the real vendor behind "Alvin"?** alvin.ai is a BigQuery tool. Find
  the sender address on the original outreach; Avery (averyiq.com) is the closest
  functional match.
- **Is "Appfull" from the first message actually AppWork?** It fits — a Rent
  Manager integration partner and a bcompliant partner.
- **Do any of the 25 buildings contain rent-stabilized units?** Now the
  highest-stakes open question — it determines whether the entire
  ancillary-revenue category is available or legally off-limits.
- **Commercial tenants in any building?** Changes drain jetting from annual to
  semi-annual and pulls in a separate compliance set.
- **Is the lead-gen pipeline live or parked?** Decides whether the HubSpot
  dependency is a real constraint or a free cancellation.

## Sources

[Rent Manager Task Automation](https://www.rentmanager.com/automate-your-operation-with-these-tools/) ·
[RM customization/UDFs](https://www.rentmanager.com/software-customization/) ·
[RM maintenance](https://www.rentmanager.com/maintenance/) ·
[RM mass communication](https://www.rentmanager.com/mass-communication-methods-rent-manager/) ·
[LeadSimple recurring tasks](https://training.leadsimple.com/en/articles/9420207-new-standalone-and-recurring-tasks) ·
[LeadSimple workflow automation](https://www.leadsimple.com/property-management/workflow-automation) ·
[RegWatch](https://regwatch.nyc/nyc-compliance-deadlines) · [RegWatch pricing](https://regwatch.nyc/pricing) ·
[Insparisk calendar](https://insparisk.com/nyc/compliance-calendar) ·
[LL152 penalties](https://www.keepmygas.nyc/local-law-152/property-managers-nyc/) ·
[NYC 2026 deadlines](https://randpc.com/news/nyc-building-compliance-2026-essential-deadlines-at-a-glance/) ·
[Haven AI](https://www.usehaven.ai/) ·
[Vendoroo × Rent Manager](https://www.rentmanager.com/integrations/vendoroo/) ·
[Vendoroo announcement](https://www.businesswire.com/news/home/20260427364445/en/Vendoroo-Joins-the-Rent-Manager-Integrations-Program-to-Bring-AI-Powered-Front-Desk-and-Maintenance-Operations-to-Rent-Manager-Customers) ·
[Vendoroo product](https://vendoroo.ai/product) ·
[Avery](https://averyiq.com/) · [Super](https://www.hiresuper.com/) ·
[Zuma Collections AI](https://www.getzuma.com/collections-ai) ·
[EliseAI](https://aitoolsbakery.com/blog/eliseai-review/) ·
[Property Meld](https://propertymeld.com/pricing/) ·
[Property Meld PM features](https://www.capterra.com/p/149045/Property-Meld/)
