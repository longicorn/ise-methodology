# Glossary

> Note: The problem identification, key concepts, and final decisions in this repository are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

This document is a glossary to organize terms used in ISE-related documents.

The terms included here are a mix of existing general terms, software engineering terms, terms from recent AI development contexts, and unique designations used by the author for explanation in these papers. The unique designations are not presented as standard terms or widely agreed-upon industry terms.

## AI-DLC / AI-SDLC

An approach or initiative to incorporate AI into the entire software development life cycle.

AI is used in each stage such as requirements, design, implementation, testing, review, operation, and maintenance. However, even with AI, the definition, verification, synchronization, and maintenance of intent are not automatically solved.

## ADR

Stands for Architecture Decision Record.

A document that records architectural decisions, reasons for adoption, alternatives, constraints, and impacts. In ISE, it is treated as one of the important artifacts that holds "Why" and rejected proposals.

## As-Is

The state as it actually is now.

It is relatively easy to reconstruct from code, tests, runtime behavior, and operational reality. On the other hand, "As-Is" alone often does not tell "what was originally intended."

## As-Intended

The state as it was originally intended.

This includes why that decision was adopted, which alternatives were discarded, and which constraints were prioritized. If it is not explicitly stated in artifacts, it is difficult to completely reconstruct it later.

## Cure

A perspective of maintenance and healing in ISE.

It refers to activities that intervene in differences, debt, dangerous spots, complexity, lack of testing, boundary violations, and design degradation to restore the health of code and artifacts.

## Drift

Discrepancies between artifacts.

This includes when specifications, designs, code, tests, issues, ADRs, operation procedures, and human memory are no longer connected to the same intent.

## Eventual Consistency

In ISE, a state of perfect match is not always required. Temporary discrepancies will occur, but instead of leaving them, we aim for a state where intent and artifacts eventually converge.

## Forward Sync

Synchronization from intent or specifications toward code, tests, operation, etc.

```text
Intent / Spec -> Code / Test / Reality
```

It is compatible with conventional forward development, but real-world development requires more than just Forward Sync.

## Harness Engineering

A concept to surround AI with constraints, tools, tests, and feedback loops to suppress runaway or haphazard generation.

It plays a role similar to SDD charters, project rules, tests, CI, review, and static analysis. However, the Harness does not substitute for the intent itself.

## IDD

A concept sometimes used as an abbreviation for Intent-Driven Development.

A direction that tries to drive AI and development processes centered on intent. However, how to externalize intent, how to synchronize it with artifacts, and how to detect drift need to be handled separately.

## IGE

Stands for Intent Guidance Engineering.

A concept for leading from ambiguous requirements, observations, pain, dissatisfaction, operational signals, or market signals to implementable intent and issues.

```text
IGE:
  vague signal / pain / request
    -> intent / issue / implementable direction
```

While ISE mainly deals with synchronization on the implementation side, IGE deals with upstream ambiguity.

## ISE

Stands for Intent-Synchronized Engineering.

It is a term proposed by the author in these papers and is not an existing standardized development methodology or a widely agreed-upon industry term.

ISE is an implementation-side framework for continuously synchronizing Intent and Reality through multiple artifacts.

```text
ISE:
  accepted intent
    -> synchronization to spec / design / code / test / docs / operation
```

## Intent

Intent.

Not just a mood or desire, but a cluster of judgments: what you want to do with the system, why you want to do it, what to prioritize, what to discard, which constraints to observe, which risks to accept, and which state to consider correct.

## Intent Recovery

The work of reconstructing lost or unstated intent from code, tests, operational reality, issues, PRs, commit logs, old documents, and human memory.

It is important but not an ideal state. What can be reconstructed is mainly "As-Is", and complete reconstruction of "As-Intended" is difficult.

## Living Spec

A specification that is not a fixed document but is constantly updated by reactions from Reality. In ISE, SSoT is treated not as a fixed stone tablet but as a Living Spec updated through negotiation.

## ODF

Stands for Orthogonal Dual-Framework.

An overall structure for connecting IGE and ISE without mixing them, with a limited interface.

