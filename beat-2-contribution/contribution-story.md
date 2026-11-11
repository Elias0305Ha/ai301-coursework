# Contribution: Document cloud chat and image data flows in the contributor guides

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Elias Hakenso
**GitHub Username:** Elias0305Ha
**Issue:** https://github.com/Swarneil1905/LocalMind/issues/10
**Pull Request:** https://github.com/Swarneil1905/LocalMind/pull/12

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Turned my issue picker into a scout with my own search strategy, then fixed two problems my first scan exposed**

I wrote four `gh search issues` queries, each traced to a sentence of my fit profile, plus ranking filters (drop assigned, stale, avoid-list labels and long threads, then fewest comments first). My first test run of query 1 returned zero results, because gh treated my quoted `dataframe OR csv OR sql OR query OR pipeline` as one exact phrase; removing the outer quotes fixed it, and the rerun returned 15 on-topic issues. The first $1 scan then graded five issues that were all opened in the last two days in repos with 0 to 5 stars, which showed me "fewest comments first" really meant "newest first", so I added `createdAt` to every query and a rule that drops issues created in the last 14 days. The scout also suggested loosening my maintainer-responsive check so a solo repo that has never merged a PR would pass; I kept the check, because that is exactly where my PR would stall.

**Unit 6: Installed the voice guard, made it actually fire on Windows, and wrote my LocalMind memory file**

The wiring test (`gh issue comment --help` through Claude Code) ran unblocked the first time. Feeding the same input straight to `check_voice.py` stopped with exit code 2, so the script worked and the hook was being skipped: Claude Code on Windows ran the command through its PowerShell tool, my hook matcher only covered `Bash`, and `check_voice.py` line 245 returned early for any tool that was not Bash. I widened the matcher to `Bash|PowerShell` and changed that line to accept both, and the rerun stopped with "no body the guard can read"; my real claim then went through the guard with `--body-file`. My CLAUDE.local.md for LocalMind (41 lines, kept out of commits with `.git/info/exclude`) records the CONTRIBUTING rules, the four files and tests my issue touches, and that the Python test commands are not verified on my machine yet because the service's dependencies are not installed.

**Unit 7: Installed the ship-gate, fixed it for Windows, and got my PR past it and my own voice guard**

The ship-gate had the same Bash-only hook gap as my voice guard, so before trusting it I widened its matcher to `Bash|PowerShell` and changed `check_gate.py` line 1369; a fresh session then stopped `gh pr create --help` with `ship-gate: STOP.` My manifest runs a link check and the repo's PR declaration tests, with the Python checks moved to the excluded table because their dependencies are not installed and the change touches no Python. The first gate run stopped my branch: pr-precheck failed plan-fidelity (a sentence in my diff that was not in plan.md) and standards-met (an unticked review box), so I recorded the deviation in plan.md and the PR description, re-ran plan-check, and the second run passed. Then my voice guard blocked the PR itself three times for posting conclusions I reached by reading code instead of running it; I reworded one line, then cut the unconfirmed follow-ups section and a code-read validation bullet, and PR #12 went through.

**Unit 8: Wrote my playbook from LocalMind's own rules and ran the PR monitor on PR #12**

My playbook sets an 8-day first-review threshold from CONTRIBUTING.md's "one polite follow-up after a week" plus a day; I could not sample five merged outside PRs because LocalMind has none yet, so the rule says that instead of inventing a number. The monitor read PR #12 (opened minutes earlier, no checks reported, merge state UNSTABLE, no comments) and drafted a wait note, not a nudge, with my threshold date of 2026-11-19, so I posted nothing. It also caught a flaw in my own nudge rule: it required saying "CI is green", but CI had not run, since first-time contributor workflows wait for maintainer approval. I rewrote that rule to state the actual CI state and never claim green unless the Checks tab shows it.

---

## Why I Chose This Issue

