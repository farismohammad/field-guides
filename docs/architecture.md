# Architecture

Field Guides is a private commercial application that provides multilingual question answering over technical manuals. This document describes its high-level engineering approach, not its proprietary implementation.

## System goals

- Make long technical manuals easier to query without requiring exact manufacturer terminology.
- Ground answers in retrieved documentation and retain a route to the original page.
- Support conversational follow-up and multilingual interaction.
- Preserve useful document structure when processing tables, figures, sections, and text.

## Components and information flow

| Stage | Responsibility | Information carried forward |
| --- | --- | --- |
| Document processing | Convert manuals into structured content using Docling and metadata extraction | Content and its relationship to the manual |
| Knowledge storage | Store content and retrieval representations using PostgreSQL and pgvector | Content, embeddings, and source metadata |
| Query processing | Rewrite or normalize questions where useful | The user's information need |
| Retrieval and reranking | Find candidates, then prioritize relevant evidence | Selected source passages |
| Context expansion | Add surrounding information where necessary | Context sufficient to interpret a passage |
| Answer generation | Use an LLM with the selected evidence | An answer grounded in the manual |
| Citation and language presentation | Connect the response to source pages and the user's language | Answer, citations, and access to the original documentation |

The application uses Python, Haystack, LLM APIs, and Docker in a self-hosted deployment. This overview does not specify service boundaries, model vendors, deployment topology, or private configuration.

## Two related flows

Ingestion prepares structured content and metadata before that material can be retrieved. Question answering uses the prepared knowledge base to select evidence for a particular question. These are logical flows; this document makes no claim about queues, scheduling, or process layout.

```text
Manuals → document processing → structured content and metadata
        → PostgreSQL + pgvector

Question → query processing → retrieval → reranking → context expansion
         → LLM → grounded answer + citations → multilingual presentation
```

## Why metadata and citations matter

Metadata can include the manual, section, region, page number, and whether figures or tables are present. It allows retrieved content to retain its relationship to the document. A useful passage without source identity is difficult to verify; a page reference without the supporting passage is not enough to establish an answer.

The intended review path is **question → answer → evidence → original manual**. Citations let the user inspect the source and surrounding context. They support verification rather than guaranteeing that every generated statement is correct.

## Scope and omissions

Exact schemas, prompts, model choices, retrieval parameters, access-control implementation, and operational configuration are intentionally omitted. This repository contains no application source, customer manuals, credentials, or production configuration. See the [retrieval pipeline](retrieval-pipeline.md) and [multilingual interaction](multilingual.md) documents for the public design overview.
