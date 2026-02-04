# 📺 YOUTUBE RAG ANALYST  
**A Fail-Safe, Transcript-Grounded YouTube Video Intelligence System**

---

## 📖 Overview

The **YouTube RAG Analyst** is an applied AI system that performs **deep video understanding and analysis** using a **retrieval-augmented generation (RAG) pipeline grounded entirely in video transcripts**.

This system is **not** a YouTube scraper demo, **not** a shallow summarizer, and **not** a browsing-based chatbot.

It is designed to remain **fully functional under hostile ingestion conditions**, including:
- YouTube IP bans
- Rate limiting (HTTP 429)
- Subtitle API failures

Every analytical response is:
- grounded **only** in transcript content
- retrieved via **semantic embeddings**
- generated through **context-bounded LLM reasoning**
- reproducible across ingestion paths

---

## 🧠 What This System Does

The system enables a user to:

1. Ingest a YouTube video transcript automatically via URL  
2. Fall back to manual transcript ingestion if auto-ingest fails  
3. Store transcript chunks in a vector index  
4. Ask arbitrary analytical questions about the video  
5. Generate structured summaries grounded in retrieved context  

Both ingestion paths feed into **the same RAG core**, ensuring identical analytical behavior.

---

## 🛡️ Design Principles

### ✅ Fail-Safe by Construction
- Dual ingestion paths (Auto + Manual)
- Explicit detection of IP bans
- Manual bypass guarantees uptime

### ✅ Transcript-Faithful
- No browsing
- No external enrichment
- No hallucinated facts

### ✅ Retrieval-First Reasoning
- Context retrieved before generation
- LLM constrained to retrieved chunks only

---

## 🚀 Key Capabilities

- Automatic YouTube subtitle ingestion (yt-dlp based)
- Manual transcript ingestion (ban-proof)
- Semantic chunking and vector indexing
- Question answering over long videos
- Full-video summarization
- Deterministic behavior across ingestion modes

---

## 🧠 System Architecture
```
YouTube Video
↓
Transcript (Auto via yt-dlp OR Manual Paste)
↓
Recursive Text Chunking
↓
SentenceTransformer Embeddings (384-dim)
↓
FAISS Vector Store (local)
↓
Top-K Semantic Retrieval
↓
LLM (Context-Bound Answer / Summary)
```

---

## 🧬 Embeddings (Executable, Not Theoretical)

- **Model:** sentence-transformers/all-MiniLM-L6-v2  
- **Embedding Dimension:** 384  
- **Vector Store:** FAISS (local, in-memory)  
- **Retrieval Strategy:** Semantic similarity (top-k = 4)  

The embedding space is **fixed, shared, and deterministic**.

---

## 🛠 Tech Stack

- **Language:** Python  
- **UI:** Gradio  
- **Transcript Engine:** yt-dlp + requests  
- **Embeddings:** SentenceTransformers  
- **Vector DB:** FAISS  
- **LLM:** GPT-4.1-mini  
- **Framework:** LangChain  

---

## 📂 Project Structure
```
youtube_rag_analyst/
│── README.md
│── yt_rag.py
│── auto_ingestion_question_answer.png
│── autoingested_summary.png
│── autoingestedvia_Youtube_link.png
│── manual_ingestion.png
│── manual_ingestion_extra_question.png
│── manual_ingestion_same_output.png
│── LICENSE

```
---

## 🧪 Step-by-Step System Demonstration

### 1️⃣ Auto Ingestion via YouTube URL

![Auto Ingest via URL](autoingestedvia_Youtube_link.png)

---

### 2️⃣ Question Answering (Auto-Ingested Transcript)

![Auto Ingestion Q&A](auto_ingestion_question_answer.png)

---

### 3️⃣ Full Video Summarization (Auto-Ingested)

![Auto Ingested Summary](autoingested_summary.png)

---

### 4️⃣ Manual Transcript Ingestion (Fail-Safe Mode)

![Manual Ingestion](manual_ingestion.png)

---

### 5️⃣ Manual Ingestion → Additional Question

![Manual Extra Question](manual_ingestion_extra_question.png)

---

### 6️⃣ Proof of Determinism (Same Output)

Auto ingestion and manual ingestion produce **semantically equivalent answers**.

![Same Output Proof](manual_ingestion_same_output.png)

---

## ⚠️ Engineering Challenges & How They Were Solved

### ❌ Challenge 1: Google Colab Transcript Failures
- **Issue:** TranscriptUnavailable errors
- **Cause:** Cloud IPs flagged by YouTube
- **Fix:** Migrated execution to local machine

---

### ❌ Challenge 2: YouTube Rate Limiting (429 Errors)
- **Issue:** IP temporarily soft-banned
- **Cause:** Repeated automated requests
- **Fix:** Replaced fragile APIs with `yt-dlp`

---

### ❌ Challenge 3: Inevitable Platform Breakage
- **Reality:** APIs and scraping methods will fail over time
- **Solution:** Introduced **Manual Ingestion Path**
- **Outcome:** Analytical core remains 100% functional

This converts a brittle system into a **resilient intelligence product**.

---

## ⚠️ Limitations & Disclosure

- No browsing or live data
- No hallucinated answers
- Knowledge limited strictly to transcript
- Requires subtitles or manual transcript
- Not a replacement for full video playback

All screenshots represent **real system executions**.

---

## ▶️ Usage

```bash
pip install yt-dlp requests gradio langchain-community langchain-openai langchain-text-splitters faiss-cpu sentence-transformers
python yt_rag.py
```
👨‍💻 Author

Shubham Singh

📜 License

MIT License