I searched two ways. My scout ran my four queries, ranked 400 unique results down to 261 survivors and graded the top five: it accepted LocalMind#10 and openpickleball/backend#15 and rejected three. On the Issue Selection site I filtered to Python and the documentation tag and graded six picks over two rounds, and the scout rejected all six for real reasons: monarch-initiative/dismech#10070 because its CONTRIBUTING says not to open PRs from forks (I had picked it as my favorite and missed that), zarr-python#2394, ai-dynamo/dynamo#13317, zarr-python#2995 and marketcalls/openalgo#1843 because each already had an open PR (one of them a classmate's), and lance#3473 because someone asked to work on it 12 days earlier. So I went back to LocalMind#10 from my scan, and since it did not come from the site I asked about it in #dts-fa26-ai301-issue-selection. After the unit 6 scope patch I regraded it: accept, difficulty moderate, because the two doc files are named but writing them accurately means reading `credentials.py`, `providers.py`, `cloud_images.py` and the API routes first.

It fits my profile directly: documentation for LLM tooling in a Python service, with a clear "Done when" list. I want to learn how to trace where user data and API keys actually travel in a real codebase and write that down accurately, which is the same skill I need as a data and ML engineer. My honest read of the scout: its checks were right every time it rejected something, and the policy check saved me from a dead end I had walked straight into. It fell short in two places. Its ranking favored brand-new throwaway repos, which I fixed with the 14-day rule, and its maintainer-responsive check passed LocalMind on the owner merging their own PR, when the repo has never merged an outside contribution. That is the same stall risk I rejected pyfect for, and my new 14-day rule would now filter LocalMind out too. I chose it anyway because, unlike pyfect's owner, this maintainer replied to issue #1 within about 8 hours, wrote a contributing guide with a PR template, and wrote these issues for newcomers; the risk is that my PR waits, and I accept that. The maintainer then replied to my claim within about 8 minutes, which supports that call.

Claim comment: https://github.com/Swarneil1905/LocalMind/issues/10#issuecomment-6786646866

**Toolkit**

- Tool: issue-scout (my search strategy and rubric), run from Claude Code: one scan, then grading my site picks and LocalMind by URL, then a regrade after the scope patch.
- What it produced: an accept for LocalMind#10 on all six checks (difficulty moderate after the patch), and rejects for all six site picks, each with the evidence quoted (an open PR number, a claim comment, a no-forks policy line).
- Difficulties: ranking rewarded newness instead of fit; maintainer-responsive cannot tell an owner's own PR from an outside one; unclaimed cannot tell a stalled open PR from an active one (zarr-python#2394's PR had been quiet for 53 days).
- What changed: added the 14-day created rule and a whole-word avoid-list match to search-strategy.md; replaced scope-bounded with scope-feasible plus the difficulty note. I would next make maintainer-responsive require a merged PR from someone other than the owner.

---

## Understanding the Issue

**Problem Description**

LocalMind's security guide and codebase guide still describe the app as Ollama-only inference with Gmail and Runway as the only stored credentials. The code has since added optional cloud chat through OpenAI, Anthropic and Gemini using keys the user supplies, plus an OpenAI image workflow, and none of that is in the contributor docs.

**Expected Behavior**

SECURITY.md and docs/CODEBASE.md should separate local Ollama inference from the optional cloud chat and image requests, say which user-approved content can leave the device and where the API keys are stored, link to the provider and image modules and their tests, and keep live-testing limits explicit.

**Current Behavior**

On main (89005c2), neither file mentions OpenAI, Anthropic or Gemini at all. SECURITY.md line 34 says local chat runs through Ollama, and CODEBASE.md line 98 says `credentials.py` stores "Gmail and Runway credentials", while `credentials.py` lines 18 to 19 also allow `chat-openai`, `chat-anthropic` and `chat-gemini`.

**Affected Components**

Docs to change: `SECURITY.md` (Network use, Credentials and stored data) and `docs/CODEBASE.md` (feature map, credential paragraph, limitations). Code they must describe: `services/ai/app/credentials.py`, `providers.py`, `provider_store.py`, `cloud_images.py`, and the routes in `api/routes/providers.py`, `chat.py` and `media.py`.

**Toolkit**

