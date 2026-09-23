# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/13

**Verdict output**

```
[PASTE YOUR FULL LIVE-MODE TERMINAL OUTPUT HERE, from "Evidence gathered
(live mode..." through the closing JSON block, verbatim]
```

---

## Eval iterations

**Run history**

1. Smoke run, `--limit 3`: agreement 0/1. Two of three bundles errored before
   grading. The harness crashed with a UnicodeEncodeError on Windows cp1252
   while passing bundle text containing emoji to the Claude CLI. Only issue-01
   completed, and it disagreed.
2. Smoke run, `--limit 3`, with UTF-8 forced: agreement 2/3. issue-01
   disagreed, failing on scope-bounded.
3. `--only issue-01` after rewriting scope-bounded: agreement 1/1.
4. Full run: agreement 10/13. Seven bundles errored on the same encoding bug,
   which had not been made permanent.
5. `--only` on the seven errored bundles: all seven errored again.
6. Same seven with `python -X utf8`: agreement 6/7. issue-09 disagreed on
   scope-bounded.
7. `--only issue-04,issue-09,issue-15,issue-19` after extending scope-bounded
   and unclaimed: agreement 3/4.
8. Full run with `--save-run`: agreement 17/20, below the bar, and the category
   floor was unmet with no match in policy.
9. `--only issue-12,issue-14,issue-19` after rewriting policy-open and
   maintainer-responsive: agreement 2/3.
10. Full run with `--save-run`: agreement 19/20, PASS, all categories matched
    including policy 1/1. This is the committed run.

**Issue analysis**

issue-12 (bookwyrm-social/bookwyrm#1133). Gold label: reject. My rubric's
verdict on the run before the fix: accept.

The issue itself reads well. It carries a `good first issue` label, the repo
pushed code on the capture date, and the ask is a contained UI change to a
reading-goal progress bar. Four of my five required checks passed on solid
evidence.

My policy-open check passed it too, and that was the error. The bundle's
repo-facts block quotes BookWyrm's contribution policy: "Meaningful human
interaction is the whole point of BookWyrm. We do not accept AI-generated code
or documentation." That is an absolute prohibition on AI-assisted
contributions. My check only failed a policy that required a signed CLA,
restricted PRs to team members, or required prior approval. An outright AI ban
matched none of those three conditions, so the check passed and the verdict
came back accept.

What made this worth investigating rather than patching is that issue-12 had
come back reject on an earlier full run, agreeing with gold, without my having
touched policy-open in between. The check had never been able to see the AI
ban. It had produced the right answer by chance and then stopped. An ambiguous
check that agrees with gold by luck is indistinguishable from one that agrees
by design until it is re-run, which is why the category floor exists: policy
was the one category my rubric was blind to, and the floor caught it even when
the total score did not.

**Check rationale**

From `tools/issue-select/rubric.md`, the policy-open check as currently
written:

  "The policy does not require a signed CLA, restrict PRs to team members,
  require prior approval before submitting, or prohibit AI-assisted
  contributions"

The first three conditions came from the general question of whether a repo
accepts outside contributions at all. The fourth clause was added after
issue-12, and it is the one that carries real weight for this course. The work
in this class is done with Claude Code. A repo that forbids AI-assisted
contributions is not a repo where I can contribute honestly without abandoning
the toolchain the course is built around, so the issue is a reject regardless
of how good it looks on every other axis.

The clause is deliberately worded around the prohibition rather than around any
particular phrasing of it. Two other bundles in the set, issue-09 and issue-14,
state policies that explicitly permit AI tools under conditions, and the
wording distinguishes those from a ban without needing to match exact language.

**Trade-offs**

The clause gives up on the middle ground. Policies that permit AI use but
attach conditions, such as conda's requirement that contributors review and
understand anything AI-generated before submitting it, pass this check
unchanged, and so do Zulip's stricter terms about closing PRs that appear
untested or not understood. My check reads those as open. A repo that tolerates
AI assistance but would object to how heavily I lean on it would slip through,
and I accept that miss: the check draws a line at outright prohibition rather
than trying to grade degrees of tolerance, because degrees are not something I
can apply reproducibly from a policy line.

I re-ran issue-12 with `--only` after the change, along with issue-14 and
issue-19 as canaries for the other edit in the same round. issue-12 flipped to
reject and issue-14 to accept, both agreeing with gold, and the confirming full
run kept every previously agreeing issue intact at 19/20 with policy 1/1.

---

## Selection rationale

**Selection rationale**

1. Fit and time. Issue #13 asks for an integration test that runs the full RAG
pipeline against a mock LLM. That sits in the part of my experience I trust
most: I have written pytest suites, and I have built a RAG system with ChromaDB
and Groq, so the pipeline shape is familiar even though this codebase is not.
My skill ranked it first of three accepted candidates on exactly that basis. It
is also the only tier-2 of the three, estimated at 4 to 6 hours against 7 to 10
for the other two, which matters because this issue carries through units 2 to
4 rather than ending here. I deliberately looked past the tier-1 issues because
I took an easy option in a previous course and regretted it, but I did not want
to pick something I could not finish.

2. What the verdict caught, and what I weighed myself. The rubric confirmed the
things I would have had to check by hand: a collaborator commented six days
before the run, main was committed to six days before the run, the contribution
policy requires only a fork, a branch convention, and green CI, and the issue
has no assignee, no comments, and no linked PRs. scope-bounded passed it for a
reason I would not have caught myself, that one of the three paths named,
tests/fixtures/sample_profiles/basic_profile.json, already exists and is read
rather than changed, so only two files actually change.

What the rubric could not weigh is the difference between the three accepted
candidates. All three passed every required check, so the verdict alone does
not choose. The run said so plainly: scope-bounded counts files, so #8 and #10
passed despite being tier-3 greenfield feature work. Fit ranking put #13 first,
but the judgment that a first contribution should be finishable rather than
impressive was mine. I also weighed that an integration test teaches me the
pipeline end to end, which sets me up better for later units than a narrow bug
fix would.

3. Anticipated difficulty in claiming it. Claiming itself should be low
friction. The issue has no assignee and no comments, and the Path Review house
rule means classmates' claims do not block anyone, so a shared issue costs
nobody anything. The real difficulty is after the claim. The issue depends on
an injection seam in review_generator.py that does not exist yet, so I have to
introduce a way to substitute a mock LLM before I can test against one, and
shared fixtures added under issue #43 are used by #5, #13 and #14, so I need to
read them rather than change them. Reproducing the current behavior in unit 2
is where I expect to spend the most time.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
