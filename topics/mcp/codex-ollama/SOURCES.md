# Source Register

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Primary-source register and repository snapshot metadata

Sources were fetched on **2026-10-15**. Inline citations in each chapter identify the evidence for particular claims. This register provides a refresh path and records the revisions behind code observations. A source's existence is not an integration test.

## Official OpenAI sources

| Source | Used for |
|---|---|
| [Codex MCP guide](https://learn.chatgpt.com/docs/extend/mcp?surface=cli) | Registration, transports, OAuth, active-server inspection |
| [CLI reference](https://learn.chatgpt.com/docs/developer-commands?surface=cli) | Command semantics; compared with installed help |
| [Configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference) | MCP tables, paths, timeouts, tool selection and approval settings |
| [Docs MCP](https://developers.openai.com/learn/docs-mcp) | Public documentation endpoint and its purpose |
| [Define tools](https://developers.openai.com/plugins/plan/tools) | User-goal-oriented tool design |
| [Build an MCP server](https://developers.openai.com/plugins/build/mcp-server) | Server responsibility, authorization, annotations |
| [Optimize metadata](https://developers.openai.com/plugins/guides/optimize-metadata) | Tool selection and descriptions |
| [Package your plugin](https://developers.openai.com/plugins/build/plugins) | Relationship between instructions, packaging, and MCP wiring |

The Codex guide links use their current `learn.chatgpt.com` destinations. Older `developers.openai.com/codex/...` redirects were followed during research; direct destination links also passed HTTP checks. Local `codex --version` and subcommand `--help` output independently establish the narrow CLI observations in [09](09-research-method-and-evidence.md).

## MCP specification and SDK documentation

| Source | Used for |
|---|---|
| [Architecture, 2026-07-28](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture) | Host/client/server responsibilities |
| [Specification, 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28) | Current protocol generation and primitives |
| [Transport overview, 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports) | stdio versus Streamable HTTP |
| [Tools, 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28/server/tools) | Tool contract, output and annotation behavior |
| [Security best practices, 2026-07-28](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices) | Trust and authorization boundaries |
| [Python SDK getting started](https://py.sdk.modelcontextprotocol.io/get-started/) | Current API and in-memory test approach |
| [Python client guide](https://py.sdk.modelcontextprotocol.io/client/) | Client lifecycle, discovery, results |
| [Running a Python server](https://py.sdk.modelcontextprotocol.io/run/) | `MCPServer.run`, stdio lifecycle, transport options |
| [Serving legacy clients](https://py.sdk.modelcontextprotocol.io/run/legacy-clients/) | Compatibility across protocol generations |
| [Structured output](https://py.sdk.modelcontextprotocol.io/servers/structured-output/) | Typed result behavior |
| [PyPI MCP 2.3.0](https://pypi.org/project/mcp/2.3.0/) | Package release used in the disposable lab |

The 2025-11-25 transport/tool/security pages were also consulted while checking older examples. The current guide uses explicit 2026 references and does not assume that an older handshake is universal.

## Official Ollama sources

| Source | Used for |
|---|---|
| [Codex integration](https://docs.ollama.com/integrations/codex) | Provider setup, launch profile, context recommendation |
| [Tool calling](https://docs.ollama.com/capabilities/tool-calling) | Application-owned tool execution loop |
| [OpenAI compatibility](https://docs.ollama.com/api/openai-compatibility) | Inference API behavior and boundaries |
| [Web search](https://docs.ollama.com/capabilities/web-search) | Networked search capability and MCP example |
| [Context length](https://docs.ollama.com/context-length) | Memory tradeoff and effective allocation |
| [FAQ](https://docs.ollama.com/faq) | Local/cloud distinction, network defaults, proxy behavior |
| [List models](https://docs.ollama.com/api/tags) | Read-only model inventory endpoint |
| [Generate embeddings](https://docs.ollama.com/api/embed) | Embedding capability used in the proposed retrieval design |

## GitLab sources

- [GitLab native MCP server](https://docs.gitlab.com/user/model_context_protocol/mcp_server/): endpoint, authentication, prerequisites, client support, and tool selection.
- [GitLab Developer Advocacy learning project](https://gitlab.com/gitlab-da/use-cases/ai/ai-research/ollama/ollama-rag-tool-agents-mcp-first-learning-steps): staged RAG/tool/agent/MCP tutorial.
- [Pinned learning client](https://gitlab.com/gitlab-da/use-cases/ai/ai-research/ollama/ollama-rag-tool-agents-mcp-first-learning-steps/-/blob/16db4e67b4a4dec6bd56381bbb53f3945a5b4769/step5_mcp_client.py): implementation read through the GitLab API. Project ID `70382529`; default branch `main`; snapshot `16db4e67b4a4dec6bd56381bbb53f3945a5b4769`; commit date `2025-05-31T21:55:04+02:00`.

The learning project's page rendered little source text in the browser, so its README and client were retrieved from the public GitLab API. No runtime claim depends on that page rendering correctly.

## GitHub snapshots

Each revision link below pins the inspected README. Archive state is the API value on the retrieval date. License values are repository API metadata; `NOASSERTION` or `unknown` means this register has no concrete SPDX identification from that field.

| Repository | Pinned README revision | Commit date (UTC) | Archived | License metadata |
|---|---|---|---|---|
| [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) | [`b8b4183e099f`](https://github.com/microsoft/playwright-mcp/blob/b8b4183e099f136cbec0388a6088d4aa2f6b9685/README.md) | 2026-10-08 | false | Apache-2.0 |
| [upstash/context7](https://github.com/upstash/context7) | [`ef472a4912d5`](https://github.com/upstash/context7/blob/ef472a4912d594fdd193a5f929fab4c62796e287/README.md) | 2026-10-14 | false | MIT |
| [oraios/serena](https://github.com/oraios/serena) | [`4f2520efb128`](https://github.com/oraios/serena/blob/4f2520efb128dd02e66a2302095bdbbb26ddd5a0/README.md) | 2026-10-14 | false | NOASSERTION |
| [github/github-mcp-server](https://github.com/github/github-mcp-server) | [`c59e8c199efb`](https://github.com/github/github-mcp-server/blob/c59e8c199efbf14467211861de44afaeeb6873e3/README.md) | 2026-10-13 | false | MIT |
| [basicmachines-co/basic-memory](https://github.com/basicmachines-co/basic-memory) | [`54b8d94b94cb`](https://github.com/basicmachines-co/basic-memory/blob/54b8d94b94cb8e4e526f438010ca771c5ad9f209/README.md) | 2026-10-14 | false | AGPL-3.0 |
| [mark3labs/mcphost](https://github.com/mark3labs/mcphost) | [`632cc1960022`](https://github.com/mark3labs/mcphost/blob/632cc196002275d42dbf93ad3f1241ff853deb80/README.md) | 2026-04-13 | true | MIT |
| [jonigl/mcp-client-for-ollama](https://github.com/jonigl/mcp-client-for-ollama) | [`f4dc1fb9fb4c`](https://github.com/jonigl/mcp-client-for-ollama/blob/f4dc1fb9fb4cf3743c5119e01d41961fca87be47/README.md) | 2026-10-01 | false | MIT |
| [mcp-use/mcp-use](https://github.com/mcp-use/mcp-use) | [`ad8fbee97af6`](https://github.com/mcp-use/mcp-use/blob/ad8fbee97af6c39fc95b46486a04142d68644822/README.md) | 2026-10-14 | false | MIT |
| [aaif-goose/goose](https://github.com/aaif-goose/goose) | [`dd17bf2021f3`](https://github.com/aaif-goose/goose/blob/dd17bf2021f3fb3b326c471593c352e1b0d62bbd/README.md) | 2026-10-15 | false | Apache-2.0 |
| [jayluxferro/ollama-mcp](https://github.com/jayluxferro/ollama-mcp) | [`c37e2b5a1b6c`](https://github.com/jayluxferro/ollama-mcp/blob/c37e2b5a1b6c19844c571054fedfb5de4a7dfcc2/README.md) | 2026-04-23 | false | MIT |
| [true-alter/mcp-ollama](https://github.com/true-alter/mcp-ollama) | [`22d77704da34`](https://github.com/true-alter/mcp-ollama/blob/22d77704da3489e1c23527e3a16c6c3c0abdb23f/README.md) | 2026-09-22 | false | Apache-2.0 |
| [trailofbits/codex-config](https://github.com/trailofbits/codex-config) | [`29a66d1aef1a`](https://github.com/trailofbits/codex-config/blob/29a66d1aef1a3e6c2696811109a781b059b95b4f/README.md) | 2026-09-01 | false | unknown |
| [modelcontextprotocol/python-sdk](https://github.com/modelcontextprotocol/python-sdk) | [`754254c08fe1`](https://github.com/modelcontextprotocol/python-sdk/blob/754254c08fe1c3eda18b69da3d336106fe8d72d6/README.md) | 2026-10-13 | false | MIT |
| [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) | [`bbb701e7ac1e`](https://github.com/modelcontextprotocol/servers/blob/bbb701e7ac1e57f3edd1c2836b94c5457eaf7b9c/README.md) | 2026-10-11 | false | NOASSERTION |
| [modelcontextprotocol/inspector](https://github.com/modelcontextprotocol/inspector) | [`074b8108f340`](https://github.com/modelcontextprotocol/inspector/blob/074b8108f340590268f2900741403755e923a11c/README.md) | 2026-10-14 | false | NOASSERTION |
| [zereight/gitlab-mcp](https://github.com/zereight/gitlab-mcp) | [`9d072c819f76`](https://github.com/zereight/gitlab-mcp/blob/9d072c819f767de67d8609a23b2553b6cb9664a2/README.md) | 2026-10-15 | false | MIT |

## Selected implementation paths inspected

- **Context7:** `packages/mcp/src/index.ts`.
- **Serena:** `docs/02-usage/020_running.md`, `docs/02-usage/030_clients.md`, `src/serena/mcp.py`, and `src/serena/resources/config/contexts/codex.yml`.
- **GitHub MCP:** `docs/installation-guides/install-codex.md` and `docs/remote-server.md`.
- **ollmcp:** `mcp_client_for_ollama/tools/manager.py`.
- **mcp-use:** `libraries/python/README.md`, `libraries/python/mcp_use/agents/mcpagent.py`, and `libraries/typescript/packages/agent/src/llm/providers/ollama/index.ts`.
- **Goose:** `crates/goose-providers/src/ollama.rs`.
- **jayluxferro/ollama-mcp:** `server.py` and `pyproject.toml`.
- **zereight/gitlab-mcp:** `docs/clients/codex.md`.
- **Python SDK:** `docs_src/index/tutorial001.py`, `docs_src/client_transports/tutorial001.py`, `docs_src/client_transports/tutorial002.py`, and package metadata.

Use the revision in the snapshot table when reconstructing any of these paths. The project chapter directly links the most relevant files. Reading these selected paths was a mechanism check, not a complete source or security audit.