- Tool: Claude Code in my LocalMind clone, asked to read the credential, provider and image modules plus their routes and tests and write `analysis-notes.md` with a file:line citation for every claim.
- What it produced: notes covering the allowed keys, OS keyring storage scoped per profile, what cloud chat and image requests send and to which endpoints, how approval works, which tests cover it, and the live-testing limits.
- Difficulties: it surfaced two extra observations (an image request may block budgeted chat for the month; image requests ignore proxy settings) that I have not confirmed and that are outside this issue.
- What changed: I kept both out of the docs and my plan. I first listed them in the PR as possible follow-ups, then cut them when my voice guard flagged them as conclusions reached by reading code; I will open separate issues only if I reproduce them.

---

## Reproduction Process

**Environment Setup**

Windows, Git Bash (MINGW64), git, GitHub CLI logged in as Elias0305Ha, Python 3.11.9 and 3.14.5 installed. I forked and cloned LocalMind with `gh repo fork Swarneil1905/LocalMind --clone` (origin is my fork, upstream is Swarneil1905), and made a branch `docs/cloud-data-flows` from main at 89005c2 (2026-11-10). This is a docs gap, so the app does not need to run. The only challenge was that the service's Python dependencies are not installed (`ModuleNotFoundError: httpx`); CONTRIBUTING says docs-only changes do not need the test suite, so I did not install them.

**Steps to Reproduce**

1. Clone the repo and check out main (89005c2).
2. Confirm nothing changed since the issue was written: `git diff cba10f6..89005c2 --stat -- services/ai/app/ SECURITY.md docs/CODEBASE.md` prints nothing.
3. Search the docs and the code for the cloud providers: `grep -n -i -E "openai|anthropic|gemini|ollama" SECURITY.md docs/CODEBASE.md` and `grep -n -i -E "openai|anthropic|gemini" services/ai/app/credentials.py services/ai/app/providers.py services/ai/app/cloud_images.py`.
4. Observed result: the docs match only "Ollama" (SECURITY.md:34, :56; CODEBASE.md:3, :14, :26-27, :44, :56, :98) and zero lines for openai, anthropic or gemini, while the code matches dozens, such as `providers.py:66` (Anthropic `x-api-key` header) and `cloud_images.py:94` (POST to `https://api.openai.com/v1/images/...` with a Bearer token).

**Reproduction Evidence**

- **Commit showing reproduction:** none (docs gap; the evidence is the grep output against main at 89005c2)
- **Screenshots/logs:** excerpt from my grep run:

```text
SECURITY.md:34:Local chat inference runs on the user's computer through Ollama. Optional features have separate network and credential requirements:
docs/CODEBASE.md:98:... credentials.py stores Gmail and Runway credentials in the native credential store, scoped by the profile path. ...
--- code ---
services/ai/app/providers.py:66:        return {"x-api-key": key, "anthropic-version": "2023-06-01"}
services/ai/app/cloud_images.py:94:            async with client.stream('POST', f'https://api.openai.com/v1/images/{endpoint}', headers={'Authorization': f'Bearer {token}'}, **arguments) as response:
```

- **My findings:** the gap the issue describes is still exactly there on today's main, and the docs are not just incomplete but wrong in one place (CODEBASE.md:98 lists only Gmail and Runway). I also found that cloud images reuse the same `chat-openai` key as cloud chat (`cloud_images.py:60`), which the docs will need to say.

**Toolkit**

- Tool: `git diff` and `grep` in my LocalMind clone, with output saved to repro-before.txt; plan-check later re-ran the same reproduction when grading my plan.
- What it produced: proof that none of the issue's files changed since cba10f6, and zero openai/anthropic/gemini lines in either doc against dozens in the code.
- Difficulties: this is a documentation gap, so there is no failing behavior to run; the "bug" is an absence, which a grep shows but cannot fully prove. I also did not run repro-check on a separate repro report: plan-check re-ran my quoted reproduction and matched it, but repro-check itself only graded my claim comment.
- What changed: I added the `git diff cba10f6..89005c2` step so the evidence shows the gap is still there today, not just at the commit the issue was written against.

---

## Solution Approach

**Analysis**

