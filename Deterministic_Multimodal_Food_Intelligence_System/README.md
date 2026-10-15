# Deterministic Multimodal Food Intelligence System

**An image-and-text food lookup pipeline using vision-based food naming, normalized sentence embeddings, and top-1 Pinecone metadata retrieval.**

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/) [![Gradio](https://img.shields.io/badge/UI-Gradio-orange.svg)](https://www.gradio.app/) [![Pinecone](https://img.shields.io/badge/Vector%20DB-Pinecone-000000.svg)](https://www.pinecone.io/) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](../LICENSE)

## Overview

This application accepts a food image, a text query, or both. When an image is provided, OpenAI `gpt-4.1-mini` is asked to identify the food and return a food-name string. The query is embedded with SentenceTransformers `all-MiniLM-L6-v2` and sent to the configured Pinecone index. The UI formats fields from the selected match's metadata.

The model identifies a query; it does **not** generate the nutrition fields. The reliability of those fields depends on the dataset and metadata used to populate Pinecone.

## Features

- Image-based food-name identification.
- Text-only lookup and combined image-plus-text queries.
- Normalized 384-dimensional embeddings from `all-MiniLM-L6-v2`.
- Pinecone nearest-neighbour retrieval with `top_k=1`.
- Metadata display for `food_name`, `calories`, `cuisine`, and `description`, plus the similarity score.
- Gradio interface for image, text, and combined input.

## Architecture

```text
Image (optional) ──> OpenAI vision model ──> food-name string ──┐
                                                               ├─> query text
Text query (optional) ──────────────────────────────────────────┘
       └─> MiniLM-L6-v2 embedding (384 dimensions, normalized)
              └─> Pinecone index: food-rag-index, top_k=1
                     └─> selected record metadata ──> Gradio output
```

## Configuration and data contract

| Setting | Checked-in value |
|---|---|
| Pinecone index | `food-rag-index` |
| Embedding model | `all-MiniLM-L6-v2` |
| Embedding dimension | `384` |
| Normalization | `normalize_embeddings=True` |
| Retrieval depth | `top_k=1` |
| Vision model | `gpt-4.1-mini` |
| Required metadata | `food_name`, `calories`, `cuisine`, `description` |

The repository folder does **not** contain the nutrition JSON dataset or an index-ingestion script. The application itself does not load a JSON dataset; the Pinecone index must already exist, use 384-dimensional vectors, and contain the expected metadata. A top-1 result is only the highest-ranked record returned by the index, not proof of an exact match.

## Demonstrations

### Image-only query

![Image-only example 1](test_1_only_image.png)

![Image-only example 2](test_2_only_image.png)

### Text-only query

![Text-only example](test_3_only_text_based.png)

`test_image_1.png` and `test_image_2.png` are additional sample inputs in the repository.

## Run in Google Colab

The checked-in `.py` file is a notebook export: it contains a `!pip` command and imports `google.colab.userdata`. It will not run unchanged as an ordinary local Python script.

1. Open a Google Colab notebook and copy the script into a code cell.
2. Add `OPENAI_API_KEY` and `PINECONE_API_KEY` in Colab Secrets and grant notebook access.
3. Install dependencies in a notebook cell:

   ```bash
   pip install gradio sentence-transformers pinecone-client openai pillow
   ```

4. Verify the pre-populated `food-rag-index` exists and matches the schema above.
5. Execute the cell and test the Gradio interface.

For a local deployment, replace Colab-specific secret access and notebook magic with normal environment-variable configuration first.

## Limitations and security

- Food identification can be wrong, especially for mixed or visually similar dishes.
- No similarity threshold, reranker, or multi-result review is implemented.
- Calorie and other nutrition values are not independently validated by the application.
- Unknown or poorly represented foods may return a misleading nearest neighbour.
- Image uploads are sent to the OpenAI API; query vectors and retrieval requests are sent to Pinecone.
- This is a software demonstration, not a medical, allergy, dietary-treatment, or clinical nutrition tool.
- The Gradio demo has no user authentication; do not expose it publicly with sensitive inputs.

## Project files

```text
Deterministic_Multimodal_Food_Intelligence_System/
├── README.md
├── deterministic_multimodal_food_intelligence_system.py
├── test_1_only_image.png
├── test_2_only_image.png
├── test_3_only_text_based.png
├── test_image_1.png
└── test_image_2.png
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
