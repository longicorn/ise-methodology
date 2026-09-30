# Review as Engineering Feedback: From Subjective Inspection to Multi-Perspective Assessment Systems

> Note: The problem identification, key concepts, and final decisions in this paper are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

In the previous paper (04), we defined the core concept of Intent-Synchronized Engineering (ISE): the "continuous synchronization of intent and artifacts."

Starting from Intent, artifacts such as Doc and Code are woven together. However, no matter how clearly intent is defined and bidirectional synchronization is pursued, artifacts in real-world system development inevitably fluctuate, introduce noise, and tend to drift away from intent.

Here, the activity of **"Review (or Assessment)"** becomes unavoidable.

In development practice, review has long been considered the cornerstone of quality assurance. Yet simultaneously, it has been the activity most prone to personalization, ritualization, and becoming a bottleneck in the development flow. Furthermore, in an era where AI generates code and documentation in vast quantities and at blinding speeds, traditional review methods are reaching their breaking point.

In this paper, we unpack decades of software engineering research and empirical findings on reviews, reinterpreting the essence of review from a "subjective approval ritual by humans" into an **"engineering feedback control mechanism."** We then examine what is required to build an effective assessment system in the AI era.

---

## 1. Pathologies of Review in Practice

In traditional software development—especially in modern workflows centered around Pull Requests (PRs)—review has been treated as an indispensable ritual. In practice, however, it suffers from several deep distortions:

1. **Over-reliance on "Eyeballs" and Diffusion of Responsibility**  
   Linus's Law ("given enough eyeballs, all bugs are shallow") is often casually invoked, and reviews are requested with the vague expectation that "someone will check it." Consequently, no one reads deeply, and accountability becomes diluted.
2. **The Gatekeeper Trap**  
   Treating review as an authoritative "gate" that must be passed before merging stalls the development flow. If reviewers are busy, work is blocked for days, stretching the feedback loop unacceptably long.
3. **Cognitive Overload and Bikeshedding**  
   When a diff exceeds hundreds or thousands of lines, it exceeds human cognitive limits. Critical architectural flaws and subtle logical breakdowns are overlooked, while discussions fixate on superficial, easily criticizable details: variable naming, formatting, and typos.
4. **Review Exhaustion from AI-Generated Code**  
   AI coding tools have accelerated generation by orders of magnitude. AI generates in minutes what used to take humans a full day. If humans attempt to inspect every line using traditional methods, the review queue inevitably overflows and collapses.

As long as review remains an idiosyncratic ritual of inspection and sign-off, speed and quality will remain locked in a perpetual trade-off.

---

## 2. The Essence of Review: What Prior Research and Standards Teach Us

Review is not merely a team habit. In the history of software engineering (SE), it represents an academic and empirical field spanning over 50 years of formalization.

The Assessment system envisioned by ISE is firmly grounded in these foundational insights:

### Fagan Inspection (1976): Formal Inspection and Process Improvement

Michael E. Fagan’s 1976 landmark paper (*Design and Code Inspections to Reduce Errors in Program Development*) established review as a rigorous engineering process.

Fagan formalized design and code reviews not as casual "look-overs," but as structured operations with explicit roles (moderator, author, reader, inspector) and defined phases: planning, preparation, inspection meeting, rework, and follow-up.

Two critical lessons emerge from Fagan's work:
- **Explicit Procedures and Roles:** Review must not rely on individual intuition; it is an engineered activity with structured steps.
- **Feedback of Defect Data:** Detected defects were classified and recorded not merely to patch the artifact, but as **feedback data to correct defects in the development process itself**.

### Perspective-Based Reading (PBR, 1995): Orthogonal Perspectives

Perspective-Based Reading (PBR), pioneered by Victor R. Basili and colleagues in the 1990s, yielded a fundamental breakthrough regarding review effectiveness.

Empirical studies on PBR demonstrated that **reviewers instructed simply to "read carefully" consistently missed critical defects**. Human attention naturally defaults to familiar domains or superficial quirks when confronted with unstructured tasks.

PBR instead equips each reviewer with a specific **Perspective** and a concrete **operational scenario (a structured set of questions)**:
- **Designer Perspective:** Can the requirements be realized architecturally? Are internal interfaces and constraints satisfied?
- **Tester Perspective:** Can comprehensive test cases be generated from this document? Are edge cases and error states omitted?
- **User Perspective:** Does the proposed behavior complete the intended business workflows? Are there usability contradictions?

Experiments proved that teams using distinct, structured perspectives discovered significantly more critical defects while drastically reducing redundant, low-value feedback.

In short: **What matters is not the number of eyes, but the variety of perspectives and the clarity of their underlying questions.**

### IEEE 1028: Separation of Review Objectives

