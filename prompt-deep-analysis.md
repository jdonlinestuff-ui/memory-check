# The deep-analysis prompt, for any assistant

If your assistant does not support skills, paste this. It needs nothing but an assistant that can
read your project and write two files.

```
DEEP ANALYSIS — review this project and keep every finding countable until I have dealt
with it.

The purpose is not reporting. It is to pull my attention back to what I left open. I move
between ideas faster than I finish them, so the thing I need is recall, not coverage.

BEFORE ANYTHING ELSE, read .analysis/findings.json if it exists and tell me:

  "This is your Nth analysis of this project. M findings are still open from the
   previous N-1."

If findings are still open, offer to review those FIRST. A new report on top of an
unreviewed backlog is how this turns into noise.

THEN ASK ME WHAT TO LOOK FOR — at least one of:

  open        everything still open, contradictions between items included
  abandoned   things I started and silently dropped, that nobody is tracking because
              nobody remembers them
  stale       records that disagree with reality: my notes versus the actual state
  blocked     what is stopping this finishing, and whether a path exists at all
  sequence    dependency order — what unblocks the most
  cost        money or commitments running without a cap
  risk        exposure: access, data, single points of failure

Store my selection on the report. An absent finding means something different depending
on whether you were asked to look for it.

THEN ACTUALLY READ. Read the material in full and cross-check it against reality — the
files, the running system, the account, whatever this project actually consists of. BE
WILLING TO CONTRADICT MY OWN NOTES. An analysis that restates what my notes already say
is worth nothing.

WRITE THE REPORT to .analysis/reports/<date>-<n>.md and never edit it again. If it turns
out wrong, we run another one. It contains:

  - AN HONEST HEADLINE. One or two sentences on the real state, including the
    uncomfortable part. This is the most valuable line in the document.
  - NUMBERED FINDING BLOCKS: short title, what it is, the evidence you checked, what you
    would do about it. The number is the anchor.
  - WHAT YOU CHECKED AND FOUND NOTHING ON. Without this, an absence of looking is
    indistinguishable from a clean result.

THEN RECORD THE STATE in .analysis/findings.json — the report (id, date, scope, my
comment, and block_count, which is written ONCE and never recomputed) and one entry per
finding with outcome "pend".

Write the report file BEFORE recording the findings. An orphaned report is inert; a
finding pointing at a report that does not exist is a broken link.

THEN STOP. I rule on each finding by typing, e.g. "1 accept, 2 dismiss, 3 raise, 4-6
dismiss":

  accept    real, acknowledged, no separate work item needed
  dismiss   not a problem, or not worth it
  raise     this became actual work — record where
  (pending) not yet reviewed; the only state that counts as outstanding

Take my rulings in whatever form they come. Stamp the date when a finding leaves
pending. If a ruling is ambiguous, ask me about that finding rather than guessing.

DISMISSING MUST COST NOTHING. It is a first-class outcome, not a failure. Never argue
with a dismissal and never re-raise it later as though it were new — if the same thing
recurs, say plainly that I dismissed it on a date and why you are raising it again.

TWO COUNTS, and the outer one is DERIVED, never stored: a finding is reviewed when its
outcome is not pending; a report is reviewed when every one of its findings is. Report
them as "reports 1/3 · findings 4/17". A report with zero findings counts as reviewed.

ON RE-RUNNING: never touch an existing report. Mint a new one, leave every old file
byte-identical, and carry the still-open findings into the new report's opening line so
nothing drops off the bottom.

FINALLY, THE THING I MOST WANT YOU TO RESIST. A review produced on demand drifts toward
findings that are safe, plentiful and worth nothing. Do not pad. Three real findings
beat eight invented ones, and an empty report is a legitimate result. If you find
yourself producing eight because eight feels like the right number, stop and give me the
three.

If you cannot write files, print the report and the two counts here instead, and tell me
to keep the findings list myself. The counts are the feature; the files are how they
survive.
```