The docs were never updated when cloud features landed. `services/ai/app/credentials.py` (lines 18 to 19) added `chat-openai`, `chat-anthropic` and `chat-gemini` to its allow-list of keys stored in the OS credential store; `providers.py` reads those keys and sends cloud chat to fixed OpenAI, Anthropic and Gemini endpoints after rechecking consent just before the request; `provider_store.py` keeps the local-only policy (on by default) and the standing per-provider approvals; and `cloud_images.py` sends image prompts to OpenAI reusing the `chat-openai` key (lines 10 and 60). Meanwhile `SECURITY.md` lines 37 to 38 and `docs/CODEBASE.md` line 98 still describe only Ollama, Gmail and Runway. Approval for cloud chat is standing per provider, model and data categories, not per message, and the preview a user sees is not bound to what gets sent (`chat.py:459`), so the docs have to be careful not to imply per-message approval.

**Proposed Solution**

Update only `SECURITY.md` (the intro, Network use, new Cloud chat and Cloud images bullets, a "What this describes" limits bullet, and Credentials and stored data) and `docs/CODEBASE.md` (intro, chat route entry, a new feature-map row for cloud chat, `cloud_images.py` in the images row, the credential paragraph, and the experimental section), so each "Done when" item in issue #10 is met. What I am deliberately not changing: no code, no other docs, the Mermaid diagram and "Follow one chat message" step 5 (they still accurately describe the default local path), and two things I noticed but could not confirm (one paid image request may block budgeted cloud chat for the month; image requests ignore proxy environment variables). I planned to list those in the PR as possible follow-ups; see the deviations below for why they came out.

**Implementation Plan**

1. Read the credential, provider, image and route modules and their tests, and write cited notes (file:line for every claim).
2. Rewrite the SECURITY.md intro and Network use bullet to separate local Ollama from optional cloud chat and image requests.
3. Add Cloud chat, Cloud images and "What this describes" bullets, stating standing approval and that the statements cover backend enforcement only.
4. Update the SECURITY.md credentials bullet to name the three chat keys and the shared OpenAI key.
5. Update docs/CODEBASE.md: intro, chat route entry, feature map rows linking the provider and image modules and their tests, the credential paragraph, and the live-testing limits.
6. Verify every new sentence against a file:line in the notes, check every relative link resolves, re-run the reproduction grep, and read the full diff.

Deviations: plan-check rejected my first plan for having no risks and unknowns section (I had not said the frontend approval screens were unread or that approval is standing, not per message), so I added one before building. During review I questioned the sentence saying local-only mode blocks image requests; Claude Code confirmed it against `cloud_images.py` (checks at lines 17, 41 to 43, 54 and 81), so I kept it. One sentence was narrowed to "The automated native credential-store check runs only on Windows" because the evidence did not support more; that sentence was not in my plan, so after the first ship-gate run flagged it I recorded it as a deviation in plan.md and in the PR description. Finally, my voice guard blocked the PR over the two unconfirmed follow-ups, so I removed them from the description and noted that in plan.md too.

**Toolkit**

- Tool: plan-check, run from Claude Code on plan.md and plan-comment.md in my LocalMind clone.
- What it produced: a reject (unknowns-honest failed, and comment-matches-plan failed as a preferred check), then an accept after revision.
- Difficulties: it was right that my planned wording ("only after the user approves") rested on backend code alone and could be read as per-message approval. It also noted its procedure is written for eval bundles and does not say where repro evidence comes from in live mode, so it graded against the repro quoted in my plan.
- What changed: added a Risks and unknowns section (frontend screens unread, standing approval, separate image approval, no tests run, code-reading only) and made the comment quote the commands and name its uncertainty.

---

## Testing Strategy

This is a docs-only change, and CONTRIBUTING.md says docs-only PRs need no test suite: check that linked files exist, that the docs match the current setup, and that no private data is included. So my checks are:

- **Reproduction re-run (before and after):** the same `grep -n -i -E "openai|anthropic|gemini"` over SECURITY.md and docs/CODEBASE.md went from 0 matching lines on main (89005c2) to 3 lines in SECURITY.md and 1 in docs/CODEBASE.md on my branch (commit 9a7530f). The count is only a proxy, so the real accuracy check is the next one.
- **Every new sentence traced to code:** each sentence maps to a file:line in my notes, for example standing approval to `provider_store.py:130-140` and `:202-206`, the recheck before sending to `providers.py:265-271`, and the shared OpenAI key to `cloud_images.py:10, 60`.
- **Link check:** all 84 relative links in the two files resolve, including three section anchors. The manifest's link-check command exits 0 on my branch, and I confirmed it can fail by breaking one link on purpose (it printed `SECURITY.md -> docs/no-such-file.md` and exited 1) before restoring the file.
- **Repo checks:** the PR declaration checker tests (`node --test scripts/contributing/*.test.mjs`) pass 14 of 14, and my pr_draft.md passes the repo's own readiness checker with 0 errors.
- **Not run:** the Python ruff, unittest and import checks, because the service dependencies (httpx, ruff, fastapi) are not installed on my machine and this change touches no Python. They are listed with that reason in my ship manifest's excluded table.

