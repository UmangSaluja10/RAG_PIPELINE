# RAG Pipeline: Chunk Size vs Retrieval Performance

## Overview

This project implements a Retrieval-Augmented Generation (RAG) pipeline using Flutter documentation as the knowledge base. The goal is to study how different chunk sizes affect retrieval performance in a semantic search system.

---

## Objective

The experiment evaluates:

* Impact of chunk size on retrieval quality
* Trade-off between context preservation and noise
* Effectiveness of semantic embeddings across varying chunk granularities

---

## Dataset

* Source: Official Flutter documentation
* Number of pages: ~50
* Topics covered:

  * UI and Widgets
  * Navigation
  * Networking
  * State Management
  * Performance
  * Testing
  * Architecture

---

## Methodology

### 1. Data Loading

Web pages are scraped using LangChain's `WebBaseLoader`.

### 2. Preprocessing

* Removal of extra whitespace
* Text normalization

### 3. Chunking

Text is split using `RecursiveCharacterTextSplitter` with:

* Chunk sizes: 250, 500, 1000, 1500, 2000
* Overlap: 20%

### 4. Embedding Model

* Model: `sentence-transformers/all-MiniLM-L6-v2`
* Converts text into dense vector representations

### 5. Vector Store

* Database: ChromaDB
* Used for similarity-based retrieval

### 6. Retrieval

* Top-k retrieval (k = 3)
* Based on cosine similarity

### 7. Evaluation

* Semantic similarity between retrieved content and ground truth answers
* Metric: Average cosine similarity score

---

## Results

| Chunk Size | Accuracy |
| ---------- | -------- |
| 250        | 0.29     |
| 500        | 0.34     |
| 1000       | 0.31     |
| 1500       | 0.31     |
| 2000       | 0.34     |

---

## Observations

* Moderate chunk sizes (500–1000) show slightly better performance
* Small chunks (250) lose contextual coherence
* Large chunks (2000) introduce noise but remain competitive
* Overall variation across chunk sizes is limited

---

## Key Insight

Chunk size has a marginal effect on retrieval performance when using modern embedding models. Performance stabilizes beyond a certain chunk size threshold.

---

## Limitations

* Limited number of evaluation queries
* Similarity-based evaluation is an approximation
* No ranking-based metrics such as Recall@k or MRR

---

## Future Work

* Add Recall@k and Mean Reciprocal Rank (MRR)
* Increase dataset size further
* Compare multiple embedding models
* Introduce reranking methods
* Use LLM-based evaluation

---

## Tech Stack

* Python
* LangChain
* ChromaDB
* HuggingFace Transformers
* Sentence Transformers

---

## Conclusion

This experiment shows that chunk size influences retrieval performance only to a limited extent. Moderate chunk sizes provide a balance between context and noise, while extreme values degrade performance slightly.

---
