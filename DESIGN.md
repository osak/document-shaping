# Document Shaping: Design Rationale

## Purpose and intended boundary

Document Shaping is a document-design skill, not a universal writing style. Its job is to transform available information into an artifact that serves a particular reader and use case. It was deliberately designed to activate only when the user explicitly requests a formal document or technical writing: design documents, specifications, ADRs, implementation plans, review documents, documentation, or substantial technical explanations.

It must not activate for normal conversation, short technical Q&A, routine code explanations, or other everyday prose. Those tasks do not reliably justify a document-design workflow, and applying one would make responses slower, heavier, and more ceremonial than useful. A narrow trigger makes the skill predictable and preserves it for cases in which information structure matters materially.

## Runtime self-containment

`SKILL.md` is the complete runtime contract for an AI agent. It must contain every definition, distinction, and decision rule required to apply Document Shaping correctly. In particular, the audience archetypes, all four requirement axes, information classes, scope modes, context boundary, leakage rules, and structural checks must remain operational without consulting this file or prior conversations.

`DESIGN.md` is a maintenance record. It preserves why the model was chosen, which failure modes shaped it, and what future revisions must protect. Runtime behavior must not depend on an agent discovering or reading `DESIGN.md`. When a design decision changes execution, update `SKILL.md` first and use this file to record the reason.

## Why this is information shaping, not a detail setting

The original problem may look like control over document "detail." That framing is too weak. A reader can need a concise decision summary while also needing a precise risk statement; a future maintainer can need deep rationale but not the exact temporary file layout; an implementing agent can need exhaustive constraints but not a polished narrative.

Detail has several independent dimensions:

- background and self-containedness;
- technical depth and implementation specificity;
- rationale and decision history;
- completeness of constraints, edge cases, and unresolved questions;
- readability, scanability, and reference structure;
- durability against context and time.

Increasing or decreasing all of them together produces poor artifacts. "Make it shorter" can erase the reason a constraint exists; "make it more detailed" can add familiar background while hiding the actual decision. The skill therefore asks where each piece of information belongs, rather than selecting one global verbosity level.

The core transformation is:

> Available information → reader requirements → information hierarchy → artifact structure

## Reader-requirements model

Full shaping derives requirements from four independent axes:

> Audience × Distance × Purpose × Lifetime

The multiplication sign is conceptual: the axes constrain each other rather than being a scoring formula.

### Audience

Audience describes capability and relationship to the work. The useful archetypes are deliberately requirements-oriented rather than permanent labels.

| Archetype | What it primarily needs |
| --- | --- |
| AI agent | Explicit requirements, identifiers, dependencies, constraints, edge cases, state, and unresolved questions. Semantic completeness can outweigh elegant prose. |
| Active developer | Concrete changes, affected boundaries, non-obvious constraints, implementation decisions, and verification. Shared background can be compressed. |
| Reviewer | Problem framing, relevant existing behavior, scope, decisions, alternatives, trade-offs, risk, compatibility, and validation. It must stand apart from the authoring conversation. |
| Future maintainer | Rationale, invariants, boundaries, non-obvious behavior, and intentional rejected alternatives. It must distinguish deliberate design from accident. |
| Non-engineer collaborator | Observable behavior, domain concepts, responsibilities, workflows, and operational consequences. The abstraction boundary should change, not merely the vocabulary. |
| Decision maker | The decision, options, evaluation criteria, trade-offs, recommendation, consequences, and risks. Technical detail is subordinate to decision relevance. |

These archetypes should guide inference, not force a template. When evidence is weak, preserve ambiguity instead of inventing a reader model.

### Distance: contextual and temporal

Distance captures how safely the artifact may rely on knowledge outside itself. It has two distinct parts.

**Contextual distance** is distance from the current task conversation. An active participant may share context; a reviewer who missed a meeting or issue thread does not. As contextual distance grows, the artifact must reconstruct the triggering problem, relevant prior state, assumptions, scope, and decisions. It must not require readers to retrieve chat logs, meetings, or undocumented discussions.

