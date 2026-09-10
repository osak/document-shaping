---
name: document-shaping
description: Shape a formal document or explicitly requested technical writing around its reader, purpose, context, and expected lifetime. Use only when the user explicitly asks to create or revise formal or technical writing (for example, a design doc, specification, ADR, implementation plan, review document, documentation, reader-facing code comment, or substantial technical explanation). Do not use for ordinary conversation, brief technical Q&A, routine code explanations, or incidental prose in a coding task.
---

# Document Shaping

Create an artifact that gives its intended reader the information they need in the order and form they need it. Quality is not a single detail level: decide what to emphasize, support, compress, reference, or omit.

Apply the model internally. Deliver the requested artifact, not a narration of the shaping process, unless the user asks for that analysis.

## Scope of application

Use this skill only after a user has explicitly requested the creation or revision of a formal document or technical writing. A technical subject alone is not sufficient. This is not a general prose style, chat-answer, or routine code-explanation skill.

Choose the smallest shaping intensity that fits the request.

### Local shaping

Use for a bounded edit: a section, a few paragraphs, a code comment, or another clearly local revision.

- Preserve the existing document's structure, conventions, and scope.
- Improve reader fit, information priority, terminology, redundancy, and instruction leakage in the requested area.
- Touch adjacent material only when necessary for coherence.
- Do not redesign the whole document, recast its outline, or introduce a broader analysis the user did not request.
- If a problem outside the requested area prevents the local edit from being correct, report it without silently expanding the rewrite.

### Full shaping

Use for a new formal document, a substantial rewrite, or an explicit request to improve organization, audience fit, or document quality.

1. Derive requirements from **Audience × Distance × Purpose × Lifetime**.
2. Identify material unknowns. Infer only what the request or source material supports; preserve important uncertainty instead of inventing facts.
3. Inventory the available information and classify it before drafting.
4. Build an information hierarchy and structure around the reader's task.
5. Write the artifact.
6. Perform a structural editing pass on the complete artifact.

## Derive the reader model

For full shaping, derive all four dimensions. For local shaping, use only the dimensions needed to make the requested area fit its reader; do not analyze or redesign the whole artifact.

Make only the inferences supported by the request and artifact type; do not invent a highly specific reader. Ask a clarifying question only when an unresolved choice would materially change the artifact and no safe, bounded assumption exists. Otherwise use the least specific model that supports the task, and surface consequential assumptions in the artifact when its reader needs them.

| Dimension | Question | Main effect |
| --- | --- | --- |
| Audience | Who will read or use this? | Technical depth, abstraction, and terminology |
| Distance | What context is shared now, and what will be forgotten later? | Required background, self-containedness, and rationale |
| Purpose | What must the reader do: execute, review, maintain, decide, reference, or take a handoff? | Information order and required evidence |
| Lifetime | Is this ephemeral, working material, or durable documentation? | Balance of stable rationale against volatile detail |

Use the following archetypes to derive requirements. They are decision aids, not rigid templates.

| Audience archetype | Prioritize |
| --- | --- |
| AI agent | Explicit requirements, identifiers, states, dependencies, constraints, edge cases, failure conditions, and unresolved questions. Favor semantic completeness over polished narrative. |
| Active developer | Concrete changes, affected boundaries, non-obvious constraints, implementation decisions, and verification. Compress background that is demonstrably shared. |
| Reviewer | Problem, relevant prior behavior, scope, decisions, alternatives, trade-offs, risks, compatibility, and validation. Make the artifact understandable without the authoring conversation. |
| Future maintainer | Rationale, invariants, boundaries, non-obvious behavior, and meaningful rejected alternatives. Preserve what makes later changes safe; avoid volatile historical detail. |
| Non-engineer collaborator | Observable behavior, domain concepts, responsibilities, workflows, and operational consequences. Change the abstraction level instead of merely defining technical terms. |
| Decision maker | Decision required, options, evaluation criteria, trade-offs, recommendation, consequences, and residual risk. Include technical detail only when it changes the decision. |

### Distance

Set contextual and temporal distance independently; do not collapse them into one label.

| Kind | Value | Requirement |
| --- | --- | --- |
| Contextual | Immediate/shared | Compress established background, but retain non-obvious constraints whose loss would cause incorrect action. |
| Contextual | Distant | Reconstruct the triggering problem, relevant prior state, scope, assumptions, and decisions. Do not require access to chats, meetings, or issue discussions. |
| Temporal | Near | Current conditions can be assumed where the reader shares them; use concrete wording for time-sensitive facts. |
| Temporal | Distant | Preserve durable rationale, constraints, invariants, and meaningful rejected alternatives. Do not retain volatile implementation history merely because it occurred. |

For example, a reviewer who missed the discussion is contextually distant but may be temporally near. A future maintainer is temporally distant even if they were once the author.

### Purpose

Organize the artifact around the reader's primary task.

| Purpose | Prioritize |
| --- | --- |
| Execution | Actions, affected components, interfaces, dependencies, constraints, edge cases, expected outcomes, and validation. |
| Review | Problem framing, scope, assumptions, decisions, alternatives, trade-offs, risks, compatibility, and verification. |
| Maintenance | Rationale, invariants, boundaries, non-obvious behavior, failure assumptions, and constraints on future changes. |
| Decision | The decision, options, evaluation criteria, recommendation, consequences, and residual risk. |
| Reference | Stable terminology, definitions, predictable headings, authoritative statements, and lookup-friendly structure. |
| Handoff | Current state, completed and remaining work, discoveries, known failures, unresolved questions, and exact next actions. |

