# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
What do we think about exposing the responder score to the handler and/or responder? Similar to how Uber exposes driver and rider scores to each side for transparency

### 2.
Nope, just brainstorming ideas. Should this brief also include the proposal for a one-time score reset due to the overly aggressive scoring decay to accelerate getting quiet responders back into good standing faster?

### 3.
Save the brief exactly as it stands now as 05-super-speed/brief.md. Show me the file when it's done.

### 4.
Take the brief you just wrote and build me a working prototype, an actual screen I can click through, not a description of one. Save it as 05-super-speed/prototype.html, a single file I can just open in my browser. Show me where this would actually happen, and make at least one thing on it respond when I click it.

### 5.
I think the status column "quiet since 4.2" is too "inside baseball". I would rather have it be a trend over a timeframe vs tied to a product release

### 6.
Do we want the handlers to have the ability to offer a fresh start? I thought we wanted to provide the handlers insight but let the scoring algorithm be less biased than a handler who may constantly reset their responders score

### 7.
"Right now a missed ping counts against a responder the same as turning one down, and the wait to answer is short, so misses add up quickly."

this is overexplaining, the other detail is enough to give some visibility to the handler

### 8.
so to summarize our fix to leadership, we enhanced visibility for both handlers and responders. Without changing numbers, whether it be the ping time or scoring adjustments, some root cause may not be fixed yet but at least the awareness and frustration can be addressed via the increased visibility

### 9.
so should the ping time reversion be part of this brief? I thought the explicit instruction were not to "change a number"

### 10.
I understand the visibility only comment goes too far but you cant observe the healing track record in the prototype that was built

### 11.
The status column is confusing because it has a mix of numerical data and then sometimes switches to text only with "in good standing" etc. I think this may require two types of status column so one can stay numeric and one can have detail text

### 12.
Can you just in general improve the quality and richness of both UIs?

### 13.
Add a breakout in the handler UI to show taken, declined and missed as separate counts
