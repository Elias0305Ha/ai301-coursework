# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

Where it lives: in the repro report, in the environment or setup block,
usually near the top and before the steps. In live mode, the same block
in the draft repro comment.

What good looks like: the OS, the language runtime version, and a
commit, branch, or dependency version are each named as concrete
values, not ranges or "latest". Where the issue names a target
environment, the values either match it or the report calls out the
difference.

## Steps

Where it lives: in the repro report, in the steps or reproduction
section, after the environment block. In live mode, the same section in
the draft repro comment.

What good looks like: the steps name a starting point (a fresh clone,
a stated directory), include every setup action needed before the
failure, and give commands exactly as typed. A reader following them in
order reaches the failure without supplying anything of their own.

## Behavior shown

Where it lives: in the repro report, in the pasted output, log excerpt,
traceback, or screenshot that follows the steps. Read it against the
issue context section, which states the failure the issue is about.

What good looks like: the artifact names the same error and the same
location the issue names. An artifact showing a different failure, such
as a missing dependency or a setup mistake, does not show the issue's
behavior no matter how real the error is.

## Honesty

Where it lives: in the repro report's stated conclusion, usually its
opening or closing line, read together with the artifact that follows
or precedes it.

What good looks like: the conclusion and the evidence agree. A stated
reproduction is followed by an artifact showing the issue's failure. A
stated cannot-reproduce is followed by what was attempted and what
happened instead. A conclusion with nothing supporting it claims more
than the package shows.

## Comms

What good looks like: every requirement the repo states is visibly met
in the comments themselves. A disclosure policy is satisfied only by an
explicit statement naming the tool and the extent of its use; silence
fails it, however strong the rest of the package is. The claim comment
names this issue's own specifics rather than boilerplate that would fit
any issue, and promises investigation rather than a fix or a date.