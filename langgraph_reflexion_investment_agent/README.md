# LangGraph Reflexion Investment Agent

**A critique-driven investment-strategy prototype that generates a draft, grades it against a target risk label, and retries through a LangGraph loop.**

[![Python](https://img.shields.io/badge/Python-blue.svg)](https://www.python.org/) [![LangGraph](https://img.shields.io/badge/Workflow-LangGraph-2E6D8E.svg)](https://www.langchain.com/langgraph) [![Pydantic](https://img.shields.io/badge/Evaluation-Pydantic-E92063.svg)](https://docs.pydantic.dev/) [![Gradio](https://img.shields.io/badge/UI-Gradio-orange.svg)](https://www.gradio.app/) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](../LICENSE)

> **Financial-use notice:** This is an agent-workflow demonstration, not financial advice or a recommendation to buy or sell an asset. It uses no live market data and does not mathematically validate portfolio allocations.

## Overview

The user enters an investor profile (age, income, savings, and goal) and chooses a target grade: `High Risk`, `Moderate Risk`, `Low Risk`, or `Safe`. The generator uses a high-growth persona for the initial draft and a balanced persona for revisions. A structured evaluator returns a grade and critique through a Pydantic `Feedback` schema. The router ends when the grade matches the target or the iteration count reaches five. The app can export a PDF report and execution logs.

The personas are role prompts, not actual advice from or endorsement by the named investors.

## Architecture

```text
Investor profile + target risk label
  -> Generator node (initial growth-oriented draft)
  -> Evaluator node (Pydantic: grade + critique)
  -> Router
       ├─ grade matches target -> END
       ├─ iteration reaches 5 -> END
       └─ otherwise -> Refine with critique -> evaluate again
  -> PDF report + logs
```

## Implementation details

| Setting | Value in source |
|---|---|
| LLM | `gpt-4.1-mini` via LangChain |
| Temperature | `0.7` (not deterministic) |
| Evaluator grades | `High Risk`, `Moderate Risk`, `Low Risk`, `Safe` |
| Loop stop | Grade match or iteration `>= 5` |
| PDF | Markdown converted to HTML and rendered with `xhtml2pdf` |
| UI | Gradio |

The Pydantic schema constrains the evaluator's grade label and critique shape. It does not guarantee that the investment plan is appropriate, diversified, lawful, or mathematically coherent.

## Demonstrations and artifacts

![Investment workbench UI](interface.png)

### Recorded runs

- Test 1 logs: [agent_log_test_1.md](agent_log_test_1.md)
- Test 1 PDF output: [pdf_output_test_1.pdf](pdf_output_test_1.pdf)
- Test 2 logs: [agent_log_test_2.md](agent_log_test_2.md)
- Test 2 PDF output: [pdf_output_test_2.pdf](pdf_output_test_2.pdf)
- Additional sample test inputs: [test_1.pdf](test_1.pdf), [test_2.pdf](test_2.pdf)

## Run in Google Colab

The `.py` file is exported from Colab and contains notebook magic and `google.colab.userdata`; use it as notebook code or adapt it before running locally.

1. Add `OPENAI_API_KEY` to Colab Secrets.
2. Install packages:

   ```bash
   pip install -U langchain langchain-openai pydantic langgraph gradio markdown xhtml2pdf
   ```

3. Execute notebook cells in order, then enter the profile and target grade in Gradio.
4. Compare the evaluator's final grade and critique with the input target; download the PDF/log outputs for inspection.

## Limitations and security

- No live market data, historical returns, asset pricing, tax modeling, or risk calculation is connected.
- Allocation math is generated as text and is not validated to sum to 100%.
- An evaluator's grade is another model output; it is not an independent suitability assessment.
- Temperature is `0.7`, so repeated runs may differ.
- `Professional_Investment_Strategy.pdf` is a fixed output filename in the source and may be overwritten by another run.
- Do not submit personally identifiable financial information to a third-party model unless the data handling is approved.

## Repository files

```text
langgraph_reflexion_investment_agent/
├── README.md
├── langgraph_reflexion_investment_agent.py
├── interface.png
├── agent_log_test_1.md
├── agent_log_test_2.md
├── pdf_output_test_1.pdf
├── pdf_output_test_2.pdf
├── test_1.pdf
└── test_2.pdf
```

## Author

**Shubham Kumar Singh**

## License

This project is licensed under the **MIT License**. The complete license text is reproduced below and is also available in the repository's [`LICENSE`](../LICENSE) file.

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
