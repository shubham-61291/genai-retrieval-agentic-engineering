# 🩻 X-Ray Agentic Radiologist (LangGraph Orchestrated)  
**A Multi-Agent, Evidence-Grounded Radiology Report Generator with RAG + Web Verification + Senior Reviewer Guardrails**

---

## 📖 Overview

The **X-Ray Agentic Radiologist** is an applied AI system that generates a **structured chest X-ray radiology report** from a **single frontal X-ray image**, using a **true agentic workflow** orchestrated with **LangGraph**.

This system is **not** a chatbot, **not** a single-pass vision demo, and **not** a “one prompt = one report” pipeline.

Instead, it is designed as a **multi-stage clinical reasoning workbench**, where the system:

- extracts findings via a **Vision Analyst**
- builds a retrieval query from extracted findings
- retrieves internal radiology knowledge via **Pinecone RAG**
- retrieves external evidence via **Tavily Web Search**
- generates a structured report via a **Radiologist Agent**
- validates and removes hallucinations via a **Senior Radiologist Reviewer**

---

## 🧠 What This System Does

The system enables a user to:

1. Upload a chest X-ray image (`.png / .jpg`)
2. (Optionally) add clinical history
3. Run a LangGraph multi-agent workflow
4. View the full execution trace
5. Expand all intermediate evidence:
   - Vision JSON
   - Pinecone RAG hits
   - Web evidence
   - Draft report
6. Receive a final radiology report in standard format

---

## 🛡️ Design Principles

### ✅ Agentic by Construction (Not Marketing)
This is a real multi-agent pipeline, implemented with LangGraph:

- each stage is a node
- each node produces explicit artifacts
- every output is auditable

---

### ✅ Evidence-Grounded (RAG + Web)
The system uses **two grounding channels**:

- **Internal Evidence (Pinecone RAG)**
- **External Evidence (Tavily Web Search)**

Both are shown in the UI for auditability.

---

### ✅ Hallucination-Resistant by Design
A dedicated **Senior Radiologist Reviewer** agent performs:

- statement verification
- hallucination removal
- safety enforcement
- format enforcement

This is not optional — it is part of the graph.

---

### ✅ Transparent UI (No Hidden Work)
Unlike most demos, this UI exposes:

- full agent logs  
- intermediate JSON  
- retrieved evidence  
- draft vs final report  

This makes the system **portfolio-grade**.

---

## 🧠 System Architecture
```
X-ray Image Upload
↓
Vision Analyst (Structured JSON Findings)
↓
Query Builder (Findings → Retrieval Query)
↓
Pinecone RAG Retriever (Embedding Search)
↓
Tavily Web Search (External Evidence)
↓
Radiologist Agent (Draft Report)
↓
Senior Radiologist Reviewer (Hallucination Removal + Final Report)
↓
Final Radiology Report (UI + Export)
```

---

## 🧬 Embeddings (Executable, Not Theoretical)

- **Vector DB:** Pinecone  
- **Index Name:** `xray-rag-1024`
- **Embedding Model:** `BAAI/bge-large-en-v1.5`
- **Embedding Dimension:** `1024` (validated at runtime)
- **Normalization:** Enabled
- **Retrieval Strategy:** `top_k = 6`

This embedding space is **fixed**, **validated**, and **deterministic**.

---

## 🛠 Tech Stack

- **Language:** Python  
- **UI:** Gradio  
- **Orchestration:** LangGraph  
- **LLM Provider:** OpenAI  
- **Vector DB:** Pinecone  
- **Embeddings:** SentenceTransformers (`bge-large-en-v1.5`)  
- **Web Search:** Tavily  
- **Output:** Radiology Report (structured clinical format)

---

## 📂 Project Structure
```
XRay_Agentic_Radiologist/
│── README.md
│── agentic_radiologist.py
│── data.docx
│── interface.png
│── output_1.docx
│── output_2.docx
│── output_3.docx
│── test_1.png
│── test_2.png
│── test_3.png

```
---

## 🖥️ System UI (Real Execution)

