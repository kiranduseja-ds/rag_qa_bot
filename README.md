
---

# RAG Document Q&A Bot

A command-line style question-answering system that retrieves answers strictly from a local document collection using Retrieval-Augmented Generation (RAG).

The bot answers only from the uploaded documents and does not rely on external knowledge, ensuring grounded and context-based responses.

---

## Tech Stack

| Library               | Purpose                           |
| --------------------- | --------------------------------- |
| Python                | Core language                     |
| langchain             | RAG pipeline framework            |
| langchain-community   | Loaders, embeddings, vector store |
| faiss-cpu             | Vector database                   |
| transformers          | Language model (GPT-2)            |
| sentence-transformers | Embedding model (MiniLM)          |
| pypdf                 | PDF text extraction               |

---

## Architecture Overview

```
User Query
    │
    ▼
[1. INGESTION]
Load PDFs from /data folder using PyPDFLoader
    │
    ▼
[2. CHUNKING]
Split into chunks (1000 characters, 100 overlap)
Store metadata (filename, page)
    │
    ▼
[3. EMBEDDING]
Convert chunks into vectors using MiniLM (all-MiniLM-L6-v2)
    │
    ▼
[4. VECTOR DATABASE]
Store embeddings in FAISS and persist locally
    │
    ▼
[5. RETRIEVAL]
Embed query and retrieve top-k relevant chunks (k=3)
    │
    ▼
[6. GENERATION]
Generate answer using GPT-2 based only on retrieved context
    │
    ▼
Answer + Source (Filename + Page Number)
```

---

## Chunking Strategy

Strategy: Fixed-size chunking with overlap

* Chunk size: 1000 characters
* Overlap: 100 characters

Reason:
This approach ensures that context is preserved between chunks while keeping the number of chunks manageable. Larger chunks improve performance and reduce processing time, while overlap prevents loss of important information at boundaries.

---

## Embedding Model and Vector Database

Embedding Model: all-MiniLM-L6-v2 (HuggingFace)

* Lightweight and fast
* Good performance for semantic similarity
* Works locally without requiring any API

Vector Database: FAISS

* Fast similarity search
* No external server required
* Stores embeddings locally for reuse

---

## Setup Instructions

### 1. Open in Google Colab

Create a new notebook in Google Colab.

---

### 2. Install Dependencies

```
pip install langchain==0.1.17 langchain-community faiss-cpu transformers sentence-transformers pypdf
```

---

### 3. Upload Documents

Run the upload cell and select your 4–5 PDF files.

---

### 4. Index Documents

Run the indexing steps:

* Load PDFs
* Perform chunking
* Generate embeddings
* Store in FAISS

This step prepares the vector database.

---

### 5. Run the Bot

Run the chatbot cell and start asking questions.

---

## Example Queries

* What is artificial intelligence?
* Explain climate change
* What are features of the Indian economy?
* What is the impact of COVID-19?
* What are major space missions?

---

## Output

The system returns:

* Generated answer
* Source metadata (file name and page number)

---

## Environment Variables

No environment variables are required.
This project uses fully open-source models.

---

## Known Limitations

* The GPT-2 model is basic, so answers may be less detailed
* The system cannot answer outside the provided documents
* Requires re-uploading files in each Colab session
* Works best with clear and structured text

---

## Final Note

This project demonstrates a complete RAG pipeline from document ingestion to answer generation. The implementation focuses on clarity, simplicity, and practical understanding, making it easy to explain and extend further.


