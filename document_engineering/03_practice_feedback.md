# Practical Insights from goose-sdd and Nagi

## Purpose

This document organizes the insights gained from the experimental results of goose-sdd and the development and operation of Nagi in order to feed them back into ISE Document Engineering.

ISE Document Engineering is difficult to establish through abstract theory alone.

When you actually have AI read and write documents, synchronize them with code, and reconstruct documents from Reality via Reverse Sync, you start to see which kinds of information tend to get mixed up, which document names are hard to resolve, and which classifications tend to break down.

goose-sdd and Nagi are practical primary sources for that purpose.

## Positioning of goose-sdd

goose-sdd was a practical starting point prior to ISE.

The central idea was to treat `docs/sdd/` as a Living Spec and manage Drift between documents and code through Forward Sync and Reverse Sync.

goose-sdd mainly had the following structure:

- system
  - Product-wide background, concepts, architecture, rules, and glossary
- feature
  - Requirements, design, tests, and implementation policies for individual features
- implement
  - Lightweight documents for small-scale, rapid implementation
- analyze
  - Documents for reverse-generating As-Is from existing code
- tool
  - Questions and auxiliary operations

This structure was effective for practicing SDD.

In particular, being able to try both Forward Sync—explicitly writing specifications first and using them for code generation—and Reverse Sync—recovering documents from existing code—was important.

## What Worked in goose-sdd

Many effective insights were obtained from goose-sdd.

### Living Spec Was Effective

Rather than writing documents once and finishing, treating them as a Living Spec updated alongside the code was effective.

In AI-assisted development, if documents remain outdated, there is a danger that AI will strongly trust outdated assumptions. Keeping them updated as a Living Spec is safer for both AI and humans.

### Forward Sync Was Effective

The flow of writing specifications and designs first and having AI implement based on them was effective.

Documenting requirements, design, constraints, and test perspectives before implementing results in fewer missed intents than writing code directly from ambiguous requests.

### Reverse Sync Was Effective

The flow of organizing As-Is from existing code and feeding it back into documentation was also effective.

This is especially meaningful in projects where code already exists or in situations where implementation has preceded documentation. Observing Reality and bringing it back to the document side reduces comprehension debt.

### The Need for a Negotiation Loop Became Clear

When using Forward Sync and Reverse Sync, situations arise where it cannot be simply decided whether the document or the code is right.

When code diverges from a specification, it may be a bug, or the specification may simply be outdated. Here, a human needs to judge and update Intent.

From this, the necessity of a Negotiation Loop—Drift, Analyze, Review, Convergence—became clear.

## Limits Observed in goose-sdd

Although goose-sdd was effective, its structure also showed limitations.

### system / feature / implement / analyze Are Too Rigid as Fixed Classifications

Classifications such as system, feature, implement, and analyze are easy to understand, but too rigid as an abstract model for ISE.

For example, it becomes ambiguous whether analyze is a layer alongside system and feature, or a different direction of work that converts Reality into Knowledge.

Also, while implement is a convenient operational form for small-scale development, it is closer to a usage mode than an essential classification of ISE.

From this experience, it became more natural in Nagi to view these not as layers, but as lanes.

### Calling Something SSoT Alone Cannot Separate Information Types

Calling `docs/sdd/` the SSoT was easy to understand in practice.

However, the information contained there is not of a single type:

- Adopted Intent
- As-Is recovered from reality
- Human-derived memos
- Design proposals
- Unresolved issues
- Decision rationale
- Test perspectives
- Implementation notes

When these are placed in the same SSoT, it becomes ambiguous what is official intent, what is an observed fact, and what is a candidate.

In ISE, because all elements continue to change independently, it does not assume a static SSoT; instead, it needs to be treated as a "snapshot of Intent adopted at that point in time (Living Spec)."

### Reverse SDD Easily Confuses As-Is and As-Intended

When reverse-generating documents from code, the current structure can be organized quite well.

However, it is dangerous to go so far as to claim "this was originally intended to be this way."

What is obtained from code is primarily As-Is. As-Intended, even if partially inferable, is incomplete.

This experience leads to the separation of gather, mosaic, synthesize, and elevate in Nagi's analyze lane.

### Document Names and Placement Directly Affect Operability

In goose-sdd, ordered file names such as `00_product_background.md` and `01_requirement.md` were used.

This is easy for humans to read. On the other hand, when resolving targets by `lane / scope / artifact` as in Nagi, file names and placement are not mere appearance, but the target surface interpreted by the resolver.

For this reason, in Nagi, it became necessary to redesign document names into a form that is both "natural to humans" and "resolvable by AI and CLI."

## Positioning of Nagi

Nagi is an implementation that builds on the insights of goose-sdd while aiming for a position close to the reference implementation of ISE.

However, Nagi is not a simple subordinate implementation of ISE. Practical problems discovered through Nagi's development also flow back into the theoretical side of ISE.

This relationship is important.

It is not a one-way street where ISE is theory and Nagi is implementation.

Nagi is also a testing ground for honing ISE Document Engineering.

## What Has Become Clear in Nagi

### Lanes Are More Natural Than Layers

goose-sdd's system / feature / implement / analyze look somewhat layer-like.

In Nagi, however, it became more natural to view work not as hierarchical relationships or phase order, but as lanes.

A lane represents the direction of how a target is changed:

- spec
  - Makes Intent explicit
- code
  - Changes Reality
- analyze
  - Reconstructs Reality as Knowledge
- cure
  - Converts problems into repairs

