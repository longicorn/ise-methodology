# Document Drift and Intent Decay: Why Intent Must Be Synchronized

> Note: The problem awareness, key concepts, and final judgments in this document are the author's (a human's). AI assistance was used for structuring, refining the wording, and generating drafts. The author is responsible for the final content.

This document is a theoretical supplement to Intent-Synchronized Engineering (ISE).

It relates to the idea of "intent synchronization" proposed in `04_intent_synchronized_engineering.md`, and supplements, from a more fundamental viewpoint, the background of that idea: document drift, software archaeology, the mosaic of information, and the limits of development methods in the AI era.

However, to think more deeply about "why must intent be synchronized?", software development methodologies alone are not enough. The question involves human memory, cognitive load, verbalization, tacit knowledge, collaboration, and the way meaning changes over time.

In this document, we reframe document drift not as a mere failure to update documents, but as **a phenomenon of intent decay and synchronization failure**.

The terms "intent decay," "intent alteration," and "intent retention attenuation" used here are organizing concepts the author uses to explain the problem awareness behind ISE. They are not presented as existing standard terms or established academic terms. However, behind them lie points that connect to existing research: the forgetting curve, memory trace decay, Goal Activation / Goal Decay, cognitive load, reconstructive memory, the curse of knowledge, common ground, conversion between tacit and explicit knowledge, documentation debt, and the vaporization of architectural knowledge.

## 1. Document Drift Is a Surface-Level Problem

Document drift is often understood as "documents becoming outdated."

A specification differs from the current implementation. A README still contains old startup instructions. A design document describes a past configuration. A comment no longer matches the refactored code. API documentation describes a parameter that has been removed.

These are certainly all document drift.

From the viewpoint of ISE, however, they are the surface.

The deeper problem is that **documents, code, tests, issues, ADRs, operational procedures, and human memory are no longer connected to the same intent**.

The problem is not that a document is outdated in itself. The problem is that outdated information keeps existing, looking just as valid as information that is still correct. What is even more serious is that the reasons for decisions, constraints, rejected alternatives, risks, and background that the document was supposed to hold become disconnected from the current set of artifacts.

For example, suppose a design document says, "This process is performed synchronously." But in the current code, it is asynchronous.

The problem here is not merely that the document and the code differ.

- Is the design document outdated?
- Is the code a temporary workaround?
- Was the decision to make it asynchronous made somewhere?
- Was the reason performance, operational convenience, or an external API constraint?
- Is there another specification that depended on the process being synchronous?
- Do the tests represent the current intent?

Being unable to answer such questions is the real problem.

In other words, document drift is not a problem of "document freshness" but **a problem of lost intent connectivity**.

## 2. Externalizing Intent Is Inherently Difficult

Intent cannot be shared while it remains only in someone's head.

In software development, intent must be converted into requirements, specifications, designs, code, tests, issues, ADRs, review comments, operational procedures, and so on.

But this conversion is not mere writing.

```text
Insufficient understanding:
  intent -> put it into text

More accurate understanding:
  intent -> convert it into a form that others can execute, verify, challenge, and update
```

Much of the difficulty that requirements engineering has long dealt with stems from this problem. Eliciting requirements is not simply writing down the user's words. It is converting implicit expectations, business constraints, priorities, exceptions, handling of failures, success conditions, and unacceptable risks into a form that is implementable and verifiable.

This is inherently difficult for humans.

Intent is often vague, partial, context-dependent, and unverbalized. Assumptions that are too obvious to the person may not be shared by others. Conversely, people sometimes understand a requirement they themselves had not yet clearly grasped only through conversation, prototyping, or implementation.

Therefore, externalizing intent is not a task completed in one pass.

It is an iterative task: expressing the intent, receiving reactions, correcting misunderstandings, making it concrete, making it verifiable, and expressing it again.

## 3. Synchronizing Intent Through Text Is an Advanced Skill

For a long time, humans have conveyed emotions and intent through voice, facial expressions, gestures, collaborative work, and the context of the moment.

Spoken language has immediacy. You can rephrase while watching the other person's reaction. Facial expressions and timing can fill gaps. If there is a misunderstanding, it can be corrected on the spot. Gestures and demonstrations can convey sensations and procedures that are hard to put into words.

