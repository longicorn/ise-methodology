# ODF (Orthogonal Dual-Framework) Overview

## 1. Introduction

ODF (Orthogonal Dual-Framework) is a framework within the Software Development Life Cycle (SDLC) that governs **two completely distinct, independent jobs**.

Attempting to explain these two roles within a single tangled narrative or diagram creates confusion. Therefore, ODF strictly defines them as **"Job 1: Proxy"** and **"Job 2: The Environment ⇄ Intent Lean Loop"**.

- **Job 1: `IGE ⇄ ODF ⇄ ISE` (The Operational Proxy)**  
  Sits between the PM's world (IGE) and the Developer's world (ISE) to balance task volume, velocity, and engineering sustainability.
- **Job 2: `Environment ⇄ ODF ⇄ Intent` (The Macro Lean Loop)**  
  Connects the external world (Environment) with mental hypotheses (Intent), driving the overarching **Lean (BML) Loop** to realize intent and empirically validate business value.

---

## 2. Job 1: `IGE ⇄ ODF ⇄ ISE` (The Operational Proxy)

The first responsibility is acting as an active **Proxy (buffer and mediator) between PMs (IGE) and developers (ISE)**.

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

## 3. Job 2: `Environment ⇄ ODF ⇄ Intent` (The Lean Loop)

The second responsibility is **governing the macro discovery cycle (Lean Loop) that links the uncertain external Environment with the mental Intent, testing how intent is realized and validated**.

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

ODF represents the synthesis of two independent axes:

1. **Proxy Axis (`IGE ⇄ ODF ⇄ ISE`):**
   - Buffers and arbitrates task volumes and velocity between PMs and developers to protect engineering health.
2. **Lean Loop Axis (`Environment ⇄ ODF ⇄ Intent`):**
   - Governs the macro BML loop that takes requirements externalized by Requirements Engineering, drives their empirical realization, and continuously updates environmental understanding based on telemetry.
