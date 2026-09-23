# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->


## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-responsive | The last 5 default-branch commits and the maintainer first-response sample in the repo-facts block | Passes if EITHER a maintainer commented on any sampled issue within 90 days of capture, OR at least one pull request was merged to the default branch within 90 days of capture; only one of the two is required | required |
| repo-active | The commit activity and release recency lines in the repo-facts block | At least one commit to the default branch within 60 days of the bundle capture date | required |
| unclaimed | The assignee and linked-PR fields in the repo-facts block, plus the comment thread | No assignee, no open linked PR, no comment claiming the work dated within 45 days of the capture date, and no history of two or more distinct contributors previously claiming this issue and going inactive or being auto-unassigned without merging a fix | required |
| scope-bounded | The issue body and comment thread | Fails if the issue requires changes to three or more distinct source code files; mechanically repeating the same small change across several similar items (for example, adding the same kind of preview or handler for each of several existing types) counts as one location, not several; documentation-only changes pass regardless of file count provided the issue specifies what content is needed; also fails if the comment thread contains an unresolved disagreement among maintainers about what the fix should be | required |
| policy-open | The stated contribution policy line in the repo-facts block | The policy does not require a signed CLA, restrict PRs to team members, require prior approval before submitting, or prohibit AI-assisted contributions | required |
| scope-labeled | The issue labels in the issue body or repo-facts block | Carries a label such as `good first issue`, `good-first-issue`, `beginner`, or `help wanted` | preferred |

## Verdict rule

Accept if every required check passes. Preferred checks never change the
verdict; they rank accepted issues against each other. Any check graded
`unclear` counts as a fail.

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->
