# History of Software Engineering and the "Unending Challenges" the Field Continues to Face

> Note: The problem identification, key concepts, and final decisions in this repository are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

The history of software engineering is a history of trial and error surrounding how to handle complexity, uncertainty, quality, and maintainability.

Software appears to be less subject to physical constraints and easy to change. In reality, however, requirements, design, implementation, testing, operation, maintenance, and organizational decision-making are intricately intertwined. A small change can affect a wide range, and a decision that was correct at the time of creation can turn into debt due to the passage of time or environmental changes.

Therefore, software development has developed not merely as a "skill of writing programs" but as an engineering endeavor to understand requirements, structure design, evaluate quality, manage change, and create systems that are maintainable over the long term.

In this paper, we organize how software engineering has developed so far as a premise for thinking about new development methods in the AI era. The purpose here is not to deny existing development methods or standards. Rather, it is to clarify the challenges that still remain in the field after confirming what they have tried to deal with.

## 1. Early Large-Scale Development and Waterfall-like Thinking

The idea of perceiving software development as a step-by-step process has existed since the era when developing large-scale computer programs became a realistic challenge.

"Production of Large Computer Programs" [1] published by Herbert D. Benington in 1956 is an important early literature that organized activities such as planning, specifications, implementation, and testing based on experience in large-scale program development. Also, "Software Requirements: Are They Really A Problem?" [2] by T.E. Bell and T.A. Thayer in 1976 is known as a study that clearly dealt with the importance and difficulty of requirements in software development.

The most famous in this flow is "Managing the Development of Large Software Systems" [3] by Winston W. Royce in 1970. Royce's paper is often talked about in connection with the "Waterfall Model" later.

However, what should be noted here is that Royce himself did not recommend a simple one-way waterfall. Royce presented a simple phase model that proceeds from requirement definition to design, implementation, and testing, and then argued that there are significant risks in such a single-pass approach. In particular, he pointed out the importance of iteration between design and implementation, early prototyping, verification, and feedback.

Nevertheless, in industry, the waterfall was understood and established as a "linear process that flows from upstream to downstream only once." This was because it was easy to handle from the perspectives of progress management, contracts, and boundaries of responsibility. Dividing it into stages of requirement definition, basic design, detailed design, implementation, and testing makes it easier to plan, manage artifacts, and clarify responsibility for each process.

However, that ease of management also created rigidity. Even when requirements change, when assumptions in design collapse, or when problems are seen only after implementation, it becomes difficult to go back through the processes. As a result, a situation was created where, although iteration is necessary in reality, it is formally required to proceed in one direction.

The achievement of waterfall-like thinking lies in the fact that it perceived software development not as chaotic work but as an engineering activity with a structure of requirements, design, implementation, and verification. On the other hand, its limit lies in the fact that it could not sufficiently handle change and uncertainty.

## 2. Standardized Software Engineering and Systematization of Forward Direction

Even after the waterfall-like phase model was criticized, software development did not become chaotic. Rather, software engineering became more widely and systematically organized through standards, bodies of knowledge, textbooks, and process models.

As a representative standard, ISO/IEC/IEEE 12207 deals with software life cycle processes. This is a standard for organizing processes involved in the entire software life cycle, such as acquisition, supply, development, operation, maintenance, and support of software [4].

ISO/IEC/IEEE 15288 deals with the wider system life cycle processes. It targets the entire system including people, hardware, software, operation, and organization, not just software alone, and organizes the life cycle from the perspective of systems engineering [5].

ISO/IEC/IEEE 29148 is a standard for requirements engineering. It deals with activities such as elicitation, analysis, specification, validation, and management of requirements, and provides a framework for handling requirements as verifiable and manageable artifacts rather than just a list of desires [6].

ISO/IEC/IEEE 42010 is a standard for architecture description. It describes architectures of systems, software, and enterprises through concepts such as stakeholders, concerns, views, and viewpoints. This is not just a standard for drawing configuration diagrams, but also a framework for organizing why that structure is necessary and whose concern it responds to [7].

