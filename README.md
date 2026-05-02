# RAG Document Q&A Bot

A command-line question-answering bot that retrieves answers exclusively from a
local document collection using Retrieval-Augmented Generation (RAG). Built with
LangChain, OpenAI, and FAISS. The bot never answers from its own training data —
only from your uploaded documents.

## Tech Stack

| Library | Version | Purpose |
|---------|---------|---------|
| Python | 3.12 | Core language |
| langchain | 0.0.350 | RAG pipeline framework |
| langchain-community | 0.0.1 | Document loaders, FAISS wrapper |
| openai | 1.3.5 | Embeddings + GPT-3.5 answer generation |
| faiss-cpu | latest | Vector database |
| pypdf | latest | PDF text extraction |
| tiktoken | latest | Token counting |
| docx2txt | 0.8 | DOCX support |

## Architecture Overview

```
User Query
    │
    ▼
[1. INGESTION] → Load PDFs from /data folder using PyPDFLoader
    │
    ▼
[2. CHUNKING] → Split into 500-char chunks, 50-char overlap, store filename + page
    │
    ▼
[3. EMBEDDING] → Batch embed all chunks using text-embedding-3-small
    │
    ▼
[4. VECTOR DB] → Store in FAISS, persist to disk in /vectorstore
    │
    ▼
[5. RETRIEVAL] → Embed query, similarity search, retrieve top-k chunks
    │
    ▼
[6. GENERATION] → GPT-3.5-turbo answers ONLY from retrieved chunks
    │
    ▼
Answer + Source Filename + Page Number
```

## Chunking Strategy

**Strategy: Fixed-size chunking with overlap**
- Chunk size: 500 characters
- Overlap: 50 characters

**Why:** Fixed-size chunking gives uniform, predictable chunks that work well
with embedding models. The 50-character overlap ensures sentences split at
chunk boundaries are not lost. Chosen over sentence-based chunking because
our documents vary in formatting and sentence length.

## Embedding Model and Vector Database

**Embedding Model: OpenAI text-embedding-3-small**
- High accuracy at low cost
- All chunks embedded in a single batched API call
- 1536-dimensional vectors

**Vector Database: FAISS**
- Persists to disk — no re-indexing on every run
- No external server needed
- Fast similarity search using L2 distance
- Clear separation: ingest.py saves, query.py loads

## Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/kiranduseja-ds/rag_qa_bot
```

### 2. Open in Google Colab
- Go to colab.research.google.com
- File → Upload Notebook → select rag_qa_bot.ipynb

### 3. Install dependencies
```bash
pip install langchain==0.0.350 langchain-community==0.0.1 openai==1.3.5 faiss-cpu pypdf tiktoken docx2txt
```

### 4. Set your API key
```python
import os
os.environ["OPENAI_API_KEY"] = "your-api-key-here"
```
Get your key from: https://platform.openai.com/api-keys

### 5. Upload documents
Run the upload cell — select your PDF files when prompted

### 6. Index documents
Run the ingestion cell — loads, chunks, embeds and saves to disk

### 7. Ask questions
Run the query cell — type any question and get answers with citations

## Environment Variables

| Variable | Description | Where to get |
|----------|-------------|--------------|
| OPENAI_API_KEY | OpenAI API key | platform.openai.com/api-keys |

**Never commit your actual API key to GitHub.**

## Example Queries

| Question | Expected Answer Theme |
|----------|-----------------------|
| What is artificial intelligence? | Definition, history, applications |
| What causes climate change? | Greenhouse gases, human activity |
| What are symptoms of COVID-19? | Fever, cough, breathing issues |
| What is ISRO and its achievements? | Indian space agency, missions |
| What are famous temples in India? | Architecture, location, significance |

## Known Limitations
- Only answers from uploaded documents, not from internet
- Colab session resets require re-running all cells
- PDFs must be re-uploaded each Colab session
- Works best with English language documents
- Very short answers may be split across chunk boundaries