**Temporal distance** is distance from the moment the artifact was created. Even the original author becomes contextually distant over time. As temporal distance grows, rationale, constraints, invariants, and significant rejected alternatives become more valuable. Temporal distance does *not* imply that every implementation detail should be retained. The durable question is: what would be difficult or unsafe to reconstruct later?

This separation is important. A reviewer can be contextually distant but temporally near; a maintainer can be both the former author and temporally distant. Their needs overlap but are not the same.

### Purpose

Purpose establishes the reader's task and should determine information order.

- **Execution:** actions, targets, interfaces, dependencies, constraints, edge cases, expected outcomes, and validation. The reader should not need to rediscover essential requirements before beginning work.
- **Review:** problem, scope, assumptions, design, alternatives, trade-offs, risks, compatibility, and verification. High-impact uncertainty must be easy to find.
- **Maintenance:** rationale, invariants, boundaries, non-obvious behavior, failure assumptions, and constraints on safe future changes.
- **Decision:** decision needed, options, criteria, trade-offs, recommendation, residual risk, and consequences.
- **Reference:** stable terminology, definitions, predictable headings, authoritative statements, and low dependence on chronological narrative.
- **Handoff:** current state, completed and remaining work, discoveries, known failures, constraints, unresolved questions, and exact next actions.

### Lifetime

Lifetime calibrates how much investment the artifact deserves and which information will remain useful.

- **Ephemeral:** immediate working notes or short-lived agent context. Optimize for current utility; dependence on the current state is acceptable.
- **Working:** active plans, pull-request descriptions, and in-progress design documents. Be structured enough for readers outside the authoring loop without preserving every historical turn.
- **Durable:** ADRs, architecture documentation, lasting reference material, and comments about important invariants. Favor stable terminology, rationale, and boundaries. Treat transient layouts, migration states, relative time language, and fragile implementation specifics with caution.

## Information hierarchy

Before prose is drafted, information is sorted into five functional classes:

| Class | Function | Typical treatment |
| --- | --- | --- |
| Foreground | Directly enables the reader's primary task | Put it first or make it highly discoverable. |
| Support | Allows the reader to understand or validate foreground material | Keep it close to the dependent claim. |
| Compress | Useful but not central | Preserve the conclusion in a concise form. |
| Reference | Worth retaining but interrupts the main flow | Link, append, or isolate for lookup. |
| Omit | Not useful to the intended artifact | Exclude it. |

The hierarchy prevents a common failure mode: reproducing all known information in the order it was learned. Documents should expose the structure the reader needs, not the chronology of research, conversation, or implementation.

## Conceptual vocabulary and implementation vocabulary

A document can select the right information and still force readers to switch abstraction levels within every sentence. Variable names, file paths, and internal component names are useful for locating code, but they interrupt a discussion about responsibilities, behavior, or design constraints when the reader has no need to navigate to that code. Defining each name does not remove that interruption.

This is separate from the authoring/artifact boundary: an implementation identifier can be a legitimate artifact fact and still belong outside a conceptual explanation. It is also separate from audience expertise. An experienced developer reviewing an architectural idea may need conceptual vocabulary in the rationale and exact identifiers in the implementation plan.

The runtime rule therefore chooses vocabulary by the job of each section. Conceptual passages retain precise domain meaning and constraints. Implementation passages retain the names needed for execution, lookup, and verification. Where readers must cross between them, an explicit transition or mapping establishes the correspondence without repeatedly inserting code names into the conceptual narrative. Mappings must come from the available source material, not invented implementation details.

For example, the constraint that retrying a request must not create a second payment stands independently of the field and handler used to enforce it. A reviewer can assess the constraint before inspecting those implementation choices. An execution plan still needs the actual field and handler names when directing a change, and a local code comment can name the symbol whose behavior it explains.

The aim is to preserve both kinds of precision. It is not a ban on identifiers, a requirement to simplify technical language, or a mandate to add an implementation appendix to every document. Local edits keep their scope, and durable records preserve implementation names only where the reader's task warrants their maintenance cost.

## Structural editing

