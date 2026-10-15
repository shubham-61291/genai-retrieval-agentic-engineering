# LangGraph Orchestrator–Worker System

**A LangGraph fan-out/fan-in demonstration that decomposes a multi-item request, runs independent worker nodes, and exports a synthesized guide with logs and a PDF.**

[![Python](https://img.shields.io/badge/Python-blue.svg)](https://www.python.org/) [![LangGraph](https://img.shields.io/badge/Orchestration-LangGraph-2E6D8E.svg)](https://www.langchain.com/langgraph) [![Pydantic](https://img.shields.io/badge/Schema-Pydantic-E92063.svg)](https://docs.pydantic.dev/) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](../LICENSE)

## Overview

The user enters a request containing one or more dishes. An orchestrator uses structured output to turn the request into typed `Dish` tasks. LangGraph's `Send()` routes each dish to a worker independently. A synthesizer combines worker outputs into a Markdown guide; helper functions also write a text log and render a PDF with `xhtml2pdf`.

The food domain is a demonstration context for orchestration patterns, not a source of verified culinary or nutrition advice.

## Architecture

```text
Request -> Orchestrator (Pydantic Dishes schema)
        -> Send() fan-out -> one chef worker per dish
        -> shared recipe/log state (operator.add)
        -> Synthesizer
        -> Markdown guide + agent_logs.txt + fancy_menu.pdf
        -> Gradio outputs
```

## Implementation details

- Model: `gpt-4.1-mini`, configured with `temperature=0`.
- Orchestrator schema: each `Dish` has `name`, `cuisine`, and `notes`.
- Fan-out: LangGraph `Send()` creates one worker task per parsed dish.
- State aggregation: `operator.add` appends recipe and log lists from workers.
- Artifacts: `agent_logs.txt` and `fancy_menu.pdf` are written using fixed filenames in the current working directory; a later run may overwrite them.
- The Gradio callback invokes the graph synchronously and returns the accumulated logs when execution completes. The current implementation is not token-by-token or genuinely live-streaming UI telemetry.

## Demonstrations

### UI

![Gradio dashboard](inerface.png)

### Agent execution trace

![Agent execution logs](agentlog.png)

### Example generated PDF

[Open the generated menu PDF](output_pdf.pdf)

[Open the PDF-download demonstration artifact](pdf_dload.pdf)

## Run in Google Colab

The checked-in source is a Colab notebook export containing `!pip` and `google.colab.userdata` access; it is not directly runnable as an ordinary Python script unchanged.

1. Add `OPENAI_API_KEY` to Colab Secrets.
2. Install dependencies in a notebook cell:

   ```bash
   pip install -U langchain langchain-openai pydantic langgraph gradio markdown xhtml2pdf
   ```

3. Run the cells in order and enter a multi-item request such as `chicken biryani, butter chicken, rumali roti`.
4. Inspect the generated guide and download log/PDF artifacts from the interface.

## Limitations and security

- The LLM supplies recipes; ingredients, cooking instructions, and food-safety details are not independently verified.
- Structured output validates task decomposition, not the correctness of the generated recipes.
- PDFs are generated from model-authored Markdown and may have formatting or content issues.
- `demo.launch(share=True, debug=True)` exposes a share URL; avoid sensitive input and shut down the session when finished.
- Output filenames are fixed and can be overwritten by subsequent runs.

## Repository files

```text
langgraph-orchestrator-worker-system/
├── README.md
├── langgraph_orchestrator_worker_pattern.py
├── agentlog.png
├── inerface.png
├── output_pdf.pdf
└── pdf_dload.pdf
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
