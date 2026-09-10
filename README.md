# Document Shaping

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

Ask your agent to read the skill and apply it to a specific writing task. For example, from the parent directory of the clone:

> Read `document-shaping/SKILL.md` and use it to write a design document from the attached notes. The audience is engineers reviewing the proposal without access to our discussion. Explain the decision, alternatives, and operational consequences.

For a bounded revision:

> Read `document-shaping/SKILL.md` and use it to revise only the retry-policy section for future maintainers. Preserve the rest of the document's structure.

To make the skill discoverable automatically, place the repository in a skill directory supported by your agent, keeping the folder name `document-shaping`. Skill discovery and installation locations depend on the host; the manual instructions above do not require automatic discovery.

## Repository contents

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Complete instructions for an agent applying the skill. |
| [DESIGN.md](DESIGN.md) | Design rationale and principles for maintaining or extending the skill. Not required at runtime. |
| [LICENSE](LICENSE) | zlib license, including redistribution terms. |

When changing the skill's behavior, keep the runtime instructions self-contained and update the design rationale to explain the change. Check that the change works for both bounded edits and complete documents without widening the activation scope.
