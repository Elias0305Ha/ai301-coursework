# Plan: add an integration test for the RAG pipeline (#13)

## Diagnosis

My repro on a fresh clone (commit f89c06f, Windows 11, Python 3.11.9, pytest 9.1.1) showed the gap:

    $ ls tests/integration
    __init__.py

    $ .venv/Scripts/python -m pytest tests/integration -v
    collected 0 items
    no tests ran in 1.18s

    $ .venv/Scripts/python -m pytest tests/unit -q
    375 passed, 53 xfailed, 3 warnings in 15.53s

So the unit tests pass, but nothing runs a query through retrieval, generation, and parsing together.

Why it is not just a missing file: reading rag/generator/review_generator.py (line 35), ReviewGenerator always builds a real client in its constructor: `self.client = openai.OpenAI(api_key=config.api_key, base_url=config.base_url)`. The `llm_provider: str = Field(default="mock")` setting in core/config.py:18 never reaches it, so there is no built-in way to run generation without a real API. A test has to supply its own fake client. This comes from reading the code, not running it.

Issue #13 asks for the test to be marked `@pytest.mark.integration`, and `make test-integration` (Makefile:43) runs `pytest tests/integration -v -m integration`, so an unmarked test would be skipped there.

## Scope

In:
- One new test file, marked `@pytest.mark.integration`, that runs retrieval, generation, and parsing together, offline, with a fake LLM client.
- Removing the temporary shim in .github/workflows/ci.yml (lines 80 to 94, keeping a plain `pytest tests/integration -v --tb=short` step) that treats "no tests collected" (exit 5) as a pass. Its own comment says to delete it once the directory holds at least one test.

Out:
- No changes to source code in rag/ or ingestion/.
- Not wiring llm_provider into ReviewGenerator. That is a design change for maintainers, not part of adding a test.
- Not fixing the seeded defects marked in rag/retriever/hybrid.py (issue #6) or rag/generator/output_parser.py. Both are marked as course material to keep.
- No new dependencies, no database, no Docker.

## Files to touch

- tests/integration/test_rag_pipeline.py (new)
- .github/workflows/ci.yml (remove the exit-5 shim only)

If fixtures get long enough to need tests/integration/conftest.py, I will record that under Deviations.

## Approach

1. Create tests/integration/test_rag_pipeline.py with one test function marked `@pytest.mark.integration`.
2. Create VectorStore(persist_dir=<pytest tmp_path>) so ChromaDB writes to a temp folder.
3. Build 2 or 3 Chunk objects (ingestion/chunking/base.py) from the sample_resume_text and sample_readme_text fixtures in tests/conftest.py, embed them with MockEmbeddingProvider (ingestion/embeddings/provider.py), and add them with add_chunks to collection profile_test.
4. Index the same chunks as dicts with KeywordSearcher.index(chunks), which is in memory, and build a HybridRetriever from both.
5. Call retrieve() with a query, profile_id "test", the mock embedding of the query, and min_score=0.0 so the mock embeddings are not filtered out.
6. Load tests/fixtures/sample_profiles/basic_profile.json as profile_data.
7. Build ReviewGenerator with a dummy ReviewConfig (api_key "test", any base_url, model "test"), then replace generator.client with a fake whose chat.completions.create returns a fixed ```json response. It returns one response per call, keyed to the section being generated, because _consolidate_feedback drops sections with duplicate names.
8. Call generate_full_review(profile_data, retrieved_chunks) and assert:
   - 5 sections come back, none with content starting "Error generating"
   - each section has confidence 0.9 and the suggestions from the fixed JSON, which proves the JSON parsing path ran, not the plaintext fallback
   - section content ends with a "Sources:" line naming the source_id of a retrieved chunk, which proves retrieval output reached generation
   - the fake was called 5 times and each prompt contains retrieved chunk text
9. Delete the exit-5 shim in .github/workflows/ci.yml lines 80 to 94, keeping a plain `pytest tests/integration -v --tb=short` step.

## Test plan

Re-run my unit 2 repro steps after the change, plus the marked run that make test-integration uses:

    .venv/Scripts/python -m pytest tests/integration -v
    .venv/Scripts/python -m pytest tests/integration -m integration -v
    .venv/Scripts/python -m pytest tests/unit -q

Before (from my repro): `collected 0 items`, `no tests ran`.
Expected after: both integration runs collect 1 item and it passes. The unit run stays at 375 passed, 53 xfailed.

Extra check that the test can fail: temporarily make the fake return plain text instead of JSON and confirm the test fails (plain text parses as one general_feedback section, so the 5-sections assertion fails first), then revert.

## Risks and unknowns

- Chunk and KeywordSearcher.index are located now, but I have not run them yet. If KeywordSearcher.index needs fields Chunk does not have, I will build the dicts by hand and note it under Deviations.
- ChromaDB on Windows may hold file locks on the temp folder during cleanup. If it does, I will close or drop the client in teardown.
- Removing the CI shim means CI now fails if the integration test fails or is not collected. That is the intent, but CI's environment may differ from mine (no Docker locally, so I cannot run the full CI job).
- The fake keys responses to the order of section_names in generate_full_review. If that list changes, the test needs updating.
- The keyword scoring bug in hybrid.py (issue #6) affects scores, so the test asserts which chunks come back and that they reach the prompt, not exact scores or ranking. MockEmbeddingProvider is not semantic, so ranking would not mean anything anyway.

## Deviations

- Step 3: the Chunk class in ingestion/chunking/base.py only has `text` and `metadata`, but VectorStore.add_chunks reads `chunk.id`, `.source_id`, `.chunk_index` and `.section`. Passing real Chunk objects would raise AttributeError. The test builds real Chunks and wraps each in a small adapter (`_as_vector_record`) before calling add_chunks. No source code changed. Nothing in the repo calls add_chunks today, so this looks like a latent bug that may deserve its own issue; I left it out of scope.
- Step 4: the keyword index dicts are built by hand with `id`, `text` and `metadata`, using the same ids as the vector store so the hybrid merge lines up. The Risks section already allowed for this.
- Everything else went as planned: one marked test, the CI exit-5 shim removed, both integration runs collect 1 item and pass, unit suite still 375 passed, 53 xfailed, and the test fails at the 5-sections assertion when the fake returns plain text.