ISO/IEC 25010 deals with quality models for system and software products. It organizes quality characteristics such as functional suitability, performance efficiency, compatibility, usability, reliability, security, maintainability, and portability, and provides a common vocabulary for handling quality as an object of requirements, design, and evaluation [8].

Also, SWEBOK (Guide to the Software Engineering Body of Knowledge) published by the IEEE Computer Society systematizes knowledge areas of software engineering such as software requirements, design, construction, testing, maintenance, configuration management, engineering management, process, quality, and security [9]. Representative textbooks such as Sommerville and Pressman have also organized knowledge for handling software engineering-wise, centered on requirements, design, implementation, testing, maintenance, and project management.

Apart from software engineering standards, PMBOK, a body of knowledge for project management, has also had a significant impact on development fields. PMBOK is not a methodology specifically for software development, but it provides a framework for managing projects from perspectives such as scope, schedule, cost, quality, risk, procurement, and stakeholders, and has been used in many organizations as a foundation for planning, estimation, progress management, and boundaries of responsibility [10].

In particular, conventional PMBOK had a strong color of structuring projects as manageable objects, centered on process groups such as initiating, planning, executing, monitoring and controlling, and closing. This thinking is compatible with predictive development management, which defines requirements, makes plans, proceeds with design and development, and manages progress and changes.

Afterward, PMI also incorporated agile and hybrid approaches, such as the Agile Practice Guide being bundled with PMBOK 6th Edition [11][12]. Furthermore, in the 7th Edition, it has largely shifted from a conventional process-centered configuration to a configuration centered on principles and performance domains [10]. This reflects that project management itself cannot handle today's complex projects with only a single predictive process.

Still, as a practical impact, the vocabulary of PMBOK-like planning, estimation, and progress management has reinforced the idea of managing software development in a forward direction. This is not necessarily wrong, but if uncertainty inherent in software development or knowledge that becomes clear only during implementation is neglected, estimates are treated as fixed commitments, creating excessive pressure in the field.

What these standards and bodies of knowledge indicate is that software development is not mere implementation work. Software is handled within a series of life cycles of defining requirements, performing design, implementing, verifying, evaluating quality, operating, and maintaining.

This systematization is very important. This is because many failures in software development arise not only from how to write code but also from ambiguity of requirements, absence of design decisions, neglect of quality characteristics, lack of change management, and recognition gaps among stakeholders.

On the other hand, many of these standards and textbooks are basically assembled around a forward structure of "requirements are defined, they are expanded into design, implemented, verified, and maintained." Of course, modern standards do not assume a simple waterfall. They also handle iteration, feedback, change management, maintenance, and coordination of the entire life cycle.

Still, at the center is the idea of "appropriately reflecting defined requirements and intent into downstream artifacts." Requirements are connected to specifications, specifications to design, design to implementation, implementation to testing, and then to operation and maintenance. This structure has functioned as a basic skeleton for handling software engineering-wise.

However, in real-world development, this order often collapses. Code is written first, tests become a proxy for specifications, and specifications are sometimes rediscovered later from behavior during operation. Contradictions in requirements may be found only after implementation. In the process of maintenance, changes may just stack up while the original design intent is lost.

In other words, standardized software engineering has provided a powerful intellectual foundation for organizing software development. But in the field, requirements, design, implementation, testing, and operation are always influencing each other, and discrepancies continue to arise between them. How to handle these discrepancies is a major challenge that remains to this day.

## 3. Deployment to Iterative, Incremental, and Risk-Driven Models

Waterfall and Agile are often talked about as being in opposition. However, actual history is not that simple. Even before Agile appeared, it was recognized that iteration, feedback, and risk management were important in software development.

The V-model attempted to clearly handle verification and validation by associating development processes with testing processes. It organizes how artifacts at each stage should be verified, such as acceptance testing against requirement definition, system testing against basic design, and unit testing against detailed design.

The significance of the V-model lies in its positioning of testing not as a mere confirmation task performed at the end of development, but as verification activities corresponding to each process. However, in actual operation, like the waterfall, it often accompanied fixation of processes and difficulty in going back.

