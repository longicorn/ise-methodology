# Why Reverse Is Continuously Needed

This draft sets out why ISE treats Reverse as an ongoing development activity. Its comparison with existing standards and research requires further work.

## Work obscured by the Forward view

Development is often described as a Forward progression from requirements to specifications, design, implementation, and verification. Yet changes frequently concern a system that is already running. Before making a change, people read code, seek specifications and history, ask stakeholders, and investigate current behavior and its background. These are Reverse activities: discovering intent and relationships from existing artifacts.

Reverse engineering, design recovery, program comprehension, and traceability have substantial research and established techniques. Standards and systematic guidelines also address parts of this space, such as ISO/IEC/IEEE 14764 for maintenance processes, ISO/IEC/IEEE 29148 for iterative requirements engineering (including the distinction between verification and validation), and CMU/SEI Architecture Reconstruction guidelines for recovering as-built architectures and reconciling them with as-designed models using human knowledge. Reverse is not absent. Nevertheless, combining these techniques in everyday development and continuous intent synchronization can depend on local experience and individual exploration. There is room to organize the ongoing Reverse activities ISE addresses into a more coherent approach alongside frameworks describing Forward lifecycles. Detailed comparison and empirical evaluation against individual standards and methods remain open work.

## Missing documentation is a conspicuous case

When specifications, documentation, and knowledgeable maintainers are missing, investigation must begin with code, operational behavior, customer reports, and fragments of history. Developers collect scattered information, infer relationships, and seek people able to make decisions. This work resembles [software archaeology](../../philosophies/02_document_drift_and_software_archaeology.md).

Documentation alone does not eliminate investigation. It may be stale, its scope uncertain, or its relationship to code unrecorded. People must still determine what actually happens and which descriptions remain applicable.

A system with no specifications or documentation and uncertain code reliability is a particularly conspicuous example of lost synchronization among intent, artifacts, and implementation. It can result from accumulated drift and failures to synchronize. Not all missing specifications are caused by drift, however: intent may never have been documented. In either case, the problem shares a basis with smaller everyday discrepancies: missing information and relationships make understanding and change harder.

## When the implementation itself may be wrong

When specifications are lost, the basis for deciding whether code is correct or its behavior should be retained may also be missing. The fact that a system runs does not establish that its behavior is desirable. Code alone cannot reliably distinguish defects, historical constraints, and changes in customer expectations.

Therefore, investigating actual behavior from evidence must be distinguished from stakeholders judging desired behavior. If past intent cannot be recovered, or no longer fits present needs, people may need to form and agree on new intent using current information. This problem is broader than recreating lost documentation from code.

Some existing requirements-recovery research already uses multiple documents and human knowledge to consider what to retain for a changed business (see [Rayson et al.](https://ucrel.lancs.ac.uk/people/paul/publications/rgs99_refsq.pdf)). Individual approaches should not all be characterized as mere extraction of current behavior. They need comparison in terms of how they address understanding the present, assessing correctness, agreeing future intent, and synchronization. Our investigation did not identify a unified approach covering these activities that is widely shared in practice; it did not prove that none exists.

## Drift between artifacts and continuous synchronization

ISE concerns continuous synchronization of artifacts such as specifications, designs, code, tests, and operational records, and their relationships to intent. Even when Forward development creates artifacts and records their relationships, delays between changes, omissions, implicit decisions, and new operational discoveries can produce drift in their contents or relationships. This is a challenge of representing a changing system in multiple forms, not necessarily an individual failure.

Synchronization requires more than carrying changes from intent to implementation. It also requires Reverse investigation of implementation and operations and reconsideration of their relationships with other artifacts. Missing specifications make the need especially visible, but Reverse checks are also needed as appropriate when artifacts exist. This does not mean recovering a complete specification for the entire system in one pass. Because ISE assumes ongoing synchronization, investigation starts from the scope needed for a given change or problem and expands incrementally as required.

Everyday changes may require tracing impact from implementation, comparing tests or execution results with specifications, and asking stakeholders about undocumented assumptions. These Reverse activities reveal discrepancies and decisions to reflect in relevant artifacts, preserving relationships for future changes. Investigation scope and depth depend on the purpose and impact of the change.

The [Lane Model](../../lane-model/01_philosophy_and_concepts.md) provides a conceptual layer for representing artifacts and states and the synchronization and transformation between them. In that model, artifacts and states can be represented as Nodes and synchronization or transformation activities as Lanes. Nodes are not limited to artifacts.

## Limits of recovery and the need for judgment

Code and execution results provide evidence of current behavior; they do not by themselves establish original intent or what should happen in the future. Lost decisions may be impossible to recover completely, and stakeholders may need to decide anew using current knowledge. It is important to distinguish facts uniquely reconstructible from code without guesswork (reconstructible), plausible interpretations inferred from general knowledge without verifiable grounding (plausibly guessable), and lost decisions or constraints (non-reconstructible) (see [Information Boundary and Reconstructibility](../../document_engineering/04_information_boundary_and_gates.md)). Forming and agreeing on intent from customer expectations and value judgments connects to IGE and ODF—whether through an explicit framework or through informal practices such as consulting a PM or stakeholders when new decisions are needed—while ISE concerns synchronization of agreed intent and artifacts.

The [next document](02_mosaic_and_reconciliation.md) considers forming provisional understanding from fragments and reconciling it with code and stakeholder knowledge. See also [Artifact Relationships and Traceability](../foundations/02_artifact_relationship_traceability.md).

## References

- Chikofsky and Cross, [Reverse Engineering and Design Recovery: A Taxonomy](https://www.dsg.tuwien.ac.at/Teaching/Courses/SWV/papers/chikofsky90.pdf), 1990 — a taxonomy of reverse engineering and related activities.
- ISO/IEC/IEEE 14764:2022, [Software engineering — Software life cycle processes — Maintenance](https://www.iso.org/standard/80710.html) — standard defining software maintenance processes.
- ISO/IEC/IEEE 29148:2018, [Systems and software engineering — Life cycle processes — Requirements engineering](https://www.iso.org/obp/ui?_escaped_fragment_=iso%3Astd%3Aiso-iec-ieee%3A29148%3Aed-2%3Av1%3Aen) — iterative requirements processes, traceability, and the distinction between verification and validation.
- Kazman, O'Brien, and Verhoef, [Architecture Reconstruction Guidelines](https://www.sei.cmu.edu/library/architecture-reconstruction-guidelines-third-edition/), CMU/SEI, 2003 — recovering as-built architectures, view fusion, and incorporating human knowledge.
- Xia et al., [Measuring Program Comprehension: A Large-Scale Field Study with Professionals](https://xin-xia.github.io/publication/TSE17.pdf), 2018 — a field study of program comprehension.
- [Document Drift and Software Archaeology](../../philosophies/02_document_drift_and_software_archaeology.md) — the problem as discussed in this repository.
