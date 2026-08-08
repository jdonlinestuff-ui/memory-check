# memory-check
<img width="800" height="533" alt="image" src="https://github.com/user-attachments/assets/b329e5fd-5e2a-45ba-9e34-df2b194a243f" />

**Two skills for finding out what your AI assistant — and your project — quietly stopped telling
you.**

| skill | asks |
|---|---|
| [`memory-check`](memory-check/SKILL.md) | What does my assistant believe that is no longer true, and what rules does it think I gave it? |
| [`deep-analysis`](deep-analysis/SKILL.md) | What did I leave open on this project, and how far through dealing with it am I? |

They share one pattern: produce a numbered report, **change nothing**, take rulings as typed
text, then apply. Use either on its own.

---

## Install

**Claude Code, or anything that reads `SKILL.md` folders:**

```bash
git clone https://github.com/jdonlinestuff-ui/memory-check.git
cp -r memory-check/memory-check memory-check/deep-analysis ~/.claude/skills/
```

Then say `memory check` or `deep analysis`.

**Any other assistant:** paste [`prompt.md`](prompt.md) or
[`prompt-deep-analysis.md`](prompt-deep-analysis.md). Neither needs anything installed — just an
assistant with access to the thing it is checking.

---

## memory-check

**Audits your AI assistant's memory before you act on it again.**

If your assistant keeps memory between sessions, some of it is already wrong. Not because
anyone made a mistake — because it was **true when it was written** and something changed
underneath it. That is the dangerous kind: a fact that was never wrong reads exactly like a fact
that is still right, and it gets acted on without anyone re-reading it.

There is a second problem nobody looks at. Your assistant is also holding a set of **rules it
believes you gave it** — things it now does without asking. You have never seen that list. Some
of it will be phrased more strongly than you meant, or more weakly.

This asks for both, in one pass, and stops before changing anything.

### What you get

One report in two sections — stale facts, and standing rules — written to a self-contained HTML
file you can open offline. Every row is numbered. You rule on each by typing:

```
1 accept, 2 use new, 3-5 later
```

Only then does it edit anything.

`later` is a ruling, not a pending item. It closes the row, so a review can be finished even
when you choose to do nothing.

---

### Why each part is there

**"Test against what is true now, not against another memory file."** Memory agrees with itself
by construction — one file copies another. Only the system outside it can disagree.

**"Grep every memory file for the dead word."** Fixing the one place you noticed guarantees the
claim comes back from the others. A single rename can leave the same dead word in five files.

**"A superseded claim left as a file's opening sentence."** Corrections get appended; the lead
sentence is what gets read and copied. One estate had a memory file that opened with *"no
compute at all"* and corrected itself a paragraph later — the lead is what propagated, and it
ended up asserted about the account running the very database it lived in.

**"Claims about what is missing."** The most confidently wrong entries in any memory, because
absence is asserted from one failed search and never rechecked.

**"An inference of my intent recorded as if it were my instruction."** The worst single entry
you can have, because it reads as a decision and nobody re-reads a decision.

**"What it makes you do — and not do."** A rule you cannot state as a behaviour is not a rule,
it is a mood. This column is where you find out that *"prefer the cheaper option"* has quietly
become *"never ask before spending under $10"*.

**"No item appears in both sections."** Otherwise you rule on the same thing twice and the two
answers can disagree.

**"Not a clean bill of health."** An absence of looking is indistinguishable from a clean
result. Make it say what it checked.

**"Do not soften a rule while rewording it."** Left unsaid, a rule gets a little more
comfortable on each pass until it is not your rule any more.

### What to expect

On a first run over a few dozen memory files, expect a handful of conflicts and for **most of
them to be inventories** — counts, versions, commit ids, endpoint lists. The fix is usually not
to correct the number. It is to delete it and point at where the number is queried, because the
same entry will be wrong again next week.

Expect the rules section to be longer than you think, and expect at least one rule to be phrased
in a way you would not have chosen. That one is the point of Section 2.

---

## deep-analysis

**A dated review of a project that stays countable until you have dealt with every finding.**

Built for people who move between ideas faster than they finish them. Most review output is read
once and lost; this produces the opposite — a report that never changes, and findings that keep
their own state so the tool can open with:

> *This is your 3rd analysis of this project. **6 findings are still open** from the previous two.*

That sentence is the entire feature. Everything in the skill exists to make it true.

### The one design decision worth understanding

**A report and its findings have different lifecycles.** A report is an artefact as at a date and
never changes. A finding is a live record: pending, accepted, dismissed, or raised as work.

Conflating them is the mistake. If re-running overwrote the last report, `3/8 reviewed` would be
meaningless — reviewed against what? So every run mints a new report, every old one stays
readable, and findings outlive the report that carried them. It needs two files:

```
.analysis/reports/2026-08-08-1.md    write once, never edited
.analysis/findings.json              live state, one record per finding
```

### Scope

You pick what it looks for, and the choice is **stored on the report** — because an absent
finding means something different depending on whether the analysis was asked to look for it.

`open` · `abandoned` · `stale` · `blocked` · `sequence` · `cost` · `risk`

**`abandoned` is the one that serves the point** — things started and silently dropped, the ones
nobody is tracking because nobody remembers them.

