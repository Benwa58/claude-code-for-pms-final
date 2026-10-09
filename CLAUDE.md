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

Customer research (Sep 2026): 4 handler interviews (Dot, Ambrose, Halloran, Kip) and 147 support tickets. Both show vanishing pings and quiet responders after 4.2. Only 1 of 147 tickets is from an interviewed handler; the heaviest ticket filers (Okafor/Undertow, Pruitt/Farlight, Fischer/Halfmoon, Demir/Ashgrove) were not interviewed and are the best follow-ups. Tickets look templated (no subject appears more than twice), so counts overstate independent reports.
Code in `00-rook/code/dispatch-routing`: a missed ping counts as a decline (-0.12 vs +0.08 for a take), scores never decay, and nothing in the wiki, handoff or code explains why the wait went 90s to 60s. First pings to a responder in the incident's own area fell from 80% to 61% after 4.2, the opposite of the proximity intent; cause unknown.
My current position: first step is reverting the ping wait to 90s, one variable at a time, after asking Marcus and Helen; then repair damaged scores (reset or decay, treat missed lighter than turned down); monitor proximity separately. Draft roadmap order: revert, score repair, measurement, proximity investigation, handler visibility, Supply urgency lane, then filters and polish.
Open checks, thin evidence: cold-weather gear failures with unanswered failure reports (Supply safety), screen reader labels, availability using the handler's time zone for travelling responders (may confound Halfmoon's quiet weeks), double-listed responders on invoices, console lockouts, innocent misses (mis-tap, no signal) that still cost score.

- `00-rook/data/callout-history.csv` (16 responders x 10 weeks, 29 Jun-31 Aug; matches rook-database): weekly acceptance 76.8% (6 full weeks before) to 68.5% (17-31 Aug), 72.7% in the last week. Week of 10 Aug (54.2%) mixes pre- and post-release days, so don't use it as the 4.2 figure. Four responders (The Undertow, Farlight, Meteor Mite, Vesper) went from ~49 pings/week to 3 (acceptance 75.9% to 12%); the other 12 recovered to 74.1% but carry ~32% more pings. The recovery in the headline is partly mechanical (the four stopped being pinged).
- The wait change is visible in the data: pass-on gap after a missed ping 92s to 62s, after a turn-down unchanged (22s). Missed pings jumped for both cohorts in the release week (the other 12: ~2% to ~12%), so the wait hits everyone. Time-to-accept can't be computed; proxy (callout to winning ping sent) averages ~21s before, ~28s after, median flat 16-18s. `callouts` has no urgency field, so the "urgent pings" rationale for 60s can't be tested.
- Proximity does not explain the quiet responders. Farlight is the only Uptown responder: pinged on 59 of 59 Uptown callouts before 4.2, 6 of 31 after (2 of 27 from 17 Aug), starting after four misses on 12-14 Aug. Meteor Mite and The Gale share Eastgate and handler Kip: Mite fell 11 to 1 pings/week while The Gale rose 13 to 21 (Mite started 9 points lower on acceptance). Only Eastgate has two responders; every other area has one. In-area missed pings rose 0.7% to 10.3%. Likely cause is the recent-acceptance score outweighing proximity, an inference until Wen Li confirms.
- Tickets vs data: they agree on the mid-August step change and on Undertow/Farlight counts (still quiet through 7 Sep). They diverge on severity and cause: Meteor Mite and Vesper filed no tickets despite the deepest drops, Halfmoon and Ashgrove file loudly for ~25-30% declines, no ticket mentions 4.2, the wait or proximity, and handlers read the quiet as an account fault ("check your side"), a visibility gap.
- Open: ask Wen Li/Marcus whether Dispatch logs routing score or rank per ping (not in the database; I can only estimate a score replay from `pings`); odd that out-of-area pings had zero takes before 4.2; no city map or area coordinates exist in the data or wiki, so any area map is schematic. A one-line message for The Undertow is drafted but the cause is still unconfirmed.

