# From Mosaic Information to Understanding and Synchronization

This draft explores an ISE approach to forming understanding from fragmented information and continually reconciling it with reality. It does not prescribe a particular tool or a fixed sequence.

## Mosaic information and a PreSpec

Information needed to understand an existing system may remain in fragments across documents, code, operational records, and people's knowledge. We call these fragments Mosaic information (for background and definition, see [Document Drift and Software Archaeology](../../philosophies/02_document_drift_and_software_archaeology.md) and [Glossary: Mosaic of Information](../glossary.md#mosaic-of-information)).

A PreSpec records provisional understanding by reconciling Mosaic information and code analysis, informed by foundational domain knowledge (also written as `Pre-Spec` in documents such as the [Lane Model](../../lane-model/01_philosophy_and_concepts.md)). It is a shared working surface for stakeholders to compare and revise their understanding, as well as a record of investigation. Preserve sources and distinguish observed facts, inferences, stakeholder views, agreements, and unknowns. A fact is limited to what its source actually establishes.

## An overall working pattern

This pattern reconciles fragmented information with code analysis to create a PreSpec, then develops it through dialogue with people. The activities repeat as needed, and people review and correct the work throughout.

This pattern does not assume recovering a complete specification for the entire system in a single pass. Because ISE assumes ongoing synchronization, investigation starts from the scope needed for the problem or change at hand (change-level) and expands incrementally using the level of confidence available at each step. Moreover, in everyday development where specifications already exist, teams do not necessarily create a separate heavyweight PreSpec for every change; the same structure can be applied lightly by treating the existing specification as one source to reconcile against the changed scope.

The diagram shows relationships between information, not a requirement to complete Mosaic gathering and code analysis before a single merge. Both activities can proceed in parallel; findings can be reconciled and reflected in the PreSpec and investigation results as they emerge. For example, an initial code reading can prompt a question to a stakeholder, whose answer can lead to further investigation of both Mosaic information and code.

In the [Lane Model](../../lane-model/01_philosophy_and_concepts.md), the organized Mosaic information, code analysis results, and PreSpec can be represented as separate Nodes, synchronized through reconciliation and updates. The choice and granularity of Nodes depend on the task. Rather than fixing a merge order, the important considerations are which versions of information were reconciled and how changes and unresolved issues are reflected in related artifacts. Parallel work does not synchronize updates automatically.

```text
Domain understanding ─┬→ Mosaic gathering (incl. existing specs) ─┐
                      └→ Code analysis (safety net / refactor) ───┤
                                                                  ↓
                                                           Reconciliation
                                                   ┌──────────────┴──────────────┐
                                                   ↓                             ↓
                                     [Lightweight delta check]                PreSpec
                                  (resolved locally via existing spec)  (large gaps/uncertainty)
                                                   │                             ↕
                                                   │               Triage and human review
                                                   │                             ↓
                                                   │        [Gate] Intent unclear or new decision needed?
                                                   │          └─ Yes → IGE/ODF (or PM/stakeholder alignment)
                                                   └──────────────┬──────────────┘
                                                                  ↓
                                                     Action, verification, ongoing sync
```

1. **Understand the domain.** Investigate the business, terminology, rules, and user problems. Separate known information from unverified assumptions. This understanding guides both streams of investigation.
2. **Gather Mosaic information.** Collect customer reports, stakeholder knowledge, historical material, and other fragments, recording sources and dates. Distinguish facts, inferences, and unknowns, including when AI assists with synthesis. People check sources and revise the material throughout; labels alone do not guarantee accuracy.
3. **Assess and analyze code.** Define the scope and baseline revision, then examine structure, dependencies, behavior, data, and external connections as appropriate. Because refactoring presupposes the existence of tests, it cannot be performed safely without them. If a PreSpec can be formed through reading and runtime observation without modifying code, proceed without refactoring; only when complexity impedes understanding and refactoring during investigation is necessary despite insufficient tests, first establish tests—such as characterization tests or Golden Master records—to pin down current behavior so unintended changes can be detected. Record the scope, unexamined areas, and evidence; distinguish implementation facts from inferences about domain meaning. People review results and investigate again as needed.
4. **Reconcile the information and create a PreSpec.** Record agreement, contradictions, gaps, and unknowns between the two streams, with their sources. A PreSpec is a shared working document preceding a settled specification. Depending on its purpose it may take a form leading to a PRD, ADR, specification, or another document, while making the provisional status of the whole and of individual statements clear. For small everyday changes, it may take a lightweight form—such as delta-level investigation notes within an existing specification or work item—rather than a standalone document.
5. **Gather human input and revise.** Review the PreSpec with customers, domain experts, developers, and others; incorporate additional information, differing views, and decisions. To avoid overloading human reviewers, prioritize checks where uncertainty, change impact, or risk is highest. People return to code and original sources as necessary. Current implementation behavior and what should happen in the future are separate judgments. When past intent is lost and future behavior must be decided anew, connect as needed to an intent-formation flow such as IGE and ODF—including informal equivalents like consulting a PM or stakeholders where explicit frameworks are not used. Human input and review also occur throughout, not only after reconciliation.
6. **Act and continue synchronization.** Reach agreement for the relevant scope, write tests that follow the PreSpec or settled specification, fix or update the implementation where code diverges from the agreed behavior, and check implementation, tests, documents, and operational results. Record decisions and unresolved questions, and reconcile affected areas when information or behavior changes. Resolving one problem does not make the entire PreSpec final.

