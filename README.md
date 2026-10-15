# GenAI Retrieval & Agentic Engineering

**A portfolio of 12 applied AI systems covering agent orchestration, retrieval-augmented generation, structured tool use, document intelligence, database querying, and artifact-producing workflows.**

[![Python](https://img.shields.io/badge/Language-Python-blue.svg)](https://www.python.org/) [![Projects](https://img.shields.io/badge/Projects-12-2D7D9A.svg)](#project-index) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

## Repository overview

This repository collects 12 standalone project folders developed to explore practical GenAI engineering patterns. The projects range from hybrid retrieval and transcript Q&A to multi-agent workflows, natural-language SQL, evaluation-oriented retrieval comparisons, and a remote Windows executable build pipeline.

The emphasis is on inspectable pipelines: explicit tool boundaries, typed or structured intermediate representations where implemented, observable execution, sample outputs, and clear disclosure of limitations. **The projects do not share one runtime, one dependency set, or one deterministic guarantee.** Each folder has its own README with setup notes and project-specific caveats.

> **Scope:** These are research, learning, and portfolio implementations. They are not production medical, legal, investment, nutrition, or security products. LLM review steps and Pydantic schemas improve structure; neither guarantees factual correctness, safety, or suitability.

## Project index

| # | Project | Main engineering focus | Principal stack / services | Source entry point | Important qualification |
|---:|---|---|---|---|---|
| 1 | [Deterministic Multimodal Food Intelligence System](./Deterministic_Multimodal_Food_Intelligence_System/README.md) | Food image naming + top-1 metadata retrieval | OpenAI vision, SentenceTransformers, Pinecone, Gradio | `deterministic_multimodal_food_intelligence_system.py` | Index must be provisioned separately; nutrition fields are not independently verified. |
| 2 | [X-Ray Agentic Radiologist](./agentic-radiologist/README.md) | Vision findings, internal retrieval, web evidence, report review | LangGraph, OpenAI, Pinecone, Tavily, Gradio | `agentic_radiologist.py` | Research prototype; not for diagnosis or clinical decisions. |
| 3 | [Agentic Windows EXE Builder](./agentic_software_development/README.md) | ReAct code generation, syntax check, remote Windows build | LangGraph, LangChain, Gradio, GitHub Actions | `agentic_windows_exe_builder.py` | Colab export; remote workflow inputs must match the companion workflow; patch-limit edge needs hardening. |
| 4 | [BeeAI Investment Analyst](./beeai-investment-analyst/README.md) | Coordinator/specialist agent handoff, SIP calculation, JSON validation | BeeAI Framework, Pydantic, Gradio, OpenAI | `beeai_investment_workbench.py` | Example asset universe and assumed 12% SIP rate; not financial advice. |
| 5 | [CrewAI Local Grocery Planner](./crewai-local-grocery-planner/README.md) | Sequential recipe-to-shopping-plan agents | CrewAI, Serper, Pydantic, Gradio | `crew_ai_local_grocery_planner.py` | BigBasket constraint is prompt-level; pricing and budget adherence are best effort. |
| 6 | [DocChat Hybrid Policy Assistant](./docchat-hybrid-policy-assistant/README.md) | Document conversion and hybrid policy retrieval | Docling, BM25, Chroma, LangGraph, Gradio | `doc_chat.py` | Upload occurs in Colab before UI launch; relevance-check exception currently fails open. |
| 7 | [LangGraph Orchestrator–Worker System](./langgraph-orchestrator-worker-system/README.md) | `Send()` fan-out and synthesis with PDF/log artifacts | LangGraph, Pydantic, xhtml2pdf, Gradio | `langgraph_orchestrator_worker_pattern.py` | UI returns collected logs after graph completion; it is not real-time log streaming. |
| 8 | [LangGraph Reflexion Investment Agent](./langgraph_reflexion_investment_agent/README.md) | Generate–evaluate–refine loop | LangGraph, LangChain, Pydantic, xhtml2pdf, Gradio | `langgraph_reflexion_investment_agent.py` | Temperature 0.7; no live market data or hard allocation validator. |
| 9 | [Legal Compliance Reflexion Agent](./legal-compliance-reflexion-agent/README.md) | Structured contract review with reflection and web research | LangGraph, LangChain, Tavily, Pydantic, FPDF, Gradio | `legal_compliance_reflexion_agent.py` | Not legal advice; research does not enforce an official-source allow-list. |
| 10 | [NL → SQL Agent (Chinook MySQL)](./nl2sql-agent-mysql/README.md) | Natural-language question to executable SQL | LangChain SQL agent, MySQL, Gradio | `nl2sql_agent_mysql.py` | Read-only access is not enforced by code; hard-coded demo credentials must be replaced. |
| 11 | [Retrieval Methods Benchmark](./retrieval-methods-benchmark/README.md) | Compare vector, multi-query, self-query, and parent-doc retrieval | BGE embeddings, Pinecone, OpenAI, Gradio | `food_rag_methodologies.py` | Deletes/recreates `food-rag-strict`; not a corpus-wide statistical evaluation. |
| 12 | [YouTube RAG Analyst](./youtube_rag_analyst/README.md) | Transcript ingestion, retrieval-based Q&A, summarization | yt-dlp, SentenceTransformers, FAISS, LangChain, Gradio | `yt_rag.py` | Summary uses only the first 15,000 transcript characters; user session state is global. |

## Engineering themes

### Agent orchestration

The repository includes sequential agent pipelines, graph-based conditional routing, specialist-as-tool handoff, and parallel fan-out/fan-in. Projects use LangGraph, CrewAI, or BeeAI depending on the pattern being demonstrated.

### Retrieval and evidence handling

Examples include dense vector search, BM25 plus vector retrieval, transcript chunk retrieval, metadata-constrained search, parent-document reconstruction, and database querying. Retrieval grounds outputs in available records, but it does not by itself establish that retrieved records are correct, current, complete, or relevant.

### Structured outputs and validation

Several projects use Pydantic models for task decomposition or output parsing. These schemas validate shape and selected invariants (for example, allocation sums in the BeeAI project); they do not validate the underlying domain recommendation unless explicit checks implement that rule.

### Artifact generation and observability

Project folders include available screenshots, example logs, PDFs, DOCX outputs, or generated-executable workflow evidence. Treat recorded artifacts as examples of individual runs, not proof of universal success.

## Repository structure

```text
genai-retrieval-agentic-engineering/
├── README.md
├── LICENSE
├── Deterministic_Multimodal_Food_Intelligence_System/
├── agentic-radiologist/
├── agentic_software_development/
├── beeai-investment-analyst/
├── crewai-local-grocery-planner/
├── docchat-hybrid-policy-assistant/
├── langgraph-orchestrator-worker-system/
├── langgraph_reflexion_investment_agent/
├── legal-compliance-reflexion-agent/
├── nl2sql-agent-mysql/
├── retrieval-methods-benchmark/
└── youtube_rag_analyst/
```

## Getting started

There is no single repository-wide install command because each project has different dependencies and external-service requirements. Start with the selected folder's README and confirm its execution environment before running it.

Several `.py` files are exported from Google Colab notebooks and contain notebook magic (such as `!pip`) or `google.colab` imports; those files must be run as notebook cells or adapted before normal local execution. `youtube_rag_analyst/yt_rag.py` is structured as a normal Python script.

General workflow:

1. Choose a project from the index above.
2. Read its configuration, data prerequisites, and limitations.
3. Put credentials in an environment variable or notebook secret store, never in committed source.
4. Use an isolated development environment and test data.
5. Inspect generated SQL, reports, code, dependencies, and executables before relying on or executing them.

## Cross-project security and quality notes

- **Credentials:** Several examples depend on OpenAI, Pinecone, or Tavily/Serper. Keep API keys out of source code and logs.
- **External UIs:** Some demos use `share=True`, which creates a public share link. Do not submit confidential contracts, policies, health information, credentials, or proprietary prompts to an exposed demo.
- **SQL:** Do not connect a model-generated SQL workflow to a privileged or production database without enforced least-privilege permissions and query controls.
- **Generated code and executables:** Review source and dependencies; syntax validation alone is not a functional or security test.
- **High-stakes use:** Radiology, legal compliance, and investment outputs require qualified human review and must not be treated as professional determinations.
- **Reproducibility:** LLM calls, web search, remote APIs, external indices, model versions, and provider latency can make results vary, even where temperature is set to zero.

## Author

**Shubham Kumar Singh**

- GitHub: [@shubham-61291](https://github.com/shubham-61291)
- Repository: [genai-retrieval-agentic-engineering](https://github.com/shubham-61291/genai-retrieval-agentic-engineering)

## License

This project is licensed under the **MIT License**. The complete license text is reproduced below and is also available in the repository's [`LICENSE`](./LICENSE) file.

```text
MIT License

Copyright (c) 2026 Shubham Kumar Singh

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