Text, on the other hand, transcends time and space.

Through text, intent, knowledge, judgment, and stories can be conveyed to people who are not there. They reach people days later, years later, and in some cases thousands of years later. That we can touch the thoughts and values of people in the past through ancient epics and historical documents is because text carries intent and knowledge beyond time.

In this sense, text is a very powerful synchronization medium.

However, reading and writing is not an obvious or natural ability for humans. Viewed over a long history, being able to read and write is itself a special and advanced skill. Moreover, merely being able to write characters is not enough. Writing intent in a form that is consistent, logical, readable by others, executable, verifiable, and updatable later is harder still.

Software development depends heavily on this difficult ability.

Specifications, designs, issues, pull requests, ADRs, code comments, test names, READMEs, operational procedures, and incident reports. Most of these are stored as text and read later. Even what was agreed orally cannot be referenced by future developers or AI unless it is eventually left in some textualized artifact.

In other words, software development needs to be thought of in a text-centered way.

This does not mean excluding people who are good at speaking, people who are bad at writing, people who think through diagrams or demonstrations, or people who think while writing code. Quite the opposite.

Humans have different forms in which they handle intent well. Some people organize their thoughts by talking. Some can organize them by writing. Some understand through diagrams. Some only form their ideas by moving their hands. Some can write short notes but struggle with long specifications. Conversely, some think more precisely in writing than in speaking.

Society is made up of such different types of people. Software development is the same.

The problem is that, although text is important as the final synchronization medium, traditional development sites have depended excessively on people who are good at verbalizing in text. The intent of people who are not good at textualization, though richly present in conversation, hardly remains in artifacts. As a result, when viewed later, the judgments, discomforts, constraints, and background that person held vanish as if they had never existed.

This is one cause of document drift.

Intent remains in speech or in the context of the moment and is not synchronized into textualized artifacts. Or it is textualized but recorded in a form different from the person's intent. Or the burden of writing itself is high, so updates are postponed. In this way, a gap arises between the intent that was shared at the site and the documents that remain afterward.

On this point, philosophical discussions about the relationship between spoken and written language also offer suggestions. For example, Derrida's critique of phonocentrism is useful for considering the problem that, while spoken intent appears alive in the moment, writing carries meaning across time and at the same time produces interpretive gaps. However, the subject of this document is not philosophy itself. What matters is that, in software development, voice and situational context alone cannot sustain intent, and textualized artifacts are indispensable.

AI can change this structure.

AI can turn oral explanations into drafts of specifications. It can organize fragmentary notes into issues. It can create ADR candidates from conversations. It can ask back about decisions made during implementation from code diffs. It can create explanatory text from diagrams or implementations. It can serve as a prosthesis that converts the intent of people who are not good at writing into text that lasts into the future.

Therefore, thinking in a text-centered way and not excluding people who are not good at textualization are not contradictory.

Precisely because ISE treats text as an important medium for intent synchronization, it must compensate for each person's strengths and weaknesses with AI so that intent remains in artifacts.

However, AI must not be allowed to decide the intent itself. AI can help with conversion, but humans must face what is intended, and what to adopt and what to discard. AI lowers the friction of textualization, but the vagueness of the intent itself becomes all the more visible.

## 4. Human Memory Is Reconstruction, Not Playback

One reason intent is lost over time is that human memory is not a simple recording device.

Humans do not play back past events as they were. They reconstruct the past based on current knowledge, experience, expectations, and context.

This is very important in software development.

Suppose a developer is asked about a process they implemented six months ago.

Right after implementation, they might have been able to explain, "There was an API rate limit, so I batched it," "At that time there was a migration deadline for customer A, so I added a temporary branch," or "I really wanted to split the model, but there was an existing DB constraint."

But six months later, the explanation changes.

It becomes, "I think it was probably for performance," "I feel like that was the spec at the time," "I don't remember why I did it this way," or "It looks odd now, so I want to fix it."

What is happening here is not mere forgetting.

