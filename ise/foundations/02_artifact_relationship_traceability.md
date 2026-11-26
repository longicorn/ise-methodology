# Artifact Relationships and Traceability

## Purpose

This document frames the discovery and maintenance of relationships among intent, specifications, designs, code, tests, documentation, and operational records as an ISE problem. Developers routinely spend time reconstructing relationships that are necessary for understanding and changing a system, but are often left implicit.

This is an initial problem statement and research direction, not a settled theory or implementation specification.

## The Problem

Software work produces multiple artifacts that express different aspects of a shared intent. Code describes how behavior is implemented; tests provide executable evidence of expected behavior; commit history records changes and, when well described, their reasons; comments can preserve constraints, rejected alternatives, and other rationale. These artifacts are useful individually, but understanding a feature often requires discovering how they relate.

Even in a well-maintained repository, locating the code, tests, decisions, and documentation related to a feature can require substantial effort. Developers develop repeatable search habits, but still rely on memory, inference, and consultation with others. The effort recurs when people join a project, modify unfamiliar areas, investigate defects, or synchronize artifacts after a change.

This is not only a documentation problem. Documentation may be absent, or it may exist without explicit links to implementation. In either case, people must reconstruct relationships among intent and artifacts. Documentation itself also has a synchronization cost: it must be updated as practice and implementation change, and its value falls when readers cannot tell whether it remains accurate.

The same problem affects AI-assisted development. An AI agent that does not know which artifacts are relevant must search, read, and infer before it can make a change. This consumes time and tokens, and can lead to missed dependencies or changes in the wrong location. Making relationships discoverable and reusable may reduce this repeated exploration, though the benefit must be measured in practice.

## Relationship to ISE

ISE synchronizes representations of accepted intent with implementation reality. That synchronization depends not only on artifact contents, but also on relationships among artifacts. A specification and an implementation can each be current in isolation while their correspondence remains unknown or has drifted.

ISE should therefore investigate how relationships among intent-bearing and implementation-bearing artifacts can be discovered, evidenced, checked, and reused. This includes relationships such as:

- a requirement, feature, or behavior is implemented by code;
- a test verifies a behavior described by a specification;
- a decision explains a design choice in the implementation;
- a comment records a constraint or explains why an alternative was rejected;
- an operational observation leads to a change in intent or implementation.

These relationships may be explicit, inferred from evidence, or unknown. A relationship should not be treated as certain solely because two artifacts occur in the same commit or contain similar words.

## Forward and Reverse Directions

Forward development can create useful evidence at the time artifacts change. If a work request, feature or behavior, document, code, and tests are connected to a Git change, later analysis can use the commit, changed paths, and diffs as evidence. Repeated co-changes and links to the same work item may strengthen a candidate relationship. Git history is a practical starting point because it records changes, but commit co-occurrence does not prove that artifacts are related in meaning.

Reverse analysis starts with existing artifacts whose relationships may never have been recorded. It must infer candidates from available evidence such as text, repository structure, tests, dependencies, execution behavior, and history. This is harder because the evidence may be incomplete or misleading. Poor structure, such as a large class that combines unrelated responsibilities, can make file-level links uninformative. Complexity and dependency analysis may indicate when deeper analysis or refactoring is needed before reliable mapping is possible.

The two directions are therefore asymmetric. Forward practices can preserve evidence for future work; Reverse analysis must recover relationships from what remains.

An early practical observation is that useful candidate discovery may be possible even when code quality is poor and the available evidence is modest. Repository history and coarse file-level signals can still narrow the search enough to help a person. This does not show that the candidates are correct, that the method generalizes across repositories, or that the underlying code is well understood. It suggests that relationship discovery can have practical value before a complete semantic model or high-quality codebase is available, and that evaluation should measure how much effort it saves as well as link accuracy.

Such discovery is also inherently time-bounded by its evidence. A method based on artifacts and history available at analysis time cannot reliably recover a later specification change that has not yet been recorded or linked. As intent evolves, previously useful relationships may become stale, and new relationships may need to be discovered. Maintaining traceability therefore includes synchronization over time, not only finding an initial set of links. This limitation is especially visible when moving from Forward work, where changes can leave contemporaneous evidence, to Reverse work, where the system must infer intent and relationships from implementation artifacts that may not preserve that context.

## Related Research Areas and Prior Work

This problem connects to several established areas. They study overlapping questions, but none should be assumed to cover the whole ISE problem, which includes relationships among multiple artifact types, ongoing synchronization, and the cost of rediscovery.

### Requirements Traceability Link Recovery

