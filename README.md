🧠 GENAI RETRIEVAL AGENTIC ENGINEERING

A Portfolio-Grade Repository of Deterministic, Evidence-Grounded, Multi-Agent Systems (LangGraph / CrewAI / BeeAI / RAG / Tooling)

📖 Overview

This repository is a curated collection of applied AI systems built with a consistent philosophy:

Agentic workflows (not single-prompt demos)

Deterministic constraints wherever possible

Tool-grounded execution

Evidence-first outputs

Auditable intermediate artifacts

Real UI proof + real run outputs

These projects are designed to be:

✅ portfolio-grade
✅ engineering-first
✅ reproducible
✅ GitHub-presentable
✅ honest (no overclaim)

This repository is not a “random demo dump”.
Each folder is a complete, standalone system with:

its own README

its own screenshots / artifacts

its own execution proof

🛡️ Core Design Principles (Repo-Wide)
✅ 1) Deterministic by Construction

Across the repo, systems enforce:

temperature = 0

strict schema outputs (Pydantic)

tool-only execution (no silent file writes)

reproducible pipelines

✅ 2) Evidence-Grounded (RAG / DB / Web)

Every system grounds its outputs using one of:

Static JSON

Vector RAG

Hybrid Retrieval

SQL execution

Web verification tools

✅ 3) No Overclaim

These projects do NOT pretend to be:

medical devices

legal counsel

financial advisors

“AGI systems”

They are engineered demonstrations of:

agent orchestration + deterministic tooling + grounded outputs

✅ 4) Portfolio-Grade UI + Artifacts

Most projects include:

Gradio UI

real screenshots

downloadable artifacts:

PDFs

DOCX reports

logs

JSON outputs

Windows EXE builds

🧠 What This Repo Demonstrates (High-Signal Skills)

This repo signals real capability in:

Agent orchestration (LangGraph / CrewAI / BeeAI)

Deterministic tool design

RAG pipelines (Pinecone / Chroma / hybrid)

Structured outputs (Pydantic)

UI engineering (Gradio workbenches)

Artifact generation (PDF, logs, JSON)

Real debugging + production-style constraints

Model limitation disclosure (GPT-4.1-mini failure modes)

📂 Repository Structure
```
GENAI RETRIEVAL AGENTIC ENGINEERING/
│
│── Deterministic_Multimodal_Food_Intelligence_System/
│── agentic-radiologist/
│── agentic_software_development/
│── beeai-investment-analyst/
│── crewai-local-grocery-planner/
│── docchat-hybrid-policy-assistant/
│── langgraph-orchestrator-worker-system/
│── langgraph_reflexion_investment_agent/
│── legal-compliance-reflexion-agent/
│── nl2sql-agent-mysql/
│── retrieval-methods-benchmark/
│── youtube_rag_analyst/
│
│── LICENSE
│── README.md

```
🚀 Projects Index (Full Repo)

Below are all systems included in this repository.

Each project folder contains its own:

detailed README

screenshots

artifacts

runnable Python code

🍽️ 1) Deterministic Multimodal Food Intelligence System

A Deterministic, JSON-Grounded, Image-First Food RAG System

📁 Folder: Deterministic_Multimodal_Food_Intelligence_System/

What it does

Upload food image

Identify food via vision model

Embed food name

Retrieve nutrition from static JSON via Pinecone

Enforce top-1 deterministic retrieval

Key Signals

deterministic RAG (top_k=1)

no external nutrition APIs

JSON-faithful outputs only

🩻 2) X-Ray Agentic Radiologist (LangGraph Orchestrated)

A Multi-Agent Radiology Report Generator with RAG + Web Verification + Senior Reviewer Guardrails

📁 Folder: agentic-radiologist/

What it does

Upload a chest X-ray image

Extract findings (Vision Analyst)

Retrieve internal radiology knowledge (Pinecone)

Retrieve external evidence (Tavily)

Draft report (Radiologist Agent)

Verify + remove hallucinations (Senior Reviewer)

Key Signals

true LangGraph orchestration

internal + external grounding

hallucination-resistant design

model comparison proof (gpt-4.1-mini vs gpt-5-mini)

🪟 3) Agentic Windows EXE Builder (GitHub Actions)

A Deterministic ReAct Coding Agent that Generates Python Apps and Ships a Real Windows .exe

📁 Folder: agentic_software_development/

What it does

user gives app requirement

agent writes:

src/main.py

requirements.txt

sanity check (py_compile)

patch loop if needed

push to GitHub branch

trigger GitHub Actions Windows runner

download artifact and extract app.exe

Key Signals

real Windows build pipeline

GitHub Actions integration

deterministic tool workflow

UI-based executable delivery

🐝 4) BeeAI Investment Analyst

A Deterministic, Multi-Agent Investment Planning Workbench with Strict JSON Validation

📁 Folder: beeai-investment-analyst/

What it does

investor profile input

multi-agent handoff:

Coordinator Agent

Risk Specialist Agent

deterministic SIP calculator tool

strict JSON output validation (Pydantic)

export final plan as JSON

Key Signals

BeeAI ReAct agents

agent-as-a-tool handoff pattern

strict JSON contract enforcement

🛒 5) CrewAI Local Grocery Planner (India)

A Multi-Agent Grocery Planning Workbench with Local Price Search (BigBasket) + Structured Outputs

