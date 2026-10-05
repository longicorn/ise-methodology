# Document Drift and Software Archaeology: A Mosaic of Information That Has Plagued the Field

> Note: The problem identification, key concepts, and final decisions in this repository are by the author (human). On the other hand, AI assistance was received for sentence structure, organization of expressions, and draft generation. Responsibility for the final content lies with the author.

In the previous paper, we organized how software engineering has developed. Standards that systematize requirements, design, implementation, verification, and maintenance. Models for handling quality. Bodies of knowledge for managing projects. Agile that emphasizes iteration, feedback, and adaptation to change.

These are all important intellectual assets for handling software development better.

However, in actual development fields, many challenges still remain. Particularly serious is the situation where, before embarking on implementation, one must first reconstruct "what is this system now?", "what is the correct information?", and "why is it like this?".

Developers in the field daily dig up specifications, design documents, code, tests, issues, pull requests, commit logs, Slack, meeting notes, operational procedures, and human memory. They compare old records with current code, read through contradictory information, guess lost intent, and discern the scope that can be changed.

This is not a mere "investigation." Rather, it is a task that should be called software archaeology.

In this paper, we organize how document drift, the mosaic of information, Vibe Management, technical debt, and understanding debt have intertwined and made development in the field difficult.

Note that the terms "mosaic of information," "mosaicification," and "Vibe Management" used in this paper are unique designations by the author to explain problems observed in the field. They are not presented as existing standard terms or widely agreed-upon academic terms.

## 1. "Excavation before Implementation" is Normal in Development Fields

When adding a new feature or realizing a fix in development fields, what developers do first is often not implementation.

First, they confirm how the target function currently works. They look for specifications. They read design documents. They follow related code. They look at tests. They look for past issues or pull requests. They check commit logs. They read records of operational procedures or incident response. If necessary, they ask people who knew at that time.

At this point, information is not monolithic.

The specification says A, but the code is B. Tests assume C. A judgment D was made in past issues. A supplementary note E remains on Slack. A workaround F is used in operation.

Developers do not just read these. They must judge which is still correct, which is a relic of the past, which is a desire, and which is a temporary response.

This work is close to software archaeology.

However, unlike normal archaeology, the object is not a static ruin. It is a product that is still moving and a system that is still being changed. Even while investigating, another team changes the code, another issue is created, and new operational constraints are born.

Developers are like building a new structure on top of moving ruins while excavating them.

## 2. Document Drift is Not a Simple Failure to Update Documents

A representative problem behind this situation is document drift.

Document drift is specifications, design documents, READMEs, operational procedures, and architecture documents deviating from actual code and system behavior.

At first glance, this looks like a simple failure to update documents. In reality, however, it contains deeper problems.

Documents may contain not just "what to do" but also "why it is done," "on what premise that judgment was made," "which constraints were prioritized," and "what was decided not to be done." Even if code is changed, if these contexts are not updated, the documents not only deviate from the current reality but also become dangerous information that incorrectly conveys past intent.

Furthermore, drift does not happen in only one direction.

The code may move ahead while documents are left behind. Conversely, future visions may be written in documents while implementation does not yet exist. Old design decisions may remain and coexist with new design decisions. Operational workarounds may have become the de facto specification, but not reflected in the formal specification.

In other words, document drift is not a simple problem of "documents being old." It is a problem of the current truth being dispersed and deviating from each other among code, tests, specifications, design, operation, and decision-making.

Once documents drift, they are no longer read. Documents that are not read are not updated. Documents that are not updated lose further trust.

Entering this cycle, an organization is gradually forced into a state of "you can understand if you read the code." However, you cannot understand everything just by reading the code. What you can tell from the code is mainly current behavior and structure. Past reasons for judgment, rejected proposals, political constraints, agreements with customers, and room for future changes are difficult to reconstruct with code alone.

## 3. Mosaic of Information

As document drift proceeds, the information space becomes mosaicified.

