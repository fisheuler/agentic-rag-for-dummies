# Plan: Document Chunker Walkthrough

**Status:** Approved by PM on 2026-11-11
**Status:** Approved by Researcher on 2026-11-11

## Goal

Explain the current DocumentChunker algorithm in a new source-linked guide under `plans/project-guide`, including worked examples and interview preparation.

Follow-up requests extend the walkthrough with search/retrieve tools and add `plans/project-guide/storage-managers.md` explaining how VectorDbManager and ParentStoreManager persist and query the resulting chunks.

## Tasks

- [x] Write `plans/project-guide/document-chunker.md` covering API contracts, configuration, every processing pass, metadata/IDs, boundary cases, and interview questions.
- [x] Add a reading-path link from the project-guide overview without changing application code.
- [x] Check source anchors and Markdown links, verify small in-memory examples, and obtain fresh-context review.
- [x] Complete the tool-search explanation and a linked storage-manager deep dive covering initialization, chunk identity/data contracts, write/query flows, collection checks, JSON operations, and consistency limits.
- [ ] Verify mocked tool/manager behavior, source links, navigation, and fresh-context factual review while preserving code, corpus, and untracked guide content.

## Validation and Boundaries

Use the existing project environment and synthetic in-memory documents. Do not initialize a RAG system, embedding model, database, or notebook; do not read user documents or write chunks. Verify that sizes are characters, the maximum parent size is an enforced bound, the minimum is best-effort, and overlap is splitter-dependent. Include the effects of rebalancing and source-name defaults without claiming stronger guarantees than the code provides. Preserve existing staged changes; do not commit, push, or stage this new work.

The subsequent folder relocation requested by the user is tracked separately in [project-guide-relocation.md](project-guide-relocation.md). It updates staged file locations, not application behavior; the new chunking/storage guide content remains untracked.

For follow-up manager checks, replace Qdrant clients, embedding providers, and filesystem paths with fakes before construction. Test create/reuse/dimension-mismatch behavior and JSON serialization/lookup without a real index, model download, or disk writes. Clearly distinguish mocked contracts from real search quality, persistence durability, and transaction guarantees.

## Follow-Up Acceptance Criteria

- Distinguish the side effects of [vector construction](../project/db/vector_db_manager.py#L11) from [get_collection](../project/db/vector_db_manager.py#L59), which returns a hybrid wrapper around the existing client/providers. Check the constructor and wrapper arguments with fakes; do not claim project-defined fusion or automatic tenant isolation.
- Check new collection creation, compatible reuse, and known dimension mismatch in [create_collection](../project/db/vector_db_manager.py#L29). State that dimension compatibility is not an embedding-model/revision/version fingerprint or a comprehensive collection-configuration validation.
- Show the stored `page_content`/`metadata` JSON from [save](../project/db/parent_store_manager.py#L15) and its conversion to `content`/`parent_id`/`metadata` by [load_content](../project/db/parent_store_manager.py#L38), distinct from the tools' formatted evidence strings.
- Check that [load](../project/db/parent_store_manager.py#L32) propagates missing/invalid-file errors, while [list_sources](../project/db/parent_store_manager.py#L55) suppresses only `OSError` and `JSONDecodeError`. Do not describe it as a general schema validator. Cover batch methods, numeric-suffix ordering, deletion, and clearing without real file operations.
- Trace [parent-first ingestion](../project/core/document_manager.py#L13) and child-search/parent-ID lookup; state that partial writes and cleanup do not form a transaction and that listing sources does not prove vector-index integrity.
- Explain shared configured directories, the tool factory's separate parent-manager instance, the parent constructor's import-time default-path binding, positional IDs, trusted-path/authorization assumptions, and local index ownership. Identify production safeguards as proposals, not existing guarantees.

## Verification Results

- Six synthetic scenario checks passed: merging/tail handling, rebalancing, forward metadata order, oversized splitting/children/IDs, short heading-based input with source-name fallback, and empty input.
- Measured examples matched the guide: 2,504-character merged parent, 4,000/2,100-character oversized split with 15 children, and 2,251/2,251-character rebalanced pair.
- All 17 guide links and 42 combined overview/guide link occurrences resolve; named source anchors match the current definitions.
- Documentation diagnostics passed. Final factual/content review: Approved by Engineer on 2026-11-11.
- No user documents, stored chunks, embedding models, databases, or notebook cells were processed. The diagram was checked as Markdown, not browser-rendered. These are focused behavior checks, not corpus-quality or performance benchmarks.