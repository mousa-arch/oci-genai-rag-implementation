# OCI GenAI & 23ai RAG Architecture

## Overview
This repository outlines the architectural framework for a secure Retrieval-Augmented Generation (RAG) implementation on Oracle Cloud Infrastructure (OCI). This PoC demonstrates how to ground Generative AI models in enterprise-specific data stored in Oracle 23ai, significantly reducing hallucinations and providing context-aware answers for internal enterprise knowledge bases.

## Architecture

```mermaid
graph TD
    %% Define Nodes
    User((Enterprise User))
    UI[Frontend Interface<br>Oracle APEX / Streamlit]
    OCI_GenAI[OCI Generative AI Service<br>LLM]
    Embed[Embedding Model<br>Cohere/OCI]
    VectorDB[(Oracle Database 23ai<br>AI Vector Search)]
    Docs[[Enterprise Knowledge Base<br>PDFs, Docs, WADs]]
    
    %% Define Workflow
    Docs -->|Ingestion & Chunking| Embed
    Embed -->|Store Vectors| VectorDB
    
    User -->|1. User Query| UI
    UI -->|2. Semantic Search| VectorDB
    VectorDB -->|3. Retrieve Context| UI
    UI -->|4. Prompt + Context| OCI_GenAI
    OCI_GenAI -->|5. Grounded Response| UI
    UI -.->|6. Final Answer| User

    %% Styling
    style VectorDB fill:#f9d0c4,stroke:#c82124,stroke-width:2px
    style OCI_GenAI fill:#e1f5fe,stroke:#0288d1,stroke-width:2px
    style Docs fill:#fff3e0,stroke:#e65100,stroke-width:2px

Frontend/Interface: Oracle APEX / Streamlit

AI Orchestration: OCI Generative AI Service

Vector Database: Oracle Database 23ai (AI Vector Search)

Data Ingestion: LangChain, Python

Technical Approach
Vector Embedding: Utilizing 23ai native vector capabilities to store and index unstructured enterprise documents.

Context Retrieval: Implementing semantic similarity search to fetch relevant chunks before passing to the LLM.

Prompt Engineering: Configuring system prompts within OCI GenAI to restrict model responses to retrieved enterprise context.

Business Value
Data Sovereignty: All data remains within OCI boundaries, maintaining compliance.

Accuracy: RAG-based search leverages specific enterprise documentation rather than generalized public training data.

Created by Mohamed Mousa | Lead Cloud Architect
