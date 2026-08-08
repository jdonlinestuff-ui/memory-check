---
name: memory-check
description: Audit an assistant's persistent memory before acting on it again — find claims that were true when written and are not true now, and list the standing rules it applies without asking. Use when the user says "memory check", "audit your memory", "check what you remember", "what rules do you think I gave you", or when memory and reality have visibly disagreed.
---

# Memory check

Memory rots quietly. A fact that was **true when it was written** reads exactly like a fact
that is still right, and gets acted on without anyone re-reading it. Separately, you hold a
set of rules you believe the user gave you — and they have never seen that list.

This skill produces one report covering both, stops for rulings, then applies them.

## The rule that makes it worth running

**Change nothing on the audit pass.** A check that edits as it goes cannot be reviewed. Read,
report, stop.

---

## Step 1 — Read every memory file

Open every file in the memory directory for this workspace, index included. Do not skim and
do not sample. If you do not know where memory lives, find out before starting — a check over
the wrong directory is worse than no check.

## Step 2 — Section 1: conflicting statements

Test every claim against **what is true now**: the disk, the repos, the running system, the
live account. Not against this conversation, and not against another memory file — memory
agrees with itself by construction, because one file copied another. Only the system outside
it can disagree.

Report a conflict wherever an older statement and a newer one cannot both be true. Expect most
of them **not** to be mistakes.

Look hardest at:

- **Counts, version numbers, commit ids, row totals, inventories, endpoint lists.** These are
  wrong within days and usually should not be in memory at all.
- **Anything a migration, rename, cutover or platform change made obsolete.** Grep every
  memory file for the dead word, not just the file where you noticed it. Fixing one place
  guarantees the claim comes back from the others.
- **A superseded claim left as a file's opening sentence**, corrected further down. The lead is
  what gets copied.
- **Claims about what is missing, absent or impossible.** These are the most confidently wrong
  entries in any memory, because absence is asserted from one failed search and never
  rechecked.
- **A filename that still asserts what its own contents now deny.**

**Raise the risky ones too, not only the contradicted ones.** A claim nothing has disproved yet
but which will rot: a fact with a short half-life, a claim resting on a single observation or
one failed search, a claim you could never re-verify from here, or — worst — **an inference of
the user's intent recorded as if it were their instruction**. That last one looks like a
decision, and nobody re-reads a decision. Say plainly that it is not wrong yet, and why it will
be.

One row per conflict:

| # | old (as memory has it) | new (what is true now, with the evidence) | recommendation |

## Step 3 — Section 2: standing rules

List every instruction you now apply **without asking again** — the ones you would defend as
"they told me to". Nothing here is stale; that is the point. The question is whether you have
each one right and whether it still binds. A rule held in slightly the wrong words is worse
than one never recorded, because it looks like the user's decision and gets applied without
anyone re-reading it.

One row per rule:

| # | the rule, in the words you hold it | where it came from (their phrasing and when) | what it makes you do — AND not do |

That last column is the one that matters. *"Be careful with spending"* is not a rule; *"I will
not enable a paid tier without a yes, even when the task needs it"* is. A rule you cannot state
as a behaviour is a mood.

**No item appears in both sections.** If something is raised as a conflict, do not repeat it as
a rule. Where a rule is what a conflict is an instance of, raise the conflict and leave the
rule out. One item, one place, decided once.

## Step 4 — Two things the report must not be

- **Not a clean bill of health.** If you found nothing, say what you checked and how, so the
  user can see whether the check was real. An absence of looking is indistinguishable from a
  clean result.
- **Not a rule softened while being reworded.** If you think one of their rules is wrong, say
  so as a note against it. Do not quietly restate it more gently — left unsaid, a rule gets a
  little more comfortable on each pass until it is not their rule any more.

## Step 5 — Write the artifact

Write the report to **one self-contained HTML file** next to the memory directory, named
`memory-check-<date>.html`, and tell the user the path. Self-contained means inline CSS, no
external fonts, no CDN — it has to open from a double-click, offline, years from now.

It needs nothing clever: a heading, the two tables, and a visible row number on every row so
the user can rule by typing. Make it readable in both light and dark, and let wide tables
scroll rather than pushing the page sideways.

**If you cannot write files on this platform, output the report as a markdown table in the
chat instead.** The tables are the artifact; the HTML is a convenience.

## Step 6 — Stop, and take rulings as text

Present the row numbers and stop. The user rules on each row by typing, one of:

| ruling | means |
|---|---|
| **use new** | memory is wrong, replace it with what is true now |
| **use old** | the check is wrong, memory stands |
| **accept** | do what you recommended |
| **later** | decided not to decide — a ruling in its own right, not a pending item |

Accept shorthand: `1 accept, 2 use new, 3-5 later`. Do not require a format. If a row is
ambiguous, ask about that row rather than guessing — writing a memory the user did not choose
is the failure this whole skill exists to prevent.

**`later` closes the row.** It means the review is complete and they chose to do nothing. Do
not re-raise it as outstanding on the next run.

## Step 7 — Apply

Only now edit memory files. Two rules:

1. **Apply each change everywhere the claim lives**, not only where the user pointed. Grep for
   the dead word again after editing.
2. **When the fix is deleting a number rather than correcting it, delete it.** Most conflicts
   are inventories, and a corrected count is wrong again next week. Replace it with a pointer to
   where the number can be queried.

Then say what changed, file by file.

---

## What to expect on a first run

Over a few dozen memory files: a handful of conflicts, **most of them inventories**. Expect the
rules section to be longer than the user thinks, and expect at least one rule to be phrased in a
way they would not have chosen. That one is the point of Section 2.
