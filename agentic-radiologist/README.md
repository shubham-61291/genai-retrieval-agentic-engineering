# X-Ray Agentic Radiologist

**A LangGraph-orchestrated radiology-report research prototype combining vision-derived findings, Pinecone retrieval, Tavily search, and a model-based report review stage.**

[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/) [![Workflow](https://img.shields.io/badge/Workflow-LangGraph-2E6D8E.svg)](https://www.langchain.com/langgraph) [![UI](https://img.shields.io/badge/UI-Gradio-orange.svg)](https://www.gradio.app/) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](../LICENSE)

> **Clinical safety:** This is a research/portfolio prototype, not a medical device. Its output is not a diagnosis and must not be used to make clinical decisions. A qualified clinician must independently interpret medical images and verify all generated content.

## Overview

The application accepts one image (`.png`, `.jpg`, or `.jpeg`) and optional clinical history. A LangGraph `StateGraph` coordinates six sequential nodes: vision analysis, retrieval-query construction, Pinecone search, Tavily search, report drafting, and report review. The Gradio UI displays the execution log and intermediate artifacts alongside the draft and reviewed report.

## Workflow

```text
Image + optional clinical history
  -> Vision Analyst (structured JSON)
  -> Query Builder
  -> Pinecone retrieval (top_k=6)
  -> Tavily web search (up to 5 results)
  -> Radiologist Agent (draft)
  -> Senior Radiologist Reviewer (revised report)
  -> Gradio: logs, evidence, draft, final text
```

## Implementation details

| Area | Checked-in configuration |
|---|---|
| Orchestration | LangGraph `StateGraph` with sequential edges |
| Vision and report model | `gpt-4.1-mini` for both configured model variables |
| Vector index | Pinecone `xray-rag-1024` |
| Embedding model | `BAAI/bge-large-en-v1.5` |
| Embedding dimension | `1024`, checked at startup |
| Vector normalization | Enabled |
| Retrieval | `top_k=6`, metadata included |
| External search | Tavily Search API, up to five results |
| UI | Gradio |

The code uses the vision JSON, retrieved records, and web results as inputs to report drafting and review. The reviewer is another LLM call instructed to remove unsupported statements; it is not a validated clinical safeguard and cannot guarantee that hallucinations are removed.

## Demonstration assets

### Interface

![Gradio interface](interface.png)

### Example images and saved reports

| Example | Image | Saved report |
|---|---|---|
| 1 | ![Test 1](test_1.png) | [output_1.docx](output_1.docx) |
| 2 | ![Test 2](test_2.png) | [output_2.docx](output_2.docx) |
| 3 | ![Test 3](test_3.png) | [output_3.docx](output_3.docx) |

The existing project description identifies the three images as examples from the OpenI/NLM Indiana University Chest X-rays collection. Reference reports may describe multi-view studies while the application accepts one uploaded image; therefore the reference is contextual, not a guaranteed one-to-one gold standard.

- [OpenI example 1](https://openi.nlm.nih.gov/detailedresult?img=CXR452_IM-2083-1001&query=Indiana%20U.%20Chest%20X-rays,%20Pneumonia&it=xg&uniq=1&req=4&npos=1)
- [OpenI example 2](https://openi.nlm.nih.gov/detailedresult?img=CXR125_IM-0169-1001&query=Indiana%20U.%20Chest%20X-rays,%20Effusion&it=xg&uniq=1&req=4&npos=2)
- [OpenI example 3](https://openi.nlm.nih.gov/detailedresult?img=CXR484_IM-2108-1001&query=Indiana%20U.%20Chest%20X-rays&it=xg&coll=cxr&uniq=1&req=4&npos=7)

## Run in Google Colab

The checked-in script is exported from Colab and includes notebook magic (`!pip`) plus `google.colab.userdata`; it is not directly executable with ordinary `python` unchanged.

1. Add `OPENAI_API_KEY`, `PINECONE_API_KEY`, and `TAVILY_API_KEY` in Colab Secrets.
2. Confirm Pinecone index `xray-rag-1024` exists, is 1,024-dimensional, and contains the expected `chunk_id` and `text` metadata.
3. Install the source dependencies in a notebook cell:

   ```bash
   pip install -U gradio openai pinecone sentence-transformers requests langgraph langchain langchain-openai pydantic
   ```

4. Run the source cell and upload a single image in Gradio.

The repository does not include a Pinecone provisioning or ingestion pipeline. The saved `.docx` reports are example artifacts already present in the repository; the current UI code does not expose a dedicated report-download control.

## Model comparison and interpretation

The original project notes describe qualitative comparisons between `gpt-4.1-mini` and `gpt-5-mini` on three examples. The currently checked-in code sets both `MODEL_VISION` and `MODEL_AGENTS` to `gpt-4.1-mini`; it does not automatically execute a two-model evaluation or calculate clinical metrics. The examples are not a controlled clinical study and do not establish clinical safety.

## Limitations and privacy

- The code analyses a single uploaded view, not a full multi-view study.
- The retrieved knowledge index and its provenance must be validated separately.
- Tavily results are generic web-search evidence, not a curated clinical evidence database.
- A Tavily non-200 response is handled with empty search results; other exceptions may interrupt execution.
- No automated test suite reports sensitivity, specificity, calibration, hallucination rate, or clinician agreement.
- Images and related prompts are sent to OpenAI; search queries are sent to Tavily; retrieval queries go to Pinecone. Do not submit identifiable patient data without appropriate authorization and safeguards.
- Do not expose the development Gradio instance publicly.

## Project files

```text
agentic-radiologist/
├── README.md
├── agentic_radiologist.py
├── data.docx
├── interface.png
├── output_1.docx
├── output_2.docx
├── output_3.docx
├── test_1.png
├── test_2.png
└── test_3.png
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
