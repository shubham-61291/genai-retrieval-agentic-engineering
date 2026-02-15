# 🛒 CrewAI Local Grocery Planner (India)
**A Multi-Agent Grocery Planning Workbench with Local Price Search (BigBasket) + Structured Outputs**

---

## 📖 Overview

The **CrewAI Local Grocery Planner** is an applied AI system that generates a **structured grocery shopping plan** for any Indian city using a **true multi-agent workflow** built on **CrewAI**.

This system is designed as a **multi-step grocery planning workbench**, where multiple agents collaborate to:

- search for an authentic recipe
- extract ingredients
- convert ingredients into a categorized shopping list
- search local prices (restricted to BigBasket)
- estimate total cost under a user budget
- suggest leftover usage (waste reduction)
- compile everything into a clean, readable final report

This project is **not** a grocery chatbot and **not** a price guarantee engine.

Every response is:
- grounded in tool-based web search (Serper)
- structured using Pydantic output schemas
- generated through sequential agent execution
- reproducible across cities, meals, and budgets

---

## 🧠 What This System Does

The system enables a user to:

1. Select **State + City** (India)
2. Enter a meal name (e.g., Pav Bhaji, Paneer Butter Masala)
3. Choose servings
4. Enter a budget limit
5. Add dietary restrictions (optional)
6. Generate a complete grocery plan including:
   - recipe ingredients
   - cooking instructions
   - categorized shopping list
   - estimated prices (BigBasket)
   - leftover tips + bonus snack recipe

---

## 🛡️ Design Principles

### ✅ Multi-Agent by Construction (CrewAI)
This system uses **5 specialized agents**:

- **Head Chef**
  - finds authentic recipes via search
- **Store Manager**
  - converts ingredients into a structured shopping list
- **Budget Specialist**
  - searches item prices in the selected city
  - estimates total cost under budget
- **Zero-Waste Chef**
  - identifies likely leftovers
  - suggests a bonus snack recipe
- **Writer**
  - compiles everything into a final Markdown report

This is a real agentic workflow, not a single LLM call.

---

### ✅ Deterministic Constraints (No Overclaim)
- Model locked: `gpt-4.1-mini`
- Local price search is constrained to:
  - `site:bigbasket.com`
- Outputs for key steps are validated using **Pydantic schemas**
- Execution follows a strict sequential pipeline (CrewAI Process.sequential)

---

### ✅ Structured Output Contracts (Pydantic)
The system enforces structured outputs for:

- **MealPlan**
  - `meal_name`
  - `ingredients[]`

- **GroceryShoppingPlan**
  - `items[]` with:
    - name
    - quantity
    - category
    - estimated_price (optional)

This ensures the system produces predictable intermediate outputs for downstream agents.

---

## 🚀 Key Capabilities (No Overclaim)

- Recipe discovery using Serper web search  
- Ingredient extraction into a validated schema  
- Store-section categorized grocery list  
- Local pricing using BigBasket-only search constraints  
- Budget check with cheaper alternative suggestions  
- Leftover detection + bonus snack idea  
- Clean final report compilation  
- Working Gradio UI  

---

## 🧠 System Architecture

```text
User Inputs (State, City, Meal, Servings, Budget, Diet)
↓
Gradio UI
↓
CrewAI Sequential Pipeline
↓
(1) Head Chef Agent  ── Serper Tool Search ──> Recipe + Ingredients (Pydantic)
↓
(2) Store Manager Agent ──> Categorized Shopping List (Pydantic)
↓
(3) Budget Specialist Agent ── Serper Tool Search (BigBasket-only) ──> Price Estimation
↓
(4) Zero-Waste Chef Agent ──> Leftover Tips + Bonus Snack
↓
(5) Writer Agent ──> Final Markdown Report
↓
Final Output (UI)
```
🛠 Tech Stack
Language: Python

Agent Framework: CrewAI

Tooling: SerperDevTool (Google Search)

Model: GPT-4.1-mini

Validation: Pydantic

UI: Gradio

Execution Environment: Google Colab (portable to local)

