# Procedure: how this skill grades a PR package

## Read order

1. Read the issue and its thread first. Note what the issue asks for, including any named requirement (a test marker, a file, a behavior).
2. Read the repo's standards next: the PR template and contributing rules (the repo-facts block in eval mode; .github/PULL_REQUEST_TEMPLATE.md and docs/CONTRIBUTING.md in live mode). Note every section the template asks for, every check it lists, how it wants the issue linked, and any required disclosure such as an AI-use statement.
3. Read the plan, including its Deviations section. Note its scope (In and Out), files to touch, test plan, and every deviation recorded.
4. Read the diff. List every changed file and, for each, what its hunks do.
5. Read the test evidence. Note each command and its output, before and after, and each repo check with its result.
6. Read the PR title and description last. Note what it claims the change does, which files it says changed, and what it discloses.
7. Do not grade anything until all six are read. The plan and the standards come before the diff because every check reads the diff against one of them.

## Evidence gathering

1. plan-fidelity: put the diff's file list next to the plan's files to touch and its Deviations. Record any changed file or behavior the plan does not name, and any planned change missing from the diff, and whether either is disclosed.
2. description-matches-diff: put the description's Summary and Changes next to the diff's file list. Record any claim with no matching change, and any changed file the description never mentions.
3. evidence-observable: put the before and after output next to the plan's test plan. Record whether both are pasted output, not claims, and whether the after shows the change the plan predicted.
4. repo-checks-run: list each check the repo's template or contributing docs name. For each, record whether its output or result appears in the evidence or Testing section, and any stated reason for a failure or skip.
5. diff-clean: scan every hunk. Record any debug print or logging, commented-out code, stray file, or edit unrelated to the plan's change, with its file and line.
6. standards-met: list each template section, the issue link, and each required disclosure. Record whether the description fills each one.
7. title-states-change: record the title and whether it names the change.

## Check execution

1. Run the checks in the order the rubric lists them.
2. Grade each one pass, fail, or unclear using only its pass condition and the evidence recorded above. Quote one fact or line for every grade.
3. Use unclear only when the evidence a check needs is absent from the package, for example no test evidence at all. If the evidence is there but weak, grade fail.
4. Before failing a check for something missing or extra, look for a disclosure in the plan's Deviations or the description. If one states what changed or was left out and why, and it is accurate, the check passes.
5. If a check passes by its pass condition but feels wrong, it still passes. Note the tension in the summary.
6. Judge the substance, never the formatting. A short description that matches the diff can pass, and a long polished one can fail.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passes.
2. A fail or unclear on any required check means reject. Unclear counts as fail.
3. Preferred checks never change the verdict.
4. In the summary, quote the deciding evidence for each required check that caused a reject. For an accept, quote the weakest passing check.
5. End with the JSON block and nothing after it.