Past intent is reconstructed to fit the current understanding. Knowledge gained after implementation, a sense of design awkwardness seen in hindsight, the current product direction, and explanations from those around replace the reasons for the judgment at the time with a different story.

As a result, even when it is the person's own memory, the current explanation does not necessarily represent the intent at the time accurately.

From the viewpoint of ISE, this property is decisive.

Intent must be synchronized before it is lost. More precisely, **it must be synchronized before it is reconstructed into a different explanation**.

## 5. Intent Does Not Merely Vanish; It Is Altered

When thinking about document drift, it is often described as "not updated," "forgotten," or "outdated."

But the problem of intent is more troublesome than that.

Intent does not simply fade and disappear. It is altered into a different intent.

```text
Simple forgetting model:
  intent -> passage of time -> fades / disappears

Decay model emphasized in ISE:
  intent -> passage of time -> post-hoc rationalization / reinterpretation / replacement by a different story
```

For example, a temporary workaround introduced because of a deadline may, years later, be explained as "this business must have needed this kind of exception." A constraint adopted to avoid a performance problem may later be spoken of as a "design philosophy." A mere implementation omission may be treated as an existing specification.

At this point, the problem is no longer simple information loss.

A wrong explanation becomes the premise of new decisions.

Based on that explanation, more code is added. Tests are written. Specifications are updated. Operational procedures are created. In this way, the altered intent becomes fixed in the set of artifacts.

In this state, it is very difficult to later recover the original intent.

Code shows the current behavior. Tests show part of the expected behavior. Commit logs show traces of changes. But with these alone, it is difficult to fully recover "why that decision was adopted at the time," "what was being avoided," and "which constraints were important."

Therefore, in ISE, synchronizing intent must be treated as part of maintenance work.

## 6. Attenuation of Intent Retention

To explain the phenomenon of intent weakening over time, one could create an intuitive metaphor. However, as far as I have investigated, it is hard to say that such a metaphor is used as an established technical term in software engineering or psychology.

Close existing concepts include Ebbinghaus's forgetting curve, memory trace decay, Goal Activation (which deals with the activation and retrieval of goals and intentions), Goal Decay (in which unused goals weaken), and Architectural Knowledge Vaporization, discussed in the field of software architecture.

So in this document, rather than setting up an original metaphor, we treat it as **attenuation of intent retention**.

Attenuation of intent retention is the phenomenon in which intent loses clarity over time, becomes harder to retrieve, and becomes more likely to be reconstructed into a different explanation.

```text
Clarity of intent

100% |●  right after implementation / discussion
     |  ●
     |    ●
     |      ●  reasons for decisions begin to blur
     |         ●  begins to be reinterpreted by current understanding
  0% |______________●________ time
```

The rate of attenuation of intent retention is not constant.

The more abstract the intent, the shorter it lasts. For example, intents such as "give users a sense of security," "make operations easier," or "make it easy to extend in the future" quickly become subject to shifting interpretations.

More concrete intents tend to remain relatively well. For example, constraints such as "this API allows up to 10 requests per second," "this column does not allow NULL," or "this process must be idempotent" are easy to verify and easy to reconfirm.

However, even for a concrete intent, the reason why the constraint was necessary is lost.

Even if the constraint "up to 10 requests per second" remains, it may be lost whether it comes from an external API contract, a provisional value based on measurements at the time, or just a safe-side placeholder.

Therefore, intent synchronization needs to preserve not only the What but also the Why.

## 7. Different Representation Forms Preserve Different Intent

Not all artifacts can hold the same kind of intent.

Code, tests, specifications, ADRs, issues, comments, operational logs, and meeting notes each hold different information.

| Representation | What it preserves well | What it tends to lose |
|---|---|---|
| Code | Current structure, control, data transformation, executable How | Reasons for decisions, rejected alternatives, business Why |
| Tests | Expected behavior, boundary conditions, regression prevention | Value judgments, design reasons, intent outside the spec |
| Specifications | Expected features, constraints, externally visible What | Decisions made during implementation, operational reality |
| ADRs | Adopted decisions, reasons, alternatives | Reality that changed after implementation, detailed behavior |
| Issues | Change requests, background, flow of conversation | Final state after implementation, formally agreed scope |
| Pull Requests | Diffs, reviews, part of the reasons for changes | Long-term design context, upstream value judgments |
| Comments | Local Why, cautions | System-wide intent, validity after updates |
| Operational logs | Reality that actually occurred, incidents, workarounds | Original design intent, future direction |
| Narratives | Experiential Why, user context, discomfort | Strict verification conditions, implementation details |

