# A New Paradigm for the Software Development Life Cycle: Environment ⇄ Intent ⇄ Reality

## 1. Foundational Three-Tier Concept

The universe surrounding software development is fundamentally categorized into three tiers:

```text
 [ Environment ]     The external world / reality. In constant flux. (Business/market is one application instance.)
        ▲
        │  (Drift occurs continuously)
        ▼
    [ Intent ]        Exists ONLY within the human mind. Hypotheses and models on solving problems.
        ▲
        │  (Drift occurs continuously)
        ▼
   [ Reality ]       Software itself. Source code, test suites, and running systems.
```

1. **Environment:**
   - The entire reality external to the software system. While commercial markets and business demands are prominent examples, Environment encompasses all external contexts—such as open-source community dynamics, academic research/computational environments, internal operational workflows, external systems, or developer usage in personal projects.
   - A non-deterministic world characterized by high uncertainty and constant change.
2. **Intent:**
   - **"Exists only inside the human mind."**
   - Mental models, hypotheses, problem awareness, and architectural concepts for solving problems in the Environment. Externalized documents, tickets, and specifications are merely downstream projections of the Intent residing in the mind.
3. **Reality:**
   - The software itself: runnable source code, test suites, compiled binaries, and live production deployments.
   - The deterministic, logical world of engineering.

---

## 2. Forward / Reverse Bi-directional Synchronization (Core Philosophy)

A vital principle of this methodology is that **drift continuously and inevitably occurs at both boundaries**:

- **Environment ⇄ Intent Drift:** The external world shifts (Environment Drift), or human understanding misunderstands reality (Understanding Drift).
- **Intent ⇄ Reality Drift:** The actual code diverges from the mental intent, introducing bugs or unintended behaviors (Implementation Drift).

To combat these drifts, one-way execution is insufficient; **bi-directional synchronization (Forward / Reverse Sync)** is essential.

```text
 [ Environment ] (External world / Operational context / Markets)
        ▲
        │  Forward: Discovering environmental problems / Setting hypotheses
        │  Reverse: Learning from telemetry & BML metrics to update/prune hypotheses
        │  【Governed by: ODF (Discovery Mediation & Lean BML Loop) & IGE (Requirements Engineering & Experiment Design)】
        ▼
    [ Intent ]    (Intent residing only in the human mind)
        ▲
        │  Forward: Externalizing intent into documents, then implementing it in code & tests
        │  Reverse: Reading code, tests & runtime back into artifacts and intent
        │  【Governed by: ISE (Developer Enablement & Nagi)】
        ▼
   [ Reality ]   (Software itself / Source code)
```

---

## 3. The Three Abstract Frameworks in the SDLC (ODF / IGE / ISE)

To operationalize this dual synchronization across the Software Development Life Cycle (SDLC), the methodology provides three frameworks corresponding directly to organizational roles.

However, the boundaries between the three frameworks are not fixed. Where responsibility lies is decided through agreement among ODF, IGE, and ISE. When circumstances change or a mistake is found, they may agree again.

Although the boundaries can blur, there is a tendency for the role of judging whether an intent was wrong, based on the results of a local initiative, to lean toward IGE, and for the governance of the whole Lean BML Loop to lean toward ODF.

### 1. ISE (Intent Synchronized Engineering): Engineering for Developers
- **Scope:** `Intent ⇄ Reality`
- **Objective:** Augment software developers using AI. **It does not eliminate developers; it empowers them**.
- **Forward Sync:** Externalizing intent through documents and other artifacts, then deterministically implementing the adopted intent in code and tests. Because artifacts and implementation can drift, ISE continuously checks and resynchronizes them.
- **Reverse Sync:** Reading code, tests, runtime behavior, and history back into artifacts and intent. This supports legacy analysis, drift detection, and intent reconstruction when documents are missing or outdated.
- **Instrumentation for Measurement:** Implementing logging and telemetry in the software (code) belongs to ISE. This differs from "Measure" in the Lean BML Loop, which is about evaluating results and making judgments. Checking logs for bug investigation and similar work belongs to operations.

### 2. IGE (Intent Guidance Engineering): Engineering for Product Managers (PMs)
- **Scope:** `Intent ⇄ Document (Specification & Requirements Engineering)`
- **Objective:** Takes the ambiguous Intent residing in the human mind, formulates requirement documents, designs features, and breaks them into tasks ready for engineering communication.
- **Applying Requirements Engineering (RE):** Goal-oriented requirements analysis (Why -> What), acceptance criteria formulation, and non-functional constraints.

