# Retrieval Methods Benchmark

**A side-by-side demonstration of vector similarity, query expansion, metadata self-query, and parent-document reconstruction over a food dataset.**

[![Python](https://img.shields.io/badge/Python-blue.svg)](https://www.python.org/) [![Retrieval](https://img.shields.io/badge/Retrieval-4%20methods-2D7D9A.svg)](https://www.pinecone.io/) [![Embeddings](https://img.shields.io/badge/Embeddings-BGE%20Large-5B7FDC.svg)](https://www.sbert.net/) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](../LICENSE)

## Overview

This Gradio application runs four retrieval approaches for the same question and displays an answer plus elapsed time for each: vector similarity, multi-query expansion, metadata self-query, and parent-document reconstruction. Context is assembled from records indexed from the food dataset, and the answer call is prompted to use the provided context.

This is an interactive comparison prototype, not a comprehensive statistically controlled retrieval benchmark. It reports per-query latency and outputs, but does not compute corpus-wide precision, recall, MRR, nDCG, or correctness against a labelled test set.

## Compared methods

1. **Vector similarity:** query embedding and top-5 Pinecone matches.
2. **Multi-query:** an LLM creates reformulations; each query retrieves top-2 records and the resulting contexts are combined.
3. **Self-query:** an LLM extracts `cuisine_type` and `cooking_method` values, which are accepted only if they exactly match the vocabulary extracted from the dataset; top-5 filtered matches are then retrieved.
4. **Parent document:** retrieves top-5 chunks, selects up to two parent IDs, and reconstructs parent text before answering.

## Configuration verified in source

| Setting | Value |
|---|---|
| LLM answer model | `gpt-4.1-mini`, temperature `0` |
| Embedding model | `BAAI/bge-large-en-v1.5` |
| Vector dimension | `1024` |
| Index | `food-rag-strict` |
| Metric | Cosine |
| Chunk size / overlap | `700` / `100` characters |
| Dataset filename | `FoodDataSet.json.1` |
| UI | Gradio |

### Important data and index warning

The script loads `FoodDataSet.json.1`, then deletes the Pinecone index `food-rag-strict` if it already exists and creates it again before upserting the dataset. **Running the application can destroy and replace the existing index with that name.** Use a dedicated development index and verify the target before execution. The checked-in filename is `FoodDataSet.json.1`, not `FoodDataSet.json`.

The multi-query and filter-extraction calls do not explicitly set temperature, even though the final answering call uses temperature `0`; not all stages are deterministic.

## Demonstration screenshots

### Basic retrieval / calories

![Test 1](output_test_1.png)

### Strict metadata filter

![Test 2](output_test_2.png)

### Nested feature extraction

![Test 3](output_test_3.png)

### Cooking-method wording variation

![Test 4](output_test_4.png)

### Null-filter fallback

![Test 5](output_test_5.png)

## Run in Google Colab

The source includes notebook magic and uses Colab Secrets when available. Treat it as notebook-oriented until the installation and secrets cells are moved into normal Python setup.

1. Add `OPENAI_API_KEY` and `PINECONE_API_KEY` to Colab Secrets.
2. Check that `FoodDataSet.json.1` is present and inspect the configured Pinecone index name before running.
3. Install the dependencies used by the notebook:

   ```bash
   pip install numpy==1.26.4 scikit-learn==1.3.2 sentence-transformers==2.7.0 langchain-core==0.2.43 langchain-text-splitters==0.2.4 langchain-openai pinecone openai tiktoken gradio nest_asyncio
   ```

4. Run the notebook cells in order and submit one question to compare all four outputs.

## Interpretation and limitations

- Per-query elapsed time includes model/API work; it varies with provider latency, network, prompt length, and generated response length.
- No repeated-trial aggregation or statistical uncertainty is computed.
- Multi-query expansion is LLM-generated and may vary across runs.
- Self-query filters can miss relevant records if wording cannot be mapped to the dataset vocabulary.
- The dataset concerns food information; the results are not medical nutrition advice.
- External OpenAI/Pinecone service availability and API costs apply.

## Repository files

```text
retrieval-methods-benchmark/
├── README.md
├── food_rag_methodologies.py
├── FoodDataSet.json.1
├── output_test_1.png
├── output_test_2.png
├── output_test_3.png
├── output_test_4.png
└── output_test_5.png
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
