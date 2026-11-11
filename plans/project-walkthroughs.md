# Plan: Project Walkthroughs for RAG Interviews

**Status:** Approved by PM on 2026-11-11
**Status:** Approved by Researcher on 2026-11-11

## Goal

Add source-linked study guides in the style of Graph.md so the user can explain this project's design, follow a request end to end, and discuss RAG tradeoffs without confusing existing features with proposed improvements.

## Scope

Create `docs/project-guide/README.md`, `core.md`, `db.md`, and `rag-agent.md`. Reuse the existing Graph.md for graph wiring and DEPENDENCIES.md for package details. Link the reading path from the root README and Graph.md. Keep all application code, dependencies, notebooks, stored documents, and running services unchanged. Do not generate a separate whole-repository archive or PDF.

## Tasks

- [x] Write the overview and three module walkthroughs, each with current code links, relevant diagrams, lifecycle/data-flow explanations, and interview questions with answer guidance.
- [x] Add navigation from the root README and Graph.md, preserving the completed graph-document polish.
- [x] Verify link targets and code anchors, Markdown structure, module coverage, factual scope, and fresh-context review.

## Acceptance Criteria

- Overview: connect startup, ingestion, and answering; assign responsibilities to modules; include evaluation and security/performance discussion with current behavior separated from proposed improvements.
- Core guide: explain bootstrap, document lifecycle and partial-failure handling, streaming, clarification resumption, session ownership, and optional tracing.
- Database guide: explain JSON parent identity, hybrid Qdrant child retrieval, the character-based chunking contract, consistency/versioning limitations, and local index ownership.
- Agent guide: explain shared/worker state and reducers, nodes and tools, research budgets, compression, and failure/grounding limitations without repeating the full graph wiring.
- Explain PDF/Markdown ingestion, bounded parent-child chunking, dense/sparse child retrieval, parent lookup, LangGraph orchestration, streaming, clarification, session state, and optional tracing.
- Distinguish chunk sizes in characters from context-token estimates; durable document stores from in-memory checkpoints; retrieval relevance from answer faithfulness.
- Discuss actual local-demo limitations: single-user state, local-index locking, nontransactional ingestion, duplicate/source identity, error sentinels, and missing production features.
- Reading order: overview, core, db, the existing Graph.md, then rag-agent. Learning outcomes: give a concise project pitch, trace ingestion and chat/clarification, and explain retrieval relevance versus answer faithfulness.
- Include practical interview preparation: question/answer prompts and tradeoff/improvement exercises. Do not invent benchmark results, assert exhaustive repository scanning, or claim production readiness.
- Link implementation claims to current symbols and source-line anchors. Verify links, Markdown structure, and anchors through file reads and AST parsing only; do not import project modules, initialize managers/graphs, execute the application or notebook cells, or operate on databases.

## Review Gates

Preimplementation plan approval is separate from completed-content approval. Capture PM and Researcher plan approvals before writing. After writing, run static checks and obtain fresh-context factual review of all four guides, navigation changes, and the polished Graph.md. The already observed 35 Graph.md code links and two diagrams are a static baseline, not factual approval.

## Verification Results

- All four guides exist and cover the approved reading order, component responsibilities, 20 interview Q&A prompts, and tradeoff exercises.
- Static checks passed for 120 guide link occurrences, current code anchors, Markdown structure, and five new diagram blocks.
- Combined guide/Graph validation passed for 156 local link occurrences and seven diagram blocks; README and Graph navigation both reach the overview.
- Documentation diagnostics and the scoped whitespace check passed. The copied tool transcript was removed from Graph.md while retaining its source-linked explanation.
- Final factual/content review: Approved by Engineer on 2026-11-11 for documentation only, including all four guides, Graph.md, and root navigation.
- No models, managers, application code, notebooks, databases, or services were executed or changed for validation. Mermaid blocks were checked as Markdown but not browser-rendered; no runtime, performance, or production-readiness result is claimed.