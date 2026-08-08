---
name: deep-analysis
description: Produce a dated, immutable analysis of a project and track each finding until it is dealt with — built for people who move between ideas quickly and lose track of what they left open. Use when the user says "deep analysis", "what have I forgotten", "what's still open", "review this project properly", "what did I leave half-done", or asks how far through a previous analysis they got.
---

# Deep analysis

Most review output is read once and lost. This produces the opposite: a report that never
changes, and a set of findings that stay countable until each one is dealt with.

**The purpose is not reporting. It is a point of presence** — something that pulls attention
back to what was left open. It is built for the person who jumps between ideas and cannot hold
all of them at once, which means the design goal is *recall*, not coverage. Protect that property
above everything else when trading anything off.

---

## 1. The decision that shapes everything else

**A report and its findings have different lifecycles, and conflating them is the mistake.**

- **A report is an artefact as at a date.** What was true when it was written. It never changes.
- **A finding is a live record with state.** Reviewed, dismissed, raised as work.

You can only answer *"you have 6 items still open from your previous two analyses"* if findings
outlive the report that carried them. That sentence is the entire feature. Everything below
exists to make it true.

**Immutability is the crux.** If re-running an analysis overwrote the last one, `3/8 reviewed`
would be meaningless — reviewed against what? **Every run mints a new report and every earlier
report stays readable, or the counter is a lie.**

## 2. Where things live

Two things, because two are genuinely needed and one is not enough:

```
.analysis/
  reports/2026-08-08-1.md     write once, never edited, never deleted
  findings.json               live state, one record per finding
```

`findings.json`:

```json
{
  "reports": [
    { "id": "2026-08-08-1", "at": "2026-08-08", "scope": ["open", "abandoned"],
      "comment": "what the user asked for at request time", "block_count": 8 }
  ],
  "findings": [
    { "id": "2026-08-08-1#3", "report": "2026-08-08-1", "n": 3,
      "title": "short line, the same one used as the heading in the report",
      "severity": "high", "outcome": "pend",
      "raised_ref": null, "reviewed_at": null, "note": null }
  ]
}
```

**`block_count` is written once and never recomputed.** The denominator has to describe the
report as issued. If you later change how you count blocks, every historical `3/8` silently
becomes something else.

**Write the report file before recording its findings.** An orphaned report is inert and costs
nothing; a finding pointing at a report that does not exist is a broken link the user will hit.

## 3. Four outcomes, and dismissing must be free

| outcome | means |
|---|---|
| `pend` | not yet reviewed — the default, and the only one that counts as outstanding |
| `accepted` | real, acknowledged, no separate work item needed |
| `dismissed` | not a problem, or not worth it |
| `raised` | became actual work; put the reference in `raised_ref` |

**A dismissed finding costs nothing and is not a failure.** A reviewer who cannot dismiss freely
will stop reviewing, and then the counter stops meaning anything. Never argue with a dismissal
and never re-raise it in a later report as though it were new — if the same thing recurs, say
explicitly that it was dismissed on a previous date and why you are raising it again.

## 4. "Reviewed" exists at two levels, and the outer one is derived

Both numbers are wanted, and they answer different questions — *have I dealt with this report at
all*, and *how far through it am I*.

- **A finding is reviewed** when its outcome is not `pend`.
- **A report is reviewed** when **every** finding in it is reviewed.

**Never store the report-level flag.** A stored flag can disagree with its own findings — one
record saying done while three blocks sit at `pend` — and there is no honest way to reconcile
that afterwards. Compute it every time. It costs one pass over the array and cannot drift.

Report it as two pairs: `reports 1/3 · findings 4/17`.

**A report with zero findings counts as reviewed.** An analysis that found nothing is a
legitimate and useful outcome, and it must not sit there forever waiting for an action that does
not exist.

## 5. Running one

### Step 1 — Say what is already open, before doing anything

Open `findings.json` first and lead with the carry-forward:

> *This is your 3rd analysis of this project. **6 findings are still open** from the previous two.*

