# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
| environment-recorded | The repro report's environment record, read against what the issue's setup involves | Names enough for a reader to assemble the same setup: the platform it ran on, and a version or identifier pinning the software under test. Whatever else that setup turns on (install method, runtime, commit, dependency versions) is named where it would change the result. Fails when a value is absent or given vaguely. | required |
| steps-followable | The repro report's reproduction steps, read as a stranger starting fresh | A reader could rebuild the same attempt from what is written: the starting point is stated, the setup actions are accounted for, and the commands are recoverable, whether pasted verbatim or described precisely enough to reconstruct. Fails when a step is missing or too vague to rebuild. | required |
| behavior-matches-issue | The report's pasted output or artifact, read against the failure the issue describes and against the report's own stated result | Where the report claims a reproduction, the artifact shows the same failure the issue names, on error and location; a different failure fails. Where the report states it could not reproduce, the artifact shows the attempt targeting the issue's described behavior and what appeared instead. No artifact to compare is unclear. | required |
| outcome-honest | The report's stated conclusion, read against the evidence it attaches | The conclusion is backed by the evidence shown. A reproduction states it and shows the failure; a cannot-reproduce states it and shows what was attempted. Either conclusion fails when nothing supports it. | required |
| conventions-followed | The repo's stated contribution rules (the repo-facts block in eval mode, the repo's own docs in live mode), read against the comments in the package | Every requirement the repo states is visibly satisfied in the comments themselves. Where the repo requires a disclosure, a statement, or a named piece of information, its absence from the comments is a fail, not a neutral. Where the repo states no requirement, this passes. | required |

## Verdict rule

Accept if every required check passes. Any required check grading fail
or unclear produces reject. Preferred checks never change the verdict.
Unclear counts as fail: proof that cannot be verified is not ready to
post.