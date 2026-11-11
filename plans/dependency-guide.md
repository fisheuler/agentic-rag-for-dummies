# Plan: Dependency Usage Guide

**Status:** Approved by PM on 2026-11-11
**Status:** Approved by Researcher on 2026-11-11

## Goal

Explain every directly declared library's role in this repository, with source references rather than generic package descriptions.

## Tasks

- [x] Add a root-level dependency guide covering all 16 pinned libraries, their APIs, and their runtime or notebook roles.
- [x] Link the guide from the repository README without changing dependencies, notebooks, application code, or data.
- [x] Check manifest coverage, pinned versions, source references, Markdown links, and review the documentation.

## Validation

The guide must match both dependency manifests, resolve every local link, distinguish indirect embedding backends from wrappers, and avoid claiming that notebook tooling is required for every app request. Validation must not import or instantiate the RAG system, access its database, or change notebook kernels.

## Results

- All 16 guide pins match both manifests, with no missing or duplicate entries.
- All 23 local guide links resolve; the README entry points to the guide.
- Named project APIs, evaluation plotting/scoring references, and the two installed embedding backends were checked without loading the RAG system.
- Documentation diagnostics and the scoped whitespace check passed.
- Final documentation review: Approved by Researcher on 2026-11-11. This is documentation validation, not an application-runtime test.