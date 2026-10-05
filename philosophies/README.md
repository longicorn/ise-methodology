# Philosophy and Background

> Note: The problem identification, key concepts, and final decisions in this repository are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

This directory is a collection of documents that organize the ideas behind ISE (Intent-Synchronized Engineering), IGE (Intent Guidance Engineering), and ODF (Orthogonal Dual-Framework).

ISE, IGE, and ODF are terms proposed by the author in these papers and are not existing standardized development methodologies or widely agreed-upon industry terms.

For the overall picture of the three frameworks, see the [Overview](../overview.md). For ISE-specific documents, see the [ISE Documents](../ise/README.md).

## Reading Order

The documents are intended to be read in the following order:

1. [History of Software Engineering and the "Unending Challenges" the Field Continues to Face](01_history_and_unending_challenges.md)
2. [Document Drift and Software Archaeology: A Mosaic of Information That Has Plagued the Field](02_document_drift_and_software_archaeology.md)
3. [The Advent of AI, Vibe Coding, and the Limits of SDD/IDD](03_ai_paradigm_and_sdd_limits.md)
4. [Intent-Synchronized Engineering: Engineering Centered on Synchronization of Intent](04_intent_synchronized_engineering.md)
5. [Review as Engineering Feedback: From Subjective Inspection to Multi-Perspective Assessment Systems](05_feedback_systems_and_assessment_engineering.md)
6. [Redesigning Human Roles and Responsibilities in the AI Era](06_roles_and_responsibilities_in_ai_era.md)

As a supplementary discussion:

- [Document Drift and Intent Decay: Why Intent Must Be Synchronized](../ise/foundations/01_document_drift_and_intent_decay.md)

To check terms, refer to:

- [Glossary](../ise/glossary.md)

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

### 05: Review as Engineering Feedback

`05_feedback_systems_and_assessment_engineering.md` covers the assessment system and feedback control in ISE.

It redefines review from subjective human inspection or gatekeeping into an "engineering feedback control system." Drawing upon foundational research—Fagan Inspection, Perspective-Based Reading (PBR), IEEE 1028, and empirical studies at Microsoft—it discusses the orthogonal separation of evaluation objectives and mechanisms (Defect / Validity / Complexity), the division of labor between AI and humans, and the realization of engineering as a closed-loop discipline.

### 06: Roles and Responsibilities of Humans in the AI Era

`06_roles_and_responsibilities_in_ai_era.md` covers the redesign of the responsibilities humans bear in the AI era.

Instead of asking "which professions will survive," it asks "where do humans bear final accountability." It rejects the polarized views of full automation and AI rejection, and organizes the roles of engineers (from architect-leaning to craftsman-leaning) and PMs (from business/discovery-leaning to product-architect-leaning) as continuous gradients. It then shows that ISE, IGE, and ODF are the foundation supporting the responsibilities of people standing anywhere on those gradients.
