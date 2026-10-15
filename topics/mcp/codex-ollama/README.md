# Codex, MCP, and Ollama: A Practical Field Guide

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Researched guide; integrations require their own acceptance tests

This guide answers three practical questions: how to use `codex mcp`, what useful MCP workflows people have implemented, and how to build a small integration that works well with Codex and Ollama. It combines official documentation with inspection of public GitHub and GitLab projects. It is organized as lessons and decisions, rather than an installation list.

The first distinction to learn is that **a model provider and an MCP server have different jobs**. Running Codex with an Ollama model, giving Codex an Ollama-backed tool, and building an Ollama agent that calls MCP servers are separate architectures. Start with [the mental model](01-mental-model.md).

## Reading paths

| Your immediate goal | Read |
|---|---|
| Understand the moving parts | [01 — Mental model](01-mental-model.md) |
| Learn the terminal commands | [02 — `codex mcp`, step by step](02-codex-mcp-commands.md) |
| Choose a Codex/Ollama architecture | [03 — Ollama integration patterns](03-ollama-integration-patterns.md) |
| See what others actually built | [04 — Projects and workflow evidence](04-projects-and-workflows.md) |
| Design a useful server | [05 — Building a focused MCP](05-building-a-focused-mcp.md) |
| Try a concrete workflow | [06 — Practical recipes](06-practical-recipes.md) |
| Diagnose failures and evaluate usefulness | [07 — Validation and troubleshooting](07-validation-and-troubleshooting.md) |
| Decide what to adopt first | [08 — Adoption plan](08-adoption-plan.md) |
| Check the research limits and local checks | [09 — Research method and evidence](09-research-method-and-evidence.md) |
| Find official references and pinned repository snapshots | [Sources](SOURCES.md) |

For a first session, read 01 and 02, then try only the OpenAI documentation exercise. For implementation, read 03, 05, and 07 before selecting a framework.

## Recommended starting point

These are engineering recommendations, not measured rankings:

1. Learn registration, discovery, a real tool call, and removal using the public OpenAI Docs MCP. Its [official description](https://developers.openai.com/learn/docs-mcp) identifies it as a documentation service.
2. Test note retrieval against a small set of known answers. Compare it with the repository's existing Markdown search before adding an index or memory system.
3. Choose one Ollama role. Use it as Codex's provider, or expose a bounded summarization/classification operation as an MCP tool. Measure each independently.
4. Add repository, browser, or semantic code tools only when a repeated task justifies them.

The survey includes maintained projects, educational examples, and an archived project. Their differences matter more than their popularity. [The project chapter](04-projects-and-workflows.md) explains the tradeoffs and explicitly identifies MCPHost as archived.

## How to read the evidence

- **Documented:** supported by a fetched official or maintainer source.
- **Code inspected:** a specific implementation path was read; that does not establish runtime reliability.
- **Locally checked:** executed during preparation, with the narrow scope recorded in [09](09-research-method-and-evidence.md).
- **Proposed:** a design or adoption recommendation to evaluate.

Commands that add/remove registrations, authenticate, install packages, load models, or write notes have effects. They are teaching instructions unless the evidence log explicitly says they were executed. No live integration should be inferred from the existence of this guide.

The earlier [MCP learning notes](../learning-notes.md) remain a saved conversation and syllabus. This folder is a separately sourced practical reference.