The spiral model is a risk-driven development model proposed by Barry Boehm [13]. Projects proceed incrementally while repeating planning, risk analysis, engineering, and evaluation. Especially in large-scale development with high uncertainty, it emphasized discovering risks early and reducing them through prototypes and verification.

Prototype development is a method for confirming the validity of requirements and user expectations by making things that work early. It is difficult for users to judge what they truly need with only abstract specifications. By having something they can actually touch, misunderstandings or lack of requirements become clear.

Incremental development is the idea of adding functions in stages rather than completing the entire system at once. This allows for providing part of the value early and proceeding with development while receiving feedback.

RUP (Rational Unified Process) and Unified Process are also positioned as iterative and architecture-centered development processes. Instead of performing requirements, analysis, design, implementation, and testing only once, the system is matured incrementally through multiple iterations.

Looking at these flows, it can be seen that even before Agile, there was a recognition that a simple one-way process could not handle software development. The problem is not the dichotomy of waterfall vs. agile. Rather, the entire history of software development has been a search for how to incorporate uncertainty, change, risk, and feedback into the process.

## 4. The Advent of Agile and Scrum

The "Manifesto for Agile Software Development" published in 2001 brought a major shift in thinking about software development [14].

Agile prioritized working software over comprehensive documentation, customer collaboration over contract negotiation, and responding to change over following a plan. This is based on an awareness of the problem that conventional heavy processes and excessive planning rather hinder development in rapidly changing environments.

XP (Extreme Programming) emphasized developer-centered practices such as test-driven development, pair programming, continuous integration, refactoring, and small releases. This is a method that incorporated technical discipline to maintain code quality while assuming change.

Scrum emphasized delivering value and getting feedback in short cycles through mechanisms such as product backlog, sprints, daily scrums, sprint reviews, and retrospectives. It places team autonomy, transparency, inspection, and adaptation at the core, and handles complex product development as an empirical process.

The achievements of Agile and Scrum are significant. In an environment where requirements of the market and users change, it made it possible to adapt through short feedback loops rather than being fixed to long-term plans. It enabled providing working software early and correcting direction while seeing reactions of customers and users.

Also, it is important that it positioned developers not as mere workers but as members of a team who judge, improve, and learn. This clarified that software development is not routine work but continuous problem-solving.

However, the spread of Agile has also created new problems. Agile is sometimes misused in the form of "no need to write documents," "no need to plan," and "it's fine as long as something that works is put out quickly," without the technical discipline and team autonomy that Agile originally had being sufficiently understood.

As a result, even if short-term development speed increases, consistency of long-term design is lost. The overall picture of architecture becomes ambiguous, tacit knowledge is trapped within the team, documents are not updated, and reliance on individual understanding grows. Even if artifacts for each sprint seem to be piling up, as a whole system, technical debt and cognitive load increase.

Agile made adaptation to change possible. However, in order to continue adapting to change, it is necessary to maintain how to keep history of change, reasons for judgment, design constraints, and quality intent. If only speed is emphasized while this point remains weak, the field gradually becomes exhausted.

## 5. Challenges the Field Continues to Face

For over half a century, software engineering has developed. Waterfall-like phase models, life cycle standards, requirements engineering, architecture description, quality models, iterative development, risk-driven development, Agile, and Scrum. These are all important intellectual assets for handling software development better.

Nevertheless, challenges that are not resolved continue to remain in actual development fields.

### Document Drift and Absence of Documentation

The most typical problem is document drift. This refers to a state where specifications, design documents, operation procedures, and architecture documents are not updated and deviate from reality, even though reality of code and systems is changing.

Documents are useful immediately after they are created. However, as development proceeds, implementation decisions, exception handling, design changes, and operational constraints pile up. If these are not reflected in documents, the documents gradually become untrustworthy.

Once documents lose trust, they are no longer read. Documents that are not read are further not updated. As a result, only source code comes to be treated as the only reliable source of information.

However, code is not an all-powerful recording medium. Code leaves what is actually being done, but it is difficult to leave why it was done, which alternatives were discarded, which constraints were prioritized, and whose requirements were prioritized.

