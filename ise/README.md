# ISE Documents

> Note: The problem identification, key concepts, and final decisions in this repository are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

This directory is a collection of documents to organize thoughts on Intent-Synchronized Engineering (ISE).

ISE is a term proposed by the author in these papers and is not an existing standardized development methodology or a widely agreed-upon industry term. At its core is the idea of continuously synchronizing artifacts such as code, tests, specifications, designs, Issues, ADRs, and operation records to a common intent.

## Reading Order

The documents are intended to be read in the following order:

1. [History of Software Engineering and the "Unending Challenges" the Field Continues to Face](01_history_and_unending_challenges.md)
2. [Document Drift and Software Archaeology: A Mosaic of Information That Has Plagued the Field](02_document_drift_and_software_archaeology.md)
3. [The Advent of AI, Vibe Coding, and the Limits of SDD/IDD](03_ai_paradigm_and_sdd_limits.md)
4. [Intent-Synchronized Engineering: Engineering Centered on Synchronization of Intent](04_intent_synchronized_engineering.md)

As a supplementary discussion:

- [Document Drift and Intent Decay: Why Intent Must Be Synchronized](foundations/01_document_drift_and_intent_decay.md)

To check terms, refer to:

- [Glossary](glossary.md)

## Positioning of Each Document

### 01: History and Existing Methodologies

`01_history_and_unending_challenges.md` organizes how software engineering has been thought about.

It covers waterfall-like forward development, ISO/IEC/IEEE 12207, 15288, 29148, 42010, ISO/IEC 25010, SWEBOK, PMBOK, iterative development, Agile, Scrum, etc.

Here, instead of putting ISE at the forefront, we confirm what kind of awareness existing methodologies had and what kind of limits they left behind.

### 02: Drift, Archaeology, and Mosaic

`02_document_drift_and_software_archaeology.md` organizes problems that occur daily in the field.

It covers document drift, software archaeology, the mosaic of information, Vibe Management, technical debt, and understanding debt.

The focus here is the problem that developers must first reconstruct "what is this system now?", "which information is still correct?", and "why is it like this?" before writing code.

### 03: Limits of Existing Methods in the AI Era

`03_ai_paradigm_and_sdd_limits.md` organizes what has changed with the advent of AI and what has not yet been solved.

It covers Vibe Coding, SDD, IDD, Harness Engineering, AI-DLC / AI-SDLC, etc. AI accelerates code generation but does not automatically solve the definition, verification, synchronization, and maintenance of intent.

Here, we show that the problem in the AI era is shifting from "code generation" to "how to synchronize intent and artifacts."

### 04: Proposal of ISE

`04_intent_synchronized_engineering.md` is the central document of ISE.

ISE is defined as an implementation-side framework for continuously synchronizing Intent and Reality through multiple artifacts.

Here, we organize the handling of SSoT as a settlement of negotiation rather than absolute truth, handling not only forward but also backward synchronization, not fixing the main creator as either human or AI, management of cognitive load, Intent Recovery, and the reason why IGE/ODF is necessary.

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

## Future Plans

At this point, documents with high value to be added in the future are as follows:

- Introductory document for IGE
- Introductory document for ODF
- Overview of the lane-model
- Relationship between ISE/IGE/ODF and Scrum
- Concrete examples, case studies

Regarding Scrum in particular, it can be treated as a practice that attempted to synchronize product intent, stakeholder expectations, backlog priority, and team work rhythm in short cycles. On the other hand, since it did not clearly separate requirement exploration, value judgment, implementation synchronization, and artifact synchronization, there is room for reorganization from the perspective of IGE/ODF.