For this reason, the problem cannot be solved merely by deciding "which one to make the single source of truth."

With code alone, the Why is lost. With specifications alone, Reality is lost. With ADRs alone, current behavior is lost. With issues alone, the final agreement becomes vague. With tests alone, value judgments are lost.

What is needed is to keep connecting multiple artifacts to the same intent.

## 8. Common Ground Is Not Maintained Automatically

In collaborative work, participants need to share the same premises.

However, common premises are not maintained naturally. Through conversations, documents, reviews, tests, implementation, and operations, we must keep updating what has been shared, what is not yet agreed, and what has changed.

In software development, this common ground is scattered across multiple places.

One person reads the specification. Another reads the code. Another remembers a conversation on Slack. Another knows operational constraints from incident response experience. Another knows the agreement with the customer.

What is "known" to each person may not be shared by the team as a whole.

In this state, "I assumed it was obviously like this" easily occurs.

People who know have difficulty returning to the viewpoint of people who do not. They assume that premises obvious to themselves are also known to the other party. This connects to the problem known as the curse of knowledge.

From the viewpoint of ISE, intent synchronization is also the maintenance of common ground.

It is not enough to simply put information somewhere. We need to put it in a form the team can handle: which intents have been adopted, which are hypotheses, which are outdated, which have been rejected, and which have been checked against the current Reality.

## 9. The Higher the Cognitive Load, the More Drift Increases

Document drift does not occur simply because of negligence.

In many cases, people know that updates are necessary. But amid implementation, testing, review, incident response, specification adjustments, and release preparation, document updates get postponed.

This is not only a matter of weak will.

Human cognitive resources are limited. When complex code, vague requirements, fragile tests, old designs, multiple stakeholders, and imminent deadlines pile up, developers first concentrate on stabilizing the Reality in front of them.

As a result, recording the reasons for decisions, reflecting changes in specifications, updating ADRs, fixing operational procedures, and searching for and updating related documents are postponed.

And the postponed updates are made after the attenuation of intent retention has progressed.

Without understanding this structure, saying "let's write documents" does not solve the problem. What is needed is to lower the cost of synchronization, localize the targets of updates, use AI as a prosthesis, and build drift detection into the development process.

## 10. Drift Arises From Time Gaps Between Artifacts

Document drift arises because artifacts are updated at different times.

Code changes today. Tests are fixed tomorrow. The specification is updated next week. Operational procedures are corrected after an incident occurs. An ADR may never be written. An issue may remain closed.

The longer this time gap, the more intent is lost.

```text
T0: intent is born
T1: code changes
T2: tests change
T3: the specification remains outdated
T4: another developer reads the outdated specification
T5: a new change is made based on a wrong understanding
```

What matters here is not the order of development itself.

You may start from the specification, from the code, from incident response, or from a discovery made during implementation. The problem is that a change in intent that occurs in one artifact is not synchronized to the others and time passes.

Therefore, ISE needs to handle not only forward development but also backward recovery, lateral synchronization, and reflection from operations.

## 11. In the AI Era, Intent Decay Surfaces Faster

AI does not automatically solve the problem of document drift.

On the contrary, in an environment where intent is not synchronized, it may amplify the problem at high speed.

AI increases the speed of writing code. It can explain existing code. It can generate tests. It can draft documents. It can summarize diffs.

However, AI reasons within the information space it is given. If old specifications, vague issues, READMEs that deviate from reality, design documents no longer in use, and fragmentary conversation logs are mixed in, AI may integrate them plausibly.

This is the same problem as human software archaeology. But the speed and scale are different.

Where a human would feel something is off and stop, AI may generate a coherent explanation. Information a human would suspect to be "probably outdated" may be used by AI as a plausible basis in context.

