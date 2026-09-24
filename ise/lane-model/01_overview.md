# Nagi Lane Model Draft

## 1. Positioning

ISE is a model born as a restructuring after goose-sdd, and Nagi is strongly influenced by that trend.
However, it is more natural to view Nagi not as a simple subordinate implementation of ISE, but as a development/implementation with its own operational system and implementation decisions.

Therefore, an intermediate layer is needed between the abstract theory of ISE and the concrete operational system of Nagi.

In this intermediate layer, work is viewed as a `lane` rather than a `layer`.

- `goose-sdd`
  - The preceding implementation/organization that served as the lineage origin.
- `ISE`
  - The abstract model restructured afterwards.
  - Handles intent, reality, diff, synchronization, etc.
- `lane model`
  - An intermediate concept for connecting to Nagi's operational system.
  - Defines the flow of work and why it is separated.
- `Nagi`
  - An implementation that maps the lane model to actual development operations.

The key point here is that Nagi having lineage in goose-sdd and ISE is not the same as being entirely dependent on them.
Nagi inherits the background perspective but can restructure it into its own operational system rather than simply mirroring it.

> **Note**: This document is a definition of the lane model at the conceptual level.
> Since the lane model sits above concrete operational systems, it is not a document that prescribes command syntax, option names, or the execution procedure itself.
> What is handled here is the perspective on targets and the separation of responsibilities that implementations like Nagi should rely on.

## 2. Why Lane Instead of Layer?

The term `layer` tends to evoke hierarchical relationships or order dependencies. On the other hand, what we want to handle here is not a "process flowing from top to bottom," but rather flows of work that exist in parallel according to their nature.

Therefore, we adopt `lane` in this intermediate layer.

What `lane` represents:

- Not a hierarchical relationship.
- Does not assume a fixed order.
- Can start from any lane.
- Ultimately converges in synchronization.

In other words, a lane is a separation of responsibilities, not a sequence of steps.

## 3. Basic Structure of Lanes

The lane candidates Nagi currently has are:

- `spec`
  - Clarifies intent.
  - Handles To-Be, requirements, design, and constraints.
- `code`
  - Creates reality.
  - Handles implementation, prototyping, and exploratory coding.
- `analyze`
  - Observes and restructures As-Is.
  - Organizes code-backed facts and human-derived information.
- `cure`
  - Intervenes in diffs, debt, and dangerous areas.
  - Handles problem detection, prioritization, and repair.

These are not steps in a sequence, but lanes with different change vectors.

On the other hand, while `assistant` is important, it is not a lane.
It is more natural to position it as an auxiliary function to support human understanding, consultation, review, and design conversations, rather than a change vector that directly advances synchronization.

## 4. Directionality of Each Lane

Lanes have a direction in how they change the target.

- `spec`
  - Clarifies intent and connects it to reality.
- `code`
  - Changes reality and moves it closer to intent.
- `analyze`
  - Restructures reality as knowledge.
- `cure`
  - Transforms problems into repairs.

In this sense, a lane represents "what to do."

## 4.1 Assistant is a Support Interface, Not a Lane

`assistant` is a near-essential function for Nagi, but it is not a lane on the same level as `spec`, `code`, `analyze`, and `cure`.

The reasons are as follows:

- lane
  - Has a change vector that directly advances the synchronization state.
- assistant
  - Supports human understanding, confirmation, design consultation, and review conversations.
  - Makes lane artifacts and existing knowledge easier to read and handle.
  - It is not a direct change lane itself.

Especially in the AI era, this auxiliary function is quite important in practice.

- Consultations with humans have time and psychological costs.
- In the early stages of design, there are many situations where one wants to "bounce ideas off a wall" casually.
- Review and understanding support are essential but do not always need to be treated as a formal lane.

Therefore, in Nagi, `assistant` is treated as a support interface separate from lanes.
It has an argument system independent of the lane `scope` / `artifact` system and takes on the role of switching AI context or documents to refer to preferentially depending on the purpose (coding consultation, design consultation, etc.).
Lanes and `assistant` are treated independently conceptually as well.

## 5. Important Distinctions in the Analyze Lane

In `analyze`, it is important not to confuse the source and certainty of information.

- `gather`
  - Organizes code-backed facts such as code and settings.
- `mosaic`
  - Organizes background information, fragmentary information, and hypotheses derived from humans with reliability levels.
