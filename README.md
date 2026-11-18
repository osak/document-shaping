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

Clone the repository into your agent's skill directory, keeping the folder name `document-shaping`. For Claude Code:

```sh
git clone https://github.com/osak/document-shaping.git ~/.claude/skills/document-shaping
```

Then type `/document-shaping` followed by what you want written and who will read it. If your agent doesn't support skills, start with something like "Read document-shaping/SKILL.md and …" instead.

### Tell it who the reader is

A persona is a picture of who will use the finished document and how. Start with "The reader is …" and mention their role, what they already know, and what they'll do after reading. If when they read it changes what the document should keep, say so. For a partial revision, name the section too.

"The reader is a backend engineer" is enough to set the technical level. "The readers are backend engineers who know our queue but missed the proposal discussion, and they'll decide whether to approve the retry change at the next design review" also tells the agent which background to fill in and what to put up front.

### Example requests

Both short and detailed requests work. With a short one, the agent fills in the reader from the document type and the request; the more you spell out, the closer the structure gets to what you had in mind.

#### Design document for reviewers

Short:

```text
/document-shaping Write a design doc from the attached notes. It's for the people at the design review.
```

Detailed:

```text
/document-shaping Turn the attached queue retry notes into a design doc for the next review.
The readers are backend engineers who know the existing queue but missed the proposal discussion. They'll decide whether to approve the change.
Start with a quick summary of what's wrong with retries today, then compare the proposal and alternatives on duplicate processing, recovery, and rollout risk. Call out any open questions that could affect approval.
```

#### Documentation revision for future maintainers

Short:

```text
/document-shaping Update the "Retry policy" section of the ops guide using the decision notes. It's for whoever runs the queue later.
```

Detailed:

```text
/document-shaping Using the attached decision notes, revise just the "Retry policy" section of the queue operations guide.
The reader is an on-call engineer taking over the queue a few months from now. They know how queues work but won't have the original decision notes.
I'd like it to cover why the retry limit exists, what happens when it's hit, and what to check before changing it.
```

#### Implementation plan for an AI agent

Short:

```text
/document-shaping Turn the attached spec into an implementation plan for a coding agent.
```

Detailed:

```text
/document-shaping Turn the attached queue retry spec into an implementation plan.
The reader is a coding agent starting from a fresh checkout. It can read the code and run tests, but it can't see this chat, so the plan alone should be enough to implement and verify the change.
Include the files and symbols to touch, expected behavior, failure cases, and how to verify. Clearly mark anything the spec leaves open.
```

#### Rollout guide for customer support

Short:

```text
/document-shaping Write a guide for the support team from the attached release notes.
```

Detailed:

```text
/document-shaping Write a guide for customer support from the attached release notes.
The readers are support specialists who know the product and the ticket workflow. They'll use it to answer customer questions during the first week after the rollout.
Cover what customers will notice, what support can check, when to escalate, and what to include when they do.
```

## Repository contents

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Complete instructions for an agent applying the skill. |
| [DESIGN.md](DESIGN.md) | Design rationale and principles for maintaining or extending the skill. Not required at runtime. |
| [LICENSE](LICENSE) | zlib license, including redistribution terms. |

When changing the skill's behavior, keep the runtime instructions self-contained and update the design rationale to explain the change. Check that the change works for both bounded edits and complete documents without widening the activation scope.