- Module 4 (reading `dispatch-routing`): the score is `config.py` weights x proximity, recent acceptance (`history.py`) and capability match. Everyone available is always on the list; ranking only sets the order, and Dispatch stops at the first yes, so low-ranked responders get leftovers, not an exclusion. Weights 4.2: 0.60/0.25/0.15 (was 0.45/0.40/0.15). Only taking a callout adds points (+0.08); every miss or turn-down costs 0.12, with no decay, reset or manual fix in the code. Break-even take rate is 60% (0.12 / 0.20): ~77% before 4.2 drifts scores to the ceiling, ~64% after sits barely above it.
- Working view (inference, not confirmed): before 4.2 scores bunched high, so the 0.40 weight did little; the 60s wait created misses that spread scores apart, and a few responders slid so far that even the lower 0.25 weight outweighed closeness. A five-responder simulation (invented numbers, not saved) reproduced it: the slower answerer (about 40s) lost half their score and a quarter of their pings. Wording to use: "clustered high, narrow spread", not "all at 1.0".
- Open questions for Wen Li/Marcus: are scores stored anywhere that survives a restart (`_scores` is in memory only), were they reset when 4.2 shipped, and are they logged per ping? Scores are keyed by responder name, not ID. Loose end: Farlight is the only Uptown responder and should still win on proximity at 0.60, yet fell from 59/59 to 6/31 Uptown pings, so proximity gaps may be small (no map or travel-time data to check).

- Module 5 (Helen's request, `05-super-speed/director-request.txt`): she doesn't want the fix to be a quiet number change; she wants a one-pager on what we'd build, seen from the handler (Kip) and the quiet responder, ideally clickable. Wen's 2019 TODO in `history.py` (should scores ease back; should a miss equal a turn-down) is the "note next to the code". Outputs: `05-super-speed/brief.md` (the PRD; the older `prd-track-record-visibility.md` is a stale copy) and `prototype.html` (handler roster plus responder phone app, illustrative data).
- Decisions in the brief: handlers get insight, not levers. No handler reset or "mark a miss as not their fault", only "Tell us what happened" as context to the Dispatch team. No score or rank shown to anyone (the number is flawed, gameable, and a safety risk if responders feel pressed to accept). Healing (lighter misses, decay) is automatic; the one-time reset to neutral is run by Engineering after the wait and weighting are fixed, for the four quiet responders only. Visibility alone is not the fix: say so to leadership and present it as a phase.
- Reverting the wait to 90s is a separate decision for Helen and Marcus, listed as a dependency in the brief, not part of "what we'd build". Success targets (back within 25% of prior volume in 3 weeks) are my guesses. In the prototype the taken/turned-down/missed counts, miss dates and Undertow/Vesper details are generated or invented; real counts would come from the `pings` table. Open: confirm decay and miss weights with Wen, whether many simultaneous misses (an outage) should auto-correct, and a Supply check before rebalancing callout load.

- Module 6: I have a project skill `review-checklist` in `.claude/skills/review-checklist/SKILL.md`: six fixed scored checks (owner named, measurable success, scope start matches end, problem before fix, timeline, no logical inconsistencies), an unscored "PRD coverage" list of seven items, and a visual scorecard. It lives only in this folder, so it isn't in my personal skills list; to use it elsewhere copy it to `~/.claude/skills` or upload a zip. Run on `05-super-speed/brief.md`: fix first, 2 of 6 pass (owner is a role not a name; "Next step" is stale, it still says items 1 and 3 and "I'll build it"; no ship date or review date; reset depends on a wait fix the brief puts out of scope; the 49-to-3 pings and 12% acceptance use different windows). Fixes are not applied yet.
- Comparison: another student's public brief (jnaiden) scored 5 of 6 by adding a plan table with named owners and dates, phases, rollout and rollback triggers, a comms plan and a labelled-assumptions list. Worth borrowing for mine. Their open miss: a ticket baseline that doesn't match its measure.
- A scheduled task `monday-brief-review` (stored in `~/.claude/scheduled-tasks`, outside this folder with my OK) reruns the skill on `brief.md` every Monday at about 9:13, only while the app is open; it reviews only and reports what changed. If I start a new brief, update the task.
