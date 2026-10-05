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
               [ Environment (Market, Customers, Real World) ]
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
                                     │ Requirements Engineering (RE)
                                     │ (Externalizing mental intent into docs)
                                     ▼
                        [ Document (Requirements / Specs) ]
```

### 1. Why the Lean Loop Is Essential
Whether a feature or initiative will truly succeed in the market is fundamentally impossible to predict in advance. Building capabilities that no one ends up using is an everyday reality.
Therefore, rather than building massive specifications all at once, teams must operate a Lean BML Loop: **Build small, Measure outcomes, and Learn to revise understanding of the Environment**.

### 2. Division of Labor: Requirements Engineering vs. Lean
The scopes of Requirements Engineering (RE) and Lean are clearly separated:
- **Requirements Engineering Scope (`Intent ⇄ Document`):**
  - Externalizing ambiguous Intent residing inside the human mind into formal requirement documents, acceptance criteria, and feature designs (primarily executed by IGE).
- **Lean Scope (`Environment ⇄ ODF ⇄ Intent` Macro Loop):**
  - Orchestrating the overarching cycle of how externalized Intent is realized against Reality, tested in the market, and validated.

### 3. Measuring Telemetry vs. Revising Environmental Understanding
- **Measuring (Measure) is ISE's Role:** Deploying instrumentation in production (Reality) to log and measure empirical telemetry belongs to the engineering side (ISE).
- **Learning (Learn) and Revising Understanding is ODF's Role:** Ingesting those measurement results to **revise the organization's understanding of the Environment, and update, pivot, or prune the Intent** is ODF's responsibility. If a feature fails to deliver value, ODF invalidates the hypothesis, removing the feature from the Living Spec and triggering its clean deletion from the codebase.

---

## 4. Summary

ODF applies its Proxy principle across two orthogonal axes:

1. **Operational Mediation (`IGE ⇄ ODF ⇄ ISE`):**
   - Buffers and arbitrates task volumes and velocity between PMs and developers to protect engineering health.
2. **Discovery Mediation (`Environment ⇄ ODF ⇄ Intent`):**
   - Governs the macro BML loop that takes requirements externalized by Requirements Engineering, drives their empirical realization, and continuously updates environmental understanding based on telemetry.
