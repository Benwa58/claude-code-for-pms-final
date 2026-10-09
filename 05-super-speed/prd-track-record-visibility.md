# PRD (one-pager, first pass): Make a quiet responder visible, and let them come back

**To:** Helen Achebe · **From:** Dispatch PM · **Status:** Draft for discussion, 9 Oct 2026
**Helen's ask (director-request.txt):** before anyone touches the code, show what we'd *build instead of changing a number*, seen from the person it happens to: something a handler like Kip would notice and a quiet responder would feel differently about. One page, real over polished. A clickable version if possible.

## The problem, in one person's terms
A responder misses a few pings (bad signal, a mis-tap, a shorter 60s window since 4.2). Dispatch quietly lowers their track record and sends them fewer callouts. Nothing tells them or their handler. The score never recovers on its own, and a missed ping costs the same as a turn-down (`history.py`; Wen's 2019 TODO asks whether it should).

- **Responder:** The Undertow, Farlight, Meteor Mite and Vesper went from ~49 pings/week to 3 (acceptance 75.9% to 12%). They have no way to see why, or how to get back. *(callout-history.csv)*
- **Handler:** reads the silence as an account fault ("check your side"). Meteor Mite and Vesper filed no tickets at all. *(ticket analysis)*
- **Overall:** acceptance fell 76.6% to 64.0%, almost all from missed pings (2.3% to 18.0%), not turn-downs. *(rook-database)*

## What we'd build (three things a person can see)
1. **Handler: "Why is this responder quiet?"** On the roster, a quiet responder gets a plain-language line: *"Offered 3 callouts this week, down from 12. Track record dropped after 4 missed pings on 12-14 Aug."* Two actions: **Give a fresh start** (restore their standing) or **Mark a miss as not their fault** (signal, outage). This replaces the "check your side" guess and gives Kip something he can act on without a ticket.
2. **Responder: track record that heals.** Misses weigh less than a deliberate turn-down, and standing eases back toward neutral over time (the 2019 TODO). A responder who goes quiet gets a path back, and stays visible, so low standing means fewer first offers, not silence.
3. **Responder: "You're back" notice.** In the phone app, a short message when standing is restored or recovering: *"You'll start getting callouts again."* The responder sees the change, with no score or rank shown.

4. **One-time fresh start for the quiet responders.** Healing only works going forward, so the responders already buried (The Undertow, Farlight, Meteor Mite, Vesper, plus any others the data shows) wouldn't come back for weeks. We'd reset their track record once, to neutral (not the ceiling), after the ping wait and miss weighting are fixed. Otherwise the 60s wait puts them straight back into the slide. The handler sees "fresh start applied" and the responder gets the "you're back" notice. Scope is only those whose standing fell sharply after 4.2, not everyone, so real history isn't erased. It also tests our inference: if pings return, low score was the cause. We'd record pings/week for the four before and after.

**Not building:** a handler-editable routing setting (config ships in the release by design); anything keyed to identity (Security Policy 4.1).

## Success measures (weekly, aggregate, missed and turned-down reported separately)
- Quiet responders (<50% of their prior ping volume) back to within 25% of it within 3 weeks of shipping.
- Support tickets about "no pings" from ~20-32/week toward the pre-4.2 ~6/week.
- Acceptance rate recovers *without* the recovery coming from the four having stopped being pinged (the current headline is partly mechanical).

- The four quiet responders back to within 25% of their pre-4.2 volume within 3 weeks of the reset (volume: ~49 pings/week combined before 4.2, 3 now).

## Open questions and risks
- **Wen Li / Marcus:** are scores stored beyond a restart (`_scores` is in memory only)? Reset at 4.2? Logged per ping? Scores are keyed by responder name, not ID. We need this logged to measure anything. *(inference: score vs proximity explains the quiet; not confirmed)*
- **Reset timing:** `_scores` is in memory only, so a restart or the deploy itself may already wipe scores. Wen to confirm, so the reset is deliberate and we know it happened. Scope (the four vs. wider) is for Wen and Marcus.
- **Ping wait is a separate lever.** I'd still propose reverting 60s to 90s as its own one-variable test with Marcus and you. This PRD is the durable fix, not a substitute. Nobody has recorded why it moved to 60s.
- **Supply coupling:** rebalancing pings changes callout load, which changes when Supply schedules maintenance. Needs a check with the Supply side before launch.
- Ask Wen Li before choosing decay and miss weights (break-even take rate today is 60%; ~64% sits just above it). Values here are not yet proposed.
- Interview two of the heaviest ticket filers (Okafor/Undertow, Pruitt/Farlight) to test the handler wording.

## Next step
Clickable prototype of items 1 and 3 (handler roster + responder notice) for you to click through, using the four real quiet responders as the example. Say go and I'll build it.
