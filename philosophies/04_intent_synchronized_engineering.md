# Intent-Synchronized Engineering: Engineering Centered on Synchronization of Intent

> Note: The problem identification, key concepts, and final decisions in this repository are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

In the previous papers, we organized the problems that software engineering has continued to face.

Software engineering has systematized requirements, design, implementation, verification, and maintenance. Project management has managed scope, schedule, cost, risk, and stakeholders. Agile emphasized adaptation to change and short feedback loops. In the AI era, new attempts such as Vibe Coding, SDD, IDD, Harness Engineering, and AI-DLC have appeared.

Nevertheless, the same types of problems remain in the field.

Documents drift. Existing information is mosaicified. Software archaeology becomes necessary. Vibe Management causes ambiguous requirements to flow in. AI accelerates code generation but does not automatically solve the definition, verification, and maintenance of intent.

In thinking about this series of problems, there is one smallest yet most important discovery the author has reached.

That is, **synchronization of intent is important**.

In this paper, centered on this discovery, we propose a way of thinking in development that the author calls Intent-Synchronized Engineering (ISE).

Note that Intent-Synchronized Engineering / ISE is a designation defined by the author for use in this paper and is not an existing standardized development methodology or a widely agreed-upon industry term. Nor does it point to a specific tool or implementation. Implementation is just implementation, and the center of what is handled in this paper is the way of thinking of "synchronizing intent and artifacts."

## 1. Discovery: What is Important is "Synchronization of Intent"

Various artifacts are made in software development.

Requirements, specifications, design, code, tests, Issues, Pull Requests, ADRs, operational procedures, incident records, release notes, comments, and commit logs.

These have different formats, are saved in different places, and are updated at different timings. But originally, they must be connected to the same intent.

Intent here is not just a mood or desire.

Intent is a cluster of judgments: what you want to do with the system, why you want to do it, what to prioritize, what to discard, which constraints to observe, which risks to accept, and which state to consider correct.

Code leaves current behavior. Tests leave part of expected behavior. Specifications leave what should be made. ADRs leave reasons for judgment. Issues leave change requests. Operational records leave operational constraints.

However, it is difficult to maintain the entire intent with only one of them.

With code alone, why it was done is difficult to leave. With specifications alone, judgments changed during implementation are difficult to leave. With tests alone, value judgments or rejected proposals are difficult to leave. With Issues alone, reality after implementation is difficult to leave.

Therefore, what is important is not which artifact to consider as the only truth.

What is important is whether the group of artifacts continues to be synchronized to a common intent.

## 2. Definition of ISE

Intent-Synchronized Engineering (ISE) is a way of thinking in engineering that places continuous synchronization of intent and artifacts at the center.

More shortly, ISE can be defined as follows:

```text
ISE = An implementation-side framework for continuously synchronizing Intent and Reality through multiple artifacts
```

Intent here includes adopted intent, reasons for judgment, constraints, trade-offs, undetermined items, and discarded options.

Reality includes actual code, tests, runtime behavior, operational reality, and constraints of existing systems.

Artifacts mediate between them.

```text
Intent
  -> requirements / design / ADR / issue / tests / code / operations
  -> Reality
```

The purpose of ISE is not to make artifacts in large quantities. Nor is it to increase documentation. Nor is it to make AI write code.

The purpose is to maintain a state where intent is not lost and artifacts do not contradict each other.

## 3. SSoT is Not Absolute Truth but a Settlement of Negotiation

In ISE, the term SSoT (Single Source of Truth) needs to be handled carefully.

Generally, SSoT is translated as "the only truth." However, truth is not self-evident in software development.

For the same function, multiple legitimate intents exist.

For example, for an authentication function, security personnel prioritize safety. UX personnel prioritize ease of use. Business side prioritizes behavioral analysis or conversion. Legal or compliance prioritizes regulatory requirements.

None of these alone is correct and others wrong. Intent is inherently multiple and often competes.

Therefore, SSoT in ISE is not "the truth that exists only one in the world."

It is the settlement of negotiation about which intent to adopt at the moment.

```text
SSoT = A snapshot of the intent adopted at the moment
```

This understanding is important.

Because if specifications or design documents are absolutized, it becomes easy to ignore new facts found during implementation or reality returned from operation. On the other hand, if code alone is absolutized, past reasons for judgment or discarded options disappear.

In ISE, SSoT is treated not as a fixed stone tablet but as a Living Spec updated through negotiation.

## 4. Synchronization is Not Only Forward

Conventional development methods have in many cases been based on a forward flow from requirements to design, design to implementation, and implementation to testing.

This flow is important. Externalizing intent first and implementing based on it is still valid.