**Toolkit**

- Tool: my ship-gate manifest's link check plus the repro grep, run by Claude Code in my LocalMind clone, with results saved to test_evidence.md.
- What it produced: before and after grep output, an 84-link pass, a deliberate-break test showing the link check can fail, and 14/14 passing Node tests.
- Difficulties: the gate runs manifest commands through cmd.exe on Windows, not bash, so the commands had to be written and tested for that shell; the Python checks could not run without installing the service's dependencies.
- What changed: wrote the link check as a single python3 command that runs in a fresh cmd.exe, and moved the Python checks to the excluded table with a dated reason instead of deleting them.

---

## Implementation Notes

**2026-11-10**

Graded seven issues across my scan and the Issue Selection site; six were taken or closed to forks. Chose LocalMind#10, asked about it in Slack, and posted my claim through the voice guard after repro-check rejected my first draft for not naming the AI tool and I removed a "within two days" deadline that broke my own voice guide.

**2026-11-10**

Forked and cloned LocalMind, branched `docs/cloud-data-flows` from main at 89005c2, confirmed with `git diff` that no relevant file changed since cba10f6, and reproduced the gap with grep. Had Claude Code write cited analysis notes; decided to leave two unconfirmed observations (image budget blocking chat, proxies ignored for images) out of scope.

**2026-11-10**

Swarneil1905 replied to my claim within about 8 minutes, welcomed it and invited questions. Plan-check rejected my first plan for missing risks and unknowns, so I added that section, then built the fix: SECURITY.md and docs/CODEBASE.md only. I read the full diff and questioned the claim that local-only mode blocks image requests; it held up against four checks in `cloud_images.py`. Committed as 9a7530f.

**2026-11-10**

Installed the ship-gate. It had the same Bash-only hook gap as the voice guard on Windows, so I widened its matcher to `Bash|PowerShell` and changed `check_gate.py` line 1369 to accept both tools; a fresh session then stopped `gh pr create --help` with `ship-gate: STOP.` Filled the manifest with a link check and the PR declaration tests, and moved the Python checks to the excluded table because their dependencies are not installed and the change touches no Python.

**2026-11-10**

First gate run stopped on plan-fidelity and an unticked review box; recorded the deviation, re-ran plan-check (accept) and the gate (PASS). My voice guard then blocked `gh pr create` three times for code-reading conclusions, so I reworded one line and cut the follow-ups section and one validation bullet. Pushed the branch on its own, set the gh default to Swarneil1905/LocalMind, and opened PR #12.

**Toolkit**

- Tool: Claude Code in my LocalMind clone, working from plan.md and analysis-notes.md; I reviewed and approved each step.
- What it produced: the two doc edits, the cited notes, test_evidence.md, the ship manifest and pr_draft.md.
- Difficulties: it surfaced findings outside my issue and had to be kept to the plan; it also pre-ticked nothing it could not verify, leaving the "I reviewed the diff for private data" box for me.
- What changed: I scoped every request to plan.md, asked for file:line evidence before keeping any claim, and read the diff myself before committing.

---

## Pull Request

**PR Link:** https://github.com/Swarneil1905/LocalMind/pull/12