📁 Folder: crewai-local-grocery-planner/

What it does

state + city input

meal selection + servings + budget

multi-agent workflow:

Head Chef

Store Manager

Budget Specialist

Zero-Waste Chef

Writer

BigBasket-only price search

structured shopping list outputs

PDF outputs included

Key Signals

true multi-agent workflow in CrewAI

structured output enforcement

explicit GPT-4.1-mini limitation disclosure

📄 6) DOCCHAT: Hybrid Policy Assistant

A Docling-Powered, Hybrid Retrieval, LangGraph-Orchestrated Document QA System

📁 Folder: docchat-hybrid-policy-assistant/

What it does

upload policy DOCX/PDF

parse via Docling → Markdown

chunk using Markdown headers

hybrid retrieval:

BM25

Vector search

LangGraph workflow:

relevance gatekeeper

answer agent

verification agent

UI shows draft + verification report

Key Signals

hybrid retrieval (BM25 + vector)

document-grounded answers only

verification layer prevents hallucinations

🧑‍🍳 7) LangGraph Orchestrator–Worker System

A Deterministic Multi-Agent Orchestration Pattern with Worker Fan-Out, Live Logs, and PDF Artifact Generation

📁 Folder: langgraph-orchestrator-worker-system/

What it does

orchestrator splits multi-item request into tasks

worker agents fan-out via Send()

synthesizer merges outputs

generates:

live logs

downloadable logs

downloadable PDF

Key Signals

canonical orchestrator-worker pattern

fan-out routing

artifact pipeline (Markdown → HTML → PDF)

🏦 8) LangGraph Reflexion Investment Agent

A Multi-Persona Reflexion Loop for Risk-Calibrated Investment Strategy Generation

📁 Folder: langgraph_reflexion_investment_agent/

What it does

generate investment plan

evaluator grades it (Pydantic structured output)

refine using critique

retry loop until target grade achieved

export PDF + logs

Key Signals

reflexion loop design

multi-persona simulation

strict evaluator schema enforcement

⚖️ 9) Legal Compliance Reflexion Agent

A Compliance-Grade Contract Auditor for GDPR & EU AI Act (Reflexion + Tool Evidence + PDF Report)

📁 Folder: legal-compliance-reflexion-agent/

What it does

upload MSA / DPA

parse PDF/DOCX

junior auditor extracts risks

reflection generates evidence queries

Tavily retrieves supporting evidence

senior partner rewrites final audit

exports PDF report

Key Signals

compliance-first output structure

reflexion pipeline reduces hallucinations

explicit “official sources roadmap” included

🗄️ 10) NL → SQL Agent (Chinook DB)

An Agentic Natural Language to SQL System over a Relational Database

📁 Folder: nl2sql-agent-mysql/

What it does

user asks natural language question

agent inspects schema

generates executable SQL

runs query on live MySQL DB

returns grounded results

Key Signals

no RAG (pure SQL grounding)

tool-based schema inspection

reproducible, verifiable answers

🧪 11) Retrieval Methods Benchmark

A Deterministic Evaluation of Retrieval Strategies in Agentic RAG Systems

📁 Folder: retrieval-methods-benchmark/

What it does

Compares retrieval strategies under identical constraints:

Vector similarity retrieval

Multi-query expansion retrieval

Self-query (metadata constrained) retrieval

Parent document reconstruction retrieval

Key Signals

research-grade comparison framing

method isolation

deterministic benchmarking mindset

▶️ 12) YouTube RAG Analyst

A RAG-Based YouTube Transcript Intelligence System (Grounded Video QA)

📁 Folder: youtube_rag_analyst/

What it does

ingest YouTube transcript

chunk + embed

retrieve relevant context

answer grounded questions

Key Signals

practical RAG application

scalable transcript intelligence pattern

▶️ How To Run Any Project (Repo Standard)

Each project is self-contained.

1) Go into a folder
cd <project-folder>

2) Install dependencies

Each project README contains its exact install command.

Example pattern:

pip install -U <dependencies>

3) Run
python <main_file>.py

⚠️ Repo-Wide Limitations & Disclosure

This repository contains portfolio engineering systems, not production medical/legal/financial products.

Important disclaimers

Radiology output is not for diagnosis

Legal audits are not legal advice

Investment plans are not financial advice

Web search outputs may contain imperfect sources

GPT-4.1-mini is cost-efficient but can hallucinate in tool-heavy workflows

🧩 Known Issues (GPT-4.1-mini in Tool Workflows)

Across multiple systems, GPT-4.1-mini can show:

imperfect extraction from snippets

tool mismatch selection

minor schema drift

occasional overconfidence in high-stakes domains

This repo explicitly documents these failure modes and includes mitigation patterns (reviewers, verification nodes, reflexion loops).

🔮 Future Updates (Repo Roadmap)

This repo is actively evolving.

Planned upgrades:

Add unit tests for agent graphs

Add deterministic validators (math, budgets, totals)

Add official legal database grounding (EUR-Lex, gdpr-info.eu)

Add structured citation formatting across all RAG systems

Add “model switch” support (OpenAI ↔ open-source local models)

Add a shared reusable agent toolkit folder (/core_tools)

👨‍💻 Author

Shubham Singh

📜 License

MIT License