Through this view, documentation can also be organized as artifacts per lane.

### The analyze Lane Is Close to the Center of the Documentation System

Nagi's analyze lane is particularly important for ISE Document Engineering.

analyze is not mere code analysis. It is the lane that creates Knowledge from Reality and produces Pre To-Be as needed.

Therefore, the following distinctions are necessary:

- gather
  - Collect code-backed facts
- mosaic
  - Organize human-derived fragments and hypotheses
- synthesize
  - Integrate gather and mosaic into an understandable As-Is
- elevate
  - Connect to higher-confidence Knowledge or To-Be candidates

This structure is also necessary to contain the dangers of Reverse Sync.

### A Staging Area for Pre To-Be Is Needed

Through Nagi's development, it became clear that Pre To-Be is needed between As-Is and To-Be.

Improvement candidates and intent candidates can be found from Reality. However, they are not yet official Intent.

Pre To-Be is the area for receiving this intermediate state.

Information such as the following goes here:

- pain point
- drift signal
- intent candidate
- design candidate
- confidence
- provenance
- unresolved question

In the future, as IGE / ODF mature, Pre To-Be can also serve as the payload passed to IGE.

### consultant / assistant Is Not a Lane, but Deeply Relates to Documentation

In Nagi, functions for consultation, design conversation, review assistance, and comprehension support are needed.

Because this is not a lane that directly advances synchronization, it is better not to place it on the same level as spec, code, analyze, and cure.

On the other hand, consultant / assistant is strongly related to the documentation system.

This is because consultation functions need to read appropriate documents, compress them, and present them in a form easy for humans to judge.

In other words, while consultant / assistant is not a direct change lane, it is an important support interface when designing document access patterns.

### artifact namespace and resolver Are Needed

In Nagi, `lane / scope / artifact` is considered as the target surface.

```text
nagi <lane> <scope>[:artifact]
```

With this form, it is more natural for artifacts to have per-lane namespaces rather than names shared across all lanes.

For example, even with the word `design`, design in the spec lane and design-like reconstruction in the analyze lane do not mean the same thing.

Therefore, artifacts should be interpreted per lane, and resolution targets need to be assisted by namespaces and resolvers as needed.

This is important for ISE Document Engineering.

Document classification is not mere directory tidying, but the design of an operable target surface.

## Insights to Feed Back into Nagi's Document Model

From the experience of goose-sdd and Nagi, the following insights should be reflected in Nagi's document model.

### Classify Documents by Synchronization Responsibility

When documents are classified only by names like "requirements document," "design document," or "test specification," As-Is and To-Be, facts and judgments, and candidates and adopted Intent easily get mixed together.

In Nagi, it is necessary to make clear which synchronization responsibility a document bears.

### Separate Adopted Intent from Candidates

Proposals generated by AI or intent candidates inferred from Reality are not official Intent as-is.

Judgment by a human or accountable entity is required to promote them to adopted Intent.

### Reality-Derived Documents Need provenance and confidence

Facts obtained from code and configurations have relatively strong grounding.

On the other hand, human-derived fragments and AI inferences vary in confidence.

Nagi's documentation system needs to be able to express the provenance and confidence of information.

### Treat Decision as a First-Class Artifact

Decision rationale, rejected alternatives, trade-offs, and risk acceptance are hard to recover later just by reading code.

In Nagi, decisions should be treated not as auxiliary information, but as first-class artifacts.

### Design File Names for Both Humans and AI

File names and placement should not be decided by human readability alone.

It is important that AI can discover necessary documents, resolve scope and artifact, and distinguish old information from adopted information.

## Feedback Loop for ISE Document Engineering

It is natural to grow ISE Document Engineering through the following feedback loop:

1. Organize existing documentation practices
2. Formulate the problem statement as ISE
3. Implement a provisional document structure in Nagi
4. Observe where it breaks in actual development operations
5. Record it as practical insights from goose-sdd / Nagi
6. Feed it back into ISE Document Engineering theory
7. Reflect it again in Nagi's document model, lane-model, and resolver

This aligns with the philosophy of ISE itself.

Not only Forward Sync from theory to implementation, but also Reverse Sync from implementation back to theory is necessary.

## Topics to Organize Going Forward

Topics to organize further as ISE Document Engineering going forward are:

- Standard classification of artifacts
- Document namespaces per lane
- Mapping between scope and actual file paths
- Promotion rules to adopted Intent (Living Spec)
- Storage format for Pre To-Be
- Representation of provenance / confidence
- Relationship between decision and ADR
- Balancing human-readable and agent-readable
- Context sets that consultant / assistant should read
- Document structure for detecting Drift
- Design of the resolver in Nagi
- Boundary between project-specific extensions and standard artifacts

These do not all need to be decided at once.

As Nagi's implementation progresses, it is best to feed back what actually becomes necessary into ISE Document Engineering and turn it into principles.

## Summary

goose-sdd demonstrated the effectiveness of Living Spec, Forward Sync, Reverse Sync, and the Negotiation Loop.

At the same time, it also revealed the limitations of fixed classifications like system / feature / implement / analyze and a simplistic view of SSoT.

Nagi is attempting to reconstruct those insights into the lane-model, target surface, artifact namespace, analyze lane, and Pre To-Be.

ISE Document Engineering is positioned as the field that absorbs these practical insights and connects existing documentation practices with ISE's synchronization model.

In this sense, goose-sdd and Nagi are not mere implementations.

They are observational instruments for grounding ISE Document Engineering in reality, and testing grounds for honing its theory.
