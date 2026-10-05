# ODF (Orthogonal Dual-Framework) Overview

## 1. Introduction

ODF (Orthogonal Dual-Framework) applies one Proxy principle across **two orthogonal boundaries** in the Software Development Life Cycle (SDLC).

The axes are distinct in the boundaries they mediate, but they are not independent jobs. In both, ODF interprets information from one side, mediates decisions, and carries feedback across the boundary.

ODF is not merely a buffer. It mediates the two boundaries from a position that covers the whole product outside ISE and IGE, an area close to architects and executives. For this reason, its responsibilities are heavier than those of the other frameworks.

- **Proxy Axis 1: `IGE ⇄ ODF ⇄ ISE` (Operational Mediation)**
  Mediates requirements, task volume, engineering capacity, and technical constraints between PMs (IGE) and developers (ISE).
- **Proxy Axis 2: `Environment ⇄ ODF ⇄ Intent` (Discovery Mediation through the Lean Loop)**
  Mediates evidence and hypotheses between the external Environment and human Intent through the **Lean (BML) Loop**, guiding how intent is tested and revised against reality.

---

## 2. Proxy Axis 1: `IGE ⇄ ODF ⇄ ISE` (Operational Mediation)

On this axis, ODF acts as an active **Proxy (buffer and mediator) between PMs (IGE) and developers (ISE)**.

```text
      【 The PM's World 】                             【 The Developer's World 】
             IGE                                                ISE
   (Concretizing mental Intent,                        (Assisting developers with AI,
    specs, feature design, tasks)                       synchronizing Intent & Reality)
             ▲                                                  ▲
             │                                                  │
             │                  +─────────────+                 │
             └────────────────> │     ODF     │ <───────────────┘
                                │   (Proxy)   │
                                +─────────────+
                                       │
                         - Are task volumes reasonable?
                         - Is velocity sustainable?
                         - Preventing overload on ISE
```

### 1. Arbitrating Capacity and Velocity
PMs (IGE) are driven by business demands to build many features, producing a continuous stream of tasks. However, the engineering team's (ISE) capacity and cognitive bandwidth are strictly finite.
ODF arbitrates this tension:
- **"Is the volume of incoming tasks reasonable?"**
- **"Does it match the team's sustainable velocity?"**
- **"Are urgent interruptions dumped directly onto developers, breaking engineering focus?"**

### 2. Ingesting Engineering Constraints
Feedback from the engineering front line—such as high architectural blast radius or mandatory technical debt repayment—is ingested by ODF, requiring PMs to adjust priorities or trim scope.

### 3. Practical Embodiments
This Proxy role manifests in practice through **GitHub Issues, Pull Requests, sprint backlogs, and RFC review processes**.

---

## 3. Proxy Axis 2: `Environment ⇄ ODF ⇄ Intent` (Discovery Mediation through the Lean Loop)

On this axis, ODF mediates between the uncertain external Environment and human Intent through the Lean Loop, carrying evidence and hypotheses back and forth as teams test how intent is realized and validated.

```text
               [ Environment (External World, Operational Context, Market) ]
                                     ▲
                                     │
                             BML Loop (Lean)
                       - How to realize & validate value
                       - Revising environment understanding
                                     ▼
                              +-------------+
                              |     ODF     |
                              +-------------+
                                     ▲
                                     │
                                     ▼
                 [ Intent (Exists ONLY in the Human Mind) ]
                                     ▲
                                     │ Requirements Engineering (RE) & Experiment Design
                                     │ (Externalizing mental intent into docs & test plans)
                                     ▼
                        [ Document (Requirements / Specs) ]
```

### 1. Why This Document Uses the Lean Loop
Whether a new capability or initiative will truly achieve its intended outcome in the external environment is impossible to guarantee in advance. Creating features that end up unused or introduce unexpected friction is an everyday occurrence across all domains of software development, not merely in commercial business.
To explain this process, this document uses the Lean BML Loop as one straightforward approach: **Build small, Measure outcomes, and Learn to update understanding of the Environment**. Lean is not the only way to do this, and the responsibilities of ODF and IGE are not limited to Lean. A more suitable approach may replace or complement it in the future.

### 2. Division of Labor in the Discovery Cycle (ODF, IGE, and ISE)
The discovery cycle is driven by the orthogonal collaboration of the three frameworks:

- **ODF (Environmental Understanding & Discovery Mediation: `Environment ⇄ ODF ⇄ Intent`):**
  - Interprets environmental changes and measurement results to deepen understanding of the Environment and frame discovery questions.
- **IGE (Intent, Experiment & Measurement Requirements: `Intent ⇄ Document`):**
  - Translates ODF's environmental understanding into verifiable Intent, experiment designs, and measurement requirements.
- **ISE (Realization in Reality: `Intent ⇄ Reality`):**
  - Implements the agreed Intent and experiments in specifications, code, tests, and operations, including the required observation points.

Depending on the scope and decision context, ODF or IGE may lead decisions to revise or prune Intent or retire a feature. ISE carries out the resulting changes to implementation artifacts and code in Reality.

### 3. Goodhart's Law and "Metric as Sensor"
One useful perspective when considering this discovery cycle is **Goodhart's Law**: *"When a measure becomes a target, it ceases to be a good measure."*

Measurement requirements specify what to observe for an experiment; they do not make a metric a target. Equating a metric with a goal or absolute truth can make improving the number an end in itself, potentially undermining the original intent and broader system (e.g., aggressive conversion-rate optimization or meaningless tests written to inflate coverage).

Metrics can be viewed as **sensors for environmental shifts and systemic conditions**. When interpreting them, considering other evidence and the broader system context alongside an individual number can help inform environmental understanding and Intent revision.

This division describes ODF from the standpoint of mediating the whole Lean BML Loop. Decisions to revise or prune Intent may be led by ODF when they concern overall environmental understanding, or by IGE when they concern a local hypothesis or experiment. ISE changes artifacts in Reality based on that decision. See section 3 of the [Overview](../overview.md) for details.

---

## 4. Summary

ODF applies its Proxy principle across two orthogonal axes:

1. **Operational Mediation (`IGE ⇄ ODF ⇄ ISE`):**
   - Buffers and arbitrates task volumes and velocity between PMs and developers to protect engineering health.
2. **Discovery Mediation (`Environment ⇄ ODF ⇄ Intent`):**
   - Governs the macro BML loop that takes requirements externalized by Requirements Engineering, drives their empirical realization, and continuously updates environmental understanding based on telemetry.
