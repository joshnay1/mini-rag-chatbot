# Mini RAG Chatbot

## Project Overview

This project implements a simple Retrieval-Augmented Generation (RAG)
chatbot that answers questions using information from a PDF document.

The project was developed as part of AI training to understand the
complete RAG pipeline, including PDF extraction, text chunking,
embeddings, vector search, context retrieval, prompt engineering,
and LLM-based question answering.

The knowledge source used in this project is:

**A Beginner's Guide to Data & Analytics**

The chatbot retrieves relevant information from the PDF before sending
the retrieved context to the language model. This helps the model
generate answers grounded in the provided document.

---

## Objective

The objective of this project is to build a basic RAG chatbot that can:

1. Read a PDF document
2. Extract text from the PDF
3. Split the extracted text into smaller chunks
4. Generate embeddings for the chunks
5. Store embeddings in a vector database
6. Retrieve relevant chunks based on a user question
7. Pass the retrieved context to an LLM
8. Generate a final answer based on the retrieved context

---

## RAG Architecture

The implemented pipeline is:

PDF
↓
PDF Text Extraction
↓
LangChain Documents
↓
Text Chunking
↓
Embeddings
↓
FAISS Vector Database
↓
Semantic Search
↓
Relevant Context
↓
Prompt Engineering
↓
LLM
↓
Final Answer


---

## Technologies Used

- Python
- Jupyter Notebook
- PyPDF
- LangChain
- Hugging Face Sentence Transformers
- FAISS
- OpenAI API
- OpenAI GPT model

---

## Libraries Used

### PyPDF

Used to extract text from the PDF document.

### LangChain

Used for document handling, text splitting, vector search,
and LLM integration.

### Sentence Transformers

Used to generate embeddings for the document chunks.

The embedding model used is:

`sentence-transformers/all-MiniLM-L6-v2`

### FAISS

FAISS is used as the vector database for storing and searching
the document embeddings.

### OpenAI

An OpenAI language model is used to generate the final answer
from the retrieved document context.

---

## Project Structure

```text
mini-rag-chatbot/
│
├── Mini_RAG_Chatbot.ipynb
├── README.md
├── requirements.txt
│
├── sample_data/
│   └── a-beginners-guide-to-data-and-analytics.pdf
│
└── screenshots/

##File Description
File/Folder	Description
Mini_RAG_Chatbot.ipynb	Main Jupyter Notebook containing the complete RAG pipeline
README.md	Project documentation
requirements.txt	Python dependencies
sample_data/	Contains the PDF used as the knowledge source
screenshots/	Screenshots showing the project execution and results

##RAG Pipeline Implementation
#1. PDF Text Extraction

The PDF is loaded using PyPDF.

The document contains 22 pages.

The text is extracted page by page and stored along with
page metadata.

Example metadata:

{
    "source": "sample_data/a-beginners-guide-to-data-and-analytics.pdf",
    "page": 4
}

Keeping page metadata allows the retrieved information to be
traced back to the original PDF page.

#2. Text Chunking

The extracted pages are split into smaller chunks using:

RecursiveCharacterTextSplitter

The configuration used is:

chunk_size = 800
chunk_overlap = 100

The PDF produced:

22 pages
↓
51 chunks

Chunking is necessary because smaller pieces of text can be
represented and retrieved more precisely than an entire document.

The overlap helps preserve context between adjacent chunks.

#3. Embeddings

Each document chunk is converted into a numerical vector using:

sentence-transformers/all-MiniLM-L6-v2

The model produces a:

384-dimensional vector

These vectors represent the semantic meaning of the text.

This allows the system to compare the meaning of a user question
with the meaning of the document chunks.

#4. Vector Database

FAISS is used to store and search the embeddings.

The project contains:

51 chunks
↓
51 embeddings
↓
FAISS vector index

FAISS allows the system to find document chunks that are
semantically similar to a user's question.

#5. Semantic Search

When a user asks a question, the question is converted into
an embedding.

The embedding is compared with the vectors stored in FAISS.

The system retrieves the top relevant chunks.

For example:

Question:

What is data analytics?

        ↓

Semantic Search

        ↓

Top 3 relevant chunks

        ↓

Retrieved Context

For this question, the retrieved content included the section
discussing the difference between Data Science and Data Analytics.

#6. Prompt Engineering

The retrieved chunks are passed to the LLM as context.

The prompt instructs the model to:

Use only the retrieved context
Answer the user's question
Avoid using outside knowledge
State when the answer cannot be found in the document

The prompt follows this structure:

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

This helps keep the generated answer grounded in the source document.

#7. LLM-Based Question Answering

The retrieved context and user question are passed to the
OpenAI language model.

The model generates the final response based on the retrieved
context.

The complete process is:

User Question
      ↓
Question Embedding
      ↓
FAISS Semantic Search
      ↓
Relevant Chunks
      ↓
Context Construction
      ↓
Prompt
      ↓
LLM
      ↓
Final Answer
Example
Question
What is data analytics?
Retrieved Information

The system retrieves relevant chunks from the PDF, including
the Data Science vs. Data Analytics section.

Generated Answer

The chatbot explains that data analytics involves analyzing
data to answer questions, extract insights, and identify trends.

#Key Parameters
Parameter	Value
PDF pages	22
Number of chunks	51
Chunk size	800 characters
Chunk overlap	100 characters
Embedding model	all-MiniLM-L6-v2
Embedding dimensions	384
Vector database	FAISS
Retrieved chunks	Top 3
LLM temperature	0

##How to Run the Project
1. Clone the repository
git clone <your-github-repository-url>
cd mini-rag-chatbot
2. Create a virtual environment
python3 -m venv rag_env

Activate it:

source rag_env/bin/activate
3. Install dependencies
pip install -r requirements.txt
4. Set the OpenAI API Key

The API key should NOT be stored directly inside the notebook.

Set it as an environment variable.

Linux/macOS:

export OPENAI_API_KEY="your-api-key"

Verify that it is available:

python -c "import os; print(bool(os.getenv('OPENAI_API_KEY')))"

Expected output:

True
5. Start Jupyter
jupyter notebook

Open:

Mini_RAG_Chatbot.ipynb

Run the notebook cells from top to bottom.

##Security

The OpenAI API key is stored as an environment variable and is
not included in the notebook.

Never commit an API key to GitHub.

Before pushing the project, verify that no secret keys are present
in:

Jupyter Notebook
Python files
README
Configuration files
Learning Outcomes

Through this project, the following RAG concepts were practiced:

PDF text extraction
Document preprocessing
Text chunking
Chunk overlap
Embeddings
Vector databases
FAISS similarity search
Semantic retrieval
Context construction
Prompt engineering
LLM integration
Retrieval-Augmented Generation

##Conclusion

This project demonstrates a basic end-to-end Retrieval-Augmented
Generation pipeline.

Instead of directly asking an LLM a question, the system first
retrieves relevant information from the PDF and then provides that
information to the LLM as context.

This allows the chatbot to generate answers based on the selected
knowledge source.
