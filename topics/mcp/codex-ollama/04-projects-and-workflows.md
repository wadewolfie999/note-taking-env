# 04 — What People Have Built, and What Is Worth Reusing

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Primary-source survey and selected code inspection; third-party integrations not run

This survey follows workflows, not star counts. A useful candidate needs a recognizable task, an understandable interface, and an acceptance test you can reproduce. Each entry separates what the source establishes from the recommendation made here. Repository revisions, archive flags, and metadata are recorded in [SOURCES.md](SOURCES.md).

## Quick comparison

| Workflow | Concrete implementations | Adoption judgment |
|---|---|---|
| Retrieve authoritative API documentation | OpenAI Docs MCP, Context7 | Strong first experiment; low domain complexity |
| Explore a web application | Playwright MCP | Useful when persistent browser interaction matters |
| Navigate a large codebase semantically | Serena | Evaluate against existing search/navigation |
| Inspect repositories, issues, PRs, or CI | GitHub MCP; GitLab native MCP; zereight GitLab MCP | Prefer a small read surface and prove account/project targeting |
| Retrieve and capture durable notes | Basic Memory | Relevant to this repository; evaluate note ownership and synchronization |
| Run local models as tool-using agents | ollmcp, Goose, mcp-use | Choose a host before writing a new one |
| Delegate a bounded operation to Ollama | jayluxferro/ollama-mcp; true-alter/mcp-ollama | Review available operations and timeouts closely |
| Learn the underlying loop | GitLab advocacy example; archived MCPHost | Useful references with explicit age/status limits |
| Curate a Codex environment | Trail of Bits codex-config | Borrow individual decisions after review |

These judgments are proposals for this personal notes workflow, not performance measurements.

## Documentation, browser, and code workflows

### OpenAI Docs MCP: authoritative lookup

