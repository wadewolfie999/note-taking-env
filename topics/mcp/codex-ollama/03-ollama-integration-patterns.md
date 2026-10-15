# 03 — Integrating Ollama Without Mixing Up the Roles

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Documented patterns; model performance and host interoperability not benchmarked

Choose the job you want Ollama to do before choosing an MCP package. The following patterns solve different problems.

## Pattern A: Ollama supplies Codex's main model

```text
You → Codex host ↔ Ollama model
             ↘ MCP servers and other available tools
```

Use this when you want Codex's interface and execution loop with an Ollama-served model. MCP is optional; it adds capabilities after the provider is working.

For an already running Ollama service and an installed, compatible model:

```bash
# Replace the value with an installed model you intend to use.
OLLAMA_MODEL='your-installed-tool-capable-model'
codex --oss --local-provider ollama -m "$OLLAMA_MODEL"
```

This starts a real agent session and can load a model. The provider flags were checked in local Codex 0.162.1 help. Ollama also documents `ollama launch codex`, which creates or refreshes a dedicated profile/catalog. Treat that as a configuration action, and inspect its current behavior before using it. [Ollama's Codex guide](https://docs.ollama.com/integrations/codex)

Use a model that supports the required tool behavior and fits the machine. Ollama recommends at least 64k context for Codex. A larger context also needs more memory; increasing the setting does not establish useful performance. Inspect the effective allocation with `ollama ps` during your own test. [Context-length guidance](https://docs.ollama.com/context-length)

**Acceptance:** the model can complete a small ordinary response, call one known tool with valid arguments, consume its result, and stop. Test these before adding a large MCP catalog. Do not infer compatibility across the CLI, desktop, IDE, and cloud surfaces from a single CLI result.

## Pattern B: Codex calls a tool backed by Ollama

```text
You → Codex + its selected model → MCP tool → Ollama → tool result → Codex
```

Use this for a bounded second opinion, classification, draft summary, or extraction operation while keeping your preferred main model. The server is an adapter around Ollama's API.

Real implementations include [jayluxferro/ollama-mcp](https://github.com/jayluxferro/ollama-mcp) and [true-alter/mcp-ollama](https://github.com/true-alter/mcp-ollama). Their interfaces and side effects differ; the [project survey](04-projects-and-workflows.md#ollama-as-a-service-behind-mcp) explains why a wrapper should be reviewed before adoption.

A proposed focused interface for a personal research library:

| Operation | Input | Output | Server-owned constraint |
|---|---|---|---|
| `classify_note` | Text and allowed categories | One category, rationale, model identity | Fixed category list and input limit |
| `summarize_excerpt` | Text and maximum length | Draft summary and input identifier | Model allowlist, timeout, output cap |
| `extract_references` | Text | Candidate references with offsets | Preserve original text; mark unverified links |

These names are designs, not built-in Ollama or Codex tools. Prefer this narrow interface if the alternative exposes model installation, deletion, arbitrary URLs, and unrestricted generation together.

**Data consequence:** local computation does not guarantee that the main model never sees the input. The host may already have read the text and ordinarily receives the returned summary. Evaluate the whole path when deciding where private material may go.

**Cost consequence:** the main model still selects the operation and reads its result. Local inference consumes hardware time and memory. Measure total session cost and latency before claiming savings.

## Pattern C: an Ollama model chooses MCP tools in another host

```text
You → Agent host ↔ Ollama
          ↘ MCP client → MCP server → backend
```

Examples include [ollmcp](https://github.com/jonigl/mcp-client-for-ollama), [Goose](https://github.com/aaif-goose/goose), and [mcp-use](https://github.com/mcp-use/mcp-use). The [GitLab learning client](https://gitlab.com/gitlab-da/use-cases/ai/ai-research/ollama/ollama-rag-tool-agents-mcp-first-learning-steps/-/blob/16db4e67b4a4dec6bd56381bbb53f3945a5b4769/step5_mcp_client.py) shows the wiring explicitly.

The host must perform the execution loop:

1. Discover the server's tools and schemas.
2. Present compatible tool definitions to Ollama.
3. Receive the model's proposed function calls.
4. Validate names, arguments, access, and any required user decision.
5. Execute allowed operations through the MCP client.
6. Return each result to the conversation with its tool identity.
7. Continue within a step/time budget, or stop with a clear reason.

Ollama's [tool-calling documentation](https://docs.ollama.com/capabilities/tool-calling) demonstrates the model-side exchange. It is the application that executes the functions. A model emitting a function name does not prove that any operation ran.

For a custom host, preserve structured results and error indicators, handle multiple calls, and close sessions on failure. Bound recursion and repeated calls. Do not treat returned text as fresh authorization to run a different tool.

## Pattern D: local retrieval with Ollama embeddings

```text
Approved notes → chunk/index process → Ollama embeddings → local index
Codex → notes MCP search → ranked excerpts with source references
```

This is useful when exact terms do not retrieve the right notes. It adds indexing, stale-index handling, and model/version tracking. Ollama provides an [embedding API](https://docs.ollama.com/api/embed); it does not by itself provide the complete ingestion, retrieval, or MCP service.

Start with a keyword baseline. If you add embeddings, record the embedding model, document hashes, chunking rules, and index build time. Rebuild or migrate the index when the embedding space changes. Return source paths and excerpts so a reader can check the result.

## Boundaries that matter in practice

| Assumption | What to establish instead |
|---|---|
| “The URL is localhost, so it is private.” | Which process/container owns that localhost, and whether any remote backend is called |
| “It uses Ollama, so it is offline.” | The selected model is local; web search, cloud routing, and remote MCPs are separately controlled |
| “It has an OpenAI-compatible API.” | The exact endpoint, tool schema, streaming, and Responses/Chat Completions behavior the host needs |
| “The model has a large advertised window.” | Effective runtime context and acceptable memory/latency under this workload |
| “The server is read-only.” | It has no data mutation or model-management paths available to this workflow |

Ollama documents cloud-feature controls and loopback defaults in its [FAQ](https://docs.ollama.com/faq). Its [compatibility guide](https://docs.ollama.com/api/openai-compatibility) describes supported API behavior. Its [web-search service](https://docs.ollama.com/capabilities/web-search) is a separate networked capability. A local model should not be used as shorthand for a completely local workflow.

[Index](README.md) · [Next: implementations people built](04-projects-and-workflows.md)