Therefore, in the AI era, intent synchronization becomes more important.

It is not enough to hand AI a large amount of information. We need to make it possible to distinguish the source of information, its tense, its reliability, its verification status against the current Reality, adopted intent, and rejected intent.

## 12. Intent Recovery Is Necessary but Not the Ideal State

In existing systems, there are situations where lost intent must be recovered.

We cross-check code, tests, commit logs, issues, PRs, operational logs, incident records, old documents, and human memory to recover the current meaning. This work is important, and there is much room for AI to support it.

However, Intent Recovery is not the ideal state.

What can be recovered from code is mainly the As-Is. How it currently works, what structure it has, and which tests represent expectations are relatively easy to recover.

On the other hand, the As-Intended is hard to recover.

What was originally wanted? Why was this option adopted? Which alternatives were discarded? Which constraints were emphasized? Which compromises were temporary? Which decisions were based on political, contractual, or operational constraints?

These are easily lost unless they are made explicit in artifacts.

Therefore, the basic strategy of ISE is not to completely recover lost intent afterward.

It is to synchronize intent in small, local, continuous steps before it is lost.

## 13. Therefore, Intent Synchronization Is Necessary

Document drift is not a problem of documents becoming outdated.

It is a problem in which intent is disconnected from the set of artifacts, lost over time, altered, and reused as a false common ground.

Human memory is reconstructed. Tacit knowledge is not naturally shared. People who know have difficulty returning to the viewpoint of those who do not. Synchronizing intent through text is powerful but an advanced skill, and people differ in their strengths and weaknesses. In sites with high cognitive load, recording intent is postponed. Artifacts are updated at different times. AI may consume those gaps at high speed and amplify them in plausible forms.

Given this structure, the claim of ISE becomes simple.

```text
Intent is lost.
Intent is altered.
Artifacts drift apart.
Therefore, intent must be continuously synchronized.
```

What matters is not creating perfect documents once. Nor is it letting only people who are good at textualization participate in development.

What matters is making it possible to return intent to the set of artifacts when it is born, when it changes, when it is discovered during implementation, and when it is negated in operation.

ISE is a way of thinking that places this synchronization at the center of development.

---

## References

[1] W. S. Tan, M. Wagner, and C. Treude, "Detecting outdated code element references in software repository documentation", *Empirical Software Engineering*, 2023. https://link.springer.com/article/10.1007/s10664-023-10397-6

[2] H. H. Clark and S. E. Brennan, "Grounding in Communication", 1991. https://collablab.northwestern.edu/CollabolabDistro/nucmc/ClarkAndBrennan-GroundingInCommunication-1991.pdf

[3] J. Sweller, P. Ayres, and S. Kalyuga, *Cognitive Load Theory*, Springer, 2011. https://link.springer.com/book/10.1007/978-1-4419-8126-4

[4] B. Wagoner, "What makes memory constructive? A study in the serial reproduction of Bartlett's experiments", *Culture & Psychology*, 2017. https://journals.sagepub.com/doi/10.1177/1354067X17695759

[5] B. Keysar, "The Illusory Transparency of Intention: Linguistic Perspective Taking in Text", *Cognitive Psychology*, 1994. https://www.sciencedirect.com/science/article/pii/S0010028584710061

[6] C. Camerer, G. Loewenstein, and M. Weber, "The Curse of Knowledge in Economic Settings: An Experimental Analysis", *Journal of Political Economy*, 1989. https://www.cmu.edu/dietrich/sds/docs/loewenstein/CurseknowledgeEconSet.pdf

[7] ODI, "Management Techniques: SECI Approach", 2009. https://odi.org/en/publications/management-techniques-seci-approach/

[8] E. M. Altmann and J. G. Trafton, "Memory for goals: an activation-based model", *Cognitive Science*, 2002. https://www.sciencedirect.com/science/article/pii/S0364021301000581

[9] G. Borrego, A. L. Morán, R. R. Palacio, A. Vizcaíno, and F. García, "Towards a reduction in architectural knowledge vaporization during agile global software development", *Information and Software Technology*, 2019. https://www.sciencedirect.com/science/article/abs/pii/S0950584919300898
