# Document Shaping

[日本語](README.ja.md)

Document Shaping is an AI agent skill for organizing formal documents and explicitly requested technical writing around the reader's needs. It helps decide what to emphasize, explain, summarize, reference, or leave out, and in what order to present it.

## When to use it

Use it for design documents, specifications, architecture decision records (ADRs), implementation plans, review documents, durable documentation, and explicitly requested reader-facing code comments or substantial technical explanations.

The skill applies when the user explicitly requests that writing work. Ordinary conversation, brief technical Q&A, routine code explanations, and incidental prose during coding are outside its scope.

## What it does

The skill derives writing requirements from four dimensions: the audience, the context and time separating the reader from the work, the reader's purpose, and the document's expected lifetime. These determine the information hierarchy rather than a single verbosity setting.

It keeps conceptual explanations in domain terms and places exact implementation names where readers need them to locate, change, or verify code. It also separates reader-relevant facts from the prompts and drafting instructions used to produce the document.

A bounded edit receives local shaping that preserves the surrounding structure. A new document or substantial rewrite receives full shaping, including organization and a structural review. The output is the requested artifact; the internal shaping analysis is included only when requested.

## Use with an agent

Clone the repository into your workspace.

```sh
git clone https://github.com/osak/document-shaping.git
```

Ask your agent to read the skill and apply it to a specific writing task. The examples below assume you are working from the parent directory of the clone.

To make the skill discoverable automatically, place the repository in a skill directory supported by your agent, keeping the folder name `document-shaping`. If the skill is already available, name it directly in your request.

### Specify the intended reader

Use the persona to describe who will use the finished document. Start with “The reader is …” and give their role, what they already know, and what they need to do after reading. Mention when they will use the document if that affects what it should preserve. Include the source material and name the section if the request is a bounded revision.

“The reader is a backend engineer” gives the agent a starting point. “The readers are backend engineers who know our queue but missed the proposal discussion. They need to decide whether to approve the retry change at the next design review” also tells the agent which background to reconstruct and what evidence belongs up front.

### Design document for reviewers

```text
Read document-shaping/SKILL.md and turn the attached queue retry notes into a design document for the next review.
The readers are backend engineers who know the existing queue but missed the proposal discussion. They need to decide whether to approve the change.
Summarize the current failure mode, then compare the proposal and alternatives on duplicate processing, recovery, and rollout risk. Flag open questions that affect approval.
```

### Documentation revision for future maintainers

```text
Read document-shaping/SKILL.md and revise only the retry policy section of the queue operations guide using the attached decision notes.
The reader is an on-call engineer taking over the queue months from now. They know how queues work and will read the guide without the original decision notes.
Explain why the retry limit exists, what happens when it is reached, and what to verify before changing it.
```

### Implementation plan for an AI agent

```text
Read document-shaping/SKILL.md and turn the attached queue retry specification into an implementation plan.
The reader is a coding agent starting in a fresh checkout. It can inspect the repository and run tests. It has no access to this chat and needs to implement and verify the change.
List the affected files and symbols, required behavior, failure cases, and verification steps. Mark decisions the specification leaves open.
```

### Rollout guide for customer support

```text
Read document-shaping/SKILL.md and use the attached rollout notes to write a guide for the customer support team.
The readers are support specialists who know the product and the ticket workflow. They need to answer customer questions during the first week of the rollout.
Describe what customers will see, what support can check, when to escalate, and what to include in an escalation.
```

## Repository contents

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Complete instructions for an agent applying the skill. |
| [DESIGN.md](DESIGN.md) | Design rationale and principles for maintaining or extending the skill. Not required at runtime. |
| [LICENSE](LICENSE) | zlib license, including redistribution terms. |

When changing the skill's behavior, keep the runtime instructions self-contained and update the design rationale to explain the change. Check that the change works for both bounded edits and complete documents without widening the activation scope.