IEEE 1028 (*Standard for Software Reviews and Audits*) systematically categorizes reviews into five distinct types based on their purpose:
1. **Management Review:** Evaluates project progress, risks, and scope validity.
2. **Technical Review:** Evaluates technical solutions and design validity, exploring alternatives.
3. **Inspection:** Rigorously detects defects in artifacts against predefined rules and checklists.
4. **Walkthrough:** Fosters collective understanding, knowledge sharing, and consensus among participants.
5. **Audit:** Independently evaluates compliance with regulations, standards, or contractual requirements.

The vital takeaway is: **different purposes require different procedures, different focal points, and different handling of outcomes.** Blending all these into an undifferentiated "all-in-one PR review" is precisely why software teams suffer friction.

### Modern Code Review in Practice (Bacchelli & Bird, 2013): Knowledge Sharing and Intent Dialogue

Alberto Bacchelli and Christian Bird’s empirical study of large-scale teams at Microsoft (*Expectations, Outcomes, and Challenges of Modern Code Review*, 2013) exposed a stark divergence between expectation and reality:
- While engineers cited "finding defects" as the primary motivation for code review,
- The actual dominant benefits realized were **knowledge transfer, codebase familiarity, shared design intent, and exploring alternative solutions**.

This reveals that review cannot be narrowly modeled as a simple bug-catching filter. It is equally a communication channel where team members **dialogue over design intent, surface trade-offs, and reconstruct shared context** through artifacts.

---

## 3. Engineering Feedback Control Mechanisms

Synthesizing these insights through systems theory reveals that the essence of review is identical to a **feedback control system in control engineering**.

### Vulnerability of Open-Loop Systems

In control engineering, a system that determines control action purely based on inputs (setpoints) and produces output without verification is called an **open-loop (feedforward)** system.

The classical forward progression of `Intent -> Doc -> Code` is essentially open-loop.

The fatal limitation of an open-loop system is that in the presence of disturbances or process uncertainties—implementation friction, flawed assumptions, LLM inference hallucinations—**the output (artifacts) drifts irreversibly from the target (intent) over time without self-correction**. The "document drift" and "software archaeology" examined in Chapter 02 were inevitable symptoms of open-loop development.

### Review as Closed-Loop Control (Observation and Error Detection)

To stabilize a system and keep it aligned with its target, a **closed-loop (feedback control)** mechanism is mandatory:

```text
               +-------------------------------------------------+
               |                                                 |
               v                                                 | Feedback
[ Target: Intent ] ──────(+)──────> [ Generator (Lane) ] ────> [ Reality: Artifacts ]
                          ^                                           │
                          │                 Observe                   │
                          └──( - Error )─── [ Assessment System ] <───┘
```

In this control block diagram, the role of review (Assessment) is crystal clear:

1. **Sensor (Observation):** Measures the current state of Reality (artifacts).
2. **Error Detection:** Pinpoints the deviation (error) between the intended target (Intent / constraints) and the actual artifact.
3. **Feedback to Control Input:** Feeds the error signal back to developers or generative agents as actionable guidance to adjust artifacts or update the intent.

Review is not an administrative checkpoint to obtain permission. It is a **measurement sensor that detects error between intent and reality to stabilize the system**.

---

## 4. Requirements for an Effective Assessment System

To operationalize review as an engineering feedback mechanism, ISE emphasizes two core principles:

### 1. Decoupling Intent (Why/What) from Execution Means (Who/How)

As IEEE 1028 and PBR demonstrate, what matters most is the evaluation objective: *What question are we asking?*

Whether a review is performed by an AI agent, a human engineer, or a static analysis script is merely an **execution mechanism (Who/How)**. In ISE, evaluation criteria are defined orthogonally as **perspectives (Why/What)**.

ISE establishes three universal **Initial Baselines** that apply across all types of artifacts:

- **Defect:** *"Are there flaws, contradictions, or violations?"*  
  Focuses on syntax bugs, logical contradictions, missing boundary conditions, omitted preconditions, and security vulnerabilities. This perspective checks internal consistency and correctness.
- **Validity:** *"Is it sound and sufficient relative to the intent?"*  
  Focuses on alignment with high-level goals, soundness of architectural decisions, and requirement coverage. This perspective checks external fitness and alignment with value.
- **Complexity:** *"Is it unnecessarily complex?"*  
  Focuses on cognitive load, excessive coupling, separation of concerns, and redundant concepts in code or specifications. This perspective checks maintainability and long-term sustainability.

#### The Perspective Space is "Open-Ended"

**Crucially, these three categories do not form a closed taxonomy, nor do they represent an exhaustive set.**

Rather, they serve as the **representative baseline axes (examples of orthogonal basis vectors)** that any Doc or Code should naturally be scrutinized against.

