# 09 — Research Method and Evidence Boundaries

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Preparation record; publication is a separate Git operation

This chapter records what supports the guide, what was exercised locally, and what remains to be proven. It should make a later refresh possible without treating old observations as current facts.

## Research scope

The investigation covered official OpenAI Codex MCP/CLI/configuration documentation, OpenAI tool-design guidance, the MCP specification and Python SDK, Ollama's integration/API documentation, GitHub-hosted implementations, GitLab's native service documentation, and a GitLab-hosted learning project.

Searches included these topic groups:

- Codex MCP add/list/login and configuration scope.
- Codex with Ollama, tool calling, context allocation, and API compatibility.
- Documentation, browser, semantic-code, repository, and memory MCP workflows.
- Ollama MCP hosts, model-backed servers, and provider adapters.
- GitLab MCP authentication and Ollama client examples.

Search results were used to discover sources. Technical conclusions use fetched official documentation, maintainer material, and inspected code. Aggregators, social posts, and third-party generated guides were not used as technical authority.

Sixteen GitHub repositories were sampled at metadata/README level; selected client, provider, configuration, and server files were inspected more closely. One GitLab learning project was read through its API at a pinned commit. [SOURCES.md](SOURCES.md) records identities and paths. This is a broad targeted sample, not an exhaustive catalog of MCP projects.

## Evidence labels

| Label | Meaning |
|---|---|
| Documented | A fetched primary source states the behavior |
| Code inspected | A specific implementation path supports the described mechanism |
| Locally checked | A command or fixture was actually exercised during preparation |
| Proposed | An engineering recommendation or experiment for this library |
| Unknown/unverified | Runtime evidence was not collected for that claim |

Repository metadata was fetched on 2026-10-15. Commit dates indicate source freshness, not release versions, support guarantees, or deployed service versions. Snapshot links are pinned; documentation/service links can change.

## Observed local checks

- The working copy was cloned from `wadewolfie999/note-taking-env` at `fcf14d0f4786364a644251b7b8ed03a9c0d2d80c`, with a clean initial `git status`.
- Existing layout and note conventions were read before adding this folder. The saved MCP syllabus was retained.
- The installed executable reported `codex-cli 0.162.1`. The `mcp` command tree and help for add/get/list/login/logout/remove were inspected. The help calls emitted an environment warning about PATH alias creation, then printed their command syntax; they did not establish the state of any MCP connection.
- Python 3.12.14 was used in a disposable environment outside the repository with `mcp==2.3.0`.
- The exact teaching-server and companion-check blocks were extracted from the Markdown and exercised in memory and over a stdio subprocess. Discovery, enum schema, read-only metadata, both valid topics, repeated results, and invalid/missing/type-invalid arguments passed.
- All 74 distinct cited source URLs were checked for reachability. Three old OpenAI URLs had redirect issues in the direct checker; their current official destinations were confirmed and used instead.
- All new Markdown was checked for local link targets, code-fence balance, version/date metadata, Python syntax, shell syntax, and TOML parsing. `git diff --check` was run on the documentation changes.

These observations apply to this preparation session. They are not a claim that the user's CLI has any of the illustrated servers registered.

## Material findings that changed the guidance

1. **MCPHost is archived.** The GitHub API flag and its README agreed. It is presented as a learning reference.
2. **SDK/protocol generations differ.** The current SDK documents `MCPServer`; older examples use `FastMCP`. The current specification is dated 2026-07-28. The guide pins its lab dependency and calls for explicit host compatibility testing.
3. **GitLab servers are distinct.** The native endpoint and community adapters have different credential/configuration requirements.
4. **Wrapper scope varies.** The inspected Ollama wrapper exposes model lifecycle operations alongside inference, and its stated integration-test scope does not require real model inference.
5. **A local model can sit in a networked workflow.** Cloud providers, web search, and remote MCP backends must be considered separately from the location of inference.

The supporting sources are attached to each claim in [the project survey](04-projects-and-workflows.md) and [the integration patterns](03-ollama-integration-patterns.md).

## Remaining proof gaps

No live Ollama inference, model download, performance benchmark, account login, remote MCP authentication, third-party server installation, or Codex registration was performed. The teaching fixture was exercised with the official SDK client; it still needs an actual Codex tool call to establish that host/version combination.

The ten-question notes evaluation and the adoption sequence are proposed work. No security certification, production acceptance, universal compatibility, privacy guarantee, or cost saving is inferred from source inspection or the fixture checks.

The repository content is prepared locally. A remote write requires a separate confirmed publication step and readback; the presence of these files does not establish that GitHub has been updated.

## Refresh procedure

Recheck local CLI help, official documentation, repository archive/release status, and the exact source paths before using an old command. Repeat the fixture checks after changing its SDK version. For an adopted integration, rerun representative task and failure cases before accepting an update. Record the new evidence date and increment the affected document versions.

[Index](README.md) · [Source register](SOURCES.md)