**PR Description:** The PR follows LocalMind's template. What changed and why: SECURITY.md and docs/CODEBASE.md described only local Ollama inference and Gmail/Runway credentials, while the code has optional cloud chat (OpenAI, Anthropic, Google Gemini) with keys in the OS credential store and an OpenAI image path that reuses the OpenAI chat key; the PR updates only those two docs, describes cloud chat approval as standing per provider, model and data categories, and records one deviation from my plan (the Windows-only credential-store sentence). Limitations: it describes backend enforcement only, not the desktop approval screens, and the diagram and "Follow one chat message" step 5 still show the default local path. Related issue: Fixes #10. Validation: the before/after grep, the 84-link check (including a deliberate-break test), 14/14 Node declaration tests, and the Python suite listed as not run with the reason. AI usage: assisted, naming Claude Opus 5.5 in Claude Code and what it did. Team review: not run, with the manual review I did instead, and both review boxes ticked.

**Maintainer Feedback:**

- 2026-11-10: Swarneil1905 replied to my claim comment within about 8 minutes, thanked me for taking it on and invited questions about the issue or codebase.
- 2026-11-10: No reply needed; I answered by opening the PR.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (my manifest's link check and the repo's Node declaration tests, then my pr-precheck on the branch, plan, evidence and description), run with `check_gate.py --branch`, then `gh pr create` through Claude Code so the voice guard and ship-gate both checked it.
- What it produced: a STOP on the first run (plan-fidelity and standards-met failed), `ship-gate: PASS` on the second, and PR #12 on Swarneil1905/LocalMind after the voice guard cleared the description.
- Difficulties: the hook only covered the Bash tool on Windows until I fixed it; the session I first opened the PR from had started before the gate was installed; and the voice guard blocked my PR three times on my own "never post a conclusion reached by reading code" rule.
- What changed: recorded the deviation in plan.md and the PR, ticked the review box only after confirming I had reviewed the diff, cut the unconfirmed follow-ups and a code-read validation bullet from the description, and pushed separately from `gh pr create` so the guard could not block the push.

---

## Learnings and Reflections

**Technical Skills Gained**

- Tracing where user data and API keys travel in a real Python service: from the credential allow-list in `credentials.py`, through consent checks in `provider_store.py` and `providers.py`, to the outgoing requests in `cloud_images.py`.
- Writing security docs that separate what the code enforces from what has been verified live, and not letting "the code has it" become "it works".
- `gh search issues` query syntax (a quoted OR list is searched as one phrase) and ranking results from the fields gh returns.
- How Claude Code hooks work: matchers, exit code 2 as a block, and that on Windows commands can go through the PowerShell tool instead of Bash.
- Fork and upstream workflow, `.git/info/exclude` for local-only files, and `gh repo set-default` so a PR lands on the original repo.

**Challenges Overcome**

- Six of my seven candidate issues were already taken, closed to fork PRs or claimed by a classmate. My scout's evidence showed why each one failed, and I went back to the one issue it had accepted and asked about it in Slack.
- Both my voice guard and my ship-gate silently skipped commands on Windows because they only watched the Bash tool. I found it with the wiring test, confirmed the script itself worked by feeding it input directly, and widened both.
- My own voice guard blocked my PR three times for stating code-reading conclusions as facts. Instead of rewording around it, I cut what I could not back with a run.

**What I'd Do Differently Next Time**

- Read CONTRIBUTING.md before falling for an issue: I picked dismech as my favorite before learning it does not accept PRs from forks.
- Set up a virtual environment and run the repo's tests early, so more of my claims rest on a run instead of on reading code.
- Install every hook first and start one fresh session afterward, instead of discovering mid-task that the current session started before a hook existed.
- Spread the work across the four weeks it was designed for, so the tools get iterated instead of just installed.

---

## Resources Used

- LocalMind issue #10 and its "Done when" list, which set the scope of the fix: https://github.com/Swarneil1905/LocalMind/issues/10
- LocalMind CONTRIBUTING.md (fork and branch rules, the docs-only check rule, the PR template): https://github.com/Swarneil1905/LocalMind/blob/main/CONTRIBUTING.md
- LocalMind docs/AI_CONTRIBUTIONS.md (how to fill AI usage and Team review honestly): https://github.com/Swarneil1905/LocalMind/blob/main/docs/AI_CONTRIBUTIONS.md
- Swarneil1905's reply to my claim, which confirmed the maintainer was active: https://github.com/Swarneil1905/LocalMind/issues/10
- The INSTALL.md files in the unit 6 and unit 7 starter repos, especially the troubleshooting notes that pointed me at the hook command when the wiring test did not fire.
