# Support-KB-Agent

The Support KB Agent is a minimal, end-to-end Retrieval-Augmented Generation (RAG) system that answers user questions based on a small support knowledge base (PDFs / markdown documents).

This project demonstrates:

1. How to ingest documents and build a RAG pipeline
2. How to orchestrate reasoning steps using LangGraph
3. How to expose retrieval and metadata as MCP-style tools
4. How to generate grounded, source-aware answers using Hugging Face models

# Architecture

```
User Question
     ↓
LangGraph Orchestration
     ↓
[Retrieve] → [Draft Answer] → [Cite Sources] → [Final Answer]
     ↓
Vector Store (FAISS + HF Embeddings)
     ↓
Knowledge Base (PDFs / Markdown)
```

# Tech Stack

```
| Component      | Technology                             |
| -------------- | -------------------------------------- |
| Language       | Python 3.10+                           |
| Embeddings     | Hugging Face (`BAAI/bge-base-en-v1.5`) |
| Vector Store   | FAISS                                  |
| LLM            | Hugging Face (`google/flan-t5-base`)   |
| Orchestration  | LangGraph                              |
| Tool Interface | MCP-style tools (Python functions)     |
| Environment    | Google Colab / Local                   |
```

# Project Structure

```
support-kb-agent/
│
├── data/
│   └── faq.pdf                  # Knowledge base documents
│
├── ingest.py                    # Document ingestion + indexing
├── tools.py                     # MCP-style tools (retrieval, metadata)
├── graph.py                     # LangGraph workflow definition
├── main.py                      # Entry point to run the agent
│
├── vectorstore/                 # FAISS index (auto-generated)
│
├── requirements.txt
└── README.md
```

# Setup Instructions

1 Install Dependencies

```
pip install -r requirements.txt
```

requirements.txt

```
langchain
langchain-community
langgraph
transformers
sentence-transformers
faiss-cpu
pypdf
torch
```

2 Ingest Documents (Build Vector Store)

```
python ingest.py
```

3 Run the Agent

```
python main.py
```

4 Sample Query & Output

<img width="1510" height="473" alt="200" src="https://github.com/user-attachments/assets/c89556d9-9524-4fdd-9e2f-edf822e4d08f" />




# MCP-Style Tools

The agent uses MCP-inspired tools implemented as Python functions:
```
| Tool                   | Description                  |
| ---------------------- | ---------------------------- |
| `search_kb(question)`  | Retrieves relevant documents |
| `get_metadata(docs)`   | Extracts document sources    |
| `format_context(docs)` | Prepares context for the LLM |
```

These tools are injected into the LangGraph flow, simulating how MCP provides tools and context to agents.

# LangGraph Flow

The reasoning flow is explicitly modeled using LangGraph:

Retrieve Node
Fetch relevant documents from FAISS

Draft Answer Node
Generate an answer using retrieved context

Cite Sources Node
Attach document references

Final Node
Produce the final grounded response

This makes the agent deterministic, debuggable, and extensible.
