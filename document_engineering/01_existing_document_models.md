# Organizing Existing Document Perspectives

## Purpose

This document organizes how documentation has been treated in existing software engineering, systems engineering, requirements engineering, architecture description, and quality models as a premise for considering ISE Document Engineering.

The goal here is not to reject existing standards or practices. Rather, it is to clarify what they handle strongly, what they assume, and where their limits lie.

ISE does not ignore existing documentation practices. However, given AI-assisted development, Reverse Sync, continuous synchronization between Intent and Reality, and the lane-model, adopting existing document perspectives as-is is not enough.

Therefore, we first organize existing document perspectives as a baseline for comparison from the standpoint of ISE.

## Core of Existing Document Perspectives

In traditional software and systems development, documentation has typically served the following roles:

- Clarifying requirements
- Explaining designs
- Sharing assumptions for implementation and verification
- Recording agreements among stakeholders
- Preserving knowledge for maintenance and handover
- Managing quality, constraints, risks, and responsibilities
- Supporting auditing and accountability

These are important and do not become unnecessary in ISE.

However, traditional document perspectives often view documentation as a "phase deliverable." In the requirements phase it is a requirements specification, in the design phase a design document, in the verification phase a test specification, and in the operations phase an operations manual.

This view is natural in human-centered development processes. Under the assumption that phases are relatively well-separated and work proceeds through staged reviews and agreements of deliverables, documentation functions as an interface between phases.

In ISE, however, documentation is not viewed solely as a phase deliverable. A document is an artifact that supports the synchronization state between Intent and Reality, and an object operated on by both Forward Sync and Reverse Sync.

## Documents in Lifecycle Standards

Lifecycle standards such as ISO/IEC/IEEE 12207 and ISO/IEC/IEEE 15288 organize processes, activities, deliverables, and responsibilities across the entire lifecycle of software or systems.

In this type of standard, documentation primarily serves the following roles:

- Clarifying process inputs and outputs
- Recording the results of activities
- Providing objects for agreement, approval, and verification
- Supporting traceability across the entire lifecycle
- Enabling organizational management, auditing, and maintenance

This perspective is strong for treating development as a series of manageable activities.

On the other hand, from ISE's perspective, while lifecycle standards are strong at organizing "where documents appear in the process," they do not center on "what state a document holds in the synchronization between Intent and Reality."

In particular, the following points need to be considered additionally on the ISE side:

- How to handle a document when implementation precedes documentation
- How to distinguish As-Is recovered from code from adopted Intent
- How to represent temporary Drift not as an anomaly, but as part of the synchronization process
- How to handle the provenance and confidence of documents generated or updated by AI
- Where to place Pre To-Be before a human makes an adoption decision

Lifecycle standards are important as a foundation, but they do not define ISE's synchronization model itself.

## Documents in Requirements Engineering

In the context of requirements engineering, represented by ISO/IEC/IEEE 29148, documentation is the central artifact for defining, analyzing, agreeing on, and making requirements verifiable.

Requirements documents generally have the following purposes:

- Clarifying stakeholder needs
- Defining conditions the system must satisfy
- Bringing ambiguous expectations closer to verifiable expressions
- Detecting contradictions and omissions among requirements
- Providing inputs for design, implementation, and testing
- Supporting traceability

This is an area particularly close to IGE. In terms of translating ambiguous requirements into implementable What, requirements engineering serves as an important reference for IGE.

However, in ISE Document Engineering, rather than placing requirements documents directly at the center, it is necessary to distinguish which stage of Intent a requirement is in:

- Still-ambiguous needs
- Requirement candidates under organization
- Implementable What
- Adopted Intent
- Missing requirements discovered after implementation
- Intent candidates obtained in reverse from Reality

In traditional requirements documents, these tend to be mixed together within the same document. In ISE, it is especially important not to confuse things obtained through Reverse Sync with official requirements.

Facts observed from code are not requirements themselves. Intent inferred from code is not yet adopted Intent either. Promotion judgment by a human or accountable entity is required there.

## Documents in Architecture Description

In architecture description thinking, represented by ISO/IEC/IEEE 42010, a system's architecture is explained through stakeholders, concerns, views, viewpoints, and the like.

Documentation in this area is not merely a structural diagram. It is a representation for making a system's key structural decisions understandable per concern.

Its main roles are:

- Explaining the structural intent of the system
- Providing different views per concern
- Preserving the rationale behind key design decisions
- Sharing understanding among multiple stakeholders
- Providing decision criteria for implementation and change

This is also important for ISE. In particular, architecture bears high-level structural constraints between Intent and Reality.