### 3. ODF (Orthogonal Dual-Framework): Proxy Mediation Across Two Boundaries

ODF applies the same Proxy principle at two orthogonal boundaries. These are distinct axes of mediation, not independent jobs: in both, ODF interprets information from one side, mediates decisions, and carries feedback across the boundary.

#### Proxy Axis 1: `IGE ⇄ ODF ⇄ ISE` (Operational Mediation)
Between PMs (IGE) and developers (ISE), ODF mediates requirements, task volume, engineering capacity, and technical constraints to preserve engineering health.
```text
  【 PM (IGE) 】 ──> [ ODF (Proxy) ] <── 【 Developers (ISE) 】
                            │
              - Are task volumes reasonable? Is velocity healthy?
              - Preventing overload on ISE
```
- **Arbitration Focus:** Rate-limiting task intake, protecting sustainable development velocity, and channeling engineering constraints back to PMs.
- **Embodiments:** GitHub Issues, Pull Requests, sprint backlogs, and RFC reviews.

#### Proxy Axis 2: `Environment ⇄ ODF ⇄ Intent` (Discovery Mediation through the Lean Loop)
Between the uncertain external Environment and human Intent, ODF mediates evidence and hypotheses through the **Lean BML Loop**, guiding how intent is tested and revised against reality.
```text
  [ Environment ] ──(BML Loop)──> [ ODF ] ──> [ Intent ] ──(Requirements Eng.)──> [ Document ]
```

Whether an initiative or feature will have its intended effect in the external environment cannot be known in advance. ODF, IGE, and ISE coordinate the discovery loop:

- **ODF (Environmental Understanding & Discovery Mediation):** Interprets environmental changes and measurement results to deepen understanding of the Environment and frame discovery questions.
- **IGE (Intent, Experiment & Measurement Requirements):** Translates ODF's environmental understanding into verifiable Intent, experiment designs, and measurement requirements.
- **ISE (Realization in Reality):** Implements the agreed Intent and experiments in specifications, code, tests, and operations, including the required observation points.

Depending on the scope and decision context, ODF or IGE may lead decisions to revise or prune Intent or retire a feature. ISE carries out the resulting changes to implementation artifacts and code in Reality.

##### Goodhart's Law and "Metric as Sensor"
One useful perspective when considering this discovery loop is **Goodhart's Law**: *"When a measure becomes a target, it ceases to be a good measure."*

Measurement requirements specify what to observe for an experiment; they do not make a metric a target. Equating a metric with a goal or absolute truth can make improving the number an end in itself, potentially undermining the original intent and broader system. This applies to commercial metrics (e.g., CVR, revenue) and software metrics (e.g., test coverage, commit volume, isolated latency benchmarks).

Metrics can be viewed as **sensors for environmental shifts and systemic conditions**. When interpreting them, considering other evidence and the broader system context alongside an individual number can help inform environmental understanding and Intent revision.

---

## 4. Why This Architecture Is Essential Now: The AI Throughput Explosion

Historically, human communication speed was the bottleneck. Methodologies like Agile and Scrum attempted to address all three drifts **simultaneously and implicitly** through short iterations. However, without explicit drift classification, and under typical "business-first" delivery pressures, engineering consistency was frequently sacrificed, accumulating crippling technical debt.

In the AI era, **code implementation throughput has accelerated exponentially**. This unprecedented velocity violently surfaces hidden distortions:
1. **Instructing AI with flawed understanding (B) mass-produces useless code at superhuman speeds.**
2. **Implementation without environmental validation (A) accelerates divergence from market realities.**
3. **Subtle AI hallucinations and undocumented alterations (C) outpace human cognitive review, instantly turning fresh code into legacy debt.**

Therefore, rather than relying on intuitive rituals:
- **ISE:** Assists developers with AI to support legacy recovery and drift detection (Reverse), deterministic implementation of adopted intent through artifacts (Forward), and the implementation of telemetry instrumentation for measurement, with ongoing synchronization across them.
- **IGE:** Empowers PMs to externalize mental Intent into crisp, structured Documents (requirements, tasks) via Requirements Engineering.
- **ODF:**
  - Acts as the `IGE ⇄ ODF ⇄ ISE` Proxy to balance task volume and velocity,
  - Governs the `Environment ⇄ ODF ⇄ Intent` Lean Loop to revise environmental understanding based on ISE telemetry and evolve hypotheses.

Through this explicit division and synergy, the explosive potential of AI is channeled safely into sustainable, high-value software engineering.
