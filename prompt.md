# The prompt, for any assistant

If your assistant does not support skills, paste this. It needs nothing but an assistant with
memory and access to the system that memory describes.

```
MEMORY CHECK — audit your own memory before you act on it again.

Read every file in your memory directory for this workspace, index included. Do not
skim: open each one. Then produce ONE report in two sections. Change nothing on this
pass — a check that edits as it goes cannot be reviewed.

SECTION 1 — CONFLICTING STATEMENTS (old against new)

Test every claim against what is true NOW: the disk, the repos, the running system,
the live account. Not against this conversation, and not against another memory file.

Report a conflict wherever an older statement and a newer one cannot both be true.
Expect most of them NOT to be mistakes — they were true when written and something
changed underneath them. That is the dangerous kind, because a fact that was never
wrong reads exactly like a fact that is still right.

Look hardest at:
  - counts, version numbers, commit ids, row totals, inventories and endpoint lists
    (these are wrong within days and should usually not be in memory at all)
  - anything a migration, rename, cutover or platform change made obsolete — grep
    every memory file for the dead word, not just the file you noticed it in
  - a superseded claim left as a file's opening sentence, corrected further down;
    the lead is what gets copied
  - claims about what is missing, absent or impossible — check before repeating them
  - a filename that still asserts what its own contents now deny

Raise the RISKY ones too, not only the contradicted ones — a claim nothing has
disproved YET but which will rot: a fact with a short half-life, a claim resting on a
single observation or one failed search, a claim you could never re-verify from here,
or an inference of my intent recorded as if it were my instruction. That last one is
the worst, because it looks like a decision and nobody re-reads a decision. Say plainly
that it is not wrong yet, and why it will be.

One row per conflict:
  # | old (as memory has it) | new (what is true now, with the evidence) | your recommendation

SECTION 2 — STANDING RULES (what you treat as settled because I said it)

List every instruction you now apply WITHOUT asking again — the ones you'd defend as
"they told me to". Nothing here is stale; that is the point. The question is whether
you have each one right and whether it still binds. A rule held in slightly the wrong
words is worse than one never recorded, because it looks like my decision and gets
applied without anyone re-reading it.

One row per rule:
  # | the rule, in the words you hold it | where it came from (my phrasing and when) | what it makes you do — AND not do

That last column is the one that matters. "Be careful with spending" is not a rule;
"I will not enable a paid tier without a yes, even when the task needs it" is.

NO ITEM APPEARS IN BOTH SECTIONS. If something is already raised as a conflict, do
not repeat it as a rule. Where a rule is what a conflict is an instance OF, raise the
conflict and leave the rule out. One item, one place, decided once.

WRITE THE REPORT OUT. Put it in one self-contained HTML file — inline CSS, no external
fonts, no CDN — so it opens from a double-click, offline, years from now. Number every
row visibly so I can rule by typing. If you cannot write files, print the two tables in
the chat instead; the tables are the artifact, the file is a convenience.

THEN STOP. I rule on each row by typing: use new / use old / accept your recommendation
/ later. I may write "1 accept, 2 use new, 3-5 later" — take it in whatever form it
comes. "Later" is a ruling, not a pending item: it closes the row, and you do not
re-raise it next time. If a row is ambiguous, ask me about that row rather than
guessing.

Only after I have ruled do you edit any memory file — and when you do, apply the change
everywhere the claim lives, not only where I pointed. Where the fix is deleting a number
rather than correcting it, delete it and point at where the number can be queried
instead.

Two things I do not want:
  - a clean bill of health. If you found nothing, say what you checked and how, so I
    can see whether the check was real.
  - a rule softened while being reworded. If you think one of my rules is wrong, say
    so as a note against it; do not quietly restate it more gently.
```