The mosaic of information is a state where information with different reliability, tense, intent, and provenance coexist in one information space, making it difficult for an outsider to determine which is the current truth.

Notion, Confluence, Google Drive, GitHub Wiki, README, Issues, Pull Requests, Slack, email, meeting notes. Many organizations use these to accumulate information. However, information being in the same place is different from having the same reliability.

In the same workspace, currently correct specifications, already deprecated design policies, someone's future vision, notes of features that were aborted midway, personal work logs, records based on misunderstandings, sales materials, and temporary responses during incidents are lined up.

These are all "information." But they are not the same kind of information.

In the mosaic state, at least the following types of information are mixed.

| Type | Description |
|---|---|
| Current Truth | Information that matches current implementation, operation, and agreement |
| Past Truth | Information that was once correct but is now obsolete |
| Future Vision / Desire | Information about visions or plans one wants to realize, but does not match current implementation |
| Temporary Response | Workarounds adopted temporarily due to incident response or deadline convenience |
| Personal Record | Work logs, notes, fragments in the middle of thinking |
| Noise | Peripheral information that cannot be directly used for current development judgment |
| Incorrect Information | Information that was not true even at the time of recording |
| Remnant | Traces of aborted plans, disappeared features, or uncompleted designs |

What is important is that these are not born from malice.

In many cases, someone is trying to write correct information at that point. Or, it was indeed correct at that point. But systems change, requirements change, organizations change, and context is lost. As a result, the information space becomes mottled.

In this state, developers do not just read information. They must classify information, estimate reliability, and check against current code and operation.

In other words, the mosaic of information requires additional cognitive work from developers.

## 4. Vibe Management: Ambiguous Intent Flows into the Field

The mosaic of information is a problem regarding the past and present of existing systems. On the other hand, ambiguous information flows into development fields from upstream as well.

Requirements are handed over ambiguously. Constraints are not clear. Priorities are not shared. Success conditions are not defined. What not to make is not decided. Why it is being made now is not explained.

However, only deadlines are clear.

This state can be called Vibe Management.

Vibe Management is a management state where development proceeds through atmosphere, pressure, deadlines, and fragmented information, without requirements, constraints, priorities, and reasons for judgment being sufficiently verbalized.

This is not a word that simply points to a manager's lack of individual ability. Rather, it is a structural problem that occurs in organizations where mechanisms to verbalize, share, and make intent verifiable are weak.

When Vibe Management occurs, developers cannot just be implementers. They must read through ambiguous requirements, guess background, supplement priorities, look for constraints, and judge for themselves what should be made.

In other words, developers are simultaneously archaeologists, requirement analysts, translators, risk evaluators, and sometimes stand-ins for product managers.

This load is very large.

Moreover, if the existing system side is already mosaicified, the problem worsens further. Developers must connect ambiguous requirements from upstream to ambiguous reality downstream. Requirements are ambiguous, reality is ambiguous, and intent connecting the two is not recorded either.

If only implementation is rushed in this state, judgments based on guesswork are embedded in the code. If those judgments are not recorded, they become a new mystery for the next developer.

Thus, Vibe Management creates new document drift and understanding debt.

## 5. Technical Debt Makes Archaeology Further Difficult

Another factor that makes software archaeology difficult is technical debt.

In systems with little technical debt, code structure, naming, responsibility boundaries, tests, and documentation are relatively consistent. Therefore, even if there is some omission in information, it is easy to guess the current structure from code and tests.

However, in systems with much technical debt, this guesswork becomes difficult.

Responsibility boundaries are broken. Naming does not represent reality. Similar processing is dispersed in multiple places. Tests are lacking. Comments are old. Exception handling is intricately piled up. Data models deviate from actual business concepts. Temporary responses have become permanent.

In such a state, even if you read the code, it is difficult to reconstruct the intent.

Code shows "how it currently works." But it cannot be judged from code alone whether that behavior is the original specification, a remnant of a past bug, a compromise due to deadline convenience, an exception for a specific customer, or a temporary response that no one can delete anymore.