But real-world development does not proceed in forward direction only.

Specifications may be reconstructed from existing systems. Design problems may be noticed only after starting to write code. Ambiguity of specifications may be seen only after writing tests. New constraints may be found from incidents during operation. Conflicts with past design intent may be exposed from a small fix.

In other words, the relationship between intent and reality is bidirectional.

```text
Forward Sync:
  Intent / Spec -> Code / Test / Reality

Reverse Sync:
  Code / Test / Runtime / History -> As-Is facts / Structural metrics -> Spec / Intent / Governance
```

ISE does not take either forward or reverse direction as the sole correct answer.

What is important is not which direction it started from, but whether intent and artifacts are continuously synchronized, and whether the system remains maintainable.

In existing software engineering discussions (including modern Spec-Driven Development), focus has heavily leaned toward Forward Sync—generating code from specifications. Even when Reverse Sync is mentioned, it is often viewed narrowly as an emergency recovery tool for legacy systems.

However, in ISE, Reverse Sync serves three distinct and essential purposes across the software lifecycle:

1. **Legacy Recovery (Rescuing Lost Intent):** Reconstructing As-Is behavior, business logic, and architectural rationales from undocumented or decayed codebases for human understanding and modernization.
2. **Drift Detection (Verifying Alignment):** Reverse-extracting specifications from actual implementation and comparing them against existing Living Specs to detect unintended deviations, undocumented workarounds, or AI hallucinations.
3. **Entropy Control (Governing Complexity):** Measuring code complexity, cognitive load, dependency coupling, and module boundaries to prevent runaway entropy and keep the codebase within cognitively manageable limits for both humans and AI.

Without Drift Detection and Entropy Control, continuous development—especially AI-accelerated development—inevitably descends into chaotic codebases that neither humans nor AI can safely reason about.

However, this does not mean "the order does not matter at all." Intent evaporates and transforms with time. Particularly, intent in the human head is most easily lost. Therefore, it becomes important to externalize intent while it is vivid and connect it to artifacts.

This background is organized in the supplementary paper "[Document Drift and Intent Decay](../ise/foundations/01_document_drift_and_intent_decay.md)" from the perspectives of cognitive load, reconstruction of memory, common ground, and decay of intent retention.

AI greatly reduces this synchronization cost.

If a developer explains the intent to AI immediately after implementation, AI can convert it to specifications, ADRs, test perspectives, or review perspectives. If AI is made to read existing code, it can organize As-Is facts. If diffs are shown to AI, it can ask back which intent might have changed.

By AI, it has become realistic to synchronize intent before it is lost.

### Redefining a Classic Challenge of Software Engineering

The assertion that synchronizing intent and artifacts and maintaining bidirectional feedback are vital is by no means entirely novel.

As Winston W. Royce pointed out in 1970 in "Managing the Development of Large Software Systems," the importance of iteration between design and implementation, early prototyping and verification, and feedback loops has been recognized since the dawn of software engineering. The fundamental issue that existing software engineering has grappled with under the names of "iterative development," "feedback control," and "change/configuration management" has always been precisely how to keep intent and reality aligned without letting them drift apart.

Artifacts change and decay over time, and as information passes between different artifacts, omission and misunderstanding (drift) inevitably arise. In past software engineering practices, the cost of closing these gaps was prohibitively high; thus, teams were forced to rely on formalistic, unidirectional processes despite knowing the intrinsic necessity of iteration.

What ISE does is not reinventing the wheel. **It reinterprets the classic challenges that past software engineering addressed as iteration and feedback through the structural lens of artifact decay over time and drift between artifacts, redefining them as "continuous synchronization in short cycles to prevent the loss and distortion of information."**

## 5. Not Fixing the Main Creator

In ISE, correctness is not judged only by who made the artifact.

Humans may write specifications and AI may write code. AI may read existing code and make a draft of documentation. Humans may directly write code and AI may later organize tests or design notes. AI may propose and humans may adopt, fix, or reject.

What is important is not the main creator but the synchronization state.

```text
Who made it:
  human / AI / human + AI

What truly should be asked:
  Does the artifact match the adopted intent?
  Does it not contradict other artifacts?
  Are reasons for judgment and constraints not lost?
```

### The Accountable Subject Is Fixed to Humans (Prohibition of Shifting Burden to the Receiver)

Not fixing the creator **does not mean that accountability for the artifacts can be blurred, nor does it mean that the burden can be shifted to the receiving side.**

