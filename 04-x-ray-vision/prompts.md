# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Tell me more about how track record is calculated

### 2.

but the miss rates were quite low before 4.2, so even with the higher weighting, the net calculated impact was likely softer in practice

### 3.

Even though we don't have real data, run a simulation of data using the before and after weighting with 5 fictional  responders to show what their scores look like

### 4.

Based on what I found, the reason some responders are getting no pings at all is ___, because ___

### 5.

Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?

### 6.

Is there anything else relevant to this question in the codebase worth mentioning to Marcus?

### 7.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 8.

Explain the math behind the breakeven point between accepting and missing/declining a callout

### 9.

Is it a fair conclusion to say that prior to 4.2, the track record variable in the scoring algorithm effectively didn't matter because it was at 1.0 for almost all responders but after 4.2 it dropped to 0 for a few responders creating a massive scoring difference where one didn't exist before, even though the weighting of track record decreased after 4.2

### 10.

Somebody has been quiet for a month. Walk me through, step by step, exactly what would have to happen for them to start getting work again.
