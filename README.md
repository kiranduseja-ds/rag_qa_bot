# RAG Document Q&A Bot

A question-answering bot that finds answers from your own documents using
Retrieval-Augmented Generation (RAG).

## What it does
- Upload any PDF documents
- Ask questions in plain English
- Bot finds the answer and tells you which file and page it came from

## Tech Stack
- Python 3.12 (Google Colab)
- LangChain — RAG pipeline
- OpenAI GPT-3.5-turbo — answer generation
- OpenAI text-embedding-3-small — text to numbers
- FAISS — vector database for fast search

## How RAG Works
1. PDFs are loaded and split into 500-character chunks (50-char overlap)
2. Each chunk is converted to embeddings (numbers representing meaning)
3. Embeddings saved in FAISS vector database
4. Question asked → 4 most similar chunks retrieved
5. GPT reads those chunks → gives answer with source file and page

## Why These Choices
- **Chunk size 500, overlap 50** — big enough for context, overlap stops sentences being cut off
- **FAISS** — saves locally, no server needed, very fast
- **temperature=0** — factual answers, not creative ones

## How to Run
1. Open `rag_qa_bot.ipynb` in Google Colab
2. Add your OpenAI API key
3. Upload your PDF documents
4. Run all cells top to bottom
5. Ask questions in the last cell

## Documents Used
- Artificial Intelligence (Wikipedia PDF)
- Climate Change (Wikipedia PDF)
- COVID-19 (Wikipedia PDF)
- Space Exploration (Wikipedia PDF)

## Example Questions
1. What is artificial intelligence?
2. What causes climate change?
3. What are symptoms of COVID-19?
4. How do rockets reach space?
5. What is machine learning?

## Limitations
- Only answers from uploaded documents, not internet
- Cannot answer questions outside document topics
- Colab session resets require re-running all cells
