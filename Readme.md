#  Flutter Docs RAG Pipeline

A Retrieval-Augmented Generation (RAG) pipeline that lets you ask natural language questions about Flutter — and actually get answers — without paying a single rupee for any API.

---

##  What It Does

Scrapes real Flutter documentation pages, breaks them into chunks, embeds them locally using a sentence transformer, stores everything in a ChromaDB vector store, and then uses **TinyLlama-1.1B** running on GPU to generate answers — all inside Google Colab.

---

##  Pipeline Architecture

```
Flutter Docs (Web)
       ↓
  WebBaseLoader
       ↓
  Text Cleaning
       ↓
  RecursiveCharacterTextSplitter (chunk_size=1000, overlap=200)
       ↓
  HuggingFace Embeddings (all-MiniLM-L6-v2)
       ↓
  ChromaDB Vector Store
       ↓
  Retriever (top 3 chunks)
       ↓
  TinyLlama-1.1B-Chat (local, on T4 GPU)
       ↓
  Answer
```

---

##  Tech Stack

| Component | Tool |
|---|---|
| Framework | LangChain |
| Document Loader | WebBaseLoader (LangChain Community) |
| Text Splitter | RecursiveCharacterTextSplitter |
| Embeddings | sentence-transformers/all-MiniLM-L6-v2 |
| Vector Store | ChromaDB |
| LLM | TinyLlama/TinyLlama-1.1B-Chat-v1.0 |
| Runtime | Google Colab (T4 GPU, free tier) |

---

##  Installation

```bash
pip install langchain_community langchainhub chromadb langchain langchain-huggingface
pip install transformers>=4.37.0 sentence-transformers huggingface_hub
```

---

##  How to Run

1. Open the notebook in **Google Colab**
2. Make sure runtime is set to **T4 GPU** — `Runtime > Change runtime type > T4 GPU`
3. Add your HuggingFace token to Colab Secrets with the name `HuggingFace_API`
4. Run all cells from top to bottom
5. Ask your Flutter question in the last cell

```python
response = rag_chain.invoke("What is Flutter and what can you build with it?")
print(response)
```

---

##  Data Sources

The pipeline scrapes these Flutter documentation pages:

- https://docs.flutter.dev/get-started/install
- https://docs.flutter.dev/get-started/codelab
- https://docs.flutter.dev/ui
- https://docs.flutter.dev/ui/widgets
- https://docs.flutter.dev/cookbook
- https://docs.flutter.dev/cookbook/navigation/navigation-basics
- https://docs.flutter.dev/cookbook/networking/fetch-data
- https://docs.flutter.dev/data-and-backend/state-mgmt/intro

---

##  Why Not OpenAI?

Started with `langchain-openai` + `ChatOpenAI` but the free quota ran out almost instantly. Switched to a fully local setup using HuggingFace models — works just as well for this use case and costs nothing.

Tried `HuggingFaceEndpoint` with Mistral and Zephyr but HF's free inference API kept routing them through third-party providers (`novita`, `featherless-ai`) that only supported `conversational` task — not `text-generation`. Final fix was loading TinyLlama directly on Colab's T4 GPU using `transformers` pipeline.

---

##  Project Structure

```
RAG_PIPELINE.ipynb   ← main notebook with all cells
README.md            ← you're reading it
```

---

##  Acknowledgements

- [LangChain](https://github.com/langchain-ai/langchain)
- [TinyLlama](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0)
- [Flutter Docs](https://docs.flutter.dev)
- [ChromaDB](https://www.trychroma.com/)
