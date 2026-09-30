# Lakshmy-Agentic-RAG

An **Agentic Retrieval-Augmented Generation (RAG) system** built with **LangGraph, LangChain, Qdrant, Google Gemini, and Hugging Face embeddings**.

This project implements an agentic RAG pipeline capable of processing PDF documents, retrieving relevant information using hybrid search, and generating grounded answers through an intelligent LangGraph workflow.

> **Project note:** This repository is a customized and extended version of the original [Agentic RAG for Dummies](https://github.com/GiovanniPasq/agentic-rag-for-dummies) project. The original project provided the foundation for the RAG architecture. This version adapts the LLM layer to **Google Gemini** and the dense embedding model to **all-MiniLM-L6-v2**, along with the corresponding dependency and configuration changes.

---

## 🚀 Features

- 📄 **PDF document ingestion**
- 🔄 **PDF → Markdown conversion**
- ✂️ **Parent-child document chunking**
- 🔎 **Hybrid retrieval**
  - Dense vector search
  - BM25 sparse retrieval
- 🗄️ **Qdrant vector database**
- 🤖 **Agentic workflow using LangGraph**
- 🔀 **Parallel retrieval**
- 🧠 **Query clarification**
- 🔍 **Self-correction and retrieval refinement**
- 📚 **Parent-document context retrieval**
- 💬 **Google Gemini-powered answer generation**
- 🖥️ **Interactive Gradio interface**
- 🧩 **Local Hugging Face embeddings**
- 🔐 Environment-based API key configuration

---

# 🏗️ Architecture

The system follows an agentic RAG architecture:

```text
                    ┌──────────────────┐
                    │   PDF Document   │
                    └────────┬─────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ PDF → Markdown      │
                  │ Conversion          │
                  └─────────┬───────────┘
                            │
                            ▼
                  ┌─────────────────────┐
                  │ Parent / Child      │
                  │ Chunking            │
                  └─────────┬───────────┘
                            │
                ┌───────────┴───────────┐
                ▼                       ▼
       ┌────────────────┐      ┌────────────────┐
       │ Dense Retrieval│      │ BM25 Retrieval│
       │ MiniLM         │      │ Sparse Search  │
       └───────┬────────┘      └───────┬────────┘
               │                       │
               └───────────┬───────────┘
                           ▼
                  ┌───────────────────┐
                  │ Qdrant Vector DB  │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │ LangGraph Agent   │
                  │ Workflow          │
                  └─────────┬─────────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
        Clarification   Retrieval      Self-Correction
                         / Search
             └──────────────┼──────────────┘
                            ▼
                  ┌───────────────────┐
                  │ Context           │
                  │ Aggregation       │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │ Google Gemini     │
                  │ Answer Generation │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │ Gradio Interface  │
                  └───────────────────┘
```

---

# 🧠 How the Agentic RAG Pipeline Works

### 1. Document ingestion

PDF documents are uploaded through the application.

The system converts the PDF into Markdown while preserving useful document structure.

### 2. Parent-child chunking

Documents are divided into:

- **Parent chunks** — larger contextual sections
- **Child chunks** — smaller searchable units

The child chunks improve retrieval precision while parent chunks provide broader context during answer generation.

### 3. Hybrid retrieval

The system combines two retrieval approaches.

#### Dense retrieval

Dense embeddings are generated using:

```text
sentence-transformers/all-MiniLM-L6-v2
```

These embeddings allow the system to retrieve semantically similar content.

#### Sparse retrieval

BM25 is used for lexical retrieval:

```text
Qdrant/bm25
```

Combining dense and sparse retrieval helps handle both semantic similarity and exact keyword matching.

### 4. Agentic reasoning

Retrieved information is passed through a **LangGraph-based agent workflow**.

The workflow can perform tasks such as:

- understanding the user's query
- determining whether clarification is required
- retrieving relevant document chunks
- performing retrieval in parallel
- refining retrieval when necessary
- aggregating the retrieved context
- generating a final response

### 5. Answer generation

The final response is generated using:

```text
Google Gemini
gemini-2.5-flash
```

The model receives the retrieved document context and generates an answer grounded in the available information.

---

# 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| LangChain | LLM and retrieval framework |
| LangGraph | Agentic workflow orchestration |
| Google Gemini | LLM for answer generation |
| Hugging Face | Dense embedding model |
| all-MiniLM-L6-v2 | Dense embeddings |
| Qdrant | Vector database |
| BM25 | Sparse retrieval |
| Gradio | Web interface |
| PyMuPDF / pymupdf4llm | PDF processing |
| Sentence Transformers | Embedding generation |

---

# 📁 Project Structure

```text
Lakshmy-Agentic-RAG/
│
├── project/
│   ├── app.py
│   ├── config.py
│   ├── .env.example
│   │
│   ├── core/
│   │   └── rag_system.py
│   │
│   ├── db/
│   │   ├── vector_db_manager.py
│   │   └── parent_store_manager.py
│   │
│   ├── agents/
│   ├── tools/
│   ├── processors/
│   └── ui/
│
├── models/
├── requirements.txt
├── .gitignore
└── README.md
```

> The exact contents of individual directories may vary as the project evolves.

---

# ⚙️ Installation

## 1. Clone the repository

```bash
git clone https://github.com/lakshmykaipraa/Lakshmy-Agentic-RAG.git
cd Lakshmy-Agentic-RAG
```

---

## 2. Create a virtual environment

### Windows

```powershell
python -m venv .venv
```

Activate it:

```powershell
.\.venv\Scripts\Activate.ps1
```

### Linux / macOS

```bash
python -m venv .venv
source .venv/bin/activate
```

---

## 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

# 🔑 Configuration

This project uses the Google Gemini API.

Create a file:

```text
project/.env
```

Add your API key:

```env
GOOGLE_API_KEY=your-gemini-api-key
```

You can use the provided template:

```text
project/.env.example
```

### Important

Never commit your real API key to GitHub.

The `.env` file is included in `.gitignore`.

---

# ▶️ Running the Application

From the project root:

```powershell
python .\project\app.py
```

The Gradio application will start locally.

Open the URL displayed in the terminal, typically:

```text
http://127.0.0.1:7860
```

---

# 💬 Example Usage

After launching the application:

1. Upload a PDF document.
2. Allow the system to process the document.
3. Enter a question about the uploaded document.
4. The agent performs retrieval.
5. Relevant document context is collected.
6. Gemini generates the final answer.

Example:

```text
User:
What are the main principles discussed in the document?

Agentic RAG:
[Retrieves relevant document chunks]
        ↓
[Aggregates context]
        ↓
[Gemini generates grounded answer]
```

---

# 🔍 Retrieval Strategy

This project uses **hybrid retrieval** rather than relying on a single retrieval method.

### Dense retrieval

Uses semantic embeddings:

```text
all-MiniLM-L6-v2
```

Useful when the query and document use different wording but have similar meaning.

### Sparse retrieval

Uses:

```text
BM25
```

Useful when exact terms, names, or keywords are important.

### Why hybrid retrieval?

The combination allows the system to use both:

```text
Semantic similarity
        +
Keyword matching
        ↓
Better document retrieval
```

---

# 🤖 Why Agentic RAG?

Traditional RAG generally follows:

```text
Query
  ↓
Retrieve
  ↓
Generate
```

This project introduces an agentic layer:

```text
Query
  ↓
Understand Query
  ↓
Clarify if necessary
  ↓
Retrieve
  ↓
Evaluate / Refine
  ↓
Aggregate Context
  ↓
Generate Answer
```

This allows the retrieval process to be more adaptive instead of treating every question as a simple search operation.

---

# 🔧 Customizations in This Version

This repository includes several changes from the original project.

### 1. Google Gemini integration

The original Ollama-based LLM integration was replaced with:

```python
from langchain_google_genai import ChatGoogleGenerativeAI
```

The configured model is:

```text
gemini-2.5-flash
```

This allows the application to use Google's Gemini API instead of requiring a large local Ollama model.

---

### 2. Dense embedding model

The original Qwen embedding model was replaced with:

```text
sentence-transformers/all-MiniLM-L6-v2
```

This provides a lightweight embedding model suitable for local development.

---

### 3. Updated dependencies

The project includes:

```text
langchain-google-genai
```

to support Gemini integration.

---

### 4. Environment configuration

Gemini API configuration is provided through:

```text
GOOGLE_API_KEY
```

The API key is kept outside the repository using `.env`.

---

# 📊 Current Configuration

```text
LLM
└── Google Gemini
    └── gemini-2.5-flash

Dense Embeddings
└── sentence-transformers/all-MiniLM-L6-v2

Sparse Retrieval
└── Qdrant BM25

Vector Database
└── Qdrant

Agent Framework
└── LangGraph

UI
└── Gradio
```

---

# 🔐 Security

API keys and other sensitive configuration should never be committed to GitHub.

The project uses:

```text
project/.env
```

for local secrets and:

```text
project/.env.example
```

as a safe configuration template.

Before pushing changes, verify:

```bash
git status
```

and ensure that `.env` is not being tracked.

---

# 🚧 Future Improvements

Possible future improvements include:

- [ ] Add support for additional LLM providers
- [ ] Add conversation memory
- [ ] Improve retrieval evaluation
- [ ] Add RAG evaluation metrics
- [ ] Add document citation support
- [ ] Improve query rewriting
- [ ] Add configurable retrieval parameters
- [ ] Add streaming responses
- [ ] Add authentication for deployment
- [ ] Deploy the application to a cloud platform
- [ ] Add automated tests
- [ ] Add CI/CD pipeline
- [ ] Add observability and tracing

---

# 📌 Learning Outcomes

This project provides hands-on experience with:

- Retrieval-Augmented Generation
- Agentic AI systems
- LangGraph workflows
- LangChain
- Hybrid information retrieval
- Vector databases
- Dense embeddings
- Sparse retrieval
- Document processing
- LLM integration
- Prompt-driven reasoning
- Gradio application development
- Environment-based configuration

---

# 🙌 Acknowledgements

This project is based on and customized from:

**Agentic RAG for Dummies**  
Original repository:  
https://github.com/GiovanniPasq/agentic-rag-for-dummies

The original project provided the foundation for the Agentic RAG architecture and implementation.

This repository focuses on adapting that foundation for:

- Google Gemini-based generation
- Lightweight MiniLM embeddings
- Personal experimentation and development

---

# 👩‍💻 Author

**Lakshmy Kaipra**

GitHub:  
https://github.com/lakshmykaipraa

---

# 📄 License

Please refer to the original project's license and retain the applicable license and attribution requirements when modifying or redistributing this project.