### Scoping and review triage

Attempting to recover specifications for an entire system at once inflates costs due to hidden edge cases and context volume, and results often go stale before work finishes. As in the Strangler Fig pattern and incremental specification, scope investigation into small, manageable slices:

- **Change-level feature slices**: Cut vertically around the feature or defect being modified and verify the inputs, outputs, and behavior needed for that slice.
- **Clear boundaries**: Start from boundaries where a safety net (such as characterization tests) is practical to place, such as external APIs, UI/batch entry points, or cohesive dependency boundaries.
- **High-churn or high-impact hotspots**: Prioritize areas that change frequently in Git history or carry high business criticality and failure risk.

Furthermore, while AI makes extracting and drafting information cheap, human review time and attention remain scarce. Reviewing everything equally leads to review fatigue and stalls progress. Triage review effort along two axes to focus human judgment where it matters most:

- **Impact and criticality**: Business-critical paths, modules with high fan-in dependencies, or areas scheduled for immediate change.
- **Uncertainty and weak grounding**: Claims not anchored to code symbols or line references, contradictions between sources, or interpretations that fluctuate across repeated AI generations.

Delegate low-risk, self-contained parts to automated tests, static checks, or post-change diff review, while routing high-risk and uncertain areas to human review and Gates (IGE/ODF or stakeholder alignment).

## Understanding code and reconciliation

Define the scope of code analysis and examine structure and behavior. Refactoring presupposes the existence of tests; without tests, code cannot be refactored safely without risking behavioral regressions. Where a PreSpec can be constructed through code reading and runtime observation alone, leave the code untouched during investigation and follow the standard synchronization path: write tests based on the PreSpec and agreed specification, then fix and synchronize any diverging code.

Only when complexity prevents comprehension and you need to refactor *before* the specification is settled, first establish a safety net—such as characterization tests or recorded outputs (e.g., Golden Master testing)—to pin down current behavior. This temporary safety net exists solely to lock in existing behavior and detect unintended drift; it does not imply that the current behavior is correct or ought to be preserved (once the specification is settled, replace or update these tests to reflect the intended behavior and fix bugs accordingly).

Preserve the pre-change version and state, relate evidence across versions, and check effects using the safety net, available tests, execution results, or other evidence. Such checks cover only the conditions exercised; recording an existing defect is distinct from deciding that it should be retained. If changes or sufficient protection are unavailable, record the constraint and continue investigation accordingly.

Distinguish implementation facts from hypotheses about their domain meaning, and compare both with the PreSpec. Reconciliation means more than combining prose: identify agreement, contradictions, omissions, and unverified points, and update understanding with its supporting evidence. A discrepancy does not decide which side is correct. Stakeholders investigate and judge, then update intent, documents, or implementation as appropriate.

For example, if code behavior conflicts with a customer's expectations, record both with their sources. If the history is unknown, retain that uncertainty while stakeholders consider what should happen now. Implementation facts alone do not decide desired behavior.

Even after sources are merged, the whole document is not necessarily verified or agreed. Keep its PreSpec status and the verification state of individual statements visible. Agreement sufficient to address one problem can coexist with unknowns elsewhere.

## Understanding the present and judging what should happen

For the same behavior, distinguish code-level implementation, observations under particular conditions, customer and stakeholder accounts, and candidates for future behavior. Record the observed version, configuration, inputs, and environment where known. While runtime observation and dynamic analysis are useful for narrowing down where features live, they depend on the input scenarios exercised and do not cover or guarantee unexecuted paths. A customer report is important evidence, but does not by itself establish behavior or intent under every condition. Accounts based on the same underlying source should not be counted as independent corroboration.