By orthogonally separating requirement exploration, value judgment, and product judgment from implementation-side intent synchronization, it avoids entanglement of contexts.

## Provenance

The source of information.

Facts obtained from code, expectations obtained from tests, reality obtained from operation logs, background obtained from human memory, and hypotheses obtained from old documents cannot be treated with the same weight. In ISE, the source and reliability of information are distinguished.

## Reality

Actual reality.

Includes code, tests, runtime behavior, operational constraints, structure of existing systems, logs, failures, actual usage by users, etc.

## Reverse Sync

Synchronization from code, tests, runtime behavior, operation, and history back to specifications or intent.

```text
Code / Test / Runtime / History -> As-Is knowledge -> Spec / Intent
```

Frequently needed in existing systems. However, what can be reconstructed by Reverse Sync is mainly "As-Is", and it is difficult to completely reconstruct "As-Intended".

## SDD

Stands for Spec-Driven Development.

A concept or toolset for proceeding with development centered on specifications. In the AI era, it is attracting attention as a method for generating implementation from specifications. However, challenges remain in existing systems, changes, drift, reverse reconstruction, and synchronization of artifacts.

## SSoT

Stands for Single Source of Truth.

Generally translated as "the only truth." In ISE, SSoT is treated not as absolute truth but as a snapshot of the intent adopted at the moment, namely, the settlement of negotiation.

## Vibe Coding

A development style where rough intent or "vibe" is conveyed to AI, leaving detailed design and implementation to AI inference.

While it allows for making things that work quickly, there is a risk that intent, constraints, reasons for judgment, and design boundaries are fixed into code without being explicitly stated.

## Vibe Management

A unique designation by the author to explain problems observed in the field.

A management state where development proceeds through atmosphere, pressure, deadlines, and fragmented information, without requirements, constraints, priorities, and reasons for judgment being sufficiently verbalized.

## Externalization of Intent

Converting intent that exists as something in the head, conversation, feeling of mismatch, experience, or tacit knowledge into a format that others can execute, verify, argue against, or update.

It is not just about writing sentences. It includes converting it into requirements, specifications, designs, issues, ADRs, tests, code comments, etc., so it can be handled as an artifact.

## Transformation of Intent

Not just intent being forgotten, but being replaced by another explanation due to the passage of time, post-hoc rationalization, current understanding, or organizational context.

In ISE, it is thought that intent not only gets lost but also transforms, so it needs to be synchronized to artifacts while it is still vivid.

## Decay of Intent Retention

A phenomenon where intent loses vividness over time, becomes harder to search for, and more easily reconstructed into another explanation.

This is a classification used by the author for explanation in these papers and is not presented as a standard term. It can be connected to the forgetting curve, decay of memory traces, Goal Activation / Goal Decay, volatility of architectural knowledge, etc.

## Mosaic of Information

A unique designation by the author to explain problems observed in the field.

A state where information with different reliability, tense, intent, and provenance coexist in one information space, making it difficult for an outsider to determine which is the current truth.

## Software Archaeology

The work of excavating the meaning, intent, constraints, and current truth of an existing system from code, tests, documents, issues, PRs, commit logs, operational records, and human memory.

It is not just an investigation, but a highly intellectual task of reading through contradictory information, estimating reliability, and judging the scope that can be changed.

## Document Drift

Specifications, design documents, READMEs, operation procedures, comments, etc., deviating from actual code, system behavior, and current intent.

In ISE, it is treated not as a simple failure to update documents, but as a problem where connectivity of intent is lost.

## AI as a Prosthetic

A concept that treats AI not just as a code generator but as a supplementary device that helps human externalization of intent.

It aims to compensate for the strengths and weaknesses of those who are good at speaking, those who are good at writing, those who think in diagrams, and those who think while writing code, using AI to connect intent to verbalized artifacts.

## Understanding Debt

A state where, even if code or systems exist, knowledge to safely understand, change, and explain them is lacking.

It mutually reinforces with technical debt. The larger the understanding debt, the more the load for pre-change investigation, review, onboarding, and incident response increases.