If there are open findings, **offer to review those before generating more.** A new report on top
of an unreviewed backlog is how the whole thing turns into noise.

### Step 2 — Agree the scope

Multi-select, at least one. The scope is stored on the report, because **an absent finding means
something different depending on whether the analysis was asked to look for it.**

| key | reads for |
|---|---|
| `open` | everything still open, contradictions between items included |
| `abandoned` | things started and silently dropped — the ones nobody is tracking because nobody remembers them |
| `stale` | records that disagree with reality: the notes versus the actual state |
| `blocked` | what is stopping this finishing, and whether a path exists at all |
| `sequence` | dependency order — what unblocks the most |
| `cost` | money or commitments running without a cap |
| `risk` | exposure: access, data, single points of failure |

`abandoned` is the one that serves the stated purpose most directly. Suggest it by default.

### Step 3 — Actually read

Read the material in full. Cross-check it against reality — the files, the running system, the
account, whatever the project actually consists of — **and be willing to contradict the project's
own notes.** An analysis that only restates what the notes already say is the templated version,
which is worth nothing. See §7.

### Step 4 — Write the report

One markdown file at `.analysis/reports/<date>-<n>.md`. Never edit it afterwards; if it is wrong,
run another one. That is what immutability buys.

Structure:

- **An honest headline.** One or two sentences saying the real state, including the
  uncomfortable part. This is the single most valuable line in the document and the first thing
  that degrades if you get comfortable.
- **Numbered finding blocks**, each with: a short title, what it is, the evidence you checked,
  and what you would do about it. The number is the anchor and it must match `n` in
  `findings.json`.
- **What you checked and found nothing on.** Without this, an absence of looking is
  indistinguishable from a clean result.

Then append the report and its findings to `findings.json`, all outcomes at `pend`.

### Step 5 — Stop, and take rulings as text

Present the numbers and stop. The user rules by typing:

```
1 accept, 2 dismiss, 3 raise, 4-6 dismiss
```

Take it in whatever form it comes. Do not require a format. Stamp `reviewed_at` when an outcome
leaves `pend`. If a ruling is ambiguous, ask about that finding rather than guessing.

**If you cannot write files on this platform**, print the report and the two counts in the chat,
and ask the user to keep the findings list somewhere themselves. The counts are the feature; the
files are how they survive.

## 6. Re-running

A new run **never** touches an existing report. It mints a new id, and the previous report file
must be byte-identical afterwards. Carry the open findings from earlier reports into the opening
line of the new one, so nothing quietly drops off the bottom.

## 7. The failure mode this is most exposed to

**Anything that generates reports on demand creates pressure toward the templated version** —
findings that are safe, plentiful, and worth nothing. What makes an analysis worth reading is
that its headline is uncomfortable, and that is not a property any mechanism preserves by itself.

Three deliberate choices push against it, and none of them is optional:

1. **Nothing counts reports generated.** Only findings, and only their review state. There must
   be no number anywhere that rewards producing more analyses.
2. **A dismissed finding costs nothing.** `dismissed` is a first-class outcome, not a failure.
3. **A report where everything was dismissed stays visible.** Show outcomes per report, not only
   summed. That is the signal an analysis was noise, and it should be legible without anyone
   going looking for it.

If you find yourself producing eight findings because eight feels like the right number, you have
already failed. Three real ones beat eight padded ones, and an empty report is allowed.

## 8. Deliberately out of scope

- **Notifications.** Checking the file is the point of presence. A notification replaces a habit
  with an interruption.
- **Editing a report.** If it is wrong, run another one.
- **Automatic runs.** An analysis nobody asked for is an analysis nobody reads.

## 9. Checks worth running once

| property | how you prove it |
|---|---|
| Reports are immutable | Re-run; the previous report file is byte-identical afterwards |
| The counter is honest | The denominator equals `block_count` as issued, not a live count |
| Nothing is lost on re-run | Open findings from earlier reports appear in the new report's opening line |
| The check was real | Every report has a "checked and found nothing on" section |
