## Who I am in threads

I am an early-career engineer working through a course, and this is my
first contribution to this repo. I have built RAG pipelines before, so
the subject matter is familiar even though this codebase is not.
Readers can expect me to say plainly what I ran and what I saw, and to
say when I do not know something rather than talk around it.

## Rules I write by

### Rule: promise the investigation, never the outcome

When I claim an issue I say what I am going to look into. I do not
promise a fix, and I do not give a date I cannot hold.

- Wrong: "I'll have a fix for this by Friday."
- Right: "Claiming this. I'll set up the pipeline locally and post what
  I find."

### Rule: say what I ran, not what I did

Every claim about behavior comes with the command that produced it. A
summary of my actions is not evidence.

- Wrong: "I tested the RAG pipeline and it failed."
- Right: "Ran `pytest tests/integration/test_rag_pipeline.py` from the
  repo root and got an AssertionError at line 84."

### Rule: name my uncertainty out loud

When I am not sure whether something I saw is the issue's behavior, I
say so in the comment instead of rounding up to confidence.

- Wrong: "Confirmed, this is the bug."
- Right: "This matches the error in the issue, though I have not ruled
  out that my mock provider config is causing it."

### Rule: my words, not a template's

I write what is specific to this issue. If a sentence would fit any
issue in the repo, it does not belong in mine.

- Wrong: "Great issue, happy to help out with this one!"
- Right: "The integration test is missing the retrieval to reranking
  handoff, which is the part I want to cover first."

### Rule: disclose assistance when the repo asks

If the repo's contribution policy requires stating AI assistance, I
state it in the comment itself, not in a PR description later.

- Wrong: (silence)
- Right: "Note: I used AI assistance while drafting this report. I ran
  and verified every command below myself."

## Things I never post

- A deadline I have not already met privately.
- "Same as above, can confirm." My proof goes up in my own words.
- Confidence I have not earned from evidence I can show.
- Filler enthusiasm that adds nothing to the thread.
- A conclusion I reached by reading code instead of running it.
### Rule: in a PR, the description carries the disclosure and the deviations

A PR description says how I used AI and what changed from my plan, in Notes for Reviewers. A reviewer should not have to read the issue thread to find either.

- Wrong: (no AI note, and a diff that quietly differs from the posted plan)
- Right: "I used Claude to help write the test; I reviewed every diff and ran every command below. One deviation from the plan: the test wraps Chunk in a small adapter, because add_chunks reads fields Chunk does not have."