For example, code and execution may show that an application rejects a request under a certain condition while a customer expects it to be accepted. The reason for the rejection and its correctness remain unconfirmed. Stakeholders can examine the business conditions and agree on the scope of requests that should be accepted before implementing and verifying a change. A present requirement can be agreed even if past intent is unknown.

For important claims, review not merely whether a citation exists but whether its content supports the claim, from a perspective independent of authorship. Choose scope and verification methods in proportion to the problem's impact, and retain what could not be examined.

## From PreSpec to an official specification

When the PreSpec has been reviewed sufficiently for its purpose, it can be treated as an official specification for a clearly defined scope with approval from the appropriate stakeholders. This transition requires care. A polished document or a merge of multiple sources does not by itself establish correctness, completeness, or agreement.

Lost intent and past decisions may be impossible to recover from the evidence that remains. If stakeholders also lack knowledge of the original circumstances and cannot determine whether the behavior is correct, it may be difficult to promote the entire PreSpec to an official specification. Unknowns that persist after investigation must not be filled with conjecture and presented as settled facts. In particular, information that can be plausibly regenerated from code using general knowledge (plausibly guessable) should be triaged as a candidate requiring confirmation rather than promoted without its underlying external constraints or rationale (see [Information Boundary, Duplication, Gates, and Reverse](../../document_engineering/04_information_boundary_and_gates.md)).

Even so, verified facts and agreed portions can be identified while unresolved portions remain provisional. Alternatively, stakeholders can decide future behavior anew based on present information and goals. Neither approach means the historical specification has been completely recovered. Because forming new intent and promoting a PreSpec to an official specification involve value judgments and requirement decisions rather than mere synchronization of facts, establish agreement through an IGE/ODF process as needed—or, where those frameworks are not formally adopted, through equivalent stakeholder and PM alignment to obtain agreed intent.

Before approval, review the version, scope and exclusions, evidence for individual claims, the distinction between implementation facts and intent, unresolved contradictions and unknowns, and areas that remain unverified. If issues remain, make their implications and intended treatment explicit so stakeholders can approve with that understanding. If only part is approved, identify that scope rather than treating the entire document as approved.

Separately verify whether the description matches evidence (verification), whether stakeholders agree that future behavior matches their intent (validation), and whether the changed implementation satisfies that agreement—aligning with the distinction between verification and validation in ISO/IEC/IEEE 29148. Even after approval, the decision applies to a particular scope and point in time. Revisit the specification and its evidence as intent, implementation, and operational conditions change, and continue synchronization.

This limitation shows why ongoing synchronization of intent and artifacts, with records of changes and decisions, matters. Synchronization cannot guarantee recovery of information already lost, but it can help uncover later drift and lost decisions and support future understanding.

## A practical starting point

In one product with no specifications or documentation and no changes for a long time, behavior that customers found incorrect required attention. Foundational domain knowledge was assembled first, and scattered information was organized into a PreSpec that explicitly distinguished facts from inferences. People reviewed and corrected it throughout, added findings from reading the code, and revised it through discussion with stakeholders familiar with the customers and product. One problem was addressed and released.

The initial PreSpec lacked sufficient information and contained inaccuracies. Even so, it provided a starting point for investigating the code and comparing understanding with stakeholders, helping the team make progress on the problem. A PreSpec can open a path to better understanding without being perfect from the outset.

One case cannot establish general effectiveness, nor does it justify treating inaccurate statements as settled specifications. It is important to make the provisional status and unknowns visible, correct errors, and continue synchronizing the PreSpec and implementation with new understanding and decisions. The PreSpec serves as a shared surface that can be revised through investigation and discussion.

## Continuous synchronization

Reverse does not end with initial recovery. Rather than aiming for system-wide completeness at once, ISE continually revisits prior understanding and artifact relationships as implementation, intent, or operational conditions change. Changes and new observations can trigger reconciliation of evidence and understanding in the affected scope, with decisions and unresolved questions recorded. While some efforts such as system-wide migrations start by mapping a broad area, everyday work typically starts from a specific problem or change target and expands only as far as needed. When pausing investigation mid-way or handing off to another person or AI agent, record the baseline version, examined and unexamined scope, decisions made, unresolved questions, and conditions for resuming.

In everyday development with existing specifications, teams do not need to spin up a heavyweight PreSpec for every change. Reconciliation can stay lightweight and limited to the relevant scope: locate specifications related to changed code, treat the existing specification itself as one source to check against actual behavior and undocumented assumptions, and update the specification or implementation when discrepancies are local. A fuller PreSpec is reserved for areas where divergence or uncertainty is too large for lightweight alignment. Systems with little specification or knowledge make this same ongoing synchronization challenge especially visible.

## Related approaches and open questions

