# ISE Documents

> Note: The problem identification, key concepts, and final decisions in this repository are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

This directory is a collection of documents to organize thoughts on Intent-Synchronized Engineering (ISE).

ISE is a term proposed by the author in these papers and is not an existing standardized development methodology or a widely agreed-upon industry term. At its core is the idea of continuously synchronizing artifacts such as code, tests, specifications, designs, Issues, ADRs, and operation records to a common intent.

Intent itself exists only in people's heads. Artifacts are representations that externalize that intent, and information may be lost in the process of externalization. ISE keeps synchronizing these representations, which people settle through negotiation, with the implementation (Reality).

## Reading Order

For the background and the proposal of ISE, follow the reading order in [philosophies/README](../philosophies/README.md).

The following documents are specific to ISE:

- [Document Drift and Intent Decay: Why Intent Must Be Synchronized](foundations/01_document_drift_and_intent_decay.md) (a supplement to `philosophies/04`)
- [Glossary](glossary.md)

## Positioning of Each Document

For the positioning of 01 to 06, see [philosophies/README](../philosophies/README.md).

### foundations: Theoretical Background

`foundations/01_document_drift_and_intent_decay.md` is a supplement to 04.

Why is intent lost? Why does document drift occur? Why is intent synchronization via writing powerful but difficult? Why does AI not exclude people who are not good at writing, but rather can assist them?

This document covers the externalization of intent, text-centricity, reconstruction of memory, attenuation of intent retention, common ground, cognitive load, and time lag between artifacts.

## Scope of Responsibility for ISE

ISE mainly deals with intent synchronization on the implementation side.

```text
ISE:
  accepted intent
    -> synchronization to spec / design / code / test / docs / operation
```

ISE's primary responsibility is not creating intent from ambiguous requirements, making value judgments, guaranteeing deadlines, or absorbing ambiguity on the management side.

The area of creating implementable intent or issues from ambiguous requirements, observations, pain, dissatisfaction, operational signals, or market signals should be handled by IGE. The overall structure that connects ISE and IGE without mixing them is handled by ODF.

```text
IGE:
  vague signal / pain / request
    -> intent / issue / implementable direction

ISE:
  accepted intent
    -> spec / design / code / test / docs / operation

ODF:
  Connects IGE and ISE without mixing them, with a limited interface
```

## Related Documents

For IGE, ODF, and the lane-model, see the following documents.

- [Overview of the three frameworks (Environment ⇄ Intent ⇄ Reality)](../overview.md)
- [IGE overview](../ige/overview.md)
- [ODF overview](../odf/overview.md)
- [Philosophy and concepts of the lane-model](../lane-model/01_philosophy_and_concepts.md)

## Future Plans

At this point, documents with high value to be added in the future are as follows:

- Relationship between ISE/IGE/ODF and Scrum
- Concrete examples, case studies

Regarding Scrum in particular, it can be treated as a practice that attempted to synchronize product intent, stakeholder expectations, backlog priority, and team work rhythm in short cycles. On the other hand, since it did not clearly separate requirement exploration, value judgment, implementation synchronization, and artifact synchronization, there is room for reorganization from the perspective of IGE/ODF.
