# Mini RAG Chatbot

A simple **Retrieval-Augmented Generation (RAG) chatbot** that answers questions using information retrieved from a PDF document.

This project was developed as part of AI training to understand and implement the complete RAG pipeline, including PDF text extraction, text chunking, embeddings, vector search, semantic retrieval, prompt engineering, LLM-based question answering, interactive chatbot functionality, and basic evaluation.

The knowledge source used in this project is:

**A Beginner's Guide to Data & Analytics**

---

## Project Overview

The chatbot retrieves relevant information from the PDF before sending the retrieved context to the language model.

The complete workflow is:

1. Read the PDF
2. Extract text page by page
3. Split the text into smaller chunks
4. Generate embeddings for each chunk
5. Store embeddings in a FAISS vector database
6. Perform semantic search for a user's question
7. Retrieve the most relevant chunks
8. Construct a context from the retrieved chunks
9. Send the context and question to the LLM
10. Generate a final answer
11. Display the source pages used for the answer

This approach helps generate answers that are grounded in the selected PDF rather than relying only on the LLM's general knowledge.

---

## Objective

The objective of this project is to build a basic RAG chatbot that can:

- Read a PDF document
- Extract PDF text
- Split text into smaller chunks
- Generate embeddings
- Store embeddings in a vector database
- Perform semantic search
- Retrieve relevant context
- Use prompt engineering
- Generate answers using an LLM
- Display source page numbers
- Handle questions whose answers are not available in the document
- Provide an interactive chatbot experience
- Evaluate the chatbot using different types of questions

---

## RAG Architecture

```text
PDF Document
     |
     v
PDF Text Extraction
     |
     v
LangChain Documents
     |
     v
Text Chunking
     |
     v
Embeddings
     |
     v
FAISS Vector Database
     |
     v
Semantic Search
     |
     v
Relevant Chunks
     |
     v
Context Construction
     |
     v
Prompt Engineering
     |
     v
LLM
     |
     v
Final Answer
     |
     v
Source Pages
