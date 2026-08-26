# Mini RAG Chatbot

A simple **Retrieval-Augmented Generation (RAG) chatbot** that answers questions using information retrieved from a PDF document.

This project was developed as part of AI training to understand and implement the complete RAG pipeline, including PDF text extraction, text chunking, embeddings, vector search, semantic retrieval, prompt engineering, LLM-based question answering, interactive chatbot functionality, and basic evaluation.

**Knowledge Source:** *A Beginner's Guide to Data & Analytics*

---

## 1. Project Overview

The chatbot retrieves relevant information from the PDF before sending the retrieved context to the language model.

The complete workflow is:

```text
PDF Document
     |
     v
PDF Text Extraction
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
OpenAI LLM
     |
     v
Final Answer
     |
     v
Source Pages
```

This approach helps generate answers grounded in the selected PDF rather than relying only on the LLM's general knowledge.

---

## 2. Objective

The objective of this project is to build a basic RAG chatbot that can:

- Read a PDF document
- Extract text from the PDF
- Split the extracted text into smaller chunks
- Generate embeddings for the chunks
- Store embeddings in a vector database
- Perform semantic search
- Retrieve relevant chunks
- Construct context from retrieved chunks
- Use prompt engineering
- Generate answers using an LLM
- Display source page numbers
- Handle questions whose answers are not available in the document
- Provide an interactive chatbot experience
- Evaluate the chatbot using different types of questions

---

## 3. Knowledge Source

The chatbot uses the following PDF as its knowledge source:

**A Beginner's Guide to Data & Analytics**

The document contains information related to topics such as:

- Data Science
- Data Analytics
- Data Literacy
- Data Ecosystem
- Machine Learning
- Data and analytical skills
- Data and analytics courses
- Business Analytics

---

## 4. Technologies Used

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| Jupyter Notebook | Development and execution environment |
| PyPDF | PDF text extraction |
| LangChain | Document processing, text splitting, vector search and LLM integration |
| Sentence Transformers | Embedding generation |
| FAISS | Vector database and similarity search |
| OpenAI | LLM-based answer generation |

---

## 5. Libraries Used

### PyPDF

Used to extract text from the PDF document page by page.

### LangChain

Used for:

- Document handling
- Text splitting
- Embedding integration
- Vector search
- LLM integration

### Sentence Transformers

Used to convert document chunks and user questions into numerical vectors.

The embedding model used is:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The model produces **384-dimensional embeddings**.

### FAISS

FAISS is used as the vector database for storing and searching document embeddings.

### OpenAI

An OpenAI language model is used to generate the final answer using the retrieved document context.

---

# 6. RAG Pipeline Implementation

## 6.1 PDF Text Extraction

The PDF is loaded using `PyPDF`.

The document contains:

```text
22 pages
```

The text is extracted page by page and stored as LangChain `Document` objects.

Each document contains page metadata.

Example:

```python
{
    "source": "sample_data/a-beginners-guide-to-data-and-analytics.pdf",
    "page": 4
}
```

Keeping page metadata allows the retrieved information to be traced back to the original PDF page.

---

## 6.2 Text Chunking

The extracted text is split into smaller chunks using:

```text
RecursiveCharacterTextSplitter
```

The configuration used is:

```text
chunk_size = 800
chunk_overlap = 100
```

The document was processed as:

```text
22 PDF pages
      |
      v
51 text chunks
```

### Why Chunking Is Required

Chunking allows the system to search smaller sections of the document instead of searching the entire document as one large block.

It helps with:

- More precise retrieval
- Efficient semantic search
- Better context selection
- Preserving useful information between adjacent sections

The chunk overlap helps preserve context between neighboring chunks.

---

## 6.3 Embeddings

Each document chunk is converted into a numerical vector using:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The embedding model generates:

```text
384-dimensional vectors
```

Conceptually:

```text
Text Chunk
    |
    v
Embedding Model
    |
    v
Numerical Vector
```

