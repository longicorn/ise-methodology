# Information Boundary, Duplication, Gates, and Reverse Design Based on Reconstructibility

## Purpose

Discussions such as `Spec as Code` vs. `Code as Spec`, or "which is the SSoT (Single Source of Truth), documentation or code?", continue to recur even after the widespread adoption of AI-assisted development.

Because ISE assumes that every Node continues to change independently over time, it views the binary opposition over a static single SSoT as meaningless in itself (see [02_ise_problem_statement.md](02_ise_problem_statement.md)). On the other hand, the practical problem where the same information is duplicated between documentation and code and one side becomes stale certainly exists.

However, because documentation and code have different scopes of responsibility, apparent duplication is not always bad. This document defines a healthy boundary of responsibilities between the two and establishes concrete principles and criteria usable for **judging duplication**, **triaging in Reverse Sync**, and **verifying at Gates**.

---

## Spec as Code / Code as Spec and the Essence of Duplication

Consider the specification: "Users under 18 years old cannot register for the service."

- **What implementation (Code) holds**: A concrete decision expression and threshold at a given point in time, such as `age >= 18`.
- **What the specification (Intent / Decision) should hold**: The dependency "follow the age permitted by law and the nature of the service," the rationale of which law or standard it is based on, and the decision criterion that it should track legal amendments.

