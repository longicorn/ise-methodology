# The Advent of AI, Vibe Coding, and the Limits of SDD/IDD

> Note: The problem identification, key concepts, and final decisions in this repository are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

In the previous paper, we organized the problems that existed in development fields since before AI.

Documents drift. The information space becomes mosaicified. Ambiguous requirements flow from upstream. Technical debt makes understanding difficult, and understanding debt makes changes dangerous. As a result, developers must first reconstruct the meaning of the existing system before writing code.

AI did not suddenly create this problem.

However, AI has rapidly amplified this problem.

With the advent of Large Language Models (LLMs) and AI agents, the cost of generating code has decreased dramatically. If you instruct in natural language, AI writes code, adds tests, changes configuration files, and even generates documentation. Software development has clearly entered a new stage if we only look at the speed of implementation itself.

On the other hand, as seen in the previous paper, the real bottleneck in the field has not always been in code generation alone. Rather, what was difficult was judging what to make, how the existing system is, which information is correct, and which intent should be observed.

What happens if only code generation by AI is accelerated in this situation?

In this paper, we organize new attempts such as Vibe Coding, SDD (Spec-Driven Development), IDD (Intent-Driven Development), Harness Engineering, and AI-DLC or AI-SDLC, and consider why they are important yet have not yet completely solved the essential problems.

Note that terms such as SDD, AI-DLC, AI-SDLC, IDD, and Harness Engineering are not necessarily established as monolithic standard terms at this point. In this paper, we organize them according to the author's awareness of the problems while referring to recent trends talked about under each name.

## 1. AI has Commodified Code Generation

In development before AI, writing code itself required significant time and skill. Of course, design, understanding requirements, verification, and maintenance were also important, but implementation clearly occupied a large amount of work.

With the advent of LLMs, this premise has changed.

AI can generate functions, classes, UIs, APIs, tests, configurations, migrations, and documentation in a short time. Agents integrated into IDEs or CLIs read files, edit multiple files, execute tests, and fix failures. They are moving toward autonomously proceeding with part of development work rather than mere completion.

An article on AI in the SDLC published by IBM in 2026 also explains that AI is entering the entire SDLC, such as planning, analysis, design, coding, testing, deployment, and maintenance [1]. Also, a survey paper from 2026 reports that generative AI shows significant time-saving effects in design, implementation, testing, and documentation, while the effects are relatively low in early phases such as planning and requirements analysis [2].

This is an important suggestion.

AI is strong at writing, arranging, and converting. However, judgments such as what to make, which requirements are correct, which constraints should be prioritized, and which existing information should be trusted are still difficult.

In other words, while implementation speed has increased due to AI, the difficulty of defining, verifying, and maintaining intent has not disappeared.

## 2. Vibe Coding: Development that Leaves Intent to Inference

As code generation by AI became common, the term Vibe Coding spread.

Vibe Coding is a development style where you convey what you want to make to AI in natural language and interactively make software while accepting the code AI generates. This term became widely known after Andrej Karpathy used it in 2025 [3].

The strength of Vibe Coding is clear.

The speed of making prototypes is very fast. It demonstrates great power for small tools, demos, personal apps, experimental UIs, and idea verification. Even non-engineers have become able to make something that works to some extent if they ask AI in natural language.

This has a large significance as the democratization of software development.

However, there is a fundamental precariousness in Vibe Coding.

In Vibe Coding, human intent is converted into code by AI inference without being strictly externalized. Even if requirements, constraints, exceptions, quality standards, and future changeability are ambiguous, AI returns an implementation that looks plausible.

The problem here is not only that AI makes mistakes.

More serious is that AI converts ambiguous intent into neat code and neat explanations. Even if input is ambiguous, output looks confident. Even if information is lacking, code is generated. Even if specifications are undecided, implementation moves forward.

This directly connects to Vibe Management mentioned in the previous paper.

Ambiguous requirements come from upstream. Existing systems are mosaicified. Technical debt is also high. In that state, if you ask AI to "implement it feel right," ambiguous intent is directly and rapidly codified.

In the era when humans were writing by hand, the slowness of implementation speed worked as a kind of friction. Developers could have time to have doubts, confirm, and rethink design while implementing. But AI greatly reduces that friction.

Reducing friction itself is good. However, if only friction is reduced while intent is ambiguous, incorrect judgments are also rapidly fixed into artifacts.

The problem with Vibe Coding is not that code generation is too fast. It is that code generation alone precedes while definition and verification of intent cannot keep up.

## 3. SDD: Attempts to Place Specifications at the Center

As a natural reaction to the precariousness of Vibe Coding, SDD (Spec-Driven Development) appeared.