### Lifetime

Select the expected useful life of the artifact; this determines which information deserves preservation.

| Lifetime | Requirement |
| --- | --- |
| Ephemeral | Optimize for immediate work. Dependence on the current state is acceptable; do not add durable explanation without a reader need. |
| Working | Support readers outside the immediate authoring loop while work is active. Preserve relevant context and decisions, not every historical turn. |
| Durable | Favor stable terminology, rationale, invariants, and architectural boundaries. Avoid relative time language and transient layouts or states likely to become false. |

### Combine the axes

The axes are independent requirements, not scores and not a verbosity formula:

- Audience determines abstraction, terminology, technical depth, and the form of evidence.
- Distance determines how much context must be reconstructed and how much rationale must survive.
- Purpose determines the main flow, ordering, and what the reader must be able to do afterward.
- Lifetime determines which facts are durable enough to preserve and which volatile details to compress or omit.

Honor explicit user requirements before inferred ones. If several audiences or purposes exist, identify the primary one from the requested outcome. Optimize the main flow for it, then serve compatible secondary needs through supporting sections or references. Do not average conflicting needs into a document that serves none of them.

## Shape information before prose

For full shaping, classify the available information before structuring the artifact. For local shaping, classify only material in the requested area and any adjacent material required for coherence.

Classify each item by its value to this reader and purpose:

- **Foreground** — directly needed to perform the reader's primary task; make it easy to find.
- **Support** — needed to understand, validate, or safely apply the foreground; keep it nearby.
- **Compress** — useful but secondary; summarize without losing the relevant conclusion.
- **Reference** — valuable for lookup but disruptive to the main flow; link, append, or isolate it.
- **Omit** — not useful to the artifact's reader; leave it out.

Organize by reader need, not by the order in which research, conversation, or implementation occurred. Keep decision-relevant items visible in review and decision documents; keep actions, constraints, edge cases, and validation visible in execution documents; keep rationale, invariants, boundaries, and durable terminology visible in maintenance documents.

Do not move essential foreground information into a reference. Reference only a source that exists and will be accessible to the intended reader. If an external source is necessary but unavailable, state the dependency or uncertainty instead of fabricating or silently assuming its contents.

## Keep vocabulary within its abstraction level

Choose the vocabulary for each section from the question it answers. Conceptual discussion uses domain concepts, responsibilities, behavior, and constraints; implementation discussion uses exact identifiers, file paths, APIs, and configuration keys when the reader needs them to act or verify a claim. A technical audience can need either level, so do not choose vocabulary from audience expertise alone.

Keep conceptual explanations in conceptual terms. Defining an identifier or putting it in parentheses does not make it useful there. Name the responsibility or behavior directly, preserving the distinctions and constraints that matter; do not replace precise meaning with vague labels such as "the system."

Connect the two vocabularies at an explicit boundary when the reader needs to navigate between them: an implementation section, a short mapping table, or a focused transition. Establish the correspondence there, then use the vocabulary of the current section consistently instead of repeatedly attaching code names to concepts. Use only mappings supported by the source material.

For example, a design rationale may say, "Retrying a request must not create a second payment." An implementation section can then identify the field that detects duplicate requests and the handler that checks it. The rationale does not need those identifiers merely because they are known. Conversely, an execution plan or API reference must retain exact names where locating or invoking the implementation is the reader's task; a local code comment may also need a symbol to explain its behavior precisely.

Apply this distinction within the requested editing scope. Add a separate section or mapping only when the reader needs the crossing; a short conceptual passage does not require an implementation appendix.

## Separate contexts and prevent instruction leakage

Maintain a hard boundary between these two kinds of information:

- **Authoring Context**: prompts, generation instructions, scratch reasoning, tool constraints, conversation history, and drafting tactics used to create the artifact.
- **Artifact Context**: facts, decisions, constraints, rationale, behavior, interfaces, risks, and next actions that the artifact's reader needs.

Never copy Authoring Context into the artifact merely because it affected generation. Translate it into a reader-relevant fact only when the fact is independently true and useful; otherwise omit it.

In particular, do not put generation instructions into code comments, documentation, plans, or review text. For example, `Do not use toString()` is an authoring instruction, not a code comment. A comment may instead explain the durable behavioral constraint that justified the implementation, if one exists.

Before finalizing, scan for prompt-like imperatives, references to the writer or model, hidden workflow, chat history, temporary drafting choices, and implementation requests presented as reader-facing explanations. Remove or rewrite them unless they are genuinely part of the artifact's subject matter.

## Structural editing pass

For full shaping, review the finished artifact as a reader rather than line-editing it in isolation.

- Can the reader find the main problem, conclusion, decision, or action quickly?
- Does each section have a job, and is its information at the right level of the hierarchy?
- Does it reconstruct necessary context without narrating the author's process?
- Are assumptions, constraints, rationale, risks, and validation visible where the purpose requires them?
- Are facts, assumptions, and unresolved questions distinguishable where confusing them would affect action or judgment?
- Are volatile details distinguished from durable truths when lifetime is long?
- Does each section use vocabulary appropriate to its abstraction level, with necessary concept-to-implementation mappings confined to clear transitions or reference material?
- Can every required reference be located by the intended reader?
- Has all instruction leakage been removed?

Reorder, merge, split, promote, demote, compress, reference, or omit material as needed. Do not add detail merely to appear complete.