If the specification merely states "Users under 18 cannot register," it is close to a mere natural-language translation of the implementation (information duplication), and why 18 is used (whether it is the civil age of majority, a separate standard such as drinking age, or the service's own safety policy) is lost. Conversely, if the specification states the premise that the age can change if the law or the nature of the service changes, then `18` in the implementation and the description in the specification are not duplication, but a **complementary relationship with different responsibilities**.

Here, if we take the `Code as Spec` standpoint and use "whether the specification can be regenerated from code" as the criterion, a trap specific to modern AI environments becomes visible. Seeing `age >= 18` in code, AI can **plausibly** regenerate a specification like "registration restriction based on the age of majority." However, that is merely an inference (extrapolation) from general background knowledge; it does not mean the rationale of "which law or policy it depends on" has been reconstructed from the code.

---

## Two Core Principles

From the above considerations, the information boundary and duplication judgment in ISE Document Engineering converge on the following two principles:

1. **The criterion for judging duplication between artifacts is not surface similarity of descriptions, but semantic reconstructibility of information.**
2. **One role of a specification is to preserve information lost in translation to implementation that is needed for future decisions.**

Even if the expressions in code and documentation look similar, if the documentation includes decision-making information that cannot be uniquely reconstructed from code, that is a legitimate division of responsibilities. Conversely, even if the expressions differ, if the documentation only states what can be reconstructed from code without guesswork, it is duplication that only incurs maintenance cost.

---

## Three-Class Model by Semantic Reconstructibility

To avoid being misled by the pseudo-regeneration of `Code as Spec`, we classify the reconstructibility of information in the direction from implementation to specification into the following three categories:

| Class | Definition | Example in the Age-18 Case | Handling in Documentation |
| --- | --- | --- | --- |
| **1. Reconstructible** | Can be uniquely reconstructed from information inside the code alone, without guesswork or external knowledge | The current decision expression, threshold, and branching condition `age >= 18` | Writing it directly into Spec tends to become **bad duplication** (a degraded copy of implementation). An area to leave to the Analyze mode (As-Is / Reality Document) or keep as a reference |
| **2. Plausibly Guessable** | AI or a reader can regenerate it "plausibly" using general knowledge, but it cannot be uniquely determined from information inside the code alone | `18` → a plausible interpretation like "restriction based on the age of majority" | **The biggest trap**. Promoting it directly to Intent in Reverse Sync mixes ungrounded assumptions into the specification |
| **3. Non-Reconstructible** | Completely lost in translation to implementation and cannot be derived from code | Rationale, change triggers, and decision history such as "follow the permitted age of Law X," "track legal amendments," or "Option Y was rejected due to risk" | **Intrinsic responsibility that Intent / Decision Documents should preserve with top priority** |

### Handling Intentional Duplication as a "Contract"

There are cases where behavior implemented in code—such as API interface definitions or business invariants—is also explicitly stated in the specification. At first glance, this looks like duplication of "reconstructible" information, but in reality it is different.

Code alone does not distinguish between "it just happens to be implemented that way right now" and "it is a boundary that must never be changed as an external contract." As long as the specification retains the **changeability decision information that "this boundary is not accidental, but a contract that must not be changed,"** that description contains information non-reconstructible from code and is treated as a legitimate responsibility.

---

## Judgment and Triage Criteria in Reverse Sync

In Reverse Sync (the process of extracting and synchronizing Intent candidates from Reality), extracted results are triaged as follows using the three-class model:

1. **Separation of Reconstructible Information (Suppressing Verbatim Translation)**
   Enumerations of control flow and values that can be uniquely reconstructed from code are kept in Reality Documents (`code-backed fact` / `observed behavior`) and are not mixed directly as Intent Document candidates.
2. **Quarantine of Plausibly Guessable Information (Preventing Promotion of Speculation)**
   "Purposes" and "backgrounds" plausibly verbalized by AI from code are treated as **plausibly guessable** information that cannot be uniquely derived from evidence in code. Without making them settled Intent, assign low confidence as `intent candidate` or `undocumented decision candidate`, and route them to the Negotiation Loop as **slots "requiring human confirmation because rationale and external constraints are missing."**
3. **Requesting Supplementation of Non-Reconstructible Information**
   For change triggers, dependency constraints, and rejected options that cannot be reconstructed from code, do not fill them with guesswork; prompt inquiries to stakeholders or the creation of Decision Records.

---

## Verification Criteria at Gates

In Forward, Reverse, and synchronization Gates, these two principles can be incorporated as concrete inspection perspectives.

### 1. Spec Quality Gate (Detecting Degraded Copies of Implementation)

* **Question**: "Does this specification description consist only of content that can be semantically reconstructed (without guesswork) by looking at the corresponding implementation code?"
* **Judgment**: If `Yes`, it is a duplication of implementation details; remand it as a signal that information needed for future change decisions (dependent external constraints, change triggers, contractuality, Why) is missing.

### 2. Sync / Drift Gate (Detecting Silent Substitution of Premises)

* **Question**: "When a numerical value or branch in the implementation changes, can the validity of that change be explained from the 'decision criteria' held by the specification?"
* **Judgment**: For example, when `18` in code is changed to `20`, code alone cannot reconstruct whether that is "the result of tracking an amendment to the same law" or "the result of switching the governing law or service policy itself." If the decision criteria (rationale / dependencies) on the specification side have not been updated, detect it as a divergence from intent (Drift).

---

## Connection to Lane Model and Reverse

The principles and classifications in this document connect with other ISE document groups as follows:

* **Lane Model (Transformation and Information Change)**: Provides the criterion for judging duplication (semantic reconstructibility) and the trap of plausibly guessable information for "synchronization does not mean exact replication" and "why reconstruction via Reverse is difficult" in [02_transformation_and_information_change.md](../lane-model/02_transformation_and_information_change.md).
* **Lane Model (Gate)**: Functions as one of the Gate inspection perspectives in "Common Application to Forward, Remand, Reverse, and Verification" in [03_gate_and_requirements_flow.md](../lane-model/03_gate_and_requirements_flow.md).
* **ISE Reverse**: Applied as the criterion for separating facts from speculation and triaging for human confirmation in [01_why_reverse_is_needed.md](../ise/reverse/01_why_reverse_is_needed.md) (Limits of Recovery and Judgment) and [02_mosaic_and_reconciliation.md](../ise/reverse/02_mosaic_and_reconciliation.md) (From PreSpec to Official Specification).
