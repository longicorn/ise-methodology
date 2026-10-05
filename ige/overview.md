# IGE (Intent Guidance Engineering) Overview

## 1. Introduction

IGE (Intent Guidance Engineering) is an engineering discipline within the Software Development Life Cycle (SDLC) designed to **empower and support the Product Manager's (PM's) world**.

A fundamental truth of software engineering is that **"Intent exists only inside the human mind."**

Ideas, hypotheses, and architectural concepts residing in the human mind are initially ambiguous, fragmented, and unstructured. IGE's mission is to guide (provide guidance to) this mental Intent—translating it into requirement specifications, designing actual features, decomposing them into actionable tasks, and enabling healthy, structured communication with the engineering team (ISE).

---

## 2. IGE as Engineering for Product Managers

Just as ISE is "engineering to assist developers with AI," IGE is **"engineering to assist Product Managers with AI and Requirements Engineering."**

```text
  [ Intent Residing Inside the PM's Mind ]
  - Why build this? (Environmental challenges, user pain points, business goals, etc.)
  - What should be built? (Solution hypotheses, value proposition, experiment concept)
               │
               ▼
  +--------------------------------------------------------------+
  |               IGE (Intent Guidance Engineering)              |
  |                                                              |
  |  1. Requirements Elicitation & Externalization               |
  |     - Surfacing tacit assumptions, prerequisites, and biases |
  |  2. Requirements Analysis & Modeling                         |
  |     - Goal-oriented breakdown (Why -> What), user stories    |
  |  3. Specification & Task Decomposition                       |
  |     - Acceptance criteria (Given-When-Then), functional/NFR  |
  |     - Experimental scope design & explicit metric requirements|
  |     - Decomposing scope into tasks actionable by developers  |
  +--------------------------------------------------------------+
               │
               ▼
  [ Externalized Requirements, Feature Designs, & Tasks ]
               │
               ▼ (Mediated & capacity-checked by ODF)
  [ Handoff to ISE (Developers) & Implementation ]
```

---

## 3. Application of Requirements Engineering (RE)

IGE grounds the PM's daily practice in academic Requirements Engineering principles:

1. **Goal-Oriented Requirements Analysis (From Why to What):**
   - Moves beyond superficial requests ("add this button") by decomposing operational goals into a structured tree of system behaviors.
2. **Acceptance Criteria Formulation:**
   - Establishes crisp, verifiable criteria (Given-When-Then) so developers can implement and test without ambiguity.
3. **Mitigating Understanding Drift:**
   - Prevents the fatal error of building the wrong thing by validating that the PM's understanding aligns with external environmental needs before engineering begins.

---

## 4. Collaboration with ODF: Protecting Engineering Health

Requirements and tasks formulated by IGE are never dumped onto engineering teams (ISE) unconditionally. All handoffs are **arbitrated through ODF (Proxy)**:

```text
       【 IGE (PM) 】                  【 ODF (Proxy) 】              【 ISE (Developers) 】
             │                                │                                │
             │── 1. Propose Requirements ────>│                                │
             │     & Task Breakdown           │── 2. Check Capacity & Load ───>│
             │                                │<── 3. Team Velocity & Bounds ──│
             │<── 4. Arbitrate Scope/Priority─│                                │
             │                                │── 5. Implement Agreed Tasks ──>│
             ▼                                ▼                                ▼
```

- **Respecting Engineering Capacity:**
  - ODF actively ensures that task volume does not outstrip development velocity, allowing PMs to prioritize high-value work without burning out the team.
- **Ingesting Engineering Feedback:**
  - Engineering constraints raised by ISE (e.g., architectural blast radius, unaddressed edge cases) are mediated by ODF, enabling PMs to iterate on requirements flexibly.

---

## 5. Summary

- **Audience:** The thinking and workflow of Product Managers (PMs).
- **Core Purpose:** Externalizes and concretizes the Intent residing only inside the mind into crisp requirements, feature designs, and developer-actionable tasks.
- **Workflow:** Concretized tasks are balanced against team capacity via ODF (Proxy) and handed off safely to developers (ISE).
