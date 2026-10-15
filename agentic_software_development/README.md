# Agentic Windows EXE Builder

**A LangGraph ReAct coding workflow that generates Python application files, checks syntax, and delegates Windows packaging to GitHub Actions.**

[![Python](https://img.shields.io/badge/Language-Python-blue.svg)](https://www.python.org/) [![LangGraph](https://img.shields.io/badge/Workflow-LangGraph-1C3C3C.svg)](https://www.langchain.com/langgraph) [![Gradio](https://img.shields.io/badge/UI-Gradio-orange.svg)](https://www.gradio.app/) [![GitHub Actions](https://img.shields.io/badge/Build-GitHub%20Actions-2088FF.svg)](https://github.com/features/actions) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](../LICENSE)

## Overview

The builder accepts natural-language application requirements, uses an LLM-backed ReAct agent to write `src/main.py` and `requirements.txt` through file tools, checks Python syntax, and attempts a bounded repair. After syntax passes, it clones a configured GitHub build repository, creates a run-specific branch, dispatches a Windows workflow, polls its status, downloads the `windows-exe` artifact, and exposes `app.exe` in the Gradio interface.

The checked-in file is a Google Colab export. It contains notebook magic (`!pip`) and uses `google.colab.userdata`; it is **not** a standalone Python script that runs unchanged in a normal terminal.

## Workflow

```text
Requirement -> ReAct agent -> write_file/read_file tools
             -> python -m py_compile src/main.py
                ├─ failure -> patch agent -> re-check
                └─ pass -> push run branch -> GitHub Actions
                           -> windows-exe artifact -> app.exe download
```

## Configuration verified in source

| Setting | Value |
|---|---|
| Model | `gpt-4.1-mini` |
| Temperature | `0` |
| Maximum ReAct steps | `18` per generation/patch phase |
| Patch counter | `4` |
| Build repository | `shubham-61291/agentic-exe-builder` |
| Workflow file | `build-exe.yml` |
| Expected artifact | `windows-exe` containing `app.exe` |
| Run branch pattern | `build-<run-id>` |
| Runtime | Google Colab |

### Important workflow prerequisites

The target repository and its workflow are external to this folder. The source dispatches the workflow with a `ref` payload. If the deployed workflow requires an additional input (such as a branch input), dispatch will fail until the request and workflow contract are aligned. Verify that the workflow artifact is named exactly `windows-exe` and contains `app.exe`.

## Demonstrations

### GitHub Actions runs

![Recorded workflow runs](GIT_WORKFLOWS.png)

### Gradio UI

![Builder interface](interface.png)

### Test 1 — PyQt browser

![Browser build request](TEST_1.png)

The recorded log and screenshots show an initial source-validation failure, a patch attempt, a successful subsequent Windows workflow, and browser output.

![Test 1 build log](TEST_1_OUTPUT.png)

![Browser output](TEST_1_OUTPUT_2.png)

![Browser navigation output](TEST_1_OUTPUT_3.png)

[Recorded Test 1 log](TEST_1_AGENT_LOG.txt)

### Test 2 — Tkinter scientific calculator

![Calculator build request](TEST_2.png)

![Calculator output](TEST_2_OUTPUT.png)

[Recorded Test 2 log](TEST_2_AGENT_LOG.txt)

The logs document these specific runs; they do not guarantee arbitrary future requirements will build successfully.

## Run in Google Colab

1. Open the notebook linked in the source docstring: [Agentic Windows EXE Builder notebook](https://colab.research.google.com/drive/1CO5UE1KKDh7YoGtF9gpEPakWgrZhnjbS).
2. Add `OPENAI_API_KEY` and `GITHUB_TOKEN` in Colab Secrets and grant notebook access.
3. Verify repository access and the `build-exe.yml` workflow before running.
4. Run the notebook's dependency-installation cell and application cells.
5. Enter a specific app request; inspect generated source and logs before executing the resulting binary.

Dependencies installed by the source:

```bash
pip install -U langchain langchain-openai pydantic langgraph gradio requests
```

Do not paste the exported `.py` directly into a normal Python terminal without removing notebook magic and replacing the Colab Secrets integration.

## Known limitations and security

- `py_compile` checks syntax only. It does not test imports at runtime, UI behavior, application logic, or Windows integration.
- The configured patch counter is not a fully enforced terminal bound in the current graph: the patch node routes back to sanity checking, and failed checks can route to patching again after the counter limit. Add and test a terminal failure edge before treating the loop as bounded.
- GitHub workflow dispatch can fail if the payload does not match the deployed workflow's required inputs.
- The token is embedded in a Git clone URL by the current implementation; protect process/log data and use a narrowly scoped token.
- The source calls `demo.launch(share=True)`, creating an externally reachable Gradio link. Do not put sensitive requirements or secrets into a public session.
- Generated applications and dependency files are untrusted until reviewed. No malware scan, code signing, or application-level test suite is implemented.

## Repository files

```text
agentic_software_development/
├── README.md
├── agentic_windows_exe_builder.py
├── GIT_WORKFLOWS.png
├── interface.png
├── TEST_1.png
├── TEST_1_AGENT_LOG.txt
├── TEST_1_OUTPUT.png
├── TEST_1_OUTPUT_2.png
├── TEST_1_OUTPUT_3.png
├── TEST_2.png
├── TEST_2_AGENT_LOG.txt
└── TEST_2_OUTPUT.png
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
