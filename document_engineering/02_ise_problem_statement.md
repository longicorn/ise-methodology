# Problem Statement for ISE Document Engineering

## Purpose

Building on existing documentation practices, this document organizes what ISE treats as problems and what kind of documentation system should be considered.

The core of ISE Document Engineering is not simply "how to write good design documents."

The central question is:

> Shouldn't documentation in ISE be an artifact that makes the synchronization state between Intent and Reality operable by both humans and AI?

From this standpoint, documentation is not a byproduct of phases, but an operational object that supports synchronization, negotiation, promotion, verification, and reconstruction.

## Redefining Documentation in ISE

ISE views the essence of development not as a fixed sequence of phases, but as continuous synchronization between Intent and Reality.

- Intent
  - How the system is considered to be / ought to be
- Reality
  - Actual code, tests, configurations, behavior, and operational state
- Synchronization
  - Detecting discrepancies between Intent and Reality, negotiating them, and bringing them back into alignment

Standing on this premise, documentation as "explanatory material" is not enough.

Documentation is an artifact for externalizing Intent, observing Reality, handling the diff between the two, and preserving the results of human judgment.

Accordingly, documentation in ISE should have the following properties:

- Holds Intent
- Makes Reality observable
- Makes Drift detectable
- Records decisions and agreements
- Receives the results of Reverse Sync
- Functions as a context surface passed to AI
- Separates information in a form that allows humans to make adoption decisions
- Can handle temporary inconsistency under Eventual Consistency

## From Phase Deliverables to Synchronization Artifacts

In traditional documentation systems, documents are often divided by phase or purpose, such as requirements documents, design documents, test specifications, and operations manuals.

ISE does not completely discard this division. However, it is insufficient as a primary classification.

This is because what matters in ISE is not "in which phase a document was created," but "what role it bears within the synchronization structure."

For example, even documents all named "design" have different meanings as follows:

- A To-Be design for upcoming implementation
- An As-Is design recovered from Reality after implementation
- A drift analysis showing the diff between reality and intent
- A design candidate prior to adoption
- An adopted design decided by humans

If these are treated as the same "design document," the documentation quickly becomes ambiguous.

In ISE Document Engineering, documents need to be treated not as phase deliverables, but as artifacts with synchronization states.

## Separating Intent / Reality / Knowledge / Decision

In an ISE documentation system, it is important not to mix at least the following four types:

### Intent Document

An Intent Document represents how things ought to be.

This includes requirements, design intent, constraints, quality policies, adopted decisions, and the like.

What matters is that an Intent Document represents the intent adopted at that point in time, not mere wishes or conjectures. Unconfirmed proposals or intent candidates inferred from code should not be mixed directly into it.

### Reality Document

A Reality Document represents how things actually are.

This is based on code, tests, configurations, operational state, actual behavior, and so on. The value of a Reality Document lies in being grounded in observed facts rather than subjective ideals.

However, a Reality Document is not Intent. Just because something is implemented does not mean it is right.

### Knowledge Document

A Knowledge Document organizes information derived from Reality or humans and reconstructs it into an understandable form.

In the analyze mode, gather, mosaic, synthesize, and elevate are strongly related to this area.

A Knowledge Document is neither a mere list of facts nor an official specification. It is an intermediate artifact for organizing facts, background, hypotheses, confidence, and provenance, bringing them closer to a decidable state.

### Decision Document

A Decision Document represents why a particular choice was made.

This includes rationale for adoption, rejected alternatives, trade-offs, risk acceptance, decision owner, and decision point in time.

Code tends to retain the How. Tests tend to retain the What. However, Why and Why not are hard to retain. A Decision Document is necessary to bring this easily lost information back into the synchronization structure.

## Separating As-Is / Pre To-Be / To-Be

What is particularly important in ISE Document Engineering is separating As-Is, Pre To-Be, and To-Be.

### As-Is

As-Is is a reconstruction of reality.

Focusing on facts observable from code, configurations, tests, execution results, and the like, it organizes how the current system actually works.

As-Is is important, but by itself it does not mean correctness. As-Is is a description of Reality, not adopted Intent.

### Pre To-Be

Pre To-Be consists of improvement candidates, intent candidates, and problem statements derived from Reality or human-derived fragments.

This includes items such as:

- pain point
- drift signal
- intent candidate
- design candidate
- risk candidate
- confidence
- provenance

Pre To-Be is an important staging area on the boundary between ISE and IGE.

Even when something becomes visible from Reality, it must not immediately be made official Intent. It is held as Pre To-Be and passed to IGE or human judgment as needed.

Note that the staging workspace for provisional specifications built by reconciling observed facts, inferences, and open questions in Reverse Sync is called **PreSpec (Pre-Spec)**. Both PreSpec and Pre To-Be are incomplete intermediate states that cannot yet be treated as official documents (adopted To-Be): while PreSpec serves as scaffolding for understanding the current state, Pre To-Be holds the intent and change candidates that emerge from it.

### To-Be