**Documented:** OpenAI hosts a documentation search/read service at `https://developers.openai.com/mcp`. **Workflow:** ask a precise Codex question, search, read the matching page, and cite it. **Useful property:** a small, understandable first task without writing a backend. **Limit:** finding a document does not prove a local installation behaves identically. [Official service guide](https://developers.openai.com/learn/docs-mcp)

### Context7: library documentation at the point of coding

**Inspected:** the maintainer README and MCP entrypoint. The service resolves libraries and retrieves relevant documentation. **Workflow:** identify the dependency/version, resolve the library, fetch focused material, and use it while implementing. **Reuse:** focused retrieval instead of returning an entire documentation site. **Tradeoff:** the repository states that its supporting backend, parser, and crawler are private; the public server source is not a complete offline deployment. Authentication/rate limits also need checking. [Project](https://github.com/upstash/context7), [inspected entrypoint](https://github.com/upstash/context7/blob/ef472a4912d594fdd193a5f929fab4c62796e287/packages/mcp/src/index.ts)

### Playwright MCP: inspect, interact, verify

**Documented:** browser automation through structured accessibility information, with Codex configuration examples. **Workflow:** navigate, inspect the page, take an action, and verify the resulting state. **Reuse:** keeping browser context across exploratory steps. **Tradeoff:** page trees and many tool calls can consume considerable context. The maintainers also recommend considering Playwright CLI plus skills for coding-agent workloads. Benchmark the task you actually repeat. [README and alternative discussion](https://github.com/microsoft/playwright-mcp/blob/b8b4183e099f136cbec0388a6088d4aa2f6b9685/README.md)

### Serena: symbols before broad text

**Inspected:** client setup, Codex context configuration, and MCP implementation. Serena exposes semantic navigation/editing and configures its tool surface for different hosts. Its Codex configuration uses `start-mcp-server`, `--project-from-cwd`, and `--context=codex`. **Workflow:** activate the correct project, find a symbol and its references, then inspect or change the smallest relevant section. **Reuse:** adapt the interface to the host instead of duplicating every built-in tool. **Tradeoff:** language tooling, indexing, project activation, and edit permissions need acceptance tests. [Client guide](https://github.com/oraios/serena/blob/4f2520efb128dd02e66a2302095bdbbb26ddd5a0/docs/02-usage/030_clients.md), [MCP code](https://github.com/oraios/serena/blob/4f2520efb128dd02e66a2302095bdbbb26ddd5a0/src/serena/mcp.py)

## Repository and knowledge workflows

### GitHub MCP: narrow repository operations

**Documented:** GitHub's maintained server offers remote HTTP and local deployment, toolsets, and read-only options. The Codex installation guide includes an environment-backed bearer token. **Workflow:** inspect one named repository, a specific PR/diff, and its CI evidence before proposing changes. **Reuse:** separate tool selection from access granted by the credential. **Tradeoff:** a small tool catalog does not narrow an overbroad token's underlying authority. The connected GitHub tools in a Codex chat are not proof that this standalone server is registered in the CLI. [Codex setup](https://github.com/github/github-mcp-server/blob/c59e8c199efbf14467211861de44afaeeb6873e3/docs/installation-guides/install-codex.md), [remote options](https://github.com/github/github-mcp-server/blob/c59e8c199efbf14467211861de44afaeeb6873e3/docs/remote-server.md)

### GitLab's native MCP service: account-scoped project access

**Documented:** an HTTP endpoint at an instance's `/api/v4/mcp`, authentication requirements, and Codex/client configuration. **Workflow:** retrieve project, issue, merge-request, or pipeline information through the platform's own interface. **Reuse:** use native authorization and a selected tool surface. **Tradeoff:** access depends on the instance configuration, user permissions, enabled features, and supported authentication. The current documentation distinguishes accepted MCP credentials from legacy PATs; do not copy a community server's token instructions into this service. [GitLab's official guide](https://docs.gitlab.com/user/model_context_protocol/mcp_server/)

### zereight/gitlab-mcp: broader community adapter

**Inspected:** README and Codex client guide. This is a separate server with its own package, PAT setup, toolsets, and permission modes. **Workflow:** expose GitLab APIs through a local adapter when that interface fits the instance and task. **Reuse:** begin with a small toolset and a restricted permission mode. **Tradeoff:** its credential model and configuration are not interchangeable with GitLab's native endpoint. Its README documents a transition from older read-only flags to `--permission-mode=readonly`; pin the release and use matching instructions. [Project](https://github.com/zereight/gitlab-mcp/blob/9d072c819f767de67d8609a23b2553b6cb9664a2/README.md), [Codex guide](https://github.com/zereight/gitlab-mcp/blob/9d072c819f767de67d8609a23b2553b6cb9664a2/docs/clients/codex.md)

### Basic Memory: notes that survive a thread

**Documented:** Markdown-based knowledge storage with MCP access and a Codex-specific plugin. Its plugin separates orientation, checkpoints, decisions, and lightweight capture. **Workflow:** retrieve relevant notes before work and save an evidence-backed decision/checkpoint afterward. **Reuse:** distinct note types and explicit project routing. **Tradeoff:** this introduces an index and synchronization behavior around notes; decide which repository/folder owns the files and inspect every write before adopting capture automation. The snapshot reports AGPL-3.0 licensing; review applicability when modifying or distributing it. [Project](https://github.com/basicmachines-co/basic-memory), [Codex plugin](https://github.com/basicmachines-co/basic-memory/blob/54b8d94b94cb8e4e526f438010ca771c5ad9f209/plugins/codex/README.md)

## Ollama as the agent's provider

### ollmcp: an interactive local-model MCP host

**Inspected:** README and tool-manager implementation. It provides a terminal host, multiple servers, model selection, and human review of calls. **Workflow:** register a small server set, choose a local model, approve a call, and inspect its result. **Reuse:** explicit host responsibilities and interactive control. **Tradeoff:** it adds a second agent environment with its own configuration and provider settings. Its command syntax belongs to `ollmcp`, not `codex mcp`. [README](https://github.com/jonigl/mcp-client-for-ollama/blob/f4dc1fb9fb4cf3743c5119e01d41961fca87be47/README.md), [tool manager](https://github.com/jonigl/mcp-client-for-ollama/blob/f4dc1fb9fb4cf3743c5119e01d41961fca87be47/mcp_client_for_ollama/tools/manager.py)

### mcp-use: build a custom agent application

**Inspected:** Python client/agent documentation and the TypeScript Ollama provider. The latter converts tool schemas to Ollama's function format and serializes tool results into the conversation. **Workflow:** connect selected MCP servers, supply a model provider, and run a bounded agent loop. **Reuse:** client lifecycle and provider boundaries. **Tradeoff:** a framework adds dependencies and behavior that must be understood; the Python and TypeScript APIs differ. Select one release/language, then validate its exact example rather than mixing snippets. [Python guide](https://github.com/mcp-use/mcp-use/blob/ad8fbee97af6c39fc95b46486a04142d68644822/libraries/python/README.md), [Ollama provider](https://github.com/mcp-use/mcp-use/blob/ad8fbee97af6c39fc95b46486a04142d68644822/libraries/typescript/packages/agent/src/llm/providers/ollama/index.ts)

### Goose: a broader extensible agent host

**Inspected:** project overview and Ollama provider implementation. Goose combines providers with MCP extensions and reusable workflows. **Workflow:** choose a provider, enable the required extensions, and run a repeated task with controlled inputs. **Reuse:** provider-independent capability packaging. **Tradeoff:** it is a complete agent host, so adopting it is a larger decision than adding one server to Codex. Compare end-to-end task quality and operational overhead. [Project](https://github.com/aaif-goose/goose), [Ollama provider code](https://github.com/aaif-goose/goose/blob/dd17bf2021f3fb3b326c471593c352e1b0d62bbd/crates/goose-providers/src/ollama.rs)

### GitLab learning project: follow the entire tool loop

**Code inspected:** `step5_mcp_client.py` launches an MCP server, lists its tools, converts schemas, sends them to `ollama.chat`, executes returned calls, and feeds results back within a five-iteration default limit. **Workflow:** query an event-schedule backend. **Reuse:** a readable explanation of where MCP stops and the model loop begins. **Limits observed:** the client makes a synchronous Ollama call inside async code and reduces results to text. Its snapshot is from 2025, and README filenames differ from the inspected client filename. Treat it as an educational reference to adapt to current APIs. [Pinned GitLab source](https://gitlab.com/gitlab-da/use-cases/ai/ai-research/ollama/ollama-rag-tool-agents-mcp-first-learning-steps/-/blob/16db4e67b4a4dec6bd56381bbb53f3945a5b4769/step5_mcp_client.py)

## Ollama as a service behind MCP

### jayluxferro/ollama-mcp: a direct API wrapper

**Code inspected:** `server.py` implements model listing, chat, generation, embeddings, and model lifecycle operations. It reuses an async HTTP client and has configurable timeouts. **Workflow:** let a host call an existing local model. **Reuse:** a straightforward API-to-tool boundary. **Limits:** model pull/copy/delete are also exposed, so the full catalog is broader than a summary task needs. Its README explicitly says CI can run without a real Ollama service; those tests establish wiring, not model quality. Restrict operations and verify proxy/endpoint behavior for your environment. [Implementation](https://github.com/jayluxferro/ollama-mcp/blob/c37e2b5a1b6c19844c571054fedfb5de4a7dfcc2/server.py), [test scope](https://github.com/jayluxferro/ollama-mcp/blob/c37e2b5a1b6c19844c571054fedfb5de4a7dfcc2/README.md#testing)

### true-alter/mcp-ollama: task-shaped local delegation

**Documented:** tools for local generation tasks such as classification, summaries, and coding drafts. The README distinguishes the server from an agent host and documents endpoint restrictions. **Reuse:** define operations around recognizable tasks instead of exposing every model API option. **Limits:** the README also describes optional local ledger writes when a related runtime is present. That side effect matters to integration review even though inference is local. No runtime, privacy, or cost claims from this project were independently tested. [Pinned README](https://github.com/true-alter/mcp-ollama/blob/22d77704da3489e1c23527e3a16c6c3c0abdb23f/README.md)

## Useful references with adoption limits

### MCPHost: archived, still instructive

**Verified metadata:** `mark3labs/mcphost` was archived at inspection; its README says it receives no further updates or bug fixes. It documents provider abstraction, MCP configuration, and Ollama usage. **Reuse:** study host/provider separation. **Adoption judgment:** do not make it the default for a new long-lived setup without deliberately accepting maintenance responsibility. [Archive notice and README](https://github.com/mark3labs/mcphost/blob/632cc196002275d42dbf93ad3f1241ff853deb80/README.md)

### Trail of Bits codex-config: a curated environment

**Documented:** a collection of Codex setup decisions and recommended MCPs for particular engineering/security workflows. **Reuse:** curate servers by purpose and document their prerequisites. **Limit:** a firm's complete security-tool environment may be excessive for a personal notes repository. Borrow individual settings after checking current official configuration and local requirements. [Pinned README](https://github.com/trailofbits/codex-config/blob/29a66d1aef1a3e6c2696811109a781b059b95b4f/README.md)

## Patterns worth keeping

The common useful design is a small domain interface: documentation lookup, symbol lookup, repository inspection, note retrieval, or a bounded local-model operation. The most important acceptance test belongs to the workflow: correct source, correct project, valid arguments, useful result, and a clean stop.

For protocol construction, use the [official SDK](https://github.com/modelcontextprotocol/python-sdk). The [reference-server repository](https://github.com/modelcontextprotocol/servers) explicitly describes its servers as educational examples, not production-ready solutions. An official origin and a production acceptance decision are different pieces of evidence.

[Index](README.md) · [Next: build a focused MCP](05-building-a-focused-mcp.md)
