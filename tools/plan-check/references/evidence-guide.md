# Evidence guide: where plan evidence lives in a package

## Package map

A package is one markdown file with these parts, in this order:

- Repo facts: the repo's contribution policy, bug report template, release info
- Issue: the original report and its labels
- Thread highlights: comments on the issue (can be empty)
- Repro evidence: the environment, steps, expected and actual from the accepted repro
- Candidate plan: Cause, Change (with In and Out), Test
- Candidate plan comment: the text that would be posted on the issue

The bundle is the whole world. Quote only from it.

## diagnosis-matches-evidence

Lives in: the Cause line of the Candidate plan, read against Repro evidence (steps and Actual).
Good looks like: the cause explains the exact behavior in Actual, including the detail that it fixes itself after leaving and re-entering the view. Bad looks like: a cause that fits a different bug, ignores a step in the repro, or quotes no repro evidence at all.

## cause-not-symptom

Lives in: the Change line of the Candidate plan, read against its Cause line.
Good looks like: the change fixes the thing the cause names (for example, adding the missing refresh in the callback that skips it). Bad looks like: a change that hides the symptom (forcing a redraw on a timer, special-casing the display) while the stale data stays.

## scope-bounded

Lives in: the In and Out lines under Change in the Candidate plan.
Good looks like: one change in a named place, with a real Out list that names what stays untouched. Bad looks like: no Out list, extra cleanups or refactors bundled in, or many files when the cause needs one.

## stranger-could-start

Lives in: the Change line (file path and what to add) plus the Cause line.
Good looks like: you can name the first file to open and the first edit without asking anything, like a file path and the callback to change. Bad looks like: "fix the refresh logic" with no file or location.

## test-plan-observable

Lives in: the Test line of the Candidate plan, read against the Steps and Expected in Repro evidence.
Good looks like: it re-runs the repro steps and names the exact step where a visible result must change (for example, step 3 shows the pushed color without leaving the view). Bad looks like: "verify it works", or a test that never touches the repro steps.

## unknowns-honest

Lives in: any hedging or open questions in the Candidate plan and the plan comment. The package may have no separate risks section, so also scan the Cause line for guesses written as facts.
Good looks like: gaps the repro leaves open (for example, whether other callers share the code path) are named as things to check. Bad looks like: a guess stated as certain, or a confident cause with nothing the repro actually proved behind it.

## comment-fits-thread-and-conventions

Lives in: the Candidate plan comment, read against Thread highlights and the contribution policy line in Repo facts.
Good looks like: the comment answers anything the thread asks and visibly respects each stated repo rule (for example, the note about limited PR review time, or any required disclosure). An empty thread and no stated requirement is a pass. Bad looks like: ignoring a question in the thread, or skipping something the repo requires, like a disclosure.

## comment-matches-plan

Lives in: the Candidate plan comment, read against the Candidate plan.
Good looks like: same cause, same scope, same test as the plan. Bad looks like: the comment promises more, or something different, than the plan.