📂 Project Structure
```
crewai-local-grocery-planner/
│── crew_ai_local_grocery_planner.py
│── interface.png
│── test_1_agent_log.docx
│── test_1_output.pdf
│── test_2_agent_log.docx
│── test_2_output.pdf
│── README.md
```
🖥️ UI Preview
This screenshot shows the live Gradio UI for the system.
![India Local Grocery Planner UI](interface.png)


## 🧪 Step-by-Step System Demonstration (Real Runs)

1️⃣ Test Run #1 — Full Agent Execution (Logs)
This document contains the full CrewAI agent execution logs for Test 1.

📄 Agent Log: test_1_agent_log.docx

📄 Final Output PDF: test_1_output.pdf

2️⃣ Test Run #2 — Full Agent Execution (Logs)
This document contains the full CrewAI agent execution logs for Test 2.

📄 Agent Log: test_2_agent_log.docx

📄 Final Output PDF: test_2_output.pdf

## ⚠️ Known Issues (GPT-4.1-mini Limitations)

This system intentionally uses GPT-4.1-mini to keep inference cost low and execution fast.

However, GPT-4.1-mini introduces predictable limitations in tool-heavy workflows.

1) Imperfect Price Matching from Search Snippets
Even with strict domain filtering (site:bigbasket.com), the model may:

pick the wrong listing when multiple variants exist

extract a price from an unrelated snippet

infer missing values when the snippet is incomplete

As a result, the pricing table should be treated as best-effort approximation, not ground truth.

2) Inconsistent Quantity Normalization
GPT-4.1-mini may sometimes:

mix units (grams vs pieces)

assume weights for vegetables (e.g., “1 medium onion ≈ 300g”)

generalize bundle quantities

This is expected because the system does not currently include a deterministic unit-conversion tool.

3) Category Label Drift
The organizer agent may occasionally classify items inconsistently:

butter under “Dairy” (correct)

pav under “Bakery” (correct)

masala under “Spices” (correct)

but sometimes salt may appear under “Staples” depending on phrasing

This is a minor labeling issue and does not break execution.

4) Budget Enforcement is Advisory
The budget constraint is enforced through reasoning, not a hard numeric validator.
The system attempts to stay under the user budget, but it does not guarantee strict compliance.

## ⚠️ Limitations & Disclosure (Important)

BigBasket pricing is approximate and depends on:

listing availability

snippet quality

local delivery catalog differences

This system does not guarantee exact prices

This system does not place orders

The budget check is advisory (best-effort)

Recipe authenticity depends on search results returned

All screenshots and PDFs in this repository are generated from real executions.

## 🔮 Future Improvements
This project is intentionally kept minimal and focused on agentic workflow correctness.
The following upgrades are planned to improve reliability and production readiness.

1) Deterministic Price Extraction (High Priority)
Add a price parser that extracts:

numeric ₹ values

unit sizes (250g / 1kg)

pack vs loose

Reject any snippet that does not contain a valid price pattern

2) Hard Budget Validator (Pydantic + Numeric)
Convert budget input into numeric form

Enforce:

total_estimated_cost <= budget

If exceeded:

auto-trigger a swap loop

3) Unit Conversion Tool
Introduce a deterministic helper for:

grams ↔ kg conversion

typical weights for common Indian produce

“pieces to grams” approximations

4) Structured Output for Pricing Task
Currently Task 3 returns free-form text.
A future version will enforce:

per-item price schema

total price schema

confidence score per item

5) Multi-Website Pricing Support
Add an option to choose pricing source:

BigBasket

JioMart

Blinkit

Zepto

This will reduce “catalog missing” cases for certain cities.

6) Export as PDF (Optional)
Add a downloadable PDF report generator for the final output.

▶️ Usage
1) Install dependencies
pip install crewai crewai-tools gradio pydantic
2) Set environment variables
export OPENAI_API_KEY="YOUR_KEY"
export SERPER_API_KEY="YOUR_KEY"
3) Run
python crew_ai_local_grocery_planner.py

👨‍💻 Author

Shubham Singh

📜 License

MIT License


