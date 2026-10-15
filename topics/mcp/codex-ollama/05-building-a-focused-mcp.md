# 05 — Build a Focused MCP Around a Useful Task

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Proposed design guidance plus a locally exercised teaching example

A viable MCP starts with a job you can recognize and verify: “find the note that explains this command and return the relevant passage.” The server should own the rules for accessing notes; the host should own the conversation and execution decisions.

OpenAI recommends tools that correspond to clear user goals, focused operations, and server-side authorization. Its metadata guidance explains why names and descriptions affect tool selection. These are useful design principles even when no custom UI is involved. [Define tools](https://developers.openai.com/plugins/plan/tools), [Build an MCP server](https://developers.openai.com/plugins/build/mcp-server), [Optimize metadata](https://developers.openai.com/plugins/guides/optimize-metadata)

## Define the contract before adding a framework

For a real notes adapter, a reasonable initial contract is:

| Operation | Inputs | Result | Constraint |
|---|---|---|---|
| `search_notes` | Query, result limit | IDs, titles, excerpts, source references | Bounded query and result size |
| `read_note` | Stable note ID, optional bounded section | Text, source path, content hash | Only approved notes |
| `propose_note_update`, later | ID, expected content hash, proposed text | Reviewable change | Does not silently publish |

These are proposed names. Begin with the first two. A write interface should have explicit ownership, conflict detection, duplicate handling, and a recoverable record of what changed.

Keep responsibilities small:

```text
Host: prompts, model choice, review, tool selection
MCP boundary: schemas, tool descriptions, result/error format
Domain service: search/read rules, size limits, identifiers
Adapters: filesystem or database; optional Ollama API
```

Avoid a generic `run_command` or `execute_anything` interface for this task. It moves the important domain rules into a prompt and makes behavior harder to test.

## A complete, deliberately small teaching server

This fixture demonstrates a genuine MCP tool without opening files, connecting accounts, or loading a model. It reads two built-in notes. It is not a search engine for this repository.

**Tested stack:** Python 3.12.14 and `mcp==2.3.0`. The current official SDK uses `MCPServer`; older examples commonly use the v1 `FastMCP` interface. Keep the dependency and example API aligned. [Official Python SDK](https://github.com/modelcontextprotocol/python-sdk), [SDK getting started](https://py.sdk.modelcontextprotocol.io/get-started/)

For a future hands-on exercise, create a new lab directory and a virtual environment. First check that `python3 --version` reports Python 3.11 or newer; otherwise select an installed compatible interpreter. The install command downloads packages into this new environment:

```bash
mkdir notes-mcp-lab
cd notes-mcp-lab
python3 -m venv .venv
.venv/bin/python -m pip install 'mcp==2.3.0'
```

Save the following as `notes_demo.py` in that lab directory:

```python
"""Notes MCP teaching fixture, version 0.0.1."""
from typing import Literal

from mcp.server import MCPServer
from mcp.types import ToolAnnotations

mcp = MCPServer("notes-demo", version="0.0.1")

NOTES = {
    "mcp": "MCP connects an AI application to tools and context.",
    "ollama": "Ollama serves models; an agent host executes tool calls.",
}


@mcp.tool(
    annotations=ToolAnnotations(
        readOnlyHint=True,
        destructiveHint=False,
        idempotentHint=True,
        openWorldHint=False,
    )
)
def read_demo_note(topic: Literal["mcp", "ollama"]) -> dict[str, str]:
    """Read one built-in teaching note. No files or network are accessed."""
    return {"topic": topic, "text": NOTES[topic], "source": f"fixture://{topic}"}


if __name__ == "__main__":
    mcp.run()
```

The enum makes the allowed input explicit. The result carries a source identifier. The annotations describe actual behavior, but are hints to a host rather than authorization controls. [MCP tool specification](https://modelcontextprotocol.io/specification/2026-07-28/server/tools)

Test it using the [companion check](07-validation-and-troubleshooting.md#run-the-teaching-example-without-a-model). Only after that, register it using the absolute paths from [02](02-codex-mcp-commands.md#4-a-local-process-uses----not---url). A new Codex session should be able to call `read_demo_note` with `topic="mcp"` and return `fixture://mcp`.

If you run the file directly, it waits for MCP input. The main guard prevents imports by tests or tooling from starting the server. The SDK documents this lifecycle in [Running your server](https://py.sdk.modelcontextprotocol.io/run/).

## Turning the fixture into a useful notes service

The following are implementation requirements to decide and test, not claims that the fixture implements them:

1. **Choose the allowed corpus.** Use a dedicated directory or manifest of note IDs. Do not let a model choose arbitrary filesystem roots.
2. **Enforce access at the read boundary.** Handle traversal, absolute paths, symlinks, unsupported file types, and concurrent file changes. Canonicalizing a path alone does not solve every filesystem race.
3. **Bound work and output.** Cap query length, document bytes, result count, and returned excerpt length. Provide pagination for genuinely larger results.
4. **Preserve provenance.** Return a path or stable ID, content hash/revision, and the exact excerpt. Separate retrieved facts from a generated summary.
5. **Use explicit failures.** Distinguish no matches, invalid arguments, forbidden access, stale content, and a backend timeout. Do not return a success-shaped paragraph for every error.
6. **Keep the service independently testable.** A search/read backend should work without an LLM. Add Ollama through a focused adapter after retrieval has passed its own tests.

For local single-user work, stdio is a simple starting point. A shared HTTP service also needs identity, authorization, request limits, cancellation, and an operational lifecycle. Choose it when there is a real sharing requirement. [MCP transport overview](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports), [security guidance](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices)

## Add Ollama through one bounded operation

A proposed `summarize_excerpt` operation should own the endpoint and allowed model, validate input length, cap generation, apply request timeouts, and return the model identity plus input hash. It should report timeout or malformed output clearly. Keep note mutation outside this operation.

If building an Ollama agent host instead, validate model-proposed tool names and arguments before execution and use a total step/time budget. The host must feed results back to the model; see [Ollama tool calling](https://docs.ollama.com/capabilities/tool-calling) and [the inspected mcp-use provider](https://github.com/mcp-use/mcp-use/blob/ad8fbee97af6c39fc95b46486a04142d68644822/libraries/typescript/packages/agent/src/llm/providers/ollama/index.ts).

## Compatibility is a measured property

The current MCP specification is dated 2026-07-28 and uses stateless, self-contained requests. Older tutorials show an initialization/session model. Use an SDK's supported compatibility behavior and test the exact Codex version instead of hand-writing a handshake from a dated blog post. [Current specification](https://modelcontextprotocol.io/specification/2026-07-28), [SDK legacy-client support](https://py.sdk.modelcontextprotocol.io/run/legacy-clients/)

The fixture was tested with the SDK's client in memory and over stdio. It was not registered in the user's Codex configuration, so full Codex interoperability remains a separate acceptance step.

[Index](README.md) · [Next: practical recipes](06-practical-recipes.md)
