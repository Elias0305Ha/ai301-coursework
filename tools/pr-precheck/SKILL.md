---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: is this ready to submit? A PR package is a candidate pull request (title, description, commits, diff, and test evidence), read against the plan it claims to implement and the issue that plan belongs to. Do not grade anything else, do not grade more than one package per run, and do not answer from gut feel. Answer only by executing rubric.md through procedure.md.

## Inputs and modes

Run in exactly one of two modes.

Live mode: the student's own PR, checked before it is opened. Read these inputs:
- plan.md in the working directory, including its ## Deviations section.
- The branch's diff: run `git diff main...HEAD` (three dots) from the working copy. This is every committed change the branch makes compared with main. Uncommitted changes are not part of the PR, so do not grade them.
- The draft PR title and description in pr_draft.md. The first line is the title.
- The test evidence in test_evidence.md.
- The issue named in the request: read its body and thread with `gh` or the GitHub web page.
- The repo's stated standards: .github/PULL_REQUEST_TEMPLATE.md and docs/CONTRIBUTING.md in the working copy.
A house-chain student uses the house plan and the house repro pack in place of their own plan and repro. Grade them with the same checks.
If a named input file is missing, grade every check that needs it as unclear and say which file was missing.

Eval mode: the input is one package bundle from eval/packages/. The bundle is the whole world. Take every fact from the bundle text. Do not fetch anything, run git, or read other files. Always grade every check and apply the full verdict rule.

## The scope seam (live mode only)

In live mode, read scope.md before anything else. If its Repo: line still holds a placeholder in angle brackets, stop without grading and tell the student to fill the Repo: line in scope.md with their section's Path Review repo. Never guess a scope. If the issue in the request is not in the scoped repo, refuse to grade and say why. Apply the house rules scope.md lists when reading evidence, for example that a classmate's PR on the same issue does not block this one. In eval mode, ignore scope.md entirely.

## The voice seam (live mode only)

In live mode, read voice-guide.md and hold the outgoing PR text (the title and description in pr_draft.md) against each rule. In the summary, quote each rule the draft breaks and the line that breaks it. The voice guide never changes the verdict on its own unless a rubric check reads it. In eval mode, ignore voice-guide.md entirely.

## Component reads

Read rubric.md for the checks and the verdict rule. Use references/evidence-guide.md as the map of where each kind of evidence lives, in a bundle and in a live PR. Execute procedure.md as written, stage by stage. If procedure.md is silent on a step you need, do not invent one: grade with what the rubric states and report the gap under "Gaps in procedure.md" in the summary. If rubric.md has no checks, or procedure.md has no steps of its own (only template comments), refuse to grade and say which file is empty.

## Verdict and output

The verdict is binary: accept means ready to submit, reject means hold. There is no third verdict. Put reservations in check evidence lines, not in the verdict. Before the JSON you may print a short readable summary: one line per check, the deciding evidence for a reject, voice-guide notes in live mode, and any procedure gaps. End the reply with this fenced JSON block, valid and last, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff, the description, and the evidence against the plan, the issue, and the repo's standards. A short complete PR can be ready, and a polished one can hide drift.
- The rubric decides: if a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary.
- The procedure decides how: follow procedure.md and report its gaps instead of inventing steps.
- Unclear: treat it as the rubric's verdict rule says. If the rule is silent, unclear counts as fail, because a PR you cannot verify is not ready to submit.