Line editing cannot reliably repair misplaced information. A dedicated structural editing pass is therefore part of full shaping. It checks whether the reader can find the document's primary problem, conclusion, decision, or action; whether each section has a distinct job; whether required context precedes dependency; and whether evidence, constraints, risks, rationale, and validation have the prominence appropriate to the purpose.

The pass may reorder, merge, split, promote, demote, compress, reference, or omit material. It is not a mandate to expand. The intended effect is to improve information architecture, including by removing content that competes with the reader's actual task.

## Authoring Context and Artifact Context

The skill maintains a strict boundary between the information used to create a document and the information the finished document should communicate.

**Authoring Context** includes prompts, model or generation instructions, scratch reasoning, tool constraints, conversation history, drafting tactics, and temporary editorial choices. It helps the author make the artifact but is not automatically part of it.

**Artifact Context** includes reader-relevant facts, behavior, decisions, constraints, rationale, interfaces, risks, and next actions. It is what survives in the document, plan, comment, or review text.

This distinction matters especially for AI-authored materials. An instruction can influence an implementation without being a truth about the system. Treating it as reader-facing text produces misleading documentation and comments.

### Instruction leakage

Instruction leakage is authoring context appearing in the artifact without an independent reader-facing purpose. Examples include prompt-like commands, references to the writer or model, a summary of hidden drafting workflow, and code comments that repeat generation constraints.

For example, an authoring instruction such as `Do not use toString()` must not be copied into a code comment. It says how to generate a change, not why a future reader should preserve it. If a durable constraint exists, the artifact may state that constraint in domain terms—for example, a required null-handling, serialization, compatibility, or performance property—but only when it is true and useful to the reader.

The boundary is intentionally strong. "The instruction affected the result" is never by itself a reason to document it. The author must either translate it into a valid artifact fact or omit it.

## Local shaping and full shaping

The skill has two intensities because trigger scope and editing scope are different questions.

**Local shaping** is for bounded edits: a section, a few paragraphs, or a code comment. It preserves existing architecture and applies only the checks that matter locally: reader fit, terminology, priority, redundancy, and instruction leakage. It may adjust adjacent text for coherence but must not turn a local request into a document rewrite.

**Full shaping** is for new formal documents, substantial rewrites, and requests explicitly about organization, audience, or document quality. It derives the four-axis reader model, classifies information, designs a hierarchy, drafts, and performs the structural editing pass.

The guiding principle is: apply the smallest amount of shaping necessary for the requested scope. Full shaping is not inherently superior; an unasked-for reorganization can violate the user's scope, disrupt established conventions, and create unnecessary review churn.

## Design principles for future revisions

When changing this skill, preserve these principles unless there is strong evidence that a real use case requires a change.

1. **Keep activation narrow.** Do not turn Document Shaping into a catch-all style guide. Add a trigger only when it is clearly formal-document or technical-writing work and needs information architecture.
2. **Model independent requirements independently.** Do not collapse audience, shared context, time, purpose, and lifetime into one persona or one detail dial.
3. **Start from reader work.** The skill should prioritize what the reader needs to do, not what the author happened to learn or the model happened to be told.
4. **Respect requested scope.** A local edit remains local unless the user requests broader revision. Changes to this rule require special caution because they affect user control.
5. **Protect the authoring/artifact boundary.** New guidance must not normalize prompt disclosure, hidden-reasoning summaries, tool narration, or generation constraints in reader-facing content.
6. **Prefer durable meaning over historical exhaustiveness.** For long-lived artifacts, preserve rationale, constraints, and invariants; do not fossilize details that will soon become false.
7. **Use structure as a tool, not ceremony.** Headings, tables, outlines, and structural passes should earn their place by improving retrieval, judgment, execution, or maintenance.
8. **Validate with contrasting examples.** Test proposed changes against at least a local comment edit, a reviewer-facing design document, an execution plan, and a future-maintainer record. Check both over-activation and leakage.
9. **Keep the runtime contract self-contained.** `DESIGN.md` may explain a rule but must never be the only place where an executing agent can discover that rule.

A good future amendment makes a concrete decision better without expanding the skill into generic advice or a compulsory multi-step ritual.
