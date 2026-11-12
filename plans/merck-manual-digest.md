# Plan: Limited Merck Manual Digest

Code review: Approved by Architect on 2026-11-05; approval recorded in the conversation after the ingestion guard was strengthened.

## Goal

Demonstrate the existing agentic-RAG project on a small original digest of official Merck Professional Version topics. This is not a full-manual import or a clinically validated system.

## Current Result

Source preparation and isolated retrieval passed. Faithful model synthesis is blocked: three comparison attempts added clinical details absent from the digest, including the final attempt requesting verbatim quotations. The recorded answer is explicitly marked `FAILED_SOURCE_GROUNDING`. No further prompt retries, model installations, or original-corpus changes were made.

## Tasks

- [x] Verify the official reference, permissions, and three selected topic pages.
- [x] Create a brief original educational digest with source links, dates, and coverage limits.
- [ ] Verify isolated indexing, source-backed agent output, and the requested topic comparison.

## Scope

Inputs: [the educational digest](../docs/merck_manual/merck_manual_digest.md).
Runner: [run_digest.py](../docs/merck_manual/run_digest.py).
Only hypertension, type 1 diabetes, and community-acquired pneumonia are covered.
No full articles, treatment tables, medication doses, or patient-specific plans are imported.
All runtime stores reside under `docs/merck_manual/`; the default corpus and evaluation notebook are not changed.
No new models, dependencies, commits, or branches are needed for this demonstration.

## Verification

- Functional: first ingestion added one document; repeated ingestion added zero and skipped the existing document.
- Index: 16 child vectors verified with an exact `metadata.source` filter; parent-store source also required.
- Agent: the scope question returned all three topics with a digest citation and retrieved context.
- Regression: three incomplete parent/vector states rejected before graph execution, using isolated mocks without modifying a live store.
- Diagnostics: no reported errors in the runner.
- Topic comparison: failed source grounding in [last_query.json](../docs/merck_manual/last_query.json). Examples absent from the source include ACE inhibitors, GAD antibodies, and CURB severity scoring. Correct retrieval and citations did not prevent unsupported generation.
- Runtime: all comparison processes finished; there is no pending query.

## Run

From the repository root, with Ollama running and `granite4.1:8b` installed:

```powershell
.\.venv\Scripts\python.exe -X utf8 .\docs\merck_manual\run_digest.py
```

An optional quoted question replaces the default scope question. Each successful query overwrites `last_query.json` with its answer and retrieval evidence; first ingestion also creates the isolated Markdown, parent, and vector stores. The digest does not provide medical advice; whole-manual import still requires an authorized source file or appropriate reuse permission.

The recorded validation applies only to the captured output. Any rerun needs a fresh source check; the runner has no automatic medical-faithfulness validator. Do not treat generated clinical text as verified. A different model or enforced extractive workflow is a separate follow-up requiring verification.