Requirements traceability link recovery searches for links between upstream artifacts, such as requirements and specifications, and downstream artifacts, such as source code and tests. Information retrieval (IR) methods compare terms in artifacts; later approaches also use artifact structure, machine learning, and human review. A 2024 empirical study compared six methods across thirteen datasets and found that effectiveness varies by method and dataset, which argues against relying on one universal ranking method. A 2025 systematic mapping study reviewed forty IR-based studies published from 2014 through 2024 and organized strategies involving artifact text, structural information, model optimization, and human intervention. [Wang et al. (2024)](https://doi.org/10.1016/j.jksuci.2024.102118); [systematic mapping study (2025)](https://www.sciencedirect.com/science/article/pii/S0306457325002286).

Earlier IR work on linking source code and documentation, and subsequent surveys of its assumptions and development, provide useful background. Such methods often depend on meaningful identifiers and vocabulary shared by the artifacts. Methods that use text structure, such as consensual biterms, aim to improve links when simple term overlap is insufficient. [Antoniol et al. (2025)](https://doi.org/10.1109/TSE.2025.3534027); [Gao et al. (2022)](https://arxiv.org/abs/2209.01734).

### Feature Location

Feature location asks which parts of a codebase implement a requested feature or behavior. Techniques include searching source text and identifiers, following dependencies, and using runtime information. Empirical evaluations show that techniques do not perform uniformly across projects. Field studies of developers also describe feature location as an iterative activity involving query formulation, exploratory search, and navigation through program structure. This area is close to finding code for a user-selected feature, while traceability recovery more directly studies links between artifacts. [Razzaq et al. (2020)](https://link.springer.com/article/10.1007/s10664-019-09734-5); [Damevski et al. (2015)](https://sites.udel.edu/se-research/2015/01/01/178/).

### Mining Software Repositories and Change Coupling

Repository mining uses version histories to identify patterns such as files or code elements that change together. Change coupling can suggest that artifacts are related, but it records development behavior rather than proving a semantic or design relationship. ROSE, for example, mines co-changed code elements from change transactions and uses support and confidence to recommend additional changes. File-level co-change tools such as GitLore expose similar history-based signals to developers. These methods motivate using commit history as evidence, while also requiring safeguards against large commits, formatting-only changes, merges, and unrelated bundled work. [Zimmermann et al. (2004)](https://thomas-zimmermann.com/publications/files/zimmermann-icse-2004.pdf); [GitLore](https://github.com/rajjadon/gitlore).

### Program Comprehension and Program Analysis

Program comprehension studies how developers understand software. Static and dynamic analysis provide structural and behavioral evidence, including dependency graphs, call relations, data flow, and runtime traces. These signals can help locate relevant code when names and documentation do not share vocabulary with the implementation. They may also expose when a file-level association is too coarse, though deeper analysis does not by itself establish the intent behind the code.

### LLM- and RAG-Based Traceability

Recent work applies large language models (LLMs) and retrieval-augmented generation (RAG) to traceability link recovery. LiSSA is a research framework for links across multiple artifact types, including requirements-to-code and documentation-to-code. It is relevant to the use of AI after candidate retrieval, but the reviewed public material did not establish Git co-change history as one of its primary evidence sources. This illustrates the need to evaluate evidence combinations rather than assume that an LLM replaces history, structural analysis, or human confirmation. [LiSSA repository](https://github.com/ArDoCo/LiSSA); [ICSE 2025 paper](https://doi.org/10.1109/ICSE55347.2025.00186).

### Implications for ISE

These fields provide techniques and evaluation practices for parts of the problem: artifact link recovery, code location, history mining, and program understanding. ISE can bring these together around the broader question of how relationships among intent and implementation artifacts are discovered, confirmed, and kept useful as work changes. The combination should be evaluated empirically; the existence of related methods does not establish that any one method or tool solves this broader problem.

## Evidence and Confidence

A relationship discovery process may combine different evidence sources:

- lexical similarity between intent, documentation, identifiers, and code;
- repository structure and dependency or call relationships;
- tests and observed behavior;
- Git commits, diffs, repeated co-changes, and work-item references;
- human confirmation and project-specific knowledge.

The output should be treated as candidates with supporting evidence, not as an unquestionable path list. It should communicate why an artifact was selected, what evidence supports the relationship, and where uncertainty remains. Where repository structure prevents meaningful attribution, the analysis should be able to report that limitation rather than imply precision.

The appropriate relationship granularity depends on the task and repository. File-level links are a useful minimum, but some tasks may require symbols, code regions, tests, or other units. The starting point may be a user-selected feature, behavior, requirement, or document section; the analysis should not assume one universal way to divide the subject.

## Research Questions

- Which artifact relationships are most costly to reconstruct, and for whom?
- What evidence sources improve candidate quality in Forward and Reverse work?
- How should evidence, confidence, and uncertainty be represented?
- At what granularity should links be made, and how should the subject of analysis be specified?
- How can analysis detect when code structure makes a mapping unreliable?
- When is refactoring a prerequisite for useful traceability?
- How much can relationship discovery reduce human search effort, AI exploration time, and token use without increasing missed or incorrect links?
- How useful are coarse, history-based candidates in repositories with weak structure or low code quality, and what review effort do they still require?
- How should relationships be invalidated, revised, or rediscovered when specifications and implementation evolve at different times?
- Which relationships should be regenerated on each analysis, and which confirmed results should be retained?

## Initial Direction

Treat artifact relationship discovery as an ISE problem and develop the theory from existing traceability, feature-location, repository-mining, program-comprehension, and human-learning research. Begin with focused Forward-development experiments using readily available repository evidence, including Git history and explicit work context where available. Early practical use suggests that even coarse signals can produce useful search candidates in imperfect codebases; evaluate that usefulness by measuring candidate quality, review effort, and reduction in repeated exploration rather than assuming that plausible output establishes correctness. Use the results to define an evaluation method before extending toward the more difficult Reverse case and the continuing synchronization problem as intent and implementation evolve.

The objective is not to assume that every relationship can be recovered automatically. It is to make relevant relationships easier to find, explain the evidence behind them, expose uncertainty, and reduce repeated exploration where the evidence supports doing so.
