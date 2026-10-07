# Unit 3: Plan and Build

## Posted upstream

**GitHub username**

Elias0305Ha

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/13#issuecomment-6029299968
Plan for this one, built on my repro above (tests/integration has only `__init__.py`, `pytest tests/integration` collects 0 items, unit suite passes at 375 passed / 53 xfailed).

What I found by reading review_generator.py:35 (not by running it): ReviewGenerator always builds a real OpenAI client in its constructor, and the `llm_provider="mock"` setting in core/config.py never reaches it. So the test has to bring its own fake client.

Plan: add tests/integration/test_rag_pipeline.py with one test marked `@pytest.mark.integration`, so `make test-integration` picks it up. It runs a real ChromaDB VectorStore in a temp folder with MockEmbeddingProvider, retrieves through HybridRetriever with KeywordSearcher indexed in memory, then runs generate_full_review with basic_profile.json and a fake client that returns fixed JSON. It checks that all 5 sections parse as JSON, carry source citations from the retrieved chunks, and that the prompts include the retrieved text. Since the folder will hold a test, I will also remove the exit-5 shim in ci.yml, as its comment asks.

Out of scope: no source changes, no wiring llm_provider into ReviewGenerator, and I am not touching the seeded defects in hybrid.py (#6) or output_parser.py.

Test: re-run my repro plus `pytest tests/integration -m integration -v`. Expect both integration runs to collect 1 test and pass, and the unit suite to stay at 375 passed / 53 xfailed.

Open questions: whether ChromaDB cleans up its temp folder on Windows, and whether CI behaves the same as my machine once the shim is gone. I will post here if either changes the plan.

I used Claude to help draft this plan and will use it while writing the test.


---

## Your branch

**Branch**

test/13-rag-pipeline-integration-test

**Evidence**

Before (my Unit 2 repro steps on main, before the change):

$part2 = @'


---

## Your branch

**Branch**

test/13-rag-pipeline-integration-test

**Evidence**

Before (my Unit 2 repro steps on main, before the change):

    $ .venv\Scripts\python -m pytest tests/integration -m integration -v
    collecting ... collected 0 items
    ============================ no tests ran in 0.53s ============================

    $ .venv\Scripts\python -m pytest tests/unit -q
    375 passed, 53 xfailed, 2 warnings in 16.39s

After (same steps on test/13-rag-pipeline-integration-test):

    $ .venv\Scripts\python -m pytest tests/integration -v
    collecting ... collected 1 item
    tests/integration/test_rag_pipeline.py::test_rag_pipeline_retrieval_generation_parsing PASSED [100%]
    ============================== 1 passed in 2.84s ==============================

    $ .venv\Scripts\python -m pytest tests/integration -m integration -v
    collecting ... collected 1 item
    tests/integration/test_rag_pipeline.py::test_rag_pipeline_retrieval_generation_parsing PASSED [100%]
    ============================== 1 passed in 2.63s ==============================

    $ .venv\Scripts\python -m pytest tests/unit -q
    375 passed, 53 xfailed, 2 warnings in 11.26s

Can-fail check (fake client temporarily returns plain text, then reverted):

    tests/integration/test_rag_pipeline.py::test_rag_pipeline_retrieval_generation_parsing FAILED [100%]
    E       AssertionError: assert ['general_feedback'] == ['skills_feed...t_impression']
    ============================== 1 failed in 3.11s ==============================


## Eval iterations

**Run history**

1. First full run: 0/20 scored. All 20 packages errored with "claude exited 1" because my Claude Code login had expired, so nothing was graded and eval-run.txt was not written ("agreement: 0/0 scored items").
2. Second full run after logging back in, saved as eval-run.txt: 18/20, PASS. "agreement: 18/20 scored items  (bar: 18/20: PASS)". Categories: clear-accept 5/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4.

**Package analysis**

pkg-05 (category clear-accept). Gold label: accept. My rubric: reject, disagreeing. The only failed check was unknowns-honest.

My unknowns-honest check fails a plan "when the unknowns are empty although the evidence leaves clear gaps." The packages have no separate risks section (calib-01 shows the layout: Cause, Change, Test, then the comment), so a plan that is clear and correct but never names an open question reads to the grader as certainty with nothing behind it. The grader treated "no unknowns written down" as "unknowns hidden", while gold reads the same plan as simply having no real gaps. pkg-14 missed the same way, on unknowns-honest plus stranger-could-start.

**Check rationale**

| unknowns-honest | The risks and unknowns, read against the diagnosis and the evidence | Real gaps in the evidence are named as uncertainty, not stated as fact. Fails when a guess is presented as certain, or when the unknowns are empty although the evidence leaves clear gaps. | required |

I adapted it from my Unit 2 outcome-honest check, which failed a conclusion "when nothing supports it." For a plan, the dishonest move is not a missing artifact but a guess about the cause written as fact, so the first clause targets that. I rejected a structure version ("the plan has a risks section") because the rubric template warns that structure-shaped checks make graders disagree, and because the eval packages have no risks heading at all. The second clause is there so a plan cannot pass just by staying quiet about gaps the repro leaves open.

**Trade-offs**

This check costs two clear-accept packages: pkg-05 and pkg-14 are gold accept and my rubric rejects them, both failing unknowns-honest. The second clause ("fails when the unknowns are empty although the evidence leaves clear gaps") is what catches plans that state a guess as fact, but it also catches correct plans that simply do not list unknowns. I kept it as written because the full run met the bar with every category matched, and loosening it would risk flipping wrong-cause packages that the check helps reject, which I could only confirm with more runs. The case I accept it will miss is a plan that is honest by being narrow and correct, without saying so out loud.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in `tools/plan-check/`.
