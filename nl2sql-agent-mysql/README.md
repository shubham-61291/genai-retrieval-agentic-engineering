# 🗄️ NL → SQL Agent (Chinook DB)
**An Agentic Natural Language to SQL System over a Relational Database**

---

## 📖 Overview

The **NL → SQL Agent (Chinook DB)** is an applied AI system that enables **natural language querying of a relational SQL database** using an **agentic reasoning pipeline**.

This system translates user questions into **executable SQL queries**, runs them against a **live MySQL database**, and returns **grounded, verifiable results** through a clean UI.

This project is **not** a chatbot, **not** a vector RAG system, and **not** a prompt-only SQL generator.

Every response is:
- grounded **only** in a live SQL database
- generated via **schema-aware agent reasoning**
- executed as **real SQL queries**
- fully auditable and repeatable

---

## 🧠 What This System Does

The system enables a user to:

1. Ask a question in natural language (English)
2. Inspect the database schema dynamically
3. Generate a valid SQL query using an LLM-driven agent
4. Execute the SQL query against MySQL
5. Return the exact query result to the user via a UI

All execution paths are demonstrated below using **real backend traces and UI outputs**.

---

## 🛡️ Design Principles

### ✅ Agentic by Construction
- Explicit tool-based reasoning
- Schema inspection before query generation
- SQL execution as a first-class action
- No hardcoded queries

### ✅ Database-Grounded
- All answers come **only** from the SQL database
- No external knowledge sources
- No inferred or hallucinated values
- Query failure → surfaced, not hidden

### ✅ Deterministic Execution
- `temperature = 0`
- SQL results determine final answers
- Identical input → identical output

---

## 🚀 Key Capabilities

- Natural language → SQL translation
- Multi-table joins and aggregations
- Schema-aware reasoning
- Read-only SQL execution
- Transparent backend SQL traces
- Interactive Gradio frontend

---

## 🧠 System Architecture
```
User Question (Text)
↓
Gradio UI
↓
LangChain SQL Agent
↓
Schema Inspection (MySQL)
↓
SQL Query Generation
↓
SQL Execution (Chinook DB)
↓
Final Answer
```

---

## 🛠 Tech Stack

- **Language:** Python
- **LLM:** GPT-4.1-mini
- **Agent Framework:** LangChain
- **Database:** MySQL
- **Dataset:** Chinook (public sample DB)
- **UI:** Gradio
- **Execution Environment:** Google Colab

---

## 📂 Project Structure
```
nl2sql-agent-mysql/
│── nl2sql_agent_mysql.py
│── sql_agent_incode_testing_backend.png
│── sql_agent_test_1.png
│── sql_agent_test_2.png
│── sql_agent_test_3.png
│── sql_agent_test_4.png
│── sql_agent_test_5.png
│── README.md
```

---

## 🧪 Step-by-Step System Demonstration

### 1️⃣ Backend Agent Reasoning & SQL Execution

The agent inspects schema, generates SQL, executes it, and returns grounded results.

![Backend SQL Agent Trace](sql_agent_incode_testing_backend.png)

---

### 2️⃣ UI Test — Album Count Query

**Query:** *How many albums are there in the database?*

![Album Count Test](sql_agent_test_1.png)

---

### 3️⃣ UI Test — Country Spending Analysis

**Query:** *Which country spent the most money?*

![Country Spending Test](sql_agent_test_2.png)

---

### 4️⃣ UI Test — Artist Analytics

**Query:** *Which artist has the most albums?*

![Artist Albums Test](sql_agent_test_3.png)

---

### 5️⃣ UI Test — Track Pricing Analysis

**Query:** *What are the top 5 most expensive tracks?*

![Top Tracks Test](sql_agent_test_4.png)

---

### 6️⃣ UI Test — Customer Analytics

**Query:** *Who is the top customer by total spending?*

![Top Customer Test](sql_agent_test_5.png)

---

## 🧬 Why This Is NOT RAG

This system **does not use embeddings or vector search**.

| Aspect | This System |
|------|-------------|
| Retrieval | SQL execution |
| Grounding | Database rows |
| Reasoning | Agent + tools |
| Hallucination | Impossible without SQL |
| Verification | Re-runnable queries |

---

## ⚠️ Limitations & Disclosure

- Read-only queries only
- No write/update/delete operations
- No multi-database federation
- Knowledge limited strictly to the database
- Query failures are not masked

All screenshots represent **actual system executions**.

---

## ▶️ Usage

```bash
pip install langchain langchain-community langchain-openai mysql-connector-python gradio
python nl2sql_agent_mysql.py
```

👨‍💻 Author

Shubham Singh

📜 License

MIT License