SDD is a thinking that instead of making AI write code suddenly, you first make specifications and proceed with implementation, tasks, tests, and verification centered on those specifications. GitHub's Spec Kit tries to handle specifications not as static documents but as executable artifacts that evolve with the project [4]. In the explanation of Spec Kit, specifications are positioned as the source of truth used by AI agents for code generation, testing, and verification [5].

This direction is important.

AI needs structure. Rather than letting it implement with only ambiguous prompts, output is likely to be stable if requirements, acceptance conditions, constraints, task decomposition, and design policies are manifested. By writing specifications, humans themselves can also rethink what they want to make.

Also, SDD contains important reflections on Vibe Coding.

If you leave code to AI, the quality of context passed to AI becomes important. Arranging natural language specifications, design policies, constraints, acceptance conditions, and project conventions is indeed important in development in the AI era.

However, SDD also has its limits.

## 4. Practical Problems Faced by SDD Tools

Since 2025, tools that support SDD have been increasing. Multiple attempts to control AI development centered on specifications have appeared, such as Spec Kit, Kiro, BMad, OpenSpec, Tessl, Spec Kitty, etc. `spec-compare` compares these SDD tools and organizes practical challenges [6].

Particularly important among them is weakness toward change.

Many SDD tools demonstrate power in scenes of making new functions. Defining requirements, making specifications, making plans, decomposing into tasks, and letting AI implement. This flow is easy to understand in the case of making from 0 to 1.

However, most real-world development is not from 0 to 1. It is 1 to n development that changes existing systems. Changing button colors, fixing wording, fixing small bugs, slightly changing existing specifications, or renovating while maintaining consistency with past designs. Such tasks occur daily.

The analysis of `spec-compare` points out that many SDD tools are optimized for creating new functions and tend to become an excessive workflow for lightweight changes of existing specifications or small fixes [7]. For example, for a trivial UI change, if long requirement documents, design, task decomposition, and regeneration are required, it is too heavy in practice.

This problem is not a simple tool UX problem.

If most software development is changing existing systems, how to handle changes is indeed the central problem. Nevertheless, if many SDD tools make creating new functions the main battlefield, it deviates from reality in the field.

Designs like OpenSpec, which separate current specifications and proposed changes and handle them as diffs, show an important direction for this problem [7]. On the other hand, even if diff management is introduced, why that change is necessary, which existing intent it conflicts with, and whether the reality on the code side matches the specification are not automatically solved.

## 5. SDD Does Not Eliminate Drift but can Reoccur in Another Form

The central idea of SDD is to make specifications the source of truth.

This is very attractive. If specifications are correct and always updated, and code is generated and verified according to those specifications, document drift seems to decrease significantly.

However, there is a difficult premise here.

Specifications need to be correctly updated always.

In real-world development, urgent fixes come in. Code is fixed directly in incident response. Temporary workarounds are put in for operational convenience. Exceptions are added for customer response. Design assumptions collapse during implementation. One notices ambiguity of specifications only after writing tests.

At this point, if the discipline to go back to specifications and update them is not maintained, drift will reoccur even in SDD.

Furthermore, if the specification generated by AI itself is based on mosaic information, the specification may be contaminated from the beginning. If AI makes specifications by mixing old documents, current code, unimplementable desires, and past temporary responses, it will be a specification that looks neat but deviates from reality.

The mosaic of information mentioned in the previous paper does not disappear even in SDD. Rather, by AI formatting mosaic information into readable specifications, there is a risk that incorrect information is converted into a more authoritative form.

In other words, SDD is an important countermeasure against document drift. However, making specifications itself does not guarantee synchronization of reliable intent.

## 6. IDD: Emphasizes Intent, but Synchronization Problems Remain

On the extension of SDD, there is an idea of IDD (Intent-Driven Development).

It is a thinking of proceeding with development centered on not just specifications but intent, such as why to make it, which value you want to realize, which constraints should be observed, and which judgments should be prioritized.

This is one step deeper than SDD.

Because specifications often describe "What," but what is easily lost in development fields is "Why" and "Why not." Not just which proposal was adopted, but which proposal was discarded. Why that quality characteristic was prioritized. Why that exception was allowed. Why refactoring was not done now. These are difficult to leave in code or specifications.

IDD notices this problem.

However, just emphasizing intent is not enough.

Intent is not something that ends once you write it. It transforms during implementation. New facts are found in operation. Priorities change by user reactions. Original intent is corrected due to technical constraints. Intents of multiple stakeholders conflict.

Therefore, the problem is not just "writing intent" but "continuously synchronizing intent and artifacts."

Even if IDD points out the importance of intent, if how to synchronize intent among code, tests, specifications, operation, issues, and judgment history is not designed, drift will eventually reoccur.

It is important to place intent at the center. However, if you place intent at the center, you need even a mechanism to synchronize it.

## 7. Harness Engineering: Attempts to Enclose AI with Constraints and Verification