- `synthesize` / `elevate`
  - Connects fragments to higher-certainty knowledge or To-Be.

Here, emphasis is placed on "what can be said with what degree of certainty" rather than "what is written in the spec."

## 6. Positioning of the Cure Lane

`cure` is not just bug fixing, but is considered an intervention lane for diffs or unhealthy states.

Typically, it has the following flow:

- `health`
  - Detects problems and risks.
- `triage`
  - Organizes priority and impact scope.
- `remedy`
  - Actually repairs or improves.

While `cure` may directly change reality, its essence is "intervention in problems."

## 7. Relationship with Synchronization

All lanes ultimately converge toward consistency between intent and reality.

- `spec` prepares the intent side.
- `code` prepares the reality side.
- `analyze` converts reality into knowledge and feeds it back to spec.
- `cure` reduces inconsistency and damage.

What is essential here is not "in what order they were executed," but "how the synchronization state was advanced."

## 8. Target Surface Concept

When mapping the lane model to an operable surface, it is natural to express the target with a triplet: `lane`, `scope`, and `artifact`.

Here:

- `lane`
  - What to do.
- `scope`
  - Where to target.
- `artifact`
  - Which artifact or perspective within that scope to handle.

In this design, fixed classifications like the conventional `system`, `feature`, and `rapid` are no longer primary concepts.

Instead, they can be organized as:

- `lane` is the type of change.
- `scope` is the range of the target.
- `artifact` is the type of artifact within the target.

`assistant` has a system independent of the lane's main target surface and is conceptually distinguished from lanes.

### 8.1 Scope is Expressed as a Logical Path

`scope` is treated as a logical identifier with an appearance similar to a Unix path.

- `/`
  - Root scope corresponding to the legacy system-level scope.
- `/account`
  - Corresponds to the legacy feature scope.
- `/account/profile`
  - Can express sub-features or nested scopes under a feature.

This is not for specifying actual file paths as they are, but is an interface for representing targets intuitively.

### 8.2 Artifacts Have Per-Lane Namespaces

`artifact` is not treated as a vocabulary common to all lanes.
Each lane has its own artifact namespace according to its responsibility.

For example:

- `spec`
  - `init`, `requirement`, `design`, `tasks`
- `analyze`
  - `gather`, `mosaic`, `synthesize`, `elevate`
- `cure`
  - `health`, `triage`, `remedy`

In this form, artifact names are interpreted within the context of the lane, making them less likely to collide.
Also, extension points for each lane are easier to define.

### 8.3 Artifacts Allow Multi-Layer Paths

An `artifact` is not limited to a single element placed after `:`, but can have a logical hierarchy using `/`.

- `scope`
  - Allows multi-layer expressions similar to Unix paths as before.
- `artifact`
  - Allows multi-layer expressions using `/` as an artifact path following `:`.
- `:`
  - A separator representing the boundary between `scope` and `artifact`, and is not doubled.

Examples:

- `scope=/account`, `artifact=requirement`
- `scope=/account`, `artifact=design/ui`
- `scope=/infrastructure/aws`, `artifact=remedy/network`

With this design, the artifact side can also be managed without being forced into a flat vocabulary in the future.

### 8.4 Artifact Namespace and Resolver

Artifacts with the same name can exist in multiple resolution targets.
Therefore, in the lane model, a resolution target can be auxiliary specified for the target.

The key points here are as follows:

- `namespace`
  - An auxiliary identifier indicating which resolution space the target should be handled in.
  - If an artifact exists, it can be used to narrow down its resolution target.
  - Even if the artifact is omitted, it can be added to indicate where to resolve the default interpretation for the scope or the lane-specific default target.
- `resolver`
  - A concept representing the actual search destination or resolution strategy based on the namespace specification.

At this time, the validity and meaning of namespaces are not uniformly fixed.
Which namespaces are allowed, what meaning they have when the artifact is omitted, and whether a given target is considered resolvable are the responsibilities of the lane side.

In other words, the lane model only defines the abstraction "a resolution space can be attached to a target," and concrete acceptance conditions and default behaviors are left to each lane.

Also, the right side of `@` can be expressed as a namespace on the operational surface, but on the lane-model, it is treated not as representing "the user themselves" but as an abstract name representing the resolution target.

### 8.5 Concept of Extended Artifacts

