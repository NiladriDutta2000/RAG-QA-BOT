# Basic Document Q&A Bot using RAG (Retrieval-Augmented Generation)

## 1. Project Title & Description

### Project Title:
**Basic Document Q&A Bot using RAG with LangChain, ChromaDB, and Gemini**

### Description:
This project is an end-to-end Retrieval-Augmented Generation (RAG) system that allows users to ask natural language questions on a collection of uploaded documents (PDF, DOCX, TXT). The bot retrieves the most relevant document chunks using semantic search and generates grounded answers using Google Gemini with source citations. It is designed to reduce hallucinations by answering only from uploaded content.

---

# 2. Tech Stack

| Category | Tool / Library | Version |
|---------|----------------|---------|
| Language | Python | 3.11+ |
| Notebook | Google Colab | Latest |
| Framework | LangChain | Latest |
| Loaders | langchain-community | Latest |
| Text Splitter | langchain-text-splitters | Latest |
| Vector DB | ChromaDB | Latest |
| Embeddings | sentence-transformers | Latest |
| Embedding Model | all-MiniLM-L6-v2 | Latest |
| LLM | Google Gemini | gemini-2.5-flash |
| Gemini Integration | langchain-google-genai | Latest |
| PDF Reader | PyPDF | Latest |
| DOCX Reader | python-docx | Latest |
| UI | Gradio | Latest |

LangChain provides integrations for Gemini models via `langchain-google-genai`. :contentReference[oaicite:0]{index=0}

---

# 3. Architecture Overview

## RAG Pipeline

```text
User Uploads Documents
        ↓
Document Ingestion
(PDF / DOCX / TXT Loading)
        ↓
Text Chunking
(1000 chars + 200 overlap)
        ↓
Embedding Generation
(all-MiniLM-L6-v2)
        ↓
Store in Chroma Vector DB
        ↓
User asks Question
        ↓
Similarity Search (Top-k Retrieval)
        ↓
Relevant Chunks sent to Gemini
        ↓
Grounded Answer + Source Citations

```
## Components:
**A. Ingestion**

Reads multiple documents from local folder.

**B. Chunking**

Splits long text into smaller searchable units.

**C. Embedding**

Converts text chunks into vectors.

**D. Retrieval**

Finds top similar chunks for a user query.

**E. Generation**

Gemini creates final answer only from retrieved context.

---

# 4. Chunking Strategy

## Strategy Used:

**Recursive Character Text Splitting**

chunk_size = 1000

chunk_overlap = 200

## Why Chosen:

* Large documents cannot be processed efficiently as one block.
* Smaller chunks improve retrieval precision.
* 200 overlap preserves sentence continuity between chunks.
* Recursive splitting respects paragraphs and sentence boundaries where possible.

## Example:

Chunk 1 ends: Machine learning is widely used in

Chunk 2 begins: used in recommendation systems...

Overlap helps preserve context.

---

# 5. Embedding Model & Vector Database

## Embedding Model:

sentence-transformers/all-MiniLM-L6-v2

### Why Chosen:
* Free and open-source
* Fast inference in Colab
* Strong semantic similarity performance
* Lightweight compared to larger models

SentenceTransformers is widely used for semantic embeddings.

## Vector Database:

ChromaDB

### Why Chosen:
* Easy local setup
* Persistent storage to disk
* Integrates directly with LangChain
* Good for small to medium RAG projects

---

# 6. Setup Instructions

## Step 1: Clone Repository

git clone https://github.com/yourusername/rag-document-qa-bot.git

cd rag-document-qa-bot

## Step 2: Install Dependencies

pip install -r requirements.txt

## Step 3: Set Gemini API Key

import os

os.environ["GOOGLE_API_KEY"] = "YOUR_API_KEY"

## Step 4: Run Notebook / Script

Upload documents and run all cells.

## Step 5: Launch UI

Gradio launches a web interface automatically.

---

# 7. Environment Variables

## Required:

GOOGLE_API_KEY=your_gemini_api_key_here

## How to Set:

### Google Colab

import getpass, os

os.environ["GOOGLE_API_KEY"] = getpass.getpass("Enter API Key: ")

---

# 8. Example Queries

| Query                          | Expected Theme                   |
| ------------------------------ | -------------------------------- |
| What is photosynthesis?        | Plant biology                    |
| Explain the digestive system.  | Human body                       |
| What is Ohm’s law?             | Physics                          |
| Which planet is the largest?   | Astronomy                        |
| What is quantum teleportation? | If not in docs → No answer found |

**Example Output**

<img width="1653" height="597" alt="image" src="https://github.com/user-attachments/assets/ca060805-cd6b-4a43-a5dd-72535e7226b9" />


---

# 9. Known Limitations

**1. Small Embedding Model**

MiniLM is fast but may miss subtle meaning compared to larger models.

**2. OCR Not Included**

Scanned PDFs with images may not extract text properly.

**3. Basic Retrieval Only**

Uses similarity search only. No reranking or hybrid retrieval.

**4. No Conversation Memory**

Each query is independent.

**5. Hallucination Risk Reduced but Not Zero**

Prompt instructs Gemini to use retrieved context only, but model behavior can vary.

**6. File Size Constraints in Colab**

Very large document sets may exceed RAM limits.

---

# 10. Future Improvements

* Add FAISS / Qdrant support
* Add conversation memory
* Add reranking retriever
* Add OCR for scanned PDFs
* Add Streamlit deployment
* Add multi-user authentication

---

# 11. Conclusion

This project demonstrates the full RAG pipeline including ingestion, chunking, embeddings, retrieval, vector search, and grounded answer generation using Gemini. It is beginner-friendly while covering real-world AI engineering concepts.
