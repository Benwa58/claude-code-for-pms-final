# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

**Me:** PM for Rook Dispatch (joined Oct 2026, taking over from Priya Raghunathan, who left 21 Aug 2026 and left a handover note in `00-rook/company/notes/handoff-from-priya.docx`). Reports to Helen Achebe. Sources: that note and the rook-wiki "Company" section (About Rook, Dispatch/Supply one-pagers, Glossary, Team directory, Releases, Q3 roadmap). Today is ~6 Oct 2026. Data is queryable via the rook-database connector.

### Company
Rook Industries (founded 2014, 241 staff, HQ Site Aleph in an ice shelf, offices in Berlin/Singapore/Cornwall) sells coordination and provisioning software to independent masked responders and the handlers/quartermasters who support them. Subscription, priced per active responder. Rook employs no responders. Ships monthly on a release train; point releases are 4.x.

**Hard rule:** responder cover identities are never stored and no mapping to legal identity exists (Security Policy 4.1, contractual). Never design for, infer, or try to work out who anyone is.

### Products
- **Rook Dispatch (mine, flagship, release 4.2):** incident enters console, Dispatch ranks available responders, pings the top one's phone, they take it or turn it down; if turned down or missed, it moves to the next. Taken = responder engaged, incident assigned. Users: handlers (web console: enter incidents, watch coverage, override routing, manage availability and capability tags) and responders (native phone app). Routing config ships in the release; handlers cannot change it at runtime.
- **Rook Supply (sister product, 4.2):** requisitions -> quartermaster approval -> fulfillment -> maintenance schedule from service intervals; field failure reports feed item history.
- **Coupling to watch:** Dispatch writes the Responder Availability Record; Supply reads it to schedule maintenance into low-callout-load windows. Any change to how Dispatch calculates availability or callout load silently changes Supply's scheduling. Check with the Supply side before touching it.

### People (Dispatch team)
| Name | Role | Notes |
|---|---|---|
| Helen Achebe | Director of Product (my boss) | Owns roadmap and commitments; roadmap changes go through Product. Gives room. |
| Marcus Oyelaran | Eng Manager, Dispatch (Site Aleph) | Straight talker; first stop when unsure. Can usually pull numbers. |
| Wen Li | Staff Engineer (Berlin) | Built the pinging/ranking logic. No doc exists; learn it by talking to her. Was away 14-24 Aug. |
| Sofia Marino | Product Designer | Owns console and phone app. Ran the September interviews (wiki Research > Customer interviews). |
| Ravi Menon | Data Analyst (Singapore) | Reports weekly on how often responders take pings. |
| Nadia Hoffmann | Support Lead (Berlin) | Hears handler complaints first; Priya recommended a standing 15 minutes. |

Quartermasters are Supply users, rarely Dispatch. Wiki also has handler profiles (e.g. Aunt Dot, Mr. Ambrose, Halloran, Kip) under Research.

### Vocabulary
- **Callout:** request for a responder to attend an incident; Dispatch's unit of work. **Ping:** a callout offered to one responder. **Taken / Turned down / Missed:** yes / no / no answer before ping wait ran out. Turned down and missed both pass the ping on but are recorded separately.
- **Ping wait:** how long a ping stays before counting as missed. Same for everyone, set in the release.
- **Acceptance rate (headline metric, weekly, aggregate):** pings taken / pings offered. **Time-to-accept:** median seconds ping sent to taken. **Coverage gap:** no available responder had the required capability tags (nobody *could* go, distinct from nobody *would*).
- **Routing priority:** score ranking responders; inputs are proximity (travel-time estimate), current availability, capability match, and recent acceptance history. Turning down or missing pings lowers the recent-acceptance part, so it drops that responder in later orders (a feedback loop).
- **Capability tags:** flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation.
- **Handler** manages responders; **Responder** takes callouts; **Quartermaster** owns equipment stock/approvals; **Cover identity** public persona (see hard rule).
- **Mutual aid / shared cover:** responders covering each other's callouts across the city. Not supported; Q4 exploration.
- Supply terms on shared calls: requisition, field failure report, service interval.

### Where things stand
**Releases:** 4.0 (7 Apr: new navigation, profile redesign, routing override audit log); 4.1 (16 Jun: travel-time proximity, bulk callout, push reliability); 4.2 (12 Aug: proximity weighted up in routing, **ping wait cut 90s to 60s**, console filters persist, three fixes).

**The live problem:** since 4.2, fewer pings are being taken and more handlers are complaining. Two changes landed together (routing weighting and shorter ping wait) on top of August seasonality. Priya's read: mostly seasonal, should recover in September, and don't reopen the routing change (it was a long-requested fix for nearby responders being skipped). That is her opinion, not a verified fact. Check it against the data (compare Aug/Sep with prior years, split by routing vs. ping wait effect, look at missed vs. turned down separately) before agreeing or disagreeing. Possible mechanism to test: a shorter wait raises "missed" pings, which lowers recent-acceptance scores, which feeds back into ranking.

**Q3 roadmap (last reviewed 30 Jun, all owned by Priya, ownership now mine):**
- Change to who gets pinged: 4.2, committed. Shipped.
- Ping timeout tuning: 4.2, committed. Shipped (90 to 60s).
- **Availability Confidence** (confidence score beside stated availability): 4.2, committed. **Not in the 4.2 release notes, so likely one of the items squeezed out.** Priya says the Q3-commitment conversation with Helen has not happened; I need to have it.
- Requisition approval chains (Supply, second approval step for large requisitions): 4.3, committed.
- Handler phone app and shared cover between responders: Q4, exploring.

**Open to-dos:** (1) resolve what is still a Q3 commitment with Helen; (2) write the missing description of how pinging/ranking decides, with Wen Li; (3) get 4.2 numbers from Ravi/Marcus; (4) filter-persistence tickets from 4.2 are cosmetic noise, don't let them eat the first month; (5) look hard at long-unexamined parts of the product, since Priya was the only PM for 14 months and moved fast.

### What the data showed (rook-database, 29 Jun to 6 Sep; 4.2 = 12 Aug)
- Tables: callouts, pings, responders, handlers, support_tickets. Query parameter is `query`. No ping response timestamps or ranking/distance fields, so time-to-accept and routing rank can't be computed. No prior-year data, so Priya's seasonality theory can't be tested directly.
- Acceptance rate fell from 76.6% to 64.0% (weekly low 54.2%, recovering to 72.7%). Almost all of it is **missed** pings (2.3% to 18.0%); turned-down did not rise. Callouts needing 2+ pings went from 22% to 32%; support tickets from ~6/week to 20-32/week.
- I've concluded the 90s to 60s ping wait is the primary driver (it acts at the accept moment). Routing's independent effect is untested. Possible feedback loop: The Undertow, Farlight, Meteor Mite and Vesper went from ~12 pings/week to ~4 with 53-64% missed (small samples; Meteor Mite vs. The Gale, same area and handler, argues against pure geography). Needs Wen Li to confirm how recent acceptance is weighted.
- Open: confirm with Wen Li/Marcus; ask Ravi for a 90s vs 60s view; test reverting only the ping wait; separate missed from turned down in reporting; still need Helen conversation on Availability Confidence.
- Not yet read: wiki Research pages (customer interviews, handler profiles) and Handler Phone App page.

### Working notes
Treat wiki and handoff claims as inputs, not facts, and flag where they conflict or rest on one person's read. Cite the source (wiki page, note, or query) when stating numbers or status.
