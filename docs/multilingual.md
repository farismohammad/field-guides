# Multilingual interaction

Technical documentation is useful only if the person doing the work can find and understand the relevant information. Field Guides supports questions and responses in multiple languages while keeping the answer connected to the original manual.

## Source grounding and response language

The source documentation supplies the evidence. The user's preferred language controls how that evidence is presented. These are separate responsibilities: translation should not replace the source or become an independent authority for technical instructions.

At a high level, the system retrieves relevant source content, uses it to ground the answer, and presents the response in the user's language with citations back to the original documentation. This is a conceptual separation, not a claim that a particular translation service or sequence of model calls is implemented.

## Translation considerations

The following are engineering considerations for maintaining answer quality, not claims of implemented safeguards:

- Preserve part numbers, units, error codes, model identifiers, and other exact technical references.
- Avoid changing the meaning of warnings, conditions, negation, and ordered instructions.
- Keep terminology consistent across a question and its follow-up.
- Distinguish a translated explanation from a quotation in the original manual.
- Make the original page available when terminology is ambiguous or a diagram provides essential context.

## Follow-up questions

Conversational interaction should preserve the relationship between the current question, the relevant source evidence, and the answer language. A change of language must not be treated as a reason to change the technical source. The private conversation-state and query-processing implementation is intentionally omitted.

## Returning to the documentation

The intended workflow remains **question → answer → evidence → original manual**, regardless of response language. Page-level source references allow a technician to compare the explanation with the documentation and inspect surrounding material.

Multilingual quality and citation accuracy remain areas of active development. This showcase does not assert measured translation accuracy or equal performance across languages and document types.

See the [retrieval pipeline](retrieval-pipeline.md) and [architecture](architecture.md) for the related design overview.