Basically, artifacts are resolved by a resolver.
If the namespace is omitted, they are searched according to a default policy defined by each implementation.

- `standard`
  - A namespace pointing to a group of artifacts provided as standard by the implementation.
- `local`
  - A namespace pointing to a group of project-local artifacts.
- `user`
  - A namespace pointing to a group of user-defined artifacts.
- `auto`
  - A resolver policy that searches with a safe default priority order, rather than being a single entity.

The important point here is that default policies like `auto` are abstractions to allow extensions without breaking standard specifications, rather than just a mechanism for overriding.

The default search order or changeability is not the essence of the lane model.
The lane model only needs to define that artifacts with the same name can exist in multiple namespaces and that ambiguity can be absorbed by default policies.

Also, the meaning when an artifact is omitted shall be defined for each lane.

- In some lanes, a target that does not specify an artifact may be interpreted as the default target for that lane.
- Even in the same lane, if an artifact is specified, it may be interpreted as a more limited target.

In other words, an omitted artifact is not always complemented to a default artifact uniformly, but is interpreted according to the lane's responsibility.

### 8.6 Normalization of Target Path

Both scope and artifact are treated as logical paths.
At this time, notation fluctuations are treated as targets for normalization as much as possible, rather than syntax errors.

- Consecutive `/`
  - Although it is an unnatural expression with empty segments, it may be normalized as long as it can point to the logically same target.
- Trailing `/`
  - May be normalized if it does not mean an additional hierarchy.
- `..`
  - Not allowed because it introduces a logical parent reference.

The key point here is that the lane model prioritizes being able to handle target identity stably.
Therefore, it is more natural to canonicalize notation fluctuations that do not change the meaning than to reject them.

## 9. Concept of Scope

`scope` is treated as a logical target identifier, not an actual file path.

Examples:

- `/`
  - system-level scope
- `/account`
  - feature / domain / subsystem scope
- `/infrastructure/aws`
  - scope for infra or cross-cutting components

Resolution to actual files and directories is done by internal rules of each implementation.

At least the following symbols are treated as reserved symbols in the syntax:

- `/`
  - Separator for scope hierarchy
- `:`
  - Separator for artifact
- `@`
  - Specification indicating the resolution space of the target auxiliary

A design that can handle UTF-8 as the character set is desirable, but in actual operation, it is safe to center on ASCII printable characters for portability and visibility.
However, to maintain flexibility as a logical scope, prohibited characters are kept to a minimum.

This method allows users to concentrate on logical targets rather than internal arrangement rules.

Also, even with the same `scope`, the way it is resolved may differ depending on the lane.

- `spec`
  - Easy to connect relatively directly to documented intent-side targets.
- `code`
  - Easy to connect to implementation targets or change targets.
- `analyze` / `cure`
  - Can connect to codebase, infrastructure, generated analysis, existing docs, etc., according to the lane's responsibility.

In other words, `scope` is not a physical path, but a logical target passed to the resolver of each lane.

## 10. Concept of Artifact

`artifact` is optional and defined per lane as needed.

Examples:

- `spec`
  - `init`, `requirement`, `design`, `tasks`
- `analyze`
  - `gather`, `mosaic`, `synthesize`
- `cure`
  - `health`, `triage`, `remedy`

It is more stable to have artifacts not be too free-form but basically as reserved words or controlled vocabulary.
Then, it is natural to search for per-lane extensions only when they cannot be resolved by reserved words.

Regarding `assistant`, it is not treated on the same level as lane artifacts, but is positioned as a support interface with an argument system independent of the lane.

## 11. Design Principles

In this intermediate layer, at least the following shall be principles:

- Lanes are separation of responsibilities, not order.
- You can start from any lane.
- Distinguish between code-backed facts and human-derived information.
- Emphasize certainty rather than the information source itself.
- Accept eventual consistency.
- The operational surface receives logical targets, and physical arrangement is resolved internally.
- Assistant is a near-essential auxiliary function but is treated separately from lanes.

## 12. Summary at This Point

To organize Nagi, it is natural to place a lane model in between rather than directly connecting `ISE -> Nagi`.

Then, Nagi can be organized in the following form:

- Express the type of change by lane
- Express the target by scope
- Express the artifact type by artifact

From this perspective, Nagi can be understood not just as a set of commands, but as an implementation that restructures the background models into a developable operational system.
