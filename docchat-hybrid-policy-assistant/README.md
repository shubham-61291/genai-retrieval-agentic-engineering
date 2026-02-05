# 📄 DOCCHAT: HYBRID POLICY ASSISTANT  
**A Docling-Powered, Hybrid Retrieval, LangGraph-Orchestrated Document QA System**

---

## 📖 Overview

**DOCCHAT: Hybrid Policy Assistant** is an applied AI system that performs **document-grounded policy question answering** using a **hybrid retrieval pipeline** (BM25 + Vector Search) and an **agentic LangGraph workflow**.

This system is **not** a chatbot demo, **not** a generic LLM wrapper, and **not** a hallucination-prone Q&A toy.

Every response is:
- grounded in **only the uploaded document**
- retrieved using **hybrid search (BM25 + embeddings)**
- generated using **GPT-4.1-mini**
- verified through a second **verification agent**
- demonstrated using **real Gradio UI outputs**

---

## 🧠 What This System Does

The system enables a user to:

1. Upload a policy document (Word / PDF)
2. Convert it into structured Markdown using **Docling**
3. Split the content into retrievable chunks using Markdown headers
4. Retrieve the best context using **Hybrid Search**
5. Run an **Agent Workflow**:
   - Gatekeeper relevance check
   - Draft answer generation
   - Verification report generation
6. Display results in a clean **Gradio UI**

All execution paths are demonstrated below using **real UI outputs**.

---

## 🛡️ Design Principles

### ✅ Document-Grounded by Construction
- The assistant only answers using retrieved context  
- No “world knowledge” policy guessing  
- No fabricated leave rules, timelines, or values  

---

### ✅ Hybrid Retrieval (Not Just Embeddings)

This system uses:
- **BM25 Retriever** → keyword precision (policy tables, headings, terms)
- **Chroma Vector Retriever** → semantic matching (natural language phrasing)

Hybrid retrieval prevents failures like:
- missing “Casual Leave” because the user asked “CL”
- missing “core hours” because the user asked “availability time”
- missing “POSH inquiry timeline” due to phrasing mismatch

---

### ✅ Agentic Verification Layer
This system does **not** stop at answer generation.

It always produces:
- a draft answer
- a verification report confirming:
  - Supported: Yes/No
  - Corrections: list
  - Missing Info: list

---

## 🚀 Key Capabilities

- Upload-based policy Q&A
- Word / PDF parsing using Docling
- Markdown-aware chunking
- Hybrid retrieval (BM25 + Vector Search)
- LangGraph orchestration
- Dual agent workflow:
  - Answer Agent
  - Verification Agent
- UI output suitable for enterprise policy support

---

## 🧠 System Architecture
```
Policy Document (DOCX/PDF)
↓
Docling (DocumentConverter)
↓
Markdown Export
↓
MarkdownHeaderTextSplitter
↓
Chunks (Document objects)
↓
Hybrid Retrieval Layer
├── BM25 Retriever
└── Chroma Vector Retriever
↓
Ensemble Merge (Deduplicate)
↓
LangGraph Workflow

Relevance Gatekeeper

Draft Answer Agent (GPT-4.1-mini)

Verification Agent (GPT-4.1-mini)
↓
Gradio UI Output
├── Draft Answer
└── Verification Report
```

---

## 🧬 Retrieval Details (Executable, Not Theoretical)

- **BM25 Retriever:** `rank_bm25`
- **Vector Store:** Chroma
- **Embedding Model:** OpenAIEmbeddings
- **Retrieval Strategy:** Hybrid merge + deduplicate
- **Default Retrieval Size:** k = 10 (BM25) + k = 10 (Vector)

---

## 🛠 Tech Stack

- **Language:** Python  
- **UI:** Gradio  
- **LLM:** GPT-4.1-mini  
- **Document Parsing:** Docling  
- **Chunking:** MarkdownHeaderTextSplitter  
- **Retrieval:** BM25 + Chroma  
- **Orchestration:** LangGraph  
- **Output:** Draft Answer + Verification Report  

---

## 📂 Project Structure
```
docchat-hybrid-policy-assistant/
│── doc_chat.py
│── README.md
│── Doc_chat_test_1.png
│── Doc_chat_test_2.png
│── Doc_chat_test_3.png
│── Doc_chat_test_4.png
│── Doc_chat_test_5.png
│── Doc_chat_test_6.png
```

---

## 🧪 Step-by-Step System Demonstration (Real Outputs)

### 1️⃣ Working Hours + Core Hours Query
![DocChat Test 1](./Doc_chat_test_1.png)

---

### 2️⃣ Leave Structure Table Query (CL)
![DocChat Test 2](./Doc_chat_test_2.png)

---

### 3️⃣ POSH Redressal Mechanism Timelines (Multi-Part Question)
![DocChat Test 3](./Doc_chat_test_3.png)

---

### 4️⃣ Earned Leave Carry Forward Logic (Numerical Policy Constraint)
![DocChat Test 4](./Doc_chat_test_4.png)

---

### 5️⃣ Gifts & Entertainment Policy Scenario (Ethics + Compliance)
![DocChat Test 5](./Doc_chat_test_5.png)

---

### 6️⃣ Out-of-Scope Detection (Correct “I Don’t Know” Behavior)
![DocChat Test 6](./Doc_chat_test_6.png)

---

## 🧩 Issues & Challenges We Faced (And How We Solved Them)

This project was not “plug and play”.  
We hit multiple real-world engineering issues and solved them systematically.

---

### 1) Dependency Hell (Imports)

**Issue:**  
Repeated `ImportError` and `ModuleNotFoundError` for retrievers, LangGraph, and hybrid search imports.

**Cause:**  
LangChain ecosystem fragmentation:
- `langchain`
- `langchain-community`
- `langchain-core`

Old tutorials reference outdated import paths.

**Fix:**  
We performed a clean install and pinned stable versions:

- `langchain==0.2.1`
- `langchain-community==0.2.1`

---

### 2) LangGraph Crash: `KeyError: '__start__'`

**Issue:**  
System crashed immediately on chat invoke with:

KeyError: 'start'


**Cause:**  
LangGraph `app.invoke()` requires a `thread_id` to track versions and state history under Pregel execution.

**Fix:**  
We added a fixed session configuration:

```python
config = {"configurable": {"thread_id": "sony_policy_session"}}
```
3) Gatekeeper Too Lenient (Always Saying YES)
Issue:
The relevance checker was approving almost everything and never filtering.

Cause:
The gatekeeper prompt was keyword-based, not policy-rule-based.

Fix:
We rewrote the gatekeeper prompt as a strict compliance filter:

Answer YES only if a direct policy rule exists

Answer NO if the context lacks numeric values, eligibility, or direct statements

4) Hybrid Retrieval Output Merging
Issue:
BM25 and Vector retrievers returned overlapping chunks, leading to repeated context.

Cause:
Both retrievers were retrieving similar policy sections.

Fix:
We merged and deduplicated the results before sending to the LLM.

⚠️ Limitations & Disclosure
No multi-document citation system (yet)

No section-wise citations included in answers (yet)

Hybrid retrieval results are merged but not re-ranked using a learned reranker

Very large documents may require chunk tuning for best performance

All screenshots represent actual system executions.

▶️ Usage
1) Install
pip install -U docling langchain langchain-community langchain-openai langgraph chromadb gradio rank_bm25
2) Run
python doc_chat.py
3) Upload document in UI
Upload a DOCX/PDF policy file when prompted.

👨‍💻 Author

Shubham Singh

📜 License

MIT License
