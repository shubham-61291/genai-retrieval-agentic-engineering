# Legal Compliance Reflexion Agent

**A LangGraph-based contract-review prototype that structures potential GDPR/EU AI Act issues, generates research queries, retrieves web evidence, and revises an audit report.**

[![Python](https://img.shields.io/badge/Python-blue.svg)](https://www.python.org/) [![LangGraph](https://img.shields.io/badge/Workflow-LangGraph-2E6D8E.svg)](https://www.langchain.com/langgraph) [![Pydantic](https://img.shields.io/badge/Schema-Pydantic-E92063.svg)](https://docs.pydantic.dev/) [![Gradio](https://img.shields.io/badge/UI-Gradio-orange.svg)](https://www.gradio.app/) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](../LICENSE)

> **Legal disclaimer:** This project is not legal advice, a compliance certification, or a substitute for qualified legal counsel. Scores and flags are model-generated and must be checked against current law, official sources, jurisdiction, and the full contract context.

## Overview

The prototype accepts contract text from PDF/DOCX or pasted text and returns a structured report containing an executive summary, an overall score, and flagged clauses with risk level, clause text, explanation, and suggested rewrite. The LangGraph pipeline connects four stages: Junior Auditor, Reflector, Researcher, and Senior Partner Revisor. The reflector generates search queries; Tavily Search retrieves evidence; the revisor receives the draft and evidence to produce the final structured report. A PDF report and Gradio UI are included.

## Architecture

```text
PDF / DOCX / pasted text
  -> DocumentParser (pdfplumber / python-docx for files)
  -> Junior Auditor (Pydantic AuditReport)
  -> Reflector (missing evidence + up to 3 search queries)
  -> Tavily Search Results
  -> Senior Revisor (structured revised report)
  -> Markdown/UI report + FPDF PDF
```

## Output contract

- `executive_summary`
- `overall_compliance_score` (integer described as 0–100 in the prompt/schema)
- `flagged_clauses[]`, each with `risk_level`, `offending_clause`, `violation_explanation`, and `suggested_rewrite`

The schema structures the output; it does not prove a clause is unlawful or that a score has legal meaning.

## Demonstrations

### Main interface

![Legal audit interface](legal-compliance-reflexion-agent-interface.png)

### SaaS master agreement example

![SaaS audit example](legal-compliance-reflexion-agent-SaaS_audit.png)

[Sample contract](SaaS%20Master%20Subscription%20Agreement.docx) · [Generated sample audit PDF](Magnetar_Final_Audit_SAAS.pdf)

### Data processing addendum example

![DPA audit example](legal-compliance-reflexion-agent-DPA-audit.png)

[Sample DPA](Data%20Processing%20Addendum.pdf) · [Generated sample audit PDF](Magnetar_Final_Audit_DPA.pdf)

## Run in Google Colab

The source uses `!pip`, `google.colab.userdata`, and notebook cells that execute illustrative test cases. It should be run as notebook code or adapted before standard local execution.

1. Add `OPENAI_API_KEY` and `TAVILY_API_KEY` to Colab Secrets.
2. Install dependencies:

   ```bash
   pip install -U langchain langchain-openai langchain-community pydantic openai tavily-python pdfplumber python-docx fpdf gradio
   ```

3. Execute cells in order. Review the example test output before using the UI with another document.
4. Upload a PDF/DOCX or paste contract text, run the workflow, and inspect the full report.

## Limitations, source quality, and privacy

- Search uses Tavily queries and does not enforce an official-source domain allow-list. Search results may be outdated, incomplete, or unofficial.
- The reviewer is another model call, not an independent legal reviewer. Evidence selection and citation correctness are not formally tested.
- PDF/DOCX text extraction may omit tables, footnotes, scanned text, or layout relationships; OCR is not implemented in the shown parser.
- The score is model-assigned and should not be treated as a measurable legal compliance grade.
- The example script runs a dummy contract through illustrative setup/test cells before the interactive workflow; separate test code from application startup for a production deployment.
- `demo.launch(share=True, debug=True)` creates an externally reachable link. Contracts often contain confidential information; do not upload real client contracts to a public demo without authorization, access controls, and an approved data-processing arrangement.
- The sample PDFs use “Privileged & Confidential” wording, but generating that label does not create legal privilege.

## Repository files

```text
legal-compliance-reflexion-agent/
├── README.md
├── legal_compliance_reflexion_agent.py
├── SaaS Master Subscription Agreement.docx
├── Data Processing Addendum.pdf
├── Magnetar_Final_Audit_SAAS.pdf
├── Magnetar_Final_Audit_DPA.pdf
├── legal-compliance-reflexion-agent-interface.png
├── legal-compliance-reflexion-agent-SaaS_audit.png
└── legal-compliance-reflexion-agent-DPA-audit.png
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