### Why it is built to resist itself

Anything that generates reports on demand creates pressure toward the templated version:
findings that are safe, plentiful, and worth nothing. What makes an analysis worth reading is
that its headline is uncomfortable, and no mechanism preserves that by itself. Three choices push
against it, and none is optional:

1. **Nothing counts reports generated.** Only findings, and only their review state. No number
   anywhere rewards producing more analyses.
2. **A dismissed finding costs nothing.** Dismissal is a first-class outcome, not a failure — a
   reviewer who cannot dismiss freely stops reviewing, and then the counter means nothing.
3. **A report where everything was dismissed stays visible.** That is the signal an analysis was
   noise, and it should be legible without going looking for it.

An empty report is allowed. Three real findings beat eight padded ones.

### Two smaller rules that are easy to get wrong

**Never store "this report is reviewed".** Derive it — a stored flag can say done while three of
its own findings sit pending, and there is no honest way to reconcile that afterwards.

**Never recompute the denominator.** It has to describe the report as issued, or every historical
`3/8` silently becomes something else the next time you change how blocks are counted.

---

## Want a UI instead of a chat?

**Both skills deliberately ship as the basic version** — a prompt, a report, and rulings typed as
text. That works on every platform and needs nothing installed, which is the point.

If you would rather click than type, hand your assistant one of the blocks below. They describe
the review surface, **not** the check — the checks above stay exactly as they are. Each one names
the failure modes that bit the original, because those are the parts you would otherwise discover
the expensive way.

### For memory-check

```
Take my memory-check skill and give it a review UI.

Keep the audit itself untouched. What I want changed is only how I rule on the rows.

Build the report as a single self-contained HTML file — inline CSS and JS, no CDN, no
external fonts — that I can open by double-clicking. For each row show the old claim,
what is true now, the evidence, and your recommendation. Give each row four radio
buttons:

  DECIDE LATER · USE NEW · USE OLD · ACCEPT RECOMMENDATION

with DECIDE LATER pre-selected, so a row I skip is still a decision rather than a gap.
Put a free-text reply box on every row for when none of the four is what I mean. Add one
SAVE button that writes every ruling at once.

Four things that will bite you if you skip them:

1. PIN THE HEADER AND THE SAVE BUTTON. Cap the dialog height and let the table scroll
   inside it. A seven-row review pushed the save button below the fold once and I chose
   every answer, typed replies, and had nothing to press — and the typed replies were
   lost with that attempt.
2. SAVE THE TYPED REPLIES WITH THE RULINGS, in the same write. They are the part I
   cannot reconstruct.
3. Make the recommendation its own option, not a rewording of new-or-old. It is
   frequently neither.
4. Decide where SAVE writes to before you build anything else — a file beside the
   report, or an endpoint. A UI that collects rulings it cannot store is worse than a
   chat.

Show me the write target first and let me confirm it before you build the rest.
```

### For deep-analysis

```
Take my deep-analysis skill and give it a UI.

Keep the analysis itself untouched. What I want changed is how I request one and how I
rule on the findings.

THE REQUEST SIDE. A [Deep Analysis] button that opens a popup. Before anything else the
popup says, in plain words: "Your 3rd analysis of this project. 6 findings still open
from the previous two." Then a multi-select of the scope options with at least one
required, and a free-text box for what is on my mind right now. Store both on the
report.

THE REVIEW SIDE. A list of reports newest first, each showing its own outcome
breakdown, and a counter reading "reports 1/3 · findings 4/17". Opening a report shows
its finding blocks, each with four buttons — PENDING · ACCEPT · DISMISS · RAISE AS WORK
— and a free-text box for the ones where none of the four is what I mean.

Five things that will bite you if you skip them:

1. THE REPORT PAGE IS READ-ONLY AND STAYS THAT WAY. The UI renders it; the UI never
   writes to it. Make immutability a property of the system, not a promise about
   behaviour — if the code cannot edit an old report, it cannot silently rewrite
   history and make every counter a lie.
2. NEVER STORE "this report is reviewed". Derive it from its findings every time. A
   stored flag will eventually say done while three of its own findings sit pending,
   and there is no honest way to reconcile that afterwards.
3. RAISE AS WORK MUST BE AN EXPLICIT COPY into wherever I actually track work, never an
   implicit one. That moment is a machine finding becoming work I have accepted, and it
   should cost a deliberate click. If you cannot write to my task list yet, ship the
   button DISABLED WITH THE REASON ON IT rather than leaving it out — and put the
   outcome and the reference in the data model from the start so no migration is needed
   later.
4. PIN THE HEADER AND THE SAVE BUTTON, cap the height, scroll the list inside it. A
   seven-row review pushed the save button below the fold once and I chose every
   answer, typed replies, and had nothing to press. The typed replies were lost.
5. DO NOT ADD A COUNTER FOR REPORTS GENERATED. Count findings and their review state,
   nothing else. Any number that rewards producing more analyses will get me more
   analyses and worse ones.

Show me where SAVE writes before you build the rest.
```

---

## Licence

MIT. See [LICENSE](LICENSE).

*This started as the prompt behind a MEMORY CHECK button in a private dashboard. The button, the
review popup and the audited write-back are specific to that setup; the check itself is not, and
is published here on its own.*