To-Be is officially adopted Intent.

To become To-Be, mere AI inference is not enough. Adoption judgment by a human or accountable entity is required.

What can be called a Living Spec in ISE should not assume a static single SSoT, but rather be structured around this adopted To-Be.

## What Reverse Sync Demands of the Documentation System

Reverse Sync is the movement of returning information from Reality toward the Intent side.

This is an important characteristic of ISE, but it also carries danger.

From code, As-Is can be recovered. However, As-Intended cannot be completely recovered. From implemented structure alone, it is impossible to know completely why things were done that way, what was discarded, or which risks were accepted.

Therefore, the results of Reverse Sync must be distinguished from adopted Intent.

What is obtained through Reverse Sync is mainly one of the following:

- code-backed fact
- observed behavior
- inferred structure
- intent candidate
- drift signal
- pain point
- undocumented decision candidate

If these are mixed directly into specifications, Reality ends up being promoted to Intent unnoticed.

In ISE Document Engineering, Reverse Sync results need to carry provenance, confidence, and promotion state.

## Operation Modes (`mode`) and Documentation

When considering a documentation system, it is important to view work not as a phase order, but as **operation modes (`mode`)**. Here, `mode` represents the direction of work (change vector) as an operational unit, corresponding to an entry point that bundles transformation edges (Lanes) between Nodes in the [Lane Model](../lane-model/01_philosophy_and_concepts.md) by context.

- spec
  - Makes Intent explicit and connects it to Reality
- code
  - Changes Reality to bring it closer to Intent
- analyze
  - Reconstructs Reality as Knowledge

In this view, documentation does not belong only to the spec mode.

In the spec mode, there are documents representing Intent. In the analyze mode, there are documents representing As-Is, PreSpec, and Pre To-Be. Even in the code mode, there can be documents supporting local implementation decisions and communicating intent to AI. Furthermore, records concerning health evaluation and triage are tied to **Gate** evaluation and send-back decisions, while repair work (remedy) is handled as re-synchronization of the spec or code mode triggered by a Gate send-back.

In other words, it is natural to think of document classification as the triple `mode / scope / artifact`.

```text
mode     = what to do (operation mode)
scope    = what target area it covers
artifact = which deliverable or perspective within that scope is handled
```

Through this structure, organization closer to ISE's synchronization model becomes possible than with fixed classifications like system / feature / implement / analyze.

## Documentation as an AI Context Surface

In AI-assisted development, documentation is not only for humans to read.

It also functions as a context surface passed to AI.

From this perspective, documents need the following properties:

- AI can read only the necessary range
- The meaning of an artifact can be resolved from context
- Scope can be specified logically
- Old information and adopted information can be distinguished
- Speculation, facts, and decisions are not mixed
- It is determined which document should take priority
- Humans can verify AI output

In short, file names and directory structures are not mere tidying up.

They are part of the operational target surface for AI to resolve targets.

## SSoT Is Not Absolute Truth (ISE Does Not Assume SSoT)

ISE does not assume a static single SSoT (Single Source of Truth). This is because every Node—including specifications, code, operational reality, and the humans involved—continues to change independently over time.

Even if something equivalent to an SSoT is positioned deliberately, it is not a sole absolute truth, but a snapshot of adopted Intent where negotiation has settled at that point in time (Living Spec).

This understanding is important.

If we treat something somewhere as the sole absolute SSoT, it becomes difficult to accept counter-evidence from Reality or new intent discovered during implementation. On the other hand, if adopted Intent is treated as a mere memo, it becomes unable to constrain implementation and verification.

Adopted Intent (Living Spec) in ISE should have the following properties:

- Represents adopted Intent
- Is synchronized with Reality
- Can become an update target due to Drift
- Requires judgment and recording for changes
- Is distinguished from unadopted candidates

For this reason, around adopted Intent, a document structure is needed for placing candidates, observations, diffs, decisions, and rejected options.

## Central Proposition of ISE Document Engineering

Based on the above, the central proposition of ISE Document Engineering can be stated as follows:

> Documentation in ISE is not a phase deliverable, but an operable artifact for synchronizing, negotiating, and promoting Intent / Reality / Knowledge / Decision.

From this proposition, the following design principles follow:

- Classify documents by synchronization responsibility rather than phase order
- Do not mix Intent, Reality, Knowledge, and Decision
- Do not mix As-Is and To-Be
- Do not mix Pre To-Be with official Intent
- Give Reverse Sync results provenance and confidence
- Promote to Living Spec (adopted Intent) only what has passed human adoption judgment
- Treat rejected options, unresolved issues, constraints, and decision rationale as first-class recording targets
- Design file names and directory structures with AI resolvability in mind
- Documentation should be agent-readable as well as human-readable
- Under Eventual Consistency, structure documents so temporary Drift can be expressed

Based on these principles, ISE Document Engineering should connect to the document model, operation modes (`mode`), artifact namespace, resolver, Reverse Sync, and consultant support interface of concrete implementations and tools.
