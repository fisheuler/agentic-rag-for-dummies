# Dependency Usage Guide

This guide explains all 16 direct dependencies declared in [requirements.txt](requirements.txt) and [pyproject.toml](pyproject.toml). The shared installation includes application, notebook, evaluation, and tracing packages; not every package participates in every chat request.

## Library Reference

| Library and Pinned Version | Scope | Usage in This Project | Where to Look |
|---|---|---|---|
| `fastembed==0.9.0` | App, indirect backend | Produces sparse keyword-style vectors for hybrid retrieval. Used through the `FastEmbedSparse` adapter, rather than a direct `fastembed` import in application code. | [project/db/vector_db_manager.py](project/db/vector_db_manager.py): `VectorDbManager.__init__` |
| `gradio==6.30.0` | App UI | Builds the document-upload and chat interface using components such as `gr.Blocks`, `gr.Chatbot`, and `gr.ChatInterface`; serves the local web UI. | [project/ui/gradio_app.py](project/ui/gradio_app.py): `create_gradio_ui`; [project/app.py](project/app.py): `demo.launch` |
| `ipykernel==7.4.0` | Notebook tooling | Provides the Python kernel that executes notebook cells in VS Code/Jupyter. It is an execution dependency, not an import used by the app's retrieval pipeline. | [notebooks/agentic_rag.ipynb](notebooks/agentic_rag.ipynb), [notebooks/evaluation.ipynb](notebooks/evaluation.ipynb) |
| `langchain-huggingface==1.2.3` | App | Provides `HuggingFaceEmbeddings`, the LangChain interface that converts document chunks and search queries into dense vectors using the configured embedding model. | [project/db/vector_db_manager.py](project/db/vector_db_manager.py): `HuggingFaceEmbeddings` |
| `langchain-ollama==1.1.0` | App | Provides `ChatOllama`, connecting the agent to the local Ollama chat model for query rewriting, reasoning, context compression, and final-answer generation. | [project/core/rag_system.py](project/core/rag_system.py): `RAGSystem.initialize` |
| `langchain-qdrant==1.1.0` | App | Provides `QdrantVectorStore`, `FastEmbedSparse`, and `RetrievalMode.HYBRID`. Connects dense and sparse embeddings to the Qdrant collection used to store and search child chunks. | [project/db/vector_db_manager.py](project/db/vector_db_manager.py): `get_collection` |
| `langchain-text-splitters==1.1.3` | App | Provides `MarkdownHeaderTextSplitter` and `RecursiveCharacterTextSplitter`. Splits Markdown into parent sections, bounds parent sizes, and creates smaller overlapping child chunks. | [project/document_chunker.py](project/document_chunker.py): `DocumentChunker` |
| `langfuse==5.0.2` | App, opt-in tracing | Provides `get_client` and the LangChain `CallbackHandler` for tracing graph, tool, and model operations. The handler is added to the graph config only when tracing initializes successfully. | [project/core/observability.py](project/core/observability.py): `Observability`; [project/core/rag_system.py](project/core/rag_system.py): `get_config` |
| `langgraph==1.2.16` | App | Provides `StateGraph`, `ToolNode`, and `InMemorySaver`. Coordinates graph nodes, tool execution, subgraphs, conversation state, and pauses for clarification. Checkpoints here are in memory, not durable across process restarts. | [project/rag_agent/graph.py](project/rag_agent/graph.py): `create_agent_graph` |
| `matplotlib==3.11.2` | Evaluation notebook | Supplies `matplotlib.pyplot` (`plt`) for evaluation figures and their layout/display. It is not part of document indexing or answer generation. | [notebooks/evaluation.ipynb](notebooks/evaluation.ipynb): plotting cells |
| `pymupdf4llm==1.28.2` | Document ingestion | Provides `pymupdf4llm.to_markdown` to convert PDFs into Markdown before chunking. The current conversion skips images and excludes headers/footers. | [project/utils.py](project/utils.py): `pdf_to_markdown`, `pdfs_to_markdowns` |
| `python-dotenv==1.2.4` | App startup | Provides `load_dotenv`, loading the app's environment file before the UI and configuration-dependent components are imported. | [project/app.py](project/app.py): `load_dotenv` |
| `ragas==0.4.3` | Evaluation notebook | Supplies `llm_factory` and collection metrics scored through `ascore`: Answer Accuracy, Context Relevance, Response Groundedness, Context Precision, and Context Recall. Grades saved/generated answers and retrieved contexts; it does not run the app's chat graph. | [notebooks/evaluation.ipynb](notebooks/evaluation.ipynb): imports, judge setup, and metric-scoring cells |
| `seaborn==0.13.2` | Evaluation notebook | Supplies `seaborn` (`sns`) for statistical visualization of evaluation results, using Matplotlib figures underneath. | [notebooks/evaluation.ipynb](notebooks/evaluation.ipynb): plotting cells |
| `sentence-transformers==6.1.0` | App, indirect backend | Runs the dense embedding model behind `HuggingFaceEmbeddings`. It is the model-inference backend, while `langchain-huggingface` is the integration layer. It does not generate chat answers. | [project/db/vector_db_manager.py](project/db/vector_db_manager.py): `HuggingFaceEmbeddings` construction |
| `tiktoken==0.14.0` | App context management | Estimates message token counts to decide when the agent should compress context. Uses a cached GPT-family encoding when available and a character-count fallback otherwise. This is an estimate, not the Ollama model's exact token count. | [project/utils.py](project/utils.py): `_get_token_encoding`, `estimate_context_tokens`; [project/rag_agent/nodes.py](project/rag_agent/nodes.py): `should_compress_context` |

