# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-matches-evidence | The plan's stated cause, read against the repro evidence it quotes (commands, output, error text) | The stated cause explains the quoted evidence on error and location. Fails when the cause ignores the evidence, contradicts it, or points at something the evidence never shows. No quoted evidence to compare is unclear. | required |
| cause-not-symptom | The plan's approach, read against its own diagnosis and the repro evidence | The change goes after the cause the evidence points to. Fails when the approach only hides or works around the visible symptom while the cause stays in place. | required |
| scope-bounded | The plan's scope statement and its list of files to touch, read against the diagnosis | One bounded change, with what will not be changed named, and a file list that matches what the diagnosis needs. Fails when unrelated work is bundled in or the file list is much bigger than the cause requires. | required |
| stranger-could-start | The approach and files-to-touch, read as someone who has never seen the repo | A stranger could name the first file to open and the first edit to make without asking anything. Fails when key steps are vague, like fix the logic, with no location or action. | required |
| test-plan-observable | The test plan, read against the repro steps and the expected results | It re-runs the repro steps and states a concrete before and after that someone could see (output changes, error gone, test passes). Fails when it only says verify it works or the expected result is not observable. | required |
| unknowns-honest | The risks and unknowns, read against the diagnosis and the evidence | Real gaps in the evidence are named as uncertainty, not stated as fact. Fails when a guess is presented as certain, or when the unknowns are empty although the evidence leaves clear gaps. | required |
| comment-fits-thread-and-conventions | The plan comment, read against the thread highlights and the repo's stated contribution rules (the repo-facts block in eval mode) | The comment answers what the thread asks and visibly satisfies every requirement the repo states. Where the repo requires a disclosure, statement, or named piece of information, its absence is a fail. Where neither the thread nor the repo asks for anything, this passes. | required |
| comment-matches-plan | comment.md, read against plan.md | The comment gives the same diagnosis, scope, and test plan as the plan and contradicts nothing in it. | preferred |

## Verdict rule

Accept if every required check passes. Any required check grading fail or unclear produces reject. Preferred checks never change the verdict. Unclear counts as fail: a plan that cannot be verified is not ready to post.
