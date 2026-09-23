# Retrieval pipeline

Field Guides uses a multi-stage retrieval pipeline to assemble source evidence before generating a response. This document describes the stages and design objectives; it does not disclose proprietary prompts, ranking parameters, or implementation code.

## 1. Query processing

A question can be rewritten or normalized to improve retrieval while preserving the user's original intent. This is useful when the user's phrasing differs from terminology in a manual. The design objective is to improve the search representation without silently changing the question.

## 2. Candidate retrieval

Relevant sections are retrieved from the document knowledge base, which uses PostgreSQL and pgvector. Content remains associated with document metadata. The public description does not specify a fixed candidate count, similarity metric, or hybrid-search algorithm.

## 3. Reranking

Candidates are reranked to prioritize content likely to answer the question. Retrieval supplies possible evidence; reranking refines the selection. The objective is relevance to the question, rather than assuming that vector similarity alone identifies sufficient context.

## 4. Context expansion

Surrounding information can be retrieved where necessary. Technical instructions may depend on section context, a nearby explanation, or material associated with a table or figure. Expansion aims to avoid interpreting isolated fragments while keeping the selected evidence relevant. The exact expansion rules are intentionally omitted.

## 5. Generation

Selected context is provided to the language model for answer generation. The objective is an answer derived from the available manual, rather than unsupported model knowledge. Retrieval quality and context selection therefore constrain what the system can answer reliably.

## 6. Citation relationship

Document chunks retain metadata connecting content to its manual and page. The interface can direct the user from the response back to that source. Citation accuracy requires both relevant evidence and a correct source location; a plausible answer with an unrelated citation does not meet that objective.

## Design objectives and ongoing work

- Preserve intent through query processing.
- Retrieve useful evidence despite differences in terminology.
- Prioritize evidence that answers the question.
- Retain enough surrounding context to interpret technical passages.
- Keep the response connected to the original manual and page.
- Improve ingestion, reranking, context selection, and citation accuracy through continued development.

These are design objectives, not published benchmark results or guarantees. No evaluation scores or production reliability metrics are asserted here.

See [architecture](architecture.md) for the overall flow and [multilingual interaction](multilingual.md) for the relationship between grounding and response language.
