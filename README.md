RAG Document Q&A Bot

A simple command-line style question-answering system that retrieves answers directly from a set of uploaded documents using a Retrieval-Augmented Generation (RAG) approach.

The system does not rely on general knowledge. It only answers based on the content present in the uploaded PDF documents, ensuring grounded and document-based responses.

Tech Stack
Library	Purpose
Python	Core programming language
langchain	RAG pipeline framework
langchain-community	Document loaders, embeddings, vector store
faiss-cpu	Vector database for similarity search
transformers	Language model (GPT-2) for answer generation
sentence-transformers	Embedding model (MiniLM)
pypdf	PDF text extraction
Architecture Overview
User Query
    │
    ▼
[1. INGESTION] → Load PDFs from /data folder
    │
    ▼
[2. CHUNKING] → Split into chunks (1000 size, 100 overlap)
    │
    ▼
[3. EMBEDDING] → Convert chunks into vectors using MiniLM
    │
    ▼
[4. VECTOR DB] → Store embeddings in FAISS
    │
    ▼
[5. RETRIEVAL] → Retrieve top 3 relevant chunks
    │
    ▼
[6. GENERATION] → GPT-2 generates answer from retrieved context
    │
    ▼
Answer + Source (file name and page)
Chunking Strategy

Strategy: Fixed-size chunking with overlap

Chunk size: 1000 characters
Overlap: 100 characters

Reason:
Larger chunks reduce the total number of chunks, improving speed.
Overlap ensures that context is not lost at boundaries between chunks.

Embedding Model and Vector Database

Embedding Model: all-MiniLM-L6-v2 (HuggingFace)

Lightweight and fast
Suitable for semantic similarity tasks
Works locally without API usage

Vector Database: FAISS

Efficient similarity search
Runs locally without external setup
Stores embeddings for fast retrieval
Setup Instructions
1. Open in Google Colab
Go to Google Colab
Create a new notebook
2. Install dependencies
pip install langchain==0.1.17 langchain-community faiss-cpu transformers sentence-transformers pypdf
3. Upload documents

Run the upload cell and select your PDF files (4–5 documents).

4. Run indexing pipeline

Execute cells in order:

Load documents
Split into chunks
Generate embeddings
Store in FAISS

This step creates and saves the vector database.

5. Run query system

Run the chatbot cell and start asking questions.

Example Queries
What is artificial intelligence?
Explain climate change
What are key features of the Indian economy?
What is the impact of COVID-19?
What are important space missions?
Output

For each query, the system provides:

Generated answer
Source information (file name and page number)
Environment Variables

This project uses open-source models and does not require any API keys.

Known Limitations
The language model used (GPT-2) is basic, so answers may be less detailed
The system only answers from uploaded documents
Accuracy depends on document quality
Colab sessions reset, so files must be re-uploaded
Final Note

This project demonstrates a complete RAG pipeline including document ingestion, chunking, embedding, retrieval, and answer generation. The focus is on building a simple, understandable system that can be explained clearly and extended further.

