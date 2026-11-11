# Plan: Group Study Guides Under Plans

**Status:** Approved by PM on 2026-11-11
**Status:** Approved by Researcher on 2026-11-11

## Goal

Move the seven Markdown study guides from `docs/project-guide` into `plans/project-guide`, keeping `docs` for ingested documents and local corpus artifacts.

## Tasks

- [x] Move all seven guides without renaming or losing content; preserve Git tracking/history and existing staged-versus-unstaged content.
- [x] Update the root README and active planning references, and complete the pending chunking/storage guide cross-links. Keep completed historical plan records unchanged.
- [ ] Validate file inventory, source anchors, navigation, Markdown examples, and the staged move; obtain independent factual review of the relocation and pending retrieval/storage documentation.

## Boundaries

The destination is `plans/project-guide/`. Existing top-level plan files stay in place. Do not move ingested PDFs, converted Markdown, JSON parent records, vector indexes, notebooks, or the Merck investigation. Do not run ingestion, models, notebooks, or databases. Tracked file moves may update the index; preserve new guides as untracked and do not stage unrelated changes. No commit, push, force operation, or runtime-code change is requested.

Relative links to `../../project`, `../../notebooks`, and `../../DEPENDENCIES.md` should remain valid because the new directory has the same depth. Sibling guide links also remain valid. Rewrite live references to the old guide directory, but preserve historical path mentions in the completed project-walkthrough plan. Old published URLs are not external redirects; this change updates repository navigation only.