With the rise of AI, the cost of generating artifacts such as code and documentation has become extremely low. However, what frequently occurs as a result is that **creators submit undigested artifacts without verifying or adequately understanding them, unilaterally imposing the immense cognitive load of reading, behavioral verification, and quality assurance onto the receiving side (reviewers and team members).** This is neither collaboration nor synchronization; it is merely shifting the burden.

A human who submits or adopts an artifact must fulfill the following responsibilities before passing it to the receiver:

1. **Complete Verification**: Thoroughly test behavior, consider edge cases, and personally guarantee quality.
2. **Detailed Accountability**: Be able to clearly explain and justify in the submitter's own words "why the implementation/diff is structured this way" and "what trade-offs were considered."

Artifacts submitted with an attitude like "I don't know the details because AI wrote it" or "It seems to work, so please take a look" are treated in ISE not as "artifacts synchronized with intent," but rather as **"undigested noise."** Regardless of who the creator is, the discipline of the submitter fully assuming verification and accountability without improperly depleting the receiver's cognitive bandwidth is essential.

By this thinking, the boundary of development main creator becomes flexible.

One person may implement. Another person may implement. AI may implement. Human and AI may cooperate.

However, this does not mean "it's the same no matter who makes it."

There is code that experienced developers should write. In areas where design judgment, responsibility boundaries, performance, concurrency, security, and long-term maintainability are important, human experience and judgment can greatly exceed AI. In such areas, it may be better for humans to design, humans to implement, and AI to be for assistance or review.

On the other hand, it is not necessary for experienced developers to do all work by hand. Routine implementation, drafts of tests, organization of documentation, explanation of existing code, listing of refactoring candidates, etc., can be left to AI or assisted by AI.

Even for inexperienced developers, AI can be an assistant rather than a mere substitute. There are scenes where shortness of experience can be compensated by AI explaining design intent, presenting review perspectives, and helping how to read existing code.

In other words, ISE is not a way of thinking that makes humans unnecessary. Nor is it a way of thinking for replacing human judgment with AI. Rather, it is a way of thinking for making it easier to separate areas where humans should judge and areas that may be delegated to or assisted by AI.

However, there is a condition for that to be possible.

Intent is externalized and synchronized with artifacts.

If intent is not synchronized, it is dangerous no matter who develops. Even experienced developers will rely on guesswork. AI will fix incorrect premises into code even faster.

Conversely, if intent is synchronized, the difference in creators becomes relatively small. Because both humans and AI can update artifacts toward the same intent.

## 6. Main Features of ISE

Features of ISE can be organized into several perspectives.

### Continuous Alignment

ISE does not align specifications, design, code, tests, operation, and judgment history only once.

It aligns them continuously during development, during review, during operation, and during maintenance.

Temporary deviations occur. Implementation may precede. Code may change first in incident response. What is important is not to have an illusion that deviations do not occur, but to detect, negotiate, and resynchronize deviations.

### Bidirectional Synchronization

In ISE, not only Forward Sync from specifications to code but also Reverse Sync from code or reality at runtime back to specifications, knowledge, and structural governance is necessary.

As long as existing systems exist and codebases evolve, Reverse Sync is not an exception. Rather, in the field, Reverse Sync operates as a continuous governance engine across three purposes:

1. **Legacy Recovery:** Reconstructing the meaning, logic, and operational realities of legacy or undocumented systems to make them understandable and maintainable.
2. **Drift Detection:** Continuously extracting implemented behavior from code to compare against Living Specs, exposing undocumented departures, spec degradation, or unintended implementation deviations.
3. **Entropy Control:** Continuously inspecting structural complexity, cognitive load, dependency graph degradation, and violation of architectural boundaries.

However, Reverse Sync is not all-powerful. What can be directly reconstructed from code is mainly As-Is reality, and complete reconstruction of original As-Intended judgment remains difficult. Therefore, Reverse Sync works in tandem with Forward Sync and negotiation loops.

### Management of Cognitive Load and Entropy Control

In ISE, it is a premise that both humans and AI have cognitive limits.

Humans cannot always grasp the entire huge codebase. AI also, if handed a huge context, misses important information, hallucinates, or gets dragged by irrelevant noise. As code volume grows—especially under AI-accelerated velocity—entropy naturally compounds.

Therefore, in ISE, **Entropy Control** is a first-class engineering concern. It is important to keep the target range small: localizing necessary intent and artifacts for each function, domain, subsystem, change unit, or responsibility boundary.

This is not a simple documentation split. It is using Reverse Sync to actively monitor complexity metrics (such as cognitive complexity, cyclomatic complexity, coupling, and circular dependencies) to keep system design itself in a form that can be locally understood, changed, and verified.

By maintaining a structure that can be locally reasoned, both humans and AI can change the system safely without cognitive collapse.

