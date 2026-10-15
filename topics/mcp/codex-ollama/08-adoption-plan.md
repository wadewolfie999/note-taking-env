# 08 — A Small Adoption Plan for This Notes Library

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Proposed sequence; installations and integration decisions remain separate work

Start by making one task easier to complete and verify. This sequence keeps the useful parts of the surveyed projects while limiting setup overhead.

## Stage 1: learn the command cycle

Read [01](01-mental-model.md) and [02](02-codex-mcp-commands.md), then use the OpenAI Docs MCP for one documentation question.

**Exit evidence:** explain the server name, endpoint, transport, active tool, returned source, and removal command. The goal is understanding the mechanism. [Official Docs MCP](https://developers.openai.com/learn/docs-mcp)

## Stage 2: test a server you understand completely

Run the fixture from [05](05-building-a-focused-mcp.md) with the check from [07](07-validation-and-troubleshooting.md). Register it in Codex only after that succeeds.

**Exit evidence:** one real Codex call returns the fixture URI, and an unsupported request is handled explicitly. This separates host compatibility from a complicated backend.

## Stage 3: select one useful notes workflow

Use ten known-answer/missing-answer questions to compare ordinary file search against either a focused notes adapter or Basic Memory on a sample corpus. Keep the note files and their versions inspectable.

**Decision:** choose the simplest option that improves retrieval or continuity enough to justify its upkeep. Basic Memory is a relevant candidate because it documents a [Codex-specific memory workflow](https://github.com/basicmachines-co/basic-memory/blob/54b8d94b94cb8e4e526f438010ca771c5ad9f209/plugins/codex/README.md); its fit here remains to be tested.

## Stage 4: add one Ollama role

Choose either Codex with an Ollama provider or a bounded Ollama-backed tool. Use a small public fixture, record the actual model/context, and measure cold/warm latency and answer quality. Keep a normal search or main-model path available when local inference is unsuitable.

**Exit evidence:** correct results within an agreed task/time budget, visible failures, and a complete data path. Use [03](03-ollama-integration-patterns.md) and [recipes C/D](06-practical-recipes.md) to choose the arrangement.

## Stage 5: add specialized tools when demand appears

Evaluate Context7 for dependency documentation, Serena for semantic code work, Playwright for browser exploration, or repository MCPs for account-backed inspection. Each addition should have its own recurring task and acceptance test. [Survey and tradeoffs](04-projects-and-workflows.md)

For a stable repeated operation, compare a CLI or ordinary script before adding a server. For a new long-lived local agent host, the archived MCPHost is a learning reference, not the starting recommendation.

## Record the adoption decision

Before calling an integration adopted, save a short note containing:

| Field | What to record |
|---|---|
| Purpose | One recurring task and the baseline it improves |
| Identity | Package version/commit, host version, model if applicable |
| Access | Allowed corpus, account, repository, and data destinations |
| Evidence | Passing cases, failure cases, observed latency and quality |
| Upkeep | Owner, update method, dependency/release review |
| Recovery | How to disable the entry and restore any affected data |
| Open gaps | What remains untested or deliberately excluded |

This is a proposed decision format. It becomes evidence only after being populated with observed results.

For now, the most useful next step is [the single-server documentation exercise](02-codex-mcp-commands.md#3-first-exercise-a-documentation-server). It needs the least new machinery and teaches the full registration-to-result cycle.

[Index](README.md) · [Next: research method and evidence](09-research-method-and-evidence.md)
