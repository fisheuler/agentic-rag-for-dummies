# Plan: Hybrid Retrieval and Parallel Reasoning Guide

**Status:** Approved by PM on 2026-11-11
**Status:** Approved by Researcher on 2026-11-11
**Status:** Approved by Engineer on 2026-11-11

## Goal

Create a separate source-linked explanation of dense/sparse retrieval and the parallel-agent research, aggregation, and final-response workflow discussed with the user.

## Tasks

- [x] Write `plans/project-guide/hybrid-retrieval-and-parallel-reasoning.md` with the rationale, implementation trace, diagrams, examples, tradeoffs, and interview questions.
- [x] Link the new guide from the study overview without rewriting the existing component guides.
- [x] Validate Markdown/source links and example structures, obtain independent factual review, and preserve the existing staged relocation.

## Acceptance Criteria

- Cite current source for `Qwen/Qwen3-Embedding-0.6B` dense embeddings through HuggingFaceEmbeddings/SentenceTransformers, `Qdrant/bm25` sparse embeddings through FastEmbedSparse/FastEmbed, and the hybrid vector-store wrapper. Distinguish semantic and lexical signals over the same children; parent expansion is exact ID lookup, not another embedding search. Do not invent a project-defined fusion formula.
- Trace one Send per rewritten question, AgentState isolation and tool budgets, collect_answer preserving answer/context fields, the State.agent_answers reducer, graph fan-in, and question-index ordering. State that final aggregation receives the original query and worker answer strings rather than raw context arrays. Link the final AIMessage and aggregate-only UI streaming paths.
- State that output-format compliance does not establish truth; the shared LLM/backend may serialize execution, model-call costs increase, and sequential dependencies may need a different workflow. Neither hybrid retrieval nor parallel workers guarantee an objective quality or latency improvement.
- Validate source anchors, links, Markdown, and Python/JSON examples with AST/parser checks only, without executing the examples or initializing a model/index. Reuse the already-passed fake worker/reducer/aggregation evidence rather than claiming new end-to-end testing.

## Boundaries

Documentation only. Change only the new guide, `plans/project-guide/README.md`, and this plan's progress record. Reuse existing code evidence and the already-passed fake worker/reducer/aggregation checks. Do not initialize a model, index, application, or notebook, and do not modify Python source, other component guides, or user data. Do not stage, commit, push, or alter the pending folder-relocation changes.

## Verification

Static checks passed on 2026-11-11: the three edited documents parse; all 51 local link occurrences resolve, including 24 in the new guide; 16 named Python source anchors match AST definitions; the Send example parses with the expected input keys; and the worker-record JSON parses with the expected fields. Both Mermaid blocks were checked as Markdown source, not rendered in a browser.

The existing staged diff remained unchanged during the navigation and finalization checks. No application modules, examples, models, databases, or notebooks were executed for this documentation change. Earlier fake-helper checks support the Send payload, retained worker contexts, reducer accumulation/reset, index-sorted aggregation, and answer-only aggregation prompt; they are not end-to-end RAG evidence.

Fresh-context Engineer review approved the guide and navigation on 2026-11-11 with no factual or actionable findings. Mermaid browser rendering and end-to-end RAG/notebook execution remain outside this documentation-only verification.

## Follow-up: Split the Guide

**Status:** Approved by PM on 2026-11-11
**Status:** Approved by Researcher on 2026-11-11
**Status:** Approved by Engineer on 2026-11-11

The user requested two separate Markdown documents. Preserve the reviewed content and source anchors, placing dense/sparse retrieval in `plans/project-guide/hybrid-retrieval.md` and worker research, aggregation, and streaming in `plans/project-guide/parallel-agent-reasoning.md`. Give each guide its own introduction, diagram, tradeoffs, interview questions, and a link to the other. Replace the combined overview link and remove the old combined document rather than retaining a third guide.

- [x] Split the existing content into the two topic-specific guides and update overview navigation.
- [x] Validate Markdown, local links, source anchors, examples, old-path removal, and staged-content preservation; obtain fresh factual review.

This follow-up is limited to the old/new guide paths, the overview, and this plan record. The original implementation/verification above is historical. No runtime, dependency, corpus, notebook, staging, commit, or push changes are authorized by this split.

Split verification passed on 2026-11-11: both new guides and overview navigation parse; all 261 local link occurrences across nine study guides resolve; no live references to the removed combined file remain; and 16 named source anchors match AST definitions. Both Mermaid sources and the Python/JSON examples match their original content hashes, and the examples parse. The staged diff and tracked non-Markdown diff remain unchanged. Editor diagnostics reported no errors in the four edited Markdown files. Fresh-context Engineer review approved the split without findings. These are static documentation checks, not Mermaid browser rendering or runtime/model/database/notebook execution.