### Intent Recovery and Legacy Preservation

In ISE, work to reconstruct intent from existing code or operational reality is emphasized as **Legacy Recovery**. However, that is not an ideal end state for all development, but a rescue response to degraded situations.

What can be reconstructed from code is mainly current behavior. That is As-Is.

Why that judgment was originally adopted, which proposals were discarded, who prioritized which trade-offs, and how intent transformed during implementation cannot be completely reconstructed from syntax alone.

Therefore, while Legacy Recovery is a vital lifeline for legacy systems, the daily strategy of ISE is not to rely solely on massive post-facto recovery, but to continuously run Drift Detection and Entropy Control so intent is never lost in the first place.

### AI as a Prosthetic

AI in ISE is not just a code generator. It is a prosthetic that helps human externalization of intent.

Weak conversion steps differ from person to person. Some people are not good at putting intent into sentences. Some are not good at dropping specifications into code. Some are not good at explaining design from code. Some are not good at verbalizing review perspectives.

AI can compensate for those weak steps. Ask questions to draw out intent. Structure fragmented notes. Estimate design intent from code diffs. Convert specifications into implementation proposals. Convert conversations into ADRs or Issues. Organize differences between old documents and current code.

In this sense, AI is not so much a replacement for humans as an existence that compensates for cognitive weaknesses of each human and supports places where intent is likely to be missing.

However, AI does not make human judgment unnecessary. AI can make candidates. Find differences. Arrange explanations. But humans must decide what to adopt, what to discard, and which responsibility to take.

### Negotiation Loop

Synchronization does not hold by automatic conversion alone.

When documentation and code deviate, it may be a bug in the code. It may be a specification change. It may be a remnant of an old specification. It may be a temporary workaround in operation.

Therefore, judgment is needed after detecting a deviation.

In ISE, this judgment is perceived as a negotiation loop.

```text
Drift
  -> Analyze
  -> Compare
  -> Human judgment
  -> Update Intent or Reality
  -> Resync
```

AI can support detection of diffs, organization of candidates, estimation of impact range, and creation of update proposals. But what to adopt, what to discard, and which risk to accept is ultimately a human judgment.

### Eventual Consistency

ISE does not require a state of perfect match always.

In real-world development, temporary inconsistency is inevitable. What is important is not to leave it.

While allowing short-term deviations, we aim for a state where intent and artifacts eventually converge. In this sense, ISE assumes eventual consistency.

### Provenance and Reliability

To handle the mosaic of information, the source and reliability of information must not be dropped.

Facts obtained from code, expectations obtained from tests, reality obtained from operation logs, background obtained from human memory, and hypotheses obtained from old documents should not be treated with the same weight.

In ISE, it is necessary not just to summarize information but to distinguish where information came from, how much it can be trusted, and whether it has been checked against current Reality.

### Maintenance as Cure

Synchronization is not just aligning documentation and code.

It also includes intervening in differences, debt, dangerous spots, complexity, lack of testing, boundary violations, and design degradation.

Confirm health of code, refactor as necessary, restore design, supplement tests, and repay debt. Such maintenance activities are also an important part of synchronizing intent and Reality.

## 7. Intent is Born even during Implementation

Intent is not something that is completely defined at the beginning.

There are things found only after starting implementation. While writing code, one may notice a better design. After writing tests, one may notice ambiguity of the initial specification. After operating, constraints that were not assumed may appear.

In other words, implementation is not a mere execution of intent. Implementation itself may give birth to new intent.

What is important at this time is not to lose the newly born intent.

In many cases, a developer does not clearly realize that "now, a new intent was born." They judge naturally during implementation, change code, and proceed to the next problem. The "Why" of that moment fades rapidly with time.

ISE deals with this phenomenon by lowering synchronization cost.

When a new judgment is born, it can be recorded immediately. AI can ask back from code diffs. In each PR, why it was changed so can be organized. After implementation, it can be returned to design or specifications.

Complete automatic detection is difficult. But if the friction of synchronization is lowered, it becomes easier to catch intent before it is lost.

## 8. What ISE Reduces

When ISE functions well, it can reduce the following loads:

- Document drift
- Recognition mismatch between specification and implementation
- Time looking for why it was done so
- Cognitive load during onboarding
- Excessive confirmation during review
- Rework due to "I thought it meant that"
- Dependence on siloed (individual-dependent) knowledge
- Frequency and difficulty of software archaeology
- Accumulation of haphazard implementations by AI

This does not mean all meetings or coordination are eliminated.

Rather, it means reducing unnecessary confirmations, excessive explanations, recognition mismatches, and reworks that occurred due to desynchronization of intent.

