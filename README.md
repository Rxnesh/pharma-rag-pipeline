# Pharma RAG Pipeline

A Retrieval-Augmented Generation (RAG) chatbot that classifies, indexes, and answers natural language questions about bundled pharmaceutical documents using OCR, FAISS, and a locally-deployed open-source LLM.

## Overview

Pharmaceutical companies often receive document packages that bundle multiple document types (cover letters, certificates of quality, packaging specifications, compliance declarations) into a single PDF, with no clear boundaries between them. This project builds a pipeline that automatically segments, classifies, and indexes these documents, then answers user questions with cited sources and a confidence score.

## Pipeline


## Tech Stack

- **LLM:** Phi-3-mini-4k-instruct (Hugging Face, run locally on GPU)
- **Embeddings:** sentence-transformers (all-MiniLM-L6-v2)
- **Vector Store:** FAISS
- **Indexing/Retrieval:** LlamaIndex
- **OCR:** Tesseract (pytesseract) — fallback for scanned pages
- **UI:** Gradio
- **Environment:** Google Colab (T4 GPU)

## Features

- Automatic document type classification (7 pharmaceutical categories)
- Page-level boundary detection to segment bundled PDFs
- OCR fallback for scanned documents
- Query routing with auto-detection of relevant document type
- Adjustable retrieval settings (chunk count, document type filter)
- Source-cited answers with page ranges and relevance scores
- Fully local inference — no external API costs or rate limits

## Setup

1. Open the notebook in Google Colab
2. Set runtime to GPU (Runtime → Change runtime type → T4 GPU)
3. Run cells in order from Step 1 through Step 11
4. Upload a pharmaceutical PDF via the Gradio interface and click "Process Document"
5. Ask questions in the chat panel

## Known Limitations

- Document boundary detection currently treats most individual pages as separate documents rather than merging multi-page sections
- Query routing occasionally fails to return valid structured output from the local model, falling back to a lower-confidence default
- No formal held-out test set with verified ground truth has been built yet — testing so far is based on manual spot-checks
- OCR fallback exists in code but hasn't been validated against an actual scanned document

## Example Query

**Question:** "What lot numbers or batch numbers are mentioned in these documents?"

**Answer:** Returns 6 correctly cited lot numbers, each tied to a specific product and source document (Certificate of Quality, Chain of Custody), with a confidence score based on retrieval relevance.

## Future Improvements

- Build a proper test set with multiple documents and verified ground truth
- Improve boundary detection prompting for better multi-page document merging
- Add a reranking step to improve retrieval precision
- Separate timing metrics for retrieval vs. generation
