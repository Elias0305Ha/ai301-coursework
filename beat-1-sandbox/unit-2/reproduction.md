# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Elias0305Ha

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/13#issuecomment-5900458771

Claiming this one as a first contribution here.

My plan: stand up the repo locally following docs/SETUP.md, then look at tests/integration/, which currently holds only an empty __init__.py, and work out how to inject a fixed response into ReviewGenerator so a full retrieval → generation → parsing run can be tested against the mock LLM provider. I'll post what I find before writing anything.

No fix or timeline promised, just the investigation.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/13#issuecomment-5901556112

Confirmed the gap on a fresh clone: there is no integration test for the RAG pipeline, while the unit suite runs cleanly.

**Environment**
- OS: Windows 11, commands run in Git Bash
- Python: 3.11.9 (venv created with `py -3.11`)
- pytest: 9.1.1
- Commit: f89c06f (main)

**Steps**
From a fresh clone of the repo, at the repo root:

```
py -3.11 -m venv .venv
.venv/Scripts/python -m pip install --upgrade pip setuptools wheel
.venv/Scripts/pip install -e ".[dev]"
ls tests/integration
.venv/Scripts/python -m pytest tests/integration -v
.venv/Scripts/python -m pytest tests/unit -q
```

I installed dependencies directly rather than running `make setup`, since setup also runs migrations that need the Docker services, and Docker is unavailable on this machine. Nothing in these steps touches the database.

**What happened**

```
$ ls tests/integration
__init__.py

$ .venv/Scripts/python -m pytest tests/integration -v
collected 0 items
no tests ran in 1.18s

$ .venv/Scripts/python -m pytest tests/unit -q
375 passed, 53 xfailed, 3 warnings in 15.53s
```

**Result**
Matches the issue: the unit tests exist and pass, but `tests/integration/` contains no tests, so nothing runs a full query through retrieval, generation, and parsing together. I have not yet looked at how ReviewGenerator builds its client, which is the next thing I'll check.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--limit 3` smoke run: 3/3. Confirmed the rubric executed and produced verdicts.
2. First full run: 15/20, below the bar. Categories: clear-accept 3/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. Every miss was a gold accept graded reject: pkg-05 and pkg-12 on steps-followable, pkg-11 on environment-recorded, pkg-09 on environment-recorded and behavior-matches-issue, pkg-10 on behavior-matches-issue.
3. `--only pkg-05,pkg-09,pkg-10,pkg-11,pkg-12` after loosening those three checks: 5/5.
4. Second full run: 19/20, but category floor unmet, disclosure 0/1. pkg-20, the one disclosure package, had flipped from reject to accept.
5. `--only pkg-20,pkg-01,pkg-11` after tightening conventions-followed, with pkg-01 and pkg-11 as canaries: 3/3.
6. Third full run, saved as eval-run.txt: 19/20, PASS. Categories: clear-accept 7/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4.

**Package analysis**

pkg-20 (ghostty-org/ghostty#13604). Gold label: reject. My final rubric: reject, agreeing.

The reproduction itself is strong. It names the release build, platform, and desktop environment, gives exact launch commands, shows the wrong `997;2` answer, and includes a control run with a conditional theme pair that answers correctly. Environment, steps, behavior, and honesty all pass on the merits. What sinks it is the repo facts block: the policy requires that all AI usage in any form be disclosed, stating the tool and the extent, and neither the claim comment nor the report says anything about AI.

My rubric's reading of it changed across runs. In run 2 it rejected. In run 4 it accepted, because my conventions check only failed a comment that contradicted a stated rule, and silence contradicts nothing. The grader read "no disclosure" as "nothing unmet." In run 6 it rejects, because the revised check says outright that a required statement missing from the comments is a fail, not a neutral.

**Check rationale**

| conventions-followed | The repo's stated contribution rules (the repo-facts block in eval mode, the repo's own docs in live mode), read against the comments in the package | Every requirement the repo states is visibly satisfied in the comments themselves. Where the repo requires a disclosure, a statement, or a named piece of information, its absence from the comments is a fail, not a neutral. Where the repo states no requirement, this passes. | required |

My first version said every stated requirement had to be "satisfied by the comments" and that "no stated requirement passes." That wording let the grader treat an absent disclosure as a non-event, which is how pkg-20 flipped to accept in run 4 and emptied the disclosure category. A disclosure rule is broken by silence, not by a wrong statement, so the revised check makes absence an explicit fail and requires the requirement to be visibly met in the comments themselves. I made the same change in the Comms section of the evidence guide, so the map and the check say the same thing.

**Trade-offs**

Tightening conventions-followed changed pkg-03. It agreed in runs 2 and 4 (gold accept, graded accept) and in run 6 it was graded reject, failing only on conventions-followed. So the check now costs one clear-accept package. I accepted that trade because the disclosure category is a single package with no volume to make up the miss, while clear-accept has eight. Before the confirming run I re-ran pkg-01 and pkg-11 as canaries with `--only` to check the tighter wording did not reject clean packages from repos with no AI policy, and both held at accept. pkg-03 was not in that canary list, so the full run was the first place the cost showed up. A tighter canary set would have included every clear-accept package whose repo states a contribution policy.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