However, in ISE, architecture documents are not static explanatory materials either. If Reality changes, architecture descriptions can also drift. Conversely, if an architecture document is updated, the code side needs to synchronize with it.

Furthermore, in AI-assisted development, local implementation proceeds rapidly, making it easy for changes that subtly break architectural boundaries to accumulate. Therefore, architecture documents need to be designed not merely as explanations, but as constraint surfaces for Drift detection and re-synchronization.

## Documents in Quality Models

Quality models such as ISO/IEC 25010 organize software and system quality along perspectives such as functional suitability, performance efficiency, compatibility, usability, reliability, security, maintainability, and portability.

Rather than providing direct templates for documents, this kind of model provides a vocabulary for what should be treated as quality.

In ISE, quality models become important in the following ways:

- Embedding quality constraints into requirements and rules
- Defining perspectives for verification
- Evaluating unhealthy states at Gates
- Using them for triage prioritization
- Evaluating whether Reality has deviated from Intent

However, a quality model alone does not become a synchronization model. Even if quality attributes are defined, separate design is needed for which document holds them, which Reality they connect to, which verification checks them, and how to re-synchronize when they drift.

## ADR and Decision Records

Architecture Decision Records, or Decision Records more generally, are a practice of recording why a particular choice was made.

This shares a problem awareness very close to ISE. Code tends to retain the How, whereas the Why and Why not are easily lost. ADRs compensate for that omission.

Typical information handled by an ADR includes:

- Context
- Problem
- Chosen option
- Alternatives
- Trade-offs
- Consequences
- Impact

In ISE, decisions should be treated as first-class document artifacts.

However, ADRs are usually operated as local decision records. In ISE, in addition to that, it is necessary to consider which Intent a decision connects to, which Reality it constrains, and which Drift it helps detect.

In other words, a decision in ISE is not merely a decision memo, but part of the synchronization structure.

## Docs-as-Code and Living Documentation

Docs-as-Code is the approach of version-controlling, reviewing, automating, and continuously updating documentation just like code.

Living Documentation treats documentation not as something written once and abandoned, but as a living information asset updated alongside the system.

These are highly compatible with ISE.

In ISE, documentation should always be synchronized with Reality, and outdated documents are actually dangerous. Docs-as-Code and Living Documentation can serve as the implementation foundation for ISE in this respect.

However, they alone are not sufficient.

Docs-as-Code is strong on how to manage documents, but it does not decide which information should be separated by which synchronization responsibility. Living Documentation provides the philosophy of keeping documents updated, but it does not define the distinction between Intent / Reality / Knowledge / Decision or the promotion rules for Reverse Sync.

ISE Document Engineering needs to assume these practices and place a synchronization model on top of them.

## SDD and Spec-Driven Development

Spec-Driven Development is a development approach that places specifications at the center of implementation.

In AI-assisted development, because the quality of specifications strongly affects the quality of output, SDD-style thinking is growing in importance. Giving a clear specification for implementation is easier to control than having AI write code from ambiguous instructions.

goose-sdd was an initial implementation for practicing this idea. It treated `docs/sdd/` as a Living Spec and experimented with both Forward Sync and Reverse Sync.

However, from ISE's perspective, SDD alone is not enough.

SDD is primarily strong in the forward direction of `spec -> code`. On the other hand, in ISE, the reverse direction of `code -> analysis -> knowledge -> intent candidate -> adopted intent` is also important.

Moreover, even when calling something a specification, it contains requirements, designs, constraints, decisions, unresolved issues, rejected options, verification policies, and more. Cramming all of these into the single word "spec" makes synchronization responsibilities ambiguous.

## Summary of Existing Document Perspectives

Existing standards and practices each have important strengths:

- Lifecycle standards are strong in managing activities and deliverables
- Requirements engineering is strong in bringing ambiguous needs closer to verifiable What
- Architecture description is strong in organizing structural decisions and concerns
- Quality models provide a vocabulary of quality attributes and evaluation perspectives
- ADRs are strong in preserving decision rationale and alternatives
- Docs-as-Code is strong in treating documents as operable assets
- Living Documentation provides the philosophy of keeping documents updated
- SDD is effective as a practice for placing specifications at the center of implementation

However, what ISE needs is not a documentation system that merely lines these up side by side.

What ISE needs is a documentation system capable of handling the synchronization state between Intent and Reality.

To achieve that, while referencing existing document perspectives, we need to reconstruct around the following question:

> Which of Intent / Reality / Knowledge / Decision does that document hold, which mode does it belong to, and as which artifact in which scope does it bear which synchronization responsibility?

This question serves as the starting point for ISE Document Engineering.
