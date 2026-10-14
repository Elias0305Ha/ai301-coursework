# Unit 4: Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/182

**Branch**

test/13-rag-pipeline-integration-test

## Eval iterations

**Run history**

1. First full run: 14/20, below the bar. Categories: clear-accept 1/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. Every miss was a gold accept graded reject: pkg-02 and pkg-16 on description-matches-diff, pkg-05 and pkg-13 on description-matches-diff and repo-checks-run, pkg-08 on repo-checks-run, pkg-11 on evidence-observable.
2. Partial run with `--only pkg-02,pkg-05,pkg-08,pkg-11,pkg-13,pkg-16,pkg-03,pkg-04,pkg-07` after loosening those three checks, with pkg-03 (silent-drift) and pkg-04, pkg-07 (not-tested) as canaries: 8/9. Only pkg-11 still missed, on evidence-observable.
3. Second full run, saved as eval-run.txt: 19/20, PASS. "agreement: 19/20 scored items  (bar: 18/20: PASS)". Categories: clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3.

**Package analysis**

pkg-02 (category clear-accept). Gold label: accept. My rubric rejected it in run 1 and accepted it in my final run, so the final verdict agrees.

In run 1 the only failed check was description-matches-diff. That version said "every file the diff changes is accounted for in the description." pkg-02's description explains what the change does at a summary level instead of naming every file, so the grader read a missing file name as an unmentioned change and failed it. Nothing was actually hidden; the description just did not list files. pkg-05, pkg-13, and pkg-16 failed the same check for the same reason. After I rewrote the check to judge whether the description leaves out a substantive change rather than a file name, pkg-02 passed every check in the partial run and in the final full run.

**Check rationale**

| description-matches-diff | The PR description's claims about what the change does, read against the diff | Every change the description claims is actually in the diff, and the description does not leave out a substantive change a reviewer needs to know about (new behavior, new logic, a removed feature). A summary-level description that does not list every file passes. Fails when the description claims something the diff does not do, or the diff makes a substantive change the description never mentions. | required |

My first version required "every file the diff changes is accounted for in the description." That was structure-shaped: it judged whether the description listed files, not whether it told the truth about the change, and it cost four clear-accept packages in run 1 (pkg-02, pkg-05, pkg-13, pkg-16). I rewrote it to judge substance: does the description claim anything the diff does not do, and does it hide a change a reviewer would need to know about. I kept the first half, "Fails when the description claims something the diff does not do," on purpose, because a description claiming more than the diff delivers is one of the ways silent drift shows up, and I did not want to lose that.

**Trade-offs**

Loosening this check could have let a silent-drift package through, so I re-ran canaries with `--only` before the confirming run: pkg-03 (silent-drift) and pkg-04 and pkg-07 (not-tested) all stayed reject, matching gold. The full run then kept silent-drift at 4/4. What the check gives up: a description that skips a small but real change, like a one-line config edit it never mentions, can now pass this check if the grader does not judge that change substantive. I accept that miss because plan-fidelity still reads every changed file against the plan and its Deviations, so an undisclosed extra file is caught there instead. The one package I still miss is pkg-11, a clear-accept that fails evidence-observable; I left that check alone rather than loosen it further and risk the not-tested category.

---

Related paths: `eval-run.txt` in this directory; your skill's files in `tools/pr-precheck/`.