Also, there are many projects where sufficient documentation is not created from the beginning. In the short term, skipping document creation seems to increase development speed. However, when new members join, when incident response is needed, when large renovations are performed, or when past design decisions need to be confirmed, that debt manifests all at once.

Absence of documentation and document drift are not just problems of document management. They are problems of shared understanding about the system being lost.

### Deadlines, Estimation, and Pressure

Estimation in software development essentially includes uncertainty. Requirements may change, or technical difficulties may be found later. Constraints of existing systems may be more complex than expected. There are also limits to human understanding and communication.

Originally, an estimate is a probabilistic prediction. It should be handled including how much uncertainty there is, which risks are large, and when assumptions collapse, leading to delays.

In reality, however, estimates are often treated as commitments. A date once presented becomes an absolute goal before one knows it. Convenience of schedule is prioritized over judgments of design or quality.

As a result, sufficient design, testing, documentation, and refactoring are postponed. Even if deadlines seem to have been met in the short term, in the long term, maintainability decreases, failures increase, and changes become difficult.

What is important here is that while development methods deal with "how to make it," "which uncertainty to accept by when" is also a problem of management. If these two are confused, excessive pressure is applied to the field, and technical judgments are distorted.

### Fixation of Technical Debt

Technical debt is to choose design or implementation that makes future maintenance or change difficult for the sake of short-term convenience. This is not necessarily evil. Considering deadlines, budgets, risks, and business priorities, a decision to dare to accept debt can be rational in some cases.

The problem is that the debt is not recorded, not repaid, and eventually fixed.

Why did it become that implementation? Which constraints were there? What was compromised? When was it intended to be reviewed? If these are lost, a developer seeing it later cannot judge whether that code is just bad design or a rational choice under constraints at that time.

Code whose intent is unknown is difficult to change. This is because it is not known what will break by the change. As a result, dangerous parts are not touched, and workarounds pile up around them. Thus, technical debt turns from a mere problem of code quality into a structural problem that hinders evolution of the entire system.

### Understanding Debt

Closely related to technical debt, but should be considered separately, is understanding debt.

Understanding debt is a state where the context for understanding why a system is in its current form has been lost. Code is working. Tests are passing. Operation is running. But reasons for design decisions, trade-offs, discarded alternatives, constraints at that time, and agreements among stakeholders are lost.

In this state, every time the system is changed, developers must guess past intent. Reading code alone is not enough. It is necessary to connect old tickets, meeting notes, commit logs, Slack fragments, and memories of people who knew at that time.

Understanding debt does not only slow down development speed. It creates changes based on incorrect understanding, increases load of review, makes it difficult for new members to join, and decreases psychological safety toward the system.

If technical debt is a problem of "structure difficult to change," understanding debt is a problem of "context difficult to judge." Both plague the field, but the latter is often overlooked.

### Information Asymmetry between Management and Development Fields

In software development, there is a large information asymmetry between the management side and the development field.

The management side sees business deadlines, budgets, promises with customers, market opportunities, and organizational convenience. On the other hand, the development field sees complexity of the codebase, technical constraints, quality risks, consistency with existing design, and maintenance load.

These two viewpoints are insufficient by either one alone. However, if sufficient translation is not performed between the two, only ambiguous requirements and strong pressure come down to the field.

"I want you to make it feel right."
"Why does it take so much time?"
"Competitors have already done it."
"For now, I want you to put out something that works."

Such instructions may be understandable as business impatience. However, if development proceeds without requirements, constraints, quality standards, and priorities being clear, the field has no choice but to judge by guessing.

This state can be called Vibe Management, comparing it to Vibe Coding where AI is made to write code by atmosphere. That is, management that hands over intent that is not sufficiently verbalized to the development field by atmosphere and pressure.

Vibe Management is not a word that simply points to a manager's lack of individual ability. Rather, it is a structural problem that occurs in organizations where mechanisms to make requirements, constraints, reasons for judgment, and priorities into a sharable form are weak.

## 6. Summary: Methodologies Have Evolved, but Challenges Have Not Disappeared

