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

Place the `document-shaping` folder in your agent's skill directory. If your agent supports slash commands for skills, start your request with `/document-shaping`. You can also ask it to read `document-shaping/SKILL.md` and then give it the same writing request.

### Specify the intended reader

A persona describes who'll use the document and what they'll use it for. “It's for the team reviewing the design” is enough to get started. If you have a particular reader in mind, add what they already know or what they'll need to do with the document. With a short request, the agent can infer the rest from the kind of document you ask for and the material you give it.

### Design document for reviewers

**Short request**

```text
/document-shaping Turn these notes into a design doc for the review team.
```

**More detail**

```text
/document-shaping Turn the attached retry notes into a design doc for the next review.
The engineers know our queue and will be reading about this proposal for the first time.
Put the current problem and the risks up front so they can decide whether to go ahead.
```

### Documentation revision for future maintainers

**Short request**

```text
/document-shaping Use these notes to update the retry policy in the ops guide for the next maintainer.
```

**More detail**

```text
/document-shaping Use the attached decision notes to update just the “Retry policy” section of the ops guide.
It's for someone taking over the queue in a few months who'll be relying on the guide.
Make it easy to see why there's a retry limit and what to check before changing it.
```

### Implementation plan for an AI agent

**Short request**

```text
/document-shaping Turn the attached spec into a plan for a coding agent.
```

**More detail**

```text
/document-shaping Turn the attached retry spec into an implementation plan for another coding agent.
It'll have the repo and the plan to work from. Make it clear where to make the change and how to test it.
```

### Rollout guide for customer support

**Short request**

```text
/document-shaping Use these release notes to write a guide for the support team.
```

**More detail**

```text
/document-shaping Turn the attached release notes into a guide support can use right after the rollout.
The team already knows how to handle tickets. Focus on what customers will notice and what support should check when questions come in.
```

## Repository contents

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Complete instructions for an agent applying the skill. |
| [DESIGN.md](DESIGN.md) | Design rationale and principles for maintaining or extending the skill. Not required at runtime. |
| [LICENSE](LICENSE) | zlib license, including redistribution terms. |

When changing the skill's behavior, keep the runtime instructions self-contained and update the design rationale to explain the change. Check that the change works for both bounded edits and complete documents without widening the activation scope.
