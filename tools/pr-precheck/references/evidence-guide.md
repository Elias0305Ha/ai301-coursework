# Evidence guide: where evidence lives in a PR package

A package has these parts, in order: Repo facts, Issue, Thread highlights, Plan context, then Candidate PR (Title, Description, Commits, Diff, Test evidence). In eval mode the bundle is the whole world. In live mode the same evidence lives in the working copy and on GitHub, as listed under each heading.

## Plan fidelity (harness category: silent-drift)

Where it lives (eval): the Plan context block (its plan, "Not in scope" line, Files line, and any deviation note), read against the Candidate PR's Diff and Commits. The Description's claims about what changed are read against the Diff here too.

Where it lives (live): plan.md (Scope In and Out, Files to touch, and ## Deviations), read against `git diff main...HEAD` and the description in pr_draft.md.

What good looks like: every file and hunk in the diff falls inside the plan's files and scope, or a deviation note or the description names it and says why. Everything the plan commits to is in the diff, or its absence is disclosed. The description claims only what the diff does. Silent drift is a mismatch in either direction (more than the plan, or less) with no note, or a description that says the change is "exactly as planned" when the diff shows otherwise. In calib-01, the plan names one file and one removed line, and the diff removes exactly that line in that file.

## Test evidence (harness category: not-tested)

Where it lives (eval): the Candidate PR's Test evidence section, read against the Plan context's test plan and its repro evidence. The repo's own checks, if the Repo facts name any, show up in Test evidence or the Description.

Where it lives (live): test_evidence.md and the Testing section of pr_draft.md, read against plan.md's Test plan, and the checks listed in .github/PULL_REQUEST_TEMPLATE.md.

What good looks like: the plan's repro is re-run with pasted command output for both the before and the after, and the after shows the behavior the plan said would change (in calib-01, the DuplicateOptionError before and "parsed OK" after). Each check the repo lists shows its result. A check that fails or cannot run, reported with its output and the reason, still counts as evidence. "Tests pass" or "verified locally" with no output is not evidence.

## Diff quality (harness category: unreviewable)

Where it lives (eval): the Candidate PR's Diff and Commits.

Where it lives (live): `git diff main...HEAD` and `git log main..HEAD`.

What good looks like: the diff contains the change and nothing else, so a reviewer sees the fix right away. Debris tells: debug prints or logging left in, commented-out code, dead code, stray files (notes, scratch scripts, plan.md or pr_draft.md committed), formatting churn on untouched lines, and drive-by edits or cleanups the issue does not need. Any one of these is a fail, even when the description mentions it.

## Standards and comms (harness category: standards-wall)

Where it lives (eval): the Repo facts block (its pull requests line for template sections and asks, and its contribution policy line for rules such as AI-use disclosure), plus any maintainer direction in Thread highlights, read against the Candidate PR's Title and Description.

Where it lives (live): .github/PULL_REQUEST_TEMPLATE.md and docs/CONTRIBUTING.md in the working copy, and the issue thread on GitHub, read against pr_draft.md.

What good looks like: every section the template asks for is filled with real content, the issue is linked the way the repo asks (for example Closes #N), every disclosure the repo or course requires is present (such as how AI was used), and any explicit maintainer direction in the thread is followed or answered. Where the repo has no template and no stated policy, as in calib-01, there is nothing to fail here. A required section, link, or disclosure that is missing is a fail, not a neutral. Whether the description's claims match the diff belongs under plan fidelity, not here.
