# Field Guides

**A multilingual AI assistant for technical documentation.**

Field Guides turns complex technical manuals into a conversational knowledge system. Users can ask questions in their preferred language and receive answers grounded in the original documentation, with citations that lead back to the relevant source pages.

**Status:** Private, actively developed
**Source:** Closed source — Field Guides is an actively developed commercial product
**Website:** https://fieldguides.work

> This repository is a technical showcase of Field Guides. It contains product documentation, architecture overviews, and demonstrations, but not the application's source code.

## The Problem

Technical manuals contain the answers technicians need, but finding them quickly can be difficult.

Documentation can span hundreds or thousands of pages, terminology varies between manufacturers, and language can become another barrier between a technician and the information they need.

Field Guides is an experiment in making that knowledge easier to access without separating answers from their original source.

Instead of replacing technical documentation, it provides a conversational layer on top of it.

## What It Does

Field Guides allows users to:

* Ask natural-language questions about technical manuals
* Receive answers grounded in retrieved source material
* Trace answers back to the relevant pages
* Ask follow-up questions conversationally
* Interact with documentation across multiple languages
* Navigate complex manuals without knowing the exact terminology used by the manufacturer

## Engineering Documentation

- [Architecture](docs/architecture.md)
- [Retrieval pipeline](docs/retrieval-pipeline.md)
- [Multilingual interaction](docs/multilingual.md)

## How It Works

At a high level:

Technical Manuals
→ Document Processing
→ Structured Content + Metadata
→ PostgreSQL + pgvector
→ Query Processing
→ Retrieval
→ Reranking + Context Expansion
→ LLM
→ Grounded Answer + Citations
→ Multilingual Output

The system is designed around retrieval rather than asking a language model to answer technical questions from its own knowledge.

## Document Processing

Technical manuals are processed into structured content while preserving information needed later for retrieval and citation.

Metadata can include:

* Manual
* Section
* Region
* Page number
* Figure presence
* Table presence

This allows retrieved information to retain a relationship with the original document instead of becoming anonymous text chunks.

## Retrieval Pipeline

Field Guides uses a multi-stage retrieval pipeline rather than relying entirely on a single vector similarity search.

### Query processing

The user's question can be rewritten or normalized to improve retrieval while preserving the original intent.

### Candidate retrieval

Relevant sections are retrieved from the document knowledge base.

### Reranking

Retrieved candidates are reranked to prioritize the content most likely to answer the question.

### Context expansion

Additional surrounding information can be retrieved when necessary so that answers are not generated from isolated fragments.

### Generation

The selected context is provided to the language model to produce the final response.

The objective is simple: the model should answer from the manual rather than from what it happens to know.

## Citations

Citation quality is an important part of the system.

Document chunks retain metadata connecting retrieved content to the original manual and page.

When an answer is generated, the interface can direct the user back to the relevant source material.

This creates a workflow of:

Question → Answer → Evidence → Original Manual

rather than treating the generated answer as the final authority.

## Multilingual Interaction

Language should not determine whether someone can effectively use technical documentation.

Field Guides supports asking questions and receiving responses across multiple languages while keeping the underlying answer grounded in the source documentation.

The system separates retrieval and language presentation so that translation does not replace the original technical source.

## Technology

### AI & Retrieval

* Haystack
* LLM APIs
* Embeddings
* Vector retrieval
* Reranking
* Query rewriting
* Context expansion

### Document Processing

* Docling
* Structured metadata extraction
* Page-aware document processing

### Data

* PostgreSQL
* pgvector

### Application & Infrastructure

* Python
* Docker
* Self-hosted deployment

## Engineering Principles

### Ground answers in evidence

Technical answers should be derived from the documentation available to the system.

### Preserve the source

Generated answers should make it easy to return to the original manual.

### Retrieval quality matters

RAG is more than putting documents into a vector database. Query processing, metadata, reranking, context selection, and document structure all influence the final answer.

### Language should be an interface, not a limitation

Users should be able to interact with technical knowledge in the language most useful to them without losing the connection to the original documentation.

### Build for real documents

The system is designed around the realities of technical manuals: long documents, tables, figures, sections, inconsistent formatting, and specialized terminology.

## Current Development

Current work focuses on improving:

* Retrieval quality
* Reranking
* Context selection
* Multilingual interaction
* Citation accuracy
* Real-world document ingestion
* Production reliability

## Demo

Product screenshots and demonstrations will be added here to show the complete workflow from document ingestion through question answering and source citation.

For more information, visit https://fieldguides.work.

## About This Repository

Field Guides is an actively developed commercial project and its application source code is intentionally private.

This repository exists to document the product, architecture, engineering decisions, and selected technical challenges behind it.

It does **not** contain the production application or proprietary implementation.

