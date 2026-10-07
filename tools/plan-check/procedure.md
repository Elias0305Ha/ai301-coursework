# Procedure: how this skill grades a plan package

## Read order

1. Read the issue context and thread highlights first. Note what the issue reports and anything the thread asks for from a plan comment.
2. Read the repo-facts block next. Note every contribution rule or convention the repo states (required wording, disclosures, branch naming, process).
3. Read the repro evidence the plan quotes. Note the exact error, the location, and the commands that produced it. This is the ground truth the plan gets judged against, so it comes before the plan.
4. Read the plan: diagnosis, scope, files to touch, approach, test plan, risks and unknowns. Note what cause it claims and which evidence it quotes for that.
5. Read the plan comment last. Note what it says about diagnosis, scope, and test plan, and whether it answers the thread.
6. Do not grade anything until all five are read.

## Evidence gathering

1. diagnosis-matches-evidence: copy the plan's stated cause and the repro lines it relies on. Record whether the cause explains that error and location.
2. cause-not-symptom: copy the plan's approach next to its diagnosis. Record whether the change removes the cause or only hides the visible symptom.
3. scope-bounded: copy the scope statement and the file list. Record what is excluded, how many files are touched, and whether each file ties back to the diagnosis.
4. stranger-could-start: from the approach and file list alone, write down the first file to open and the first edit to make. If you cannot name them, record that.
5. test-plan-observable: copy the test plan and the repro steps. Record whether it re-runs those steps and states a before and after result someone could see.
6. unknowns-honest: copy the risks and unknowns. Record any claim in the plan that is a guess stated as fact, and any gap in the evidence the unknowns skip.
7. comment-fits-thread-and-conventions: copy what the thread asks for and each repo rule from step 2 of Read order. Record whether the comment visibly satisfies each one.
8. comment-matches-plan: record whether the comment's diagnosis, scope, and test plan match the plan's.

## Check execution

1. Run the checks in the order the rubric lists them.
2. Grade each one pass, fail, or unclear using only its pass condition and the evidence recorded above. Quote one fact or line for every grade.
3. Use unclear only when the evidence the check needs is truly absent from the package, for example no quoted repro evidence at all. Do not use it because the evidence is messy. If it is there and weak, grade fail.
4. If a check passes by its pass condition but feels wrong, it still passes. Note the tension in the summary.
5. Grade each check from the evidence you already recorded. Re-read the package only if a quote is missing.
6. Judge the substance, never the formatting. A short plan with a clear cause and test can pass, and a long polished one can fail.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passes.
2. A fail or unclear on any required check means reject. Unclear counts as fail.
3. Preferred checks never change the verdict.
4. In the summary, quote the evidence for each required check that decided a reject. For an accept, quote the weakest passing check.
5. End with the JSON block and nothing after it.