Reverse spans program comprehension, specification recovery, traceability, runtime and history analysis, and domain knowledge acquisition. Existing work on design recovery already considers domain knowledge when deriving higher-level representations from implementation. ISE therefore does not claim that using Mosaic information itself is novel. It examines provenance, uncertainty, reconciliation into a PreSpec, human judgment, and subsequent synchronization together. See also [Artifact Relationships and Traceability](../foundations/02_artifact_relationship_traceability.md).

AWS Reverse Spec Flow (RSF) is one practical reference: it combines baseline and scope, a code graph, analysis from multiple perspectives, source evidence, specification authoring, and independent audit. It can inform code investigation and checks of evidence and unexamined scope. Structural coverage does not establish complete domain understanding. AI Modernization Flow (AIMF) offers ideas about stages, decision points, and execution discipline for work involving changes. Neither is a mandatory ISE procedure; both should be compared with other approaches.

Further work includes the reliability and age of sources, the difference between coverage and semantic understanding, resolving contradictions and reaching agreement, triggers for renewed reconciliation, and checking the effects of refactoring. Cases should be used to evaluate candidate quality, review effort, unresolved questions, and results after changes.

## References

- [Document Drift and Software Archaeology](../../philosophies/02_document_drift_and_software_archaeology.md) — the repository's discussion of fragmented information and understanding debt.
- [Artifact Relationships and Traceability](../foundations/02_artifact_relationship_traceability.md) — relationship discovery, maintenance, and related research.
- Chikofsky and Cross, [Reverse Engineering and Design Recovery: A Taxonomy](https://www.dsg.tuwien.ac.at/Teaching/Courses/SWV/papers/chikofsky90.pdf), 1990 — reverse engineering, design recovery, and restructuring.
- Xia et al., [Measuring Program Comprehension: A Large-Scale Field Study with Professionals](https://xin-xia.github.io/publication/TSE17.pdf), 2018 — a field study of comprehension across tools and activities.
- Razzaq et al., [An Empirical Assessment of Baseline Feature Location Techniques](https://link.springer.com/article/10.1007/s10664-019-09734-5), 2020 — evaluation of techniques for locating feature implementations.
- Wilde and Scully, [Software reconnaissance: Mapping program features to code](https://onlinelibrary.wiley.com/doi/pdf/10.1002/smr.4360070105), 1995 — narrowing feature-related code via execution scenario comparison and its coverage limits.
- Ernst et al., [Daikon User Manual](https://plse.cs.washington.edu/daikon/download/doc/daikon.html) — inferring invariants from runtime observations and their dependence on observed executions.
- ISO/IEC/IEEE 29148:2018, [Systems and software engineering — Life cycle processes — Requirements engineering](https://www.iso.org/obp/ui?_escaped_fragment_=iso%3Astd%3Aiso-iec-ieee%3A29148%3Aed-2%3Av1%3Aen) — distinguishing verification of requirement descriptions from validation against stakeholder intent.
- Rayson et al., [Recovering Legacy Requirements](https://ucrel.lancs.ac.uk/people/paul/publications/rgs99_refsq.pdf) — requirements recovery using incomplete documents, human knowledge, and existing systems.
- Roehm et al., [How Do Professional Developers Comprehend Software?](https://memphis-cs.github.io/comp-7085-8085-2012-fall/papers/Roehm2012ICSE.pdf), 2012 — an observational study of program comprehension in practice.
- Subramanian et al., [Recovering Software Design from Interviews Using the NFR Approach](https://onlinelibrary.wiley.com/doi/10.1155/2014/124701), 2014 — a design recovery case using interviews and stakeholder feedback.
- Demeyer, Ducasse, and Nierstrasz, [Object-Oriented Reengineering Patterns](https://oorp.github.io/) — patterns for understanding and reengineering existing systems.
- Feathers, *Working Effectively with Legacy Code*, Prentice Hall, 2004 — using characterization tests to establish a safety net before understanding and modifying untested code.
- Fowler, [Strangler Fig Application](https://martinfowler.com/bliki/StranglerFigApplication.html), 2004 — an incremental strategy for understanding and replacing parts of a system by boundary rather than all at once.
- AWS, [Reverse Spec Flow introduction](https://aws.amazon.com/jp/blogs/news/aidm-introducing-reverse-spec-flow/) and [sample](https://github.com/aws-samples/sample-reverse-spec-flow) — a practical specification recovery flow.
- AWS, [AI Modernization Flow introduction](https://aws.amazon.com/jp/blogs/news/aidm-introducing-ai-modernization-flow/) and [sample](https://github.com/aws-samples/sample-ai-modernization-flow) — a modernization workflow and execution discipline.
