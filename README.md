# memory-check

**A skill that audits your AI assistant's memory before you act on it again.**

If your assistant keeps memory between sessions, some of it is already wrong. Not because
anyone made a mistake — because it was **true when it was written** and something changed
underneath it. That is the dangerous kind: a fact that was never wrong reads exactly like a fact
that is still right, and it gets acted on without anyone re-reading it.

There is a second problem nobody looks at. Your assistant is also holding a set of **rules it
believes you gave it** — things it now does without asking. You have never seen that list. Some
of it will be phrased more strongly than you meant, or more weakly.

This asks for both, in one pass, and stops before changing anything.

---

## Install

**Claude Code, or anything that reads `SKILL.md` folders:**

```bash
git clone https://github.com/jdonlinestuff-ui/memory-check.git
cp -r memory-check/memory-check ~/.claude/skills/
```

Then say `memory check`.

**Any other assistant:** paste [`prompt.md`](prompt.md). It needs nothing but an assistant with
memory and access to the system that memory describes.

## What you get

One report in two sections — stale facts, and standing rules — written to a self-contained HTML
file you can open offline. Every row is numbered. You rule on each by typing:

```
1 accept, 2 use new, 3-5 later
```

Only then does it edit anything.

`later` is a ruling, not a pending item. It closes the row, so a review can be finished even
when you choose to do nothing.

---

## Why each part is there

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

## What to expect

On a first run over a few dozen memory files, expect a handful of conflicts and for **most of
them to be inventories** — counts, versions, commit ids, endpoint lists. The fix is usually not
to correct the number. It is to delete it and point at where the number is queried, because the
same entry will be wrong again next week.

Expect the rules section to be longer than you think, and expect at least one rule to be phrased
in a way you would not have chosen. That one is the point of Section 2.

---

## Want it as a button instead of a chat?

This repo deliberately ships the basic version: a prompt, a report, and rulings typed as text.
That works on every platform and needs nothing installed.

If you would rather click than type, hand your assistant the block below. It describes the
review UI, not the check — the check above stays exactly as it is.

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

---

## Licence

MIT. See [LICENSE](LICENSE).

*This started as the prompt behind a MEMORY CHECK button in a private dashboard. The button, the
review popup and the audited write-back are specific to that setup; the check itself is not, and
is published here on its own.*