Technical debt does not only make changes difficult. It makes understanding difficult. When understanding becomes difficult, changes become more dangerous. Dangerous changes are avoided, and workarounds pile up around them. Workarounds further complicate the structure.

Through this cycle, technical debt and understanding debt mutually reinforce each other.

## 6. This Task Requires High Proficiency

Software archaeology is not simple information retrieval.

What is required is not just the ability to read code. Domain understanding, design judgment, operational knowledge, organizational context, estimation of past decision-making, handling of contradictory information, estimation of change impact, and risk judgment are required.

For example, suppose a specification and code contradict each other. At that time, a developer must answer questions such as:

- Is the specification old?
- Is the code wrong?
- Did the specification change but was not recorded?
- Is it an exception for a specific customer?
- Is it a bug but depended on in operation?
- Do tests represent the current specification?
- Where will break if changed now?
- Whom should I confirm with?

These judgments are difficult for inexperienced developers. Even for experienced developers, it takes time. Moreover, they often cannot judge correctly without knowing the organizational context.

This task is often treated with light words such as "investigation," "catch-up," or "understanding existing code." But in reality, it is a highly intellectual task to reconstruct the current position of the system.

The more experienced, the more they perform this task unconsciously. They empirically judge which information is suspicious, which file is reliable, whom to ask, and which change is dangerous.

However, just because it can be done unconsciously does not mean the cost does not exist.

## 7. Cost was Internalized by Individuals and was Invisible to the Organization

One of the reasons why problems of the mosaic of information and software archaeology have been overlooked for a long time is that the cost was internalized by individuals.

Developers doubt old specifications, check against code, ask people, dig past issues, and guess context. These are melted into daily operations.

From the organization's point of view, it just looks like "investigation is taking time." Or it is expressed as "this area is siloed (individual-dependent)," "communication is lacking," or "documentation culture is weak."

However, these are different appearances of the same fundamental problem.

The problem is not simply that investigation is slow. It is that reliable information does not exist in one place and the information space is mosaicified.

The problem is not simply that it is individual-dependent. It is that it has become a state where the current truth cannot be reconstructed without relying on individuals.

The problem is not simply a lack of communication. It is that intent, reasons for judgment, constraints, exceptions, and change history are not left in a sharable form.

This cost does not only lower development speed. It makes reviews heavy, makes onboarding difficult, slows down incident response, makes refactoring dangerous, and increases the psychological burden on the team.

And the most troublesome thing is that this cost is difficult to visualize.

While experienced developers are absorbing it, organizations are unlikely to notice the magnitude of the problem. When experienced developers leave, or when the system becomes too large, the problem surfaces for the first time.

## 8. Summary: The Real Bottleneck Lies in the Reconstruction of Meaning

In development fields, meaning often must be reconstructed before writing code.

What is the current specification? Why is the code like that? Which document is correct? Which information is old? Which judgment is still valid? Which constraint should be observed? What is dangerous to change?

This reconstruction work is made difficult by document drift, the mosaic of information, Vibe Management, technical debt, and understanding debt.

And despite requiring high proficiency, this work has long been treated as an implicit skill of experienced developers. Organizations have not correctly measured the cost, and methodologies have not placed this problem at the center.

What is important here is that this problem existed since before AI.

AI did not suddenly create this problem. Rather, it surfaces the already existing mosaicified information space, lost intent, ambiguous requirements, and technical debt in a form that consumes faster and has a larger impact.

What should be asked next is what AI brings to this situation. When the cost of code generation decreases, will the problem really be solved? Or will the most difficult task that experienced developers have borne—reconstruction of meaning and verification of intent—become even more important?

---

[1] Tan, W. S., Wagner, M., & Treude, C. (2023). "Detecting outdated code element references in software repository documentation". *Empirical Software Engineering*. https://doi.org/10.1007/s10664-023-10331-1