### 1️⃣ UI Home Screen

![UI Home](interface.png)

---

## 🧪 Step-by-Step System Demonstration (3 Real X-rays)

All 3 test images come from the **OpenI (NLM) Indiana University Chest X-rays Collection**.

> Important: The original OpenI report may be derived from a multi-view study, while this system processes only a **single frontal view**.

---

### 1️⃣ Test 1 (OpenI Chest X-ray)

![Test 1 X-ray](test_1.png)

**Original Report (OpenI):**
- Lungs clear
- Heart normal
- Suture material at left apex suggesting prior lung surgery

**Source Link:**  
https://openi.nlm.nih.gov/detailedresult?img=CXR452_IM-2083-1001&query=Indiana%20U.%20Chest%20X-rays,%20Pneumonia&it=xg&uniq=1&req=4&npos=1

---

### 2️⃣ Test 2 (OpenI Chest X-ray)

![Test 2 X-ray](test_2.png)

**Original Report (OpenI):**
- Sternotomy changes noted
- Lungs clear of focal infiltrates
- No pleural effusion
- Impression: No acute disease

**Source Link:**  
https://openi.nlm.nih.gov/detailedresult?img=CXR125_IM-0169-1001&query=Indiana%20U.%20Chest%20X-rays,%20Effusion&it=xg&uniq=1&req=4&npos=2

---

### 3️⃣ Test 3 (OpenI Chest X-ray)

![Test 3 X-ray](test_3.png)

**Original Report (OpenI):**
- Significant thoracolumbar rotatory scoliosis
- Atherosclerotic aorta
- Lungs hypoinflated but clear
- Impression: Hypoinflation without acute disease

**Source Link:**  
https://openi.nlm.nih.gov/detailedresult?img=CXR484_IM-2108-1001&query=Indiana%20U.%20Chest%20X-rays&it=xg&coll=cxr&uniq=1&req=4&npos=7

---

## 🔥 Key Result: Model Choice Dramatically Changes Clinical Accuracy

This project demonstrates a critical real-world truth:

> **High-stakes domains (radiology) require high-capability models. Smaller models can generate confident hallucinations.**

The system was tested with:

- **gpt-4.1-mini**
- **gpt-5-mini**

The outputs were directly compared across all 3 OpenI test images.

---

### ⚠️ Failure Case 1: Hallucinated Hardware (gpt-4.1-mini)

In Test 2, **gpt-4.1-mini** hallucinated:

> “Lines/tubes are present in the midline chest.”

However, the OpenI report only mentions sternotomy changes and no tubes.

---

### ⚠️ Failure Case 2: Overconfident Cardiomegaly (gpt-4.1-mini)

In Test 1, **gpt-4.1-mini** reported:

> “Moderate cardiomegaly (definite confidence).”

But the OpenI report explicitly states:

> “The heart … are normal.”

---

### ✅ GPT-5-mini Output is Clinically Safer and More Realistic

Across the same tests, **gpt-5-mini**:

- avoided hallucinated lines/tubes  
- avoided overconfident cardiomegaly  
- used more realistic radiology language  
- provided calibrated confidence  

This difference is visible directly in:

- `output_1.docx`
- `output_2.docx`
- `output_3.docx`

---

## 📄 Example Outputs (Download)

This system produces final formatted report artifacts.

- 📄 Output 1: `output_1.docx`  
- 📄 Output 2: `output_2.docx`  
- 📄 Output 3: `output_3.docx`  

---

## ⚠️ Limitations & Disclosure

- This system processes only **single frontal view X-rays**
- OpenI reference reports may be derived from **multi-view studies**
- This is a research/portfolio system, not a medical device
- Outputs must not be used for real patient diagnosis

All screenshots and outputs represent **real system executions**.

---

## ▶️ Usage

```bash
pip install -U gradio openai pinecone sentence-transformers requests langgraph langchain langchain-openai pydantic
python agentic_radiologist.py
```
👨‍💻 Author

Shubham Singh

📜 License

MIT License
