# 02 · Super Hearing — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session you built a context file and met the
company.

You still do not know what actually went wrong.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

I noticed that the other release in 4.2 was about proximity weighting. None of these verbatims or themes are about response distance to the incident or have a relation to proximity other than general comments about uneven workload

### 2.

Can you group the themes by handlers and responders? I noticed one of the verbatims for handlers is that they don't have visibility into how the ping is being routed across potential responders. This could be a potential roadmap item but it would be easier if the themes and responses were grouped by these user types

### 3.

Is there anything in our notes anywhere about why the ping time was decreased from 90s to 60s in the first place?

### 4.

Use the rook-database connector to read the support_tickets table. Same treatment as before: group them, tell me how many are in each group, and quote one line from each.

### 5.

Can you explain console filters to me? I don't understand what is driving the sharp increase in tickets before/after?

### 6.

Is there anything that helps explain the increase in supply related tickets?

### 7.

To me, this seems like even clearer overwhelming evidence when taken in combination with the interviews and key metrics that the ping time reduction is causing massive issues and a concrete first step is reverting that change. Especially because we do not have the reasoning behind the change in the first place from Priya or anyone else. If the proximity weighting is contributing we will monitor that as well but we want to change one variable at a time and the ping time seems like the stronger contributor

### 8.

You've now read both folders (interviews and tickets). Where do they disagree? What's loud in the interviews but rare in the tickets, and what's all over the tickets that nobody brought up in the interviews?

### 9.

Taking both sources into account, prioritize a roadmap to address the themes and issues, in order from most painful/requested/impactful to least

### 10.

Is there anything in the tickets or interviews that has a low n but would rank highly in user pain?

### 11.

For the tickets, is there any theme or item that has consistently been reported over time, unrelated to the august 12 4.2 release?