## How They Fit Together

1. **Prepare documents:** `pymupdf4llm` converts PDFs to Markdown; `langchain-text-splitters` builds parent and child chunks. Markdown uploads can skip PDF conversion.
2. **Build the index:** `langchain-huggingface` uses `sentence-transformers` for dense embeddings. `langchain-qdrant` uses `fastembed` through `FastEmbedSparse` for sparse embeddings, then connects both representations to Qdrant.
3. **Answer questions:** `langgraph` controls the workflow and tools; `langchain-ollama` supplies the chat model; `tiktoken` helps manage context size.
4. **Expose the application:** `python-dotenv` loads environment settings and `gradio` presents the upload/chat UI.
5. **Observe execution:** `langfuse` optionally records traces through callbacks. Tracing is opt-in even though the package is in the shared dependency list.
6. **Run experiments:** `ipykernel` executes notebook cells; `ragas` computes quality scores; `matplotlib` and `seaborn` visualize the results.

## Packages, Services, and Models

- **An adapter is not its backend.** `langchain-huggingface` wraps an embedding implementation; `sentence-transformers` runs the model. Similarly, `langchain-qdrant` integrates retrieval, while `fastembed` supplies its sparse embedding backend.
- **Ollama is separate software.** Installing `langchain-ollama` does not install the Ollama service or pull chat-model weights. The service and configured model must be available separately.
- **Embedding weights are separate from Python packages.** The configured Hugging Face model is loaded and cached by the embedding stack; the package version and model identifier are different settings. See [project/config.py](project/config.py).
- **Qdrant is local by default.** The app uses `QdrantClient(path=...)`, not a required standalone Qdrant server. A local index cannot be opened concurrently by both the app and a notebook.
- **Langfuse needs a backend when enabled.** Configure a cloud or self-hosted endpoint and credentials; the SDK alone is not a tracing dashboard. Without tracing, the app still runs.
- **There are transitive dependencies too.** Imports such as `qdrant_client`, `langchain_core`, `openai`, and `pandas` come from the wider installed dependency graph. This table covers the 16 explicitly declared packages, not every installed package.

## Maintenance

The versions above describe the current manifests, not suggested upgrades. After changing a dependency pin or moving an implementation, update this guide and keep its source links accurate. The guide does not change installation behavior or make the evaluation/tracing libraries mandatory parts of each answer.