## 9. What ISE Does Not Solve

ISE is not all-powerful.

What ISE handles is mainly synchronizing defined intent to design, code, tests, documentation, and operation.

Therefore, the following problems are not responsibilities of ISE alone:

### State where Intent Itself Does Not Exist

In a state where requirements, constraints, priorities, and success conditions are not defined, such as "I want you to make it feel right," "I want you to make it because competitors are doing it," or "Anyway, I want you to put it out quickly," ISE does not function sufficiently.

ISE can synchronize intent, but it does not decide non-existent intent on its own.

The problem of making implementable intent from ambiguous requirements is the area of IGE.

### Value Judgment and Acceptance of Responsibility

What to prioritize. What to discard. Which risk to accept now. Whose convenience to pass to what extent. Where to decide in incomplete information.

These are not problems of synchronization but problems of value judgment and acceptance of responsibility.

ISE can support synchronizing those judgments to artifacts after the judgments are made. But it does not substitute for the judgment itself.

### Deadlines, Estimation, and Schedule Management

ISE is not a methodology for deadline management.

Estimation, schedule, resource allocation, contracts, budget, and organizational convenience are the area of project management.

ISE can improve materials for estimation or judgment by making development transparent and making deviations between intent and reality easier to see. But it does not guarantee deadlines.

If these are confused, ISE is misused as a "management device for making things faster." This should be avoided.

### Ambiguity on the Management Side

Vibe Management is a problem outside of ISE.

The management side does not verbalize intent, does not accept responsibility, hides undetermined issues, does not manifest political constraints, and pushes only deadlines. In such a state, ISE rather visualizes problems.

How to handle visualized problems is the area of IGE and ODF.

## 10. Why IGE and ODF Become Necessary

ISE assumes that intent exists.

In reality, however, intent is not clear from the beginning. Intent candidates must be found among fragmented signals such as market, customer, operation, sales, support, developer mismatch feeling, logs, metrics, incidents, and competitor research.

This area should be handled not by ISE but by IGE.

```text
IGE:
  ambiguous requirement / signal -> implementable intent / issue

ISE:
  defined intent -> synchronization to spec / code / test / docs / operation
```

It is dangerous to mix both.

If requirement exploration and implementation synchronization are pushed into one huge process or one AI, context is contaminated and axis of judgment becomes ambiguous. Business convenience and engineering constraints are entangled, and as a result, ambiguous requirements are pushed downstream in the form of neat specifications.

Therefore, IGE and ISE must be separated and connected by a limited interface.

This overall structure is ODF.

ODF is a way of thinking for orthogonally separating requirement definition and implementation synchronization and connecting them through Issues, Living Spec, SSoT, runtime feedback, pain point, drift signal, etc.

## 11. Misuse and Degradation of ISE

ISE creates other problems if misused.

For example, if ISE is used as a "mechanism to make humans into synchronization devices," it cuts off developer contemplation. All judgments are made to be documented immediately, taking away margins and reducing mismatch feelings or objections.

If ISE is used as a "mechanism to increase development speed with AI to meet deadlines," it only ends up reinforcing the conventional overemphasis on speed.

If ISE is used as a "mechanism to increase documentation," only formal documents increase and the essence of synchronization of intent is lost.

If ISE is used as a "mechanism to force compliance with specifications," reaction from reality disappears. Facts found during implementation, constraints returned from operation, and developer mismatch feelings no longer return upstream.

ISE needs the right of reaction.

The development side must be able to return reduction of specifications, redesign, pending, objections, and presentation of risks. Otherwise, ISE becomes pushing instead of synchronization.

## 12. Summary: ISE is an "Implementation-side Framework for Not Losing Intent"

The core of ISE is simple.

Synchronization of intent is important.

In the AI era, the speed of writing code has increased dramatically. But if what should be made, why it is done so, which constraints should be observed, what was discarded, and which judgment was adopted are lost, being able to make quickly rather becomes dangerous.

ISE is a way of thinking for continuing to synchronize code, tests, documentation, specifications, operation, and judgment history to a common intent.

Thereby, we aim for a state where artifacts are updated toward the same intent whether humans develop, AI develops, or humans and AI cooperate.

However, ISE does not produce intent as if by magic. Nor does it guarantee deadlines. Nor does it absorb ambiguity on the management side.

To make intent from ambiguous requirements, IGE is needed. To connect IGE and ISE without mixing them, ODF is needed.

In the next paper, we organize upstream problems that ISE does not handle, namely how to handle ambiguous requirements and Vibe Management, and why IGE and ODF become necessary.