Software engineering has developed over a long time.

Early large-scale development showed the necessity of managing software in stages. Standardized life cycle processes created a knowledge base for handling requirements, design, implementation, verification, and maintenance systematically. Requirements engineering showed the importance of converting ambiguous desires into verifiable requirements. Architecture description showed the necessity of manifesting structure and concerns. Quality models made it possible to handle quality not as a mere sensation but as an object of discussion, design, and evaluation. Bodies of knowledge for project management, such as PMBOK, provided a vocabulary for managing plans, estimates, progress, risks, and stakeholders. Iterative, incremental, and risk-driven models incorporated the importance of uncertainty and feedback. Agile and Scrum placed adaptation to change and short feedback loops at the center of development.

These are all important progress.

But still, in the field, problems such as document drift, absence of documentation, deadline pressure, technical debt, understanding debt, information asymmetry, and Vibe Management continue to remain.

The reason is that software development is not an activity that stabilizes simply by defining processes. Requirements change. Design sways. New facts are found during implementation. New constraints are seen from operation. Human understanding is incomplete, and organizational judgments also always change.

In other words, in software development, discrepancies continue to arise between requirements, design, implementation, testing, operation, and maintenance. Those discrepancies are not mere exceptions but the normal state of the field.

Existing software engineering has provided many intellectual foundations for this problem. However, in modern development fields, especially in an era where implementation speed further increases due to AI, these discrepancies may occur faster, larger, and in a less visible form.

What should be asked next is what truly changes in software development now that AI can write code. And what we must not lose in an era where we can make things quickly.

---

[1] Benington, H. D. (1956). "Production of Large Computer Programs". *Symposium on Advanced Programming Methods for Digital Computers*. https://dl.acm.org/doi/10.5555/41765.41799

[2] Bell, T. E., & Thayer, T. A. (1976). "Software Requirements: Are They Really A Problem?". *Proceedings of the 2nd international conference on Software engineering*. https://dl.acm.org/doi/10.5555/800253.807650

[3] Royce, W. W. (1970). "Managing the Development of Large Software Systems". *Proceedings of IEEE WESCON*. https://dl.acm.org/doi/10.5555/41765.41801

[4] ISO/IEC/IEEE 12207:2017. *Systems and software engineering - Software life cycle processes*. https://www.iso.org/standard/90219.html

[5] ISO/IEC/IEEE 15288:2023. *Systems and software engineering - System life cycle processes*. https://www.iso.org/standard/81702.html

[6] ISO/IEC/IEEE 29148:2018. *Systems and software engineering - Life cycle processes - Requirements engineering*. https://www.iso.org/standard/72089.html

[7] ISO/IEC/IEEE 42010:2022. *Software, systems and enterprise - Architecture description*. https://www.iso.org/standard/74393.html

[8] ISO/IEC 25010:2023. *Systems and software engineering - Systems and software Quality Requirements and Evaluation (SQuaRE) - Product quality model*. https://www.iso.org/standard/78176.html

[9] IEEE Computer Society. *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide)*. https://www.computer.org/education/bodies-of-knowledge/software-engineering

[10] Project Management Institute. *PMBOK Guide*. https://www.pmi.org/pmbok-guide-standards/foundational/pmbok

[11] Project Management Institute. *Agile Practice Guide*. https://www.pmi.org/pmbok-guide-standards/practice-guides/agile

[12] Project Management Institute. *A Guide to the Project Management Body of Knowledge (PMBOK Guide) - Sixth Edition and Agile Practice Guide*. https://www.pmi.org/shop/p-/book/a-guide-to-the-project-management-body-of-knowledge-%28pmbok-guide%29-%E2%80%94-sixth-edition-and-agile-practice-guide-%28english%29/00101602400

[13] Boehm, B. W. (1988). "A Spiral Model of Software Development and Enhancement". *Computer*, 21(5), 61-72. https://doi.org/10.1109/2.59

[14] Beck, K., et al. (2001). *Manifesto for Agile Software Development*. https://agilemanifesto.org/