These vectors represent the semantic meaning of the text and allow the system to compare the meaning of a user's question with the meaning of document chunks.

---

## 6.4 FAISS Vector Database

The generated embeddings are stored in a FAISS vector index.

The project contains:

```text
51 chunks
     |
     v
51 embeddings
     |
     v
FAISS vector index
```

FAISS allows the chatbot to efficiently search for document chunks that are semantically similar to a user's question.

---

## 6.5 Semantic Search

When a user asks a question, the question is converted into an embedding.

The question embedding is compared with the embeddings stored in FAISS.

The system retrieves the top relevant chunks.

```text
User Question
     |
     v
Question Embedding
     |
     v
FAISS Semantic Search
     |
     v
Top 3 Relevant Chunks
     |
     v
Retrieved Context
```

For example:

**Question:**

```text
What is data analytics?
```

The system retrieves relevant content from the **Data Science vs. Data Analytics** section of the PDF.

---

## 6.6 Prompt Engineering

The retrieved chunks are passed to the LLM as context.

The prompt instructs the model to:

- Use only the provided context
- Answer the user's question
- Avoid using outside knowledge
- State when the answer cannot be found in the provided document

The prompt follows this structure:

```text
You are a helpful Data Analytics assistant.

Answer the user's question using ONLY the context provided below.

If the answer cannot be found in the context, say:
"I couldn't find the answer in the provided document."

Do not use outside knowledge.

Context:
{context}

Question:
{question}

Answer:
```

This helps keep the generated answer grounded in the retrieved document content.

---

## 6.7 LLM-Based Question Answering

The retrieved context and user question are passed to the OpenAI language model.

The LLM generates the final response using the retrieved context.

The complete process is:

```text
User Question
      |
      v
Question Embedding
      |
      v
FAISS Semantic Search
      |
      v
Relevant Chunks
      |
      v
Context Construction
      |
      v
Prompt
      |
      v
OpenAI LLM
      |
      v
Final Answer
      |
      v
Source Pages
```

---

# 7. Interactive Chatbot

The project includes an interactive chatbot that allows the user to continuously ask questions about the PDF.

The chatbot provides a command-line conversational interface.

Example:

```text
======================================================================
                     MINI RAG CHATBOT
======================================================================

Ask questions about the Data & Analytics PDF.
Type 'exit' or 'quit' to end the conversation.
======================================================================

You: What is data literacy?

Chatbot:
Data literacy is the ability to read, understand, and utilize data
in different ways (Page 7).

Source pages: [2, 7, 21]
```

The chatbot continues accepting questions until the user enters:

```text
exit
```

or:

```text
quit
```

---

# 8. Evaluation

The chatbot was evaluated using different types of questions to check:

- Retrieval quality
- Answer correctness
- Context relevance
- Source page tracking
- Document grounding
- Handling of questions outside the document

## Evaluation Questions

| Question | Type | Result |
|---|---|---|
| What is data literacy? | Direct concept | Correctly answered |
| How we can use data science? | Application | Correctly answered |
| What are the 4 types of analytics? | Information not available in document | Correctly stated that the answer was not found |
| What is data ecosystem? | Direct concept | Correctly answered |
| How to improve your skills? | Document-specific | Relevant information retrieved |
| Which data and analytics course is right for you? | Document-specific | Correctly answered |
| What is machine learning? | Technical concept | Correctly answered |

### Example: Information Not Found

**Question:**

```text
What are the 4 types of analytics?
```

**Chatbot response:**

```text
I could not find the answer in the provided document.
```

This demonstrates that the chatbot can identify when the requested information is not available in the provided document context.

---

# 9. Key Parameters

| Parameter | Value |
|---|---|
| PDF pages | 22 |
| Number of chunks | 51 |
| Chunk size | 800 characters |
| Chunk overlap | 100 characters |
| Embedding model | all-MiniLM-L6-v2 |
| Embedding dimensions | 384 |
| Vector database | FAISS |
| Retrieved chunks | Top 3 |
| LLM temperature | 0 |
