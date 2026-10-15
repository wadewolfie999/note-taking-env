# 07 — Verify the Workflow and Find the Failing Layer

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Validation guidance and a reproducible fixture check

Separate registration, protocol behavior, model behavior, and task usefulness. They need different evidence. A server in a configuration file proves only that an entry was saved.

## Run the teaching example without a model

After saving `notes_demo.py` from [05](05-building-a-focused-mcp.md), save this as `check_demo.py` in the same lab directory. It uses the SDK client, including a real subprocess connection, without registering a server in Codex or starting an HTTP listener.

```python
"""Check the notes MCP teaching fixture, version 0.0.1."""
import asyncio
import sys
from pathlib import Path

from mcp import Client, StdioServerParameters
from notes_demo import mcp


async def check(target, label):
    async with Client(target) as client:
        tools = (await client.list_tools()).tools
        assert [tool.name for tool in tools] == ["read_demo_note"]
        definition = tools[0].model_dump(by_alias=True)
        assert definition["inputSchema"]["properties"]["topic"]["enum"] == [
            "mcp", "ollama"
        ]
        assert definition["annotations"]["readOnlyHint"] is True

        for topic in ("mcp", "ollama"):
            result = await client.call_tool("read_demo_note", {"topic": topic})
            assert result.is_error is False
            assert result.structured_content["source"] == f"fixture://{topic}"
            repeated = await client.call_tool("read_demo_note", {"topic": topic})
            assert repeated.structured_content == result.structured_content

        for arguments in ({"topic": "unknown"}, {}, {"topic": 9}):
            result = await client.call_tool("read_demo_note", arguments)
            assert result.is_error is True
        print(f"PASS: {label}")


async def main():
    await check(mcp, "in-memory")
    script = Path(__file__).with_name("notes_demo.py").resolve()
    target = StdioServerParameters(command=sys.executable, args=[str(script)])
    await check(target, "stdio subprocess")


if __name__ == "__main__":
    asyncio.run(main())
```

Run from the lab directory:

```bash
.venv/bin/python check_demo.py
```

Expected success lines are `PASS: in-memory` and `PASS: stdio subprocess`. The SDK may also log expected validation rejections for the bad inputs. Both passes were observed on the stack recorded in [09](09-research-method-and-evidence.md). The approach follows the official [client guide](https://py.sdk.modelcontextprotocol.io/client/) and [in-memory testing example](https://py.sdk.modelcontextprotocol.io/get-started/).

These checks establish discovery, schema/annotation metadata, stable valid results, invalid-input handling, and stdio wiring for this fixture. They do not measure a model's tool selection or a real notes backend.

## Prove the next layer separately

| Layer | Evidence to collect | What it does not establish |
|---|---|---|
| Configuration | Correct entry from `codex mcp get NAME` | Server starts or authenticates |
| Transport and discovery | Active connection and expected tool schema | Backend operation succeeds |
| Backend | Known input returns a correct source-backed result | Model chooses the right tool |
| Host/model | Trace shows valid arguments and correct result use | Broad task reliability |
| Workflow | Repeated representative tasks meet acceptance criteria | Suitability for every task |

For a new MCP server, the [MCP Inspector](https://github.com/modelcontextprotocol/inspector) is another discovery/call debugging option. Review its current version's commands and local web/proxy behavior before starting it; the SDK client check above is sufficient for this small fixture.

## A minimal real-world evaluation

For this notes repository, start with a small manually scored set, for example ten questions:

- Three exact-term lookups with known source notes.
- Three paraphrased questions that may benefit from semantic retrieval.
- Two questions for which the corpus has no answer.
- One note with outdated/saved-context status that must be preserved.
- One instruction embedded in a retrieved note that must remain data.

Run the same set with ordinary file search and with the candidate MCP. Record source correctness, factual correctness, false positive answers, elapsed time, tool-call count, and unwanted mutations. If using a model, record its exact identity, context allocation, and inference parameters. This is a proposed evaluation, not a benchmark already run.

A useful acceptance rule is: correct sources for answerable questions, explicit uncertainty for missing evidence, no unauthorized changes, and an improvement on the problem that justified the adapter. Choose the quantitative latency/quality threshold before comparing results.

For Ollama, add cold/warm generation, unavailable model, oversized input, timeout, malformed tool call, unknown tool, and total-step-limit cases. Confirm the host returns each tool result with the correct identity. [Ollama's tool loop](https://docs.ollama.com/capabilities/tool-calling)

## Troubleshoot by symptom

| Symptom | First small check | Next action |
|---|---|---|
| `codex: command not found` | `command -v codex` in the same terminal | Locate the installed executable or follow official installation guidance |
| `unexpected argument` | `codex mcp add --help` | Match the local version; include `--` before a stdio command |
| Entry listed, no active tools | New session's `/mcp`, launch command, stderr | Diagnose process/connection/auth startup before changing the model |
| Python import error | Use the configured absolute interpreter | Install dependencies into that environment, not an unrelated Python |
| stdio server appears idle by hand | Is it awaiting protocol input? | Use an MCP client; quiet stdin waiting can be normal |
| JSON/protocol parse errors | Wrapper/import-time stdout and launch arguments | Keep diagnostics on stderr and preserve protocol output |
| Works in Terminal, fails in desktop | PATH, working directory, environment at that launch surface | Use explicit executable paths and supported credential forwarding |
| OAuth login fails | Correct endpoint, account, advertised auth, callback, local CLI version | Follow the provider's diagnostics; preserve the actual error |
| HTTP 401/403 | Credential presence and exact server auth requirements | Fix the smallest proven mismatch; do not blindly grant broad scopes |
| Inference URL rejected as MCP | Does the endpoint implement MCP? | Use a model-provider configuration or an actual MCP wrapper |
| Local Ollama call gets proxy errors | Endpoint and client proxy environment | Compare direct loopback access and review `NO_PROXY`/client behavior |
| Tool call times out | Separate startup, backend, and outer tool timings | Size the task first; then set measured timeout budgets |
| Model repeatedly calls tools | Trace, tool results, schema, stop conditions | Reduce tool surface, fix result handling, enforce total step limits |
| Model invents a tool result | Was there an executed call and returned evidence? | Report the unverified answer and test a smaller known task |

The configuration/CLI rows draw on [Codex MCP documentation](https://learn.chatgpt.com/docs/extend/mcp?surface=cli) and the checked local help. The stdio behavior is explained in the [SDK running guide](https://py.sdk.modelcontextprotocol.io/run/). Ollama documents proxy considerations in its [FAQ](https://docs.ollama.com/faq). The table gives diagnostic hypotheses; it does not establish a cause for an uninspected machine.

## Controls that belong in a real implementation

Keep authenticated identity and authorization in the server; tool annotations are descriptive hints. Treat fetched pages, repository text, and tool output as data. Use allowlisted operations, bounded output, clear failures, cancellation, and appropriate credential handling. A transport connection does not confer authority to publish or modify unrelated resources. [OpenAI server guidance](https://developers.openai.com/plugins/build/mcp-server), [MCP security guidance](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices)

For data-changing operations, test stale versions, retries after ambiguous failure, duplicate requests, and recovery. A tool returning “accepted” may need a later status/readback before completion can be claimed. Add this machinery when the operation requires it, rather than putting a background job system into a simple read tool.

[Index](README.md) · [Next: adoption plan](08-adoption-plan.md)