In parallel with SDD and IDD, the term Harness Engineering is also starting to be used.

The harness here does not point to the AI model itself, but to the execution environment, tools, constraints, tests, prompts, memory, workflow, and feedback loops surrounding the AI agent. In recent discussions, it is said that not only the performance of the model alone but also what information is passed, what tools it is allowed to use, by what constraints it is stopped, by what tests it is verified, and what history is handed over to the next time greatly influence the practicality of AI agents [10][11].

This plays a role close to charters or project rules in SDD. For example, by placing architectural boundaries, naming conventions, prohibited items, test execution, type checking, lint, security policies, review conditions, and executable quality gates around the AI agent, AI output is converged to a certain range.

This idea is important.

Just letting AI write code freely easily breaks existing designs or team conventions. Just writing rules in context makes AI forget them, misunderstand them, or mix them with other information. Thus, it becomes necessary to design not just natural language instructions but the execution environment itself including tests, types, lint, CI, scripts, templates, review procedures, and observability.

However, it is precarious to handle Harness Engineering excessively as a new invention.

In reality, much of it is also an extension of what has been emphasized in conventional software engineering. Cutting target range small. Deciding responsibility boundaries. Using correct technical terms. Manifesting design policies. Confirming with tests. Stopping if broken in CI. Refactoring periodically. Checking health of code. These were necessary since before AI.

What changed in the AI era is that the necessity to explicitly embed them in the environment where AI works, rather than leaving them only to human tacit knowledge or review, has strengthened.

On the other hand, Harness Engineering also has its limits.

Tests and lint are strong against predefined constraints. However, what should be constrained in the first place, which design intent should be observed, and which exceptions should be allowed are not decided by the harness alone. A harness that observes incorrect specifications stabilizes incorrect output. A harness that forces old architectural rules hinders necessary design changes. Rules made from mosaicified information fix incorrect premises rather than correcting AI behavior.

In other words, Harness Engineering is an important practice when using AI. However, it does not substitute for intent. Rather, to decide what should be put into the harness, clarification and update of intent become necessary.

## 8. AI-DLC / AI-SDLC: Attempts to Wrap the Entire Life Cycle with AI

Movements to incorporate AI not just into SDD or IDD but into the entire software development life cycle are also appearing.

Methodologies such as AI-DLC try to manage work of AI agents including intent, tasks, quality gates, hooks, execution states, operation, and retrospection [8]. Frameworks calling themselves AI-SDLC try to incorporate execution, review, compliance, and governance of AI agents into the entire SDLC [9].

These attempts are clearly more structured than mere Vibe Coding.

Instead of letting AI write code freely, place quality gates. Force tests and type checking. Sandwich human reviews. Prevent dangerous operations with hooks. Save states and hand them over to the next session. Include even operation and retrospection.

This is a necessary direction. However, it does not make design, testing, review, and maintenance that were necessary in conventional development unnecessary. Rather, it is a direction to manifest them in a form AI can handle and incorporate them as executable constraints.

Still, however, problems remain.

Quality gates are strong in the range where tests exist. Type checking is strong against constraints expressed in types. Reviews are strong against diffs that can be reviewed. Governance is strong against defined rules.

But if intent is ambiguous, what should be verified in the first place is not determined. If existing information is mosaicified, the context passed to AI itself is contaminated. If technical debt is high, it may be contrary to design intent even if tests pass. If specifications are old, specification compliance rather justifies incorrect implementation.

In other words, AI-DLC and AI-SDLC are important as control of AI execution. However, if intent to be controlled itself is ambiguous, old, or dispersed, even if the life cycle is arranged on top of that, essential problems remain.

## 9. Problems Existing AI Development Methods Have Not Yet Reached

As seen so far, Vibe Coding, SDD, IDD, Harness Engineering, AI-DLC, and AI-SDLC each have meaning.

Vibe Coding has power to shape ideas immediately.

SDD showed the importance of giving structure to AI and proceeding with development centered on specifications.

IDD showed the necessity of emphasizing intent behind specifications.

Harness Engineering showed the necessity of enclosing AI agents with constraints, tools, tests, and feedback loops.

AI-DLC and AI-SDLC showed the direction of controlling AI agents throughout the entire life cycle.

But still, the following problems remain:

- How to reconstruct the current truth of existing systems?
- How to select reliable information from a mosaicified information space?
- When specifications and code deviate, which to update and how?
- How to handle small changes without excessive workflows?
- How to handle lost intent in systems with much technical debt?
- When a human fixes code directly, how to return to specifications or intent?
- When AI ignores or misunderstands specifications, how to detect it?
- When intent changes, how to reflect it in code, tests, documentation, and operation?
- How to handle software archaeology, which reconstructs the meaning of existing systems, as a central problem of development methods?
- When only development speed or meeting deadlines becomes the theme of AI use, how to protect engineering quality?