In practice, the Perspective Space is an inherently **open-ended system** that can and should be freely extended and specialized based on domain specifics, product maturity, and stakeholder priorities.

Teams can establish dedicated, specialized observation axes such as:

- **Security / Safety:** *"Can it withstand adversarial attacks, privilege escalation, and data exfiltration?"* (Perspective of security auditors)
- **Observability / Operability:** *"Can production incidents be effectively diagnosed, traced, and recovered from?"* (Perspective of SRE / Operations)
- **Performance / Scalability:** *"Does it degrade or bottleneck under high concurrency or growing data volumes?"* (Perspective of performance / infrastructure engineers)
- **Evolvability:** *"Can the system accommodate future requirements through localized modifications without breaking changes?"* (Perspective of software architects)

There is no need to forcefully squeeze these concerns into the broader buckets of "Defect" or "Complexity." As PBR demonstrated, **"the more focused and concrete the perspective, the higher the precision of the observation sensor."** ISE’s assessment architecture is deliberately designed so that teams and organizations can plug in customized perspectives tailored to their unique operational needs.

### 2. Elimination of Gatekeepers: On-Demand Observation

The primary reason traditional PR reviews degrade into bottlenecks is that feedback was conflated with an **approval gate (Gatekeeper)**. Gates stall flow and disrupt developer rhythm.

In engineering feedback, sensors do not halt the machinery. Like an instrument cluster on an aircraft dashboard, **the assessment can be run on-demand at any time, allowing developers to observe error signals and self-tune autonomously**.

Whether to remediate an issue immediately, defer it, or intentionally accept the trade-off remains an explicit engineering decision by the developer. This preserves agility while maintaining a closed-loop control discipline.

---

## 5. Redefining Review in the AI Era: Substitution and Collaboration

In an era where generation has become nearly instantaneous, the division of labor in software evaluation must be fundamentally restructured.

### The Inevitability of AI Substitution and the Failure of Naive Prompting

Humans can no longer inspect every line of code generated by machines. Delegating assessment to AI is not a luxury; it is a physical necessity for software engineering to survive.

However, naively prompting an AI to "review this code" yields shallow, ineffective results. LLMs default to polite generic praise or trivial refactoring tips, missing deep architectural flaws.

Why? **Because, as PBR taught us decades ago, the AI was given neither a distinct perspective nor a concrete verification scenario.**

To transform an LLM into a high-precision sensor, PBR principles must be baked into prompts and execution contexts:
- *"Adopt a tester's perspective; evaluate exclusively for boundary-condition Defects."*
- *"Adopt a maintainer's perspective; measure solely for cognitive load and coupling Complexity."*

By narrowing the focus to a single orthogonal perspective with explicit inquiry criteria, AI agents consistently detect critical defects with speed and coverage exceeding human capacity.

### Division of Labor Between Humans and AI: Boundaries from Bacchelli & Bird

When AI assumes structured assessment, what remains for humans? The empirical findings of Bacchelli & Bird (2013) provide the answer:

| Domain | Role of AI & Automated Tools | Role of Humans |
|---|---|---|
| **Defect** | **Exhaustive detection** of syntax bugs, edge conditions, static violations, and contradictory patterns | **Final triage** on whether a detected defect constitutes real-world business or operational risk |
| **Complexity** | **Objective measurement & alerting** of cyclomatic complexity, coupling, and duplication | **Value judgment** on whether complexity is accidental or intrinsic to domain requirements |
| **Validity** | **Logical consistency check** between stated intent (Doc) and implementation (Code) | **Negotiation and consensus** on whether the intent itself is right, considering unstated stakeholder context |
| **Knowledge Transfer** | Automated summarization and extraction of design rationale | **Internalizing tacit knowledge**, building shared team context, and deep alignment |

Automated tools and AI absorb the cognitive exhaustion of initial Defect sweeps and Complexity metrics.

Freed from low-level bikeshedding, human engineers can focus their energy where human judgment is irreplaceable: **evaluating the Validity of intent, debating architectural trade-offs, and cultivating genuine team consensus.**

AI assessment does not diminish human review; it elevates it.

---

## 6. Conclusion: From Open-Loop Generation to Closed-Loop Engineering

The AI paradigm has pushed code generation speed to near infinity. Yet no matter how fast code is produced, if it drifts uncontrollably from human intent, it is not engineering—it is chaos.

The foundational criterion for software development to qualify as true Engineering is that **the forward force of creation (feedforward generation) and the reverse force of observation (feedback control) form a closed, self-correcting loop**.

By liberating review from subjective gatekeeping and reconstructing it as a multi-perspective feedback control system grounded in established research,

**Intent-Synchronized Engineering (ISE) transcends open-loop generation into a complete closed-loop discipline—one that perpetually tunes artifacts to human intent across a volatile and changing world.**