These are not problems of code generation alone.

Nor are they problems of specification creation alone.

Particularly important is that many AI development methods do not sufficiently focus on the task of reconstructing the meaning of existing systems.

Organize requirements. Write specifications. Design. Decompose into tasks. Let AI implement. Confirm with tests. This flow is easy to understand and easy to manage. However, in real-world development, before that, one must understand what the existing system is. Which specification is still valid now? Is code behavior the original specification? Is it just a remnant of a past temporary response? Do tests represent current intent? Has an operational workaround become the de facto specification?

This is the problem of software archaeology mentioned in the previous paper.

Nevertheless, many development methods in the AI era still tend to focus on "how to define and generate what to make from now." Even when they become SDD or IDD, they pay attention to writing new specifications or intent, but the task of reconstructing the current truth from mosaicified existing information and handling lost intent is unlikely to be placed at the center.

In other words, the problem is not just that they have not escaped from a one-way development order. In the first place, software archaeology is necessary as a premise of development in the field, yet that work is not handled as a central problem of methodologies.

Another problem is that the theme of AI use often leans too much toward "how to make quickly" or "how to meet deadlines."

Speed is important. Deadlines also cannot be ignored as real-world constraints. However, the purpose of software engineering is not to just put out artifacts in a short time. It is to correctly understand requirements, maintain design, verify quality, maintain changeability, and make systems that can be operated over the long term.

If AI is subordinate to this purpose, AI becomes a powerful tool. On the other hand, if AI is used only for speed and deadlines, it can also be a device that piles up ambiguous intent, shallow design, insufficient testing, and neglected debt even faster.

At the center of the problem is the relationship between intent and artifacts.

Requirements, specifications, design, code, tests, operation, issues, reviews, and judgment history are each different artifacts. But they must originally be connected to the same intent.

What truly becomes difficult in development in the AI era is not writing code itself, but maintaining whether these artifacts continue to be connected to the same intent.

## 10. Summary: From the Era of Code Generation to the Era of Intent Synchronization

AI has greatly changed the speed of software development.

However, as speed increased, problems that were previously difficult to see were also exposed. Ambiguous requirements become code while being ambiguous. Mosaicified information is converted into neat specifications. Old intent is misused as the basis for new implementation. Deviations between specifications and code spread faster and wider by AI.

Vibe Coding exposed this problem in the most naive form.

SDD appeared as an important reflection on it.

IDD showed the necessity to see intent, not just specifications.

Harness Engineering showed the necessity to enclose AI with constraints and verification.

AI-DLC and AI-SDLC showed the necessity to control AI agents throughout the entire life cycle.

But it is still not enough.

What is needed is not just a way to make AI write code. Not just a way to make specifications. Not just a way to manage AI agents.

What is needed is a way of thinking to continuously synchronize intent and real-world artifacts.

In the next paper, from this awareness of problems, we proceed to the perspective of "synchronization of intent," which the author considers important. That is, a way of thinking that perceives code, tests, documentation, specifications, operation, and judgment history not as a mere collection of artifacts but as objects that should be synchronized to a common intent.

---

[1] IBM. "AI in the SDLC". https://www.ibm.com/think/topics/ai-in-sdlc

[2] Gurgul, V., Gubela, R., & Lessmann, S. (2026). "The State of Generative AI in Software Development: Insights from Literature and a Developer Survey". arXiv:2603.16975. https://arxiv.org/abs/2603.16975

[3] Ars Technica. "Will the future of software development run on vibes?" https://arstechnica.com/ai/2025/03/is-vibe-coding-with-ai-gnarly-or-reckless-maybe-some-of-both/

[4] GitHub. "spec-driven.md". https://github.com/github/spec-kit/blob/main/spec-driven.md

[5] GitHub Blog. "Spec-driven development with AI: Get started with a new open source toolkit". https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/

[6] Cameron Sjo. "Spec-Driven Development Tools Comparison". https://github.com/cameronsjo/spec-compare

[7] Cameron Sjo. "Iterative Development & Spec Modifications". https://github.com/cameronsjo/spec-compare/blob/main/docs/iterative-development.md

[8] AI-DLC. "AI-Driven Development Lifecycle". https://ai-dlc.dev/

[9] AI-SDLC. "Govern AI Agents Across Your Entire SDLC". https://ai-sdlc.io/

[10] Yoonho Lee, et al. "Meta-Harness: End-to-End Optimization of Model Harnesses". arXiv:2603.28052. https://arxiv.org/abs/2603.28052

[11] Linyue Pan, et al. "Natural-Language Agent Harnesses". arXiv:2603.25723. https://arxiv.org/abs/2603.25723
