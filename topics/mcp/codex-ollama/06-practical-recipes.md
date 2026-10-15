# 06 — Practical Workflows to Try Deliberately

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Reviewable recipes; service registrations and model calls not executed

Each recipe has a concrete outcome and a way to recognize failure. Start with one at a time. The registration examples change local configuration; a later tool call may access data or load models. Placeholder paths and model names must be replaced before use.

## A. Answer a Codex question with the official source

**Outcome:** an explanation grounded in the current OpenAI documentation.

Register the service, then start a new Codex session:

```bash
codex mcp add openai-docs --url https://developers.openai.com/mcp
codex
```

Ask inside Codex:

```text
Use openai-docs to find and read the official description of
project-scoped MCP configuration. Explain it in plain language,
link the supporting page, and distinguish it from user configuration.
```

**Check:** the trace contains a server tool call, and the linked page supports the answer. **Failure:** the host answers from memory, cannot see the tool, or cites a search result without reading the page. Revisit [the registration lesson](02-codex-mcp-commands.md). [Official service](https://developers.openai.com/learn/docs-mcp)

## B. Find a note and verify its provenance

**Outcome:** locate the saved explanation of a topic and read its actual context/status.

First establish an ordinary search baseline from the repository root:

```bash
rg -n -i 'mcp|ollama' topics
```

Then evaluate a notes MCP with the same questions. A proposed prompt:

```text
Find my note about MCP registration. Return the source path, document
version, and a short exact excerpt. Read the note before summarizing it.
If it is a saved conversation rather than current verification, say so.
```

**Candidate:** [Basic Memory](https://github.com/basicmachines-co/basic-memory), or a focused adapter designed in [05](05-building-a-focused-mcp.md). **Check:** correct note, exact excerpt, source status preserved, no unwanted edits. **Tradeoff:** Basic Memory brings indexing and write capabilities. Test it with a separate sample corpus before choosing how it should interact with a Git-managed library.

Retrieval-augmented generation does not inherently require embeddings. Add semantic retrieval only after the keyword baseline misses important paraphrased questions. [Ollama embedding API](https://docs.ollama.com/api/embed)

## C. Delegate a short summary to Ollama

**Outcome:** Codex asks a local model to summarize a supplied public excerpt and reports that output as a draft.

The following TOML adapts the inspected `jayluxferro/ollama-mcp` interface. It assumes you separately reviewed and prepared a pinned checkout and its virtual environment. It is not an installation script.

```toml
[mcp_servers.ollama-helper]
command = "/absolute/path/to/ollama-mcp/.venv/bin/python"
args = ["/absolute/path/to/ollama-mcp/server.py"]
env = { OLLAMA_BASE_URL = "http://127.0.0.1:11434", OLLAMA_TIMEOUT = "90" }
enabled_tools = ["ollama_version", "list_models", "generate"]
startup_timeout_sec = 20
tool_timeout_sec = 100
default_tools_approval_mode = "prompt"
```

This example selects the wrapper's three named tools and keeps Codex's outer timeout longer than the configured HTTP timeout. The limits are starting choices to test. Generation may still exceed the useful workload budget; a purpose-built summary tool should enforce input, model, and output limits itself. [Wrapper source](https://github.com/jayluxferro/ollama-mcp/blob/c37e2b5a1b6c19844c571054fedfb5de4a7dfcc2/server.py), [Codex configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference)

Ask:

```text
Use ollama-helper to list available models. Before generating, identify
the selected local model and the exact public excerpt to send.
Summarize that excerpt in at most five bullets. Label the result as a
model-generated draft and retain the excerpt's source link.
```

**Check:** the actual endpoint/model is the intended one, output is useful, errors are visible, and no model-management operations are needed. **Unknown until tested:** real generation latency, output quality, proxy behavior, and whether the configured limits fit the machine. This wrapper does not make the entire Codex conversation local.

## D. Use an Ollama model as Codex's provider

**Outcome:** a Codex CLI session whose selected model comes from Ollama.

Read-only preflight for an existing local service:

```bash
ollama list
curl --fail --silent --show-error --max-time 5 \
  http://127.0.0.1:11434/api/tags
```

Then choose an installed compatible model and launch the session:

```bash
OLLAMA_MODEL='your-installed-tool-capable-model'
codex --oss --local-provider ollama -m "$OLLAMA_MODEL"
```

Model selection and inference are real runtime actions. No model download is implied by this recipe. **Check:** the session uses the intended model; one controlled MCP call succeeds with a valid result; the agent stops within its task budget. Compare with the same fixture under the usual provider. [Ollama Codex integration](https://docs.ollama.com/integrations/codex), [model-list API](https://docs.ollama.com/api/tags)

## E. Inspect one repository or CI failure

**Outcome:** a source-backed explanation of one PR or pipeline result, with exact repository and revision.

For GitHub's hosted MCP server, this illustrative configuration uses the documented read-only header and an environment-variable name for a token:

```toml
[mcp_servers.github-read]
url = "https://api.githubcopilot.com/mcp/"
bearer_token_env_var = "GITHUB_PAT_TOKEN"
http_headers = { "X-MCP-Readonly" = "true", "X-MCP-Toolsets" = "repos,pull_requests,actions" }
```

Prepare credentials separately with the least access needed, then verify the actual account/repository. A `.env` file is not automatically loaded just because it exists. Do not add a second GitHub connection if the current connected tools already meet the task. [GitHub Codex guide](https://github.com/github/github-mcp-server/blob/c59e8c199efbf14467211861de44afaeeb6873e3/docs/installation-guides/install-codex.md), [remote headers](https://github.com/github/github-mcp-server/blob/c59e8c199efbf14467211861de44afaeeb6873e3/docs/remote-server.md)

Ask:

```text
Inspect PR NUMBER in OWNER/REPOSITORY at its current head revision.
Read the diff and failed-check evidence. Explain the failure and cite
the relevant files/logs. Prepare proposed next steps for review.
```

For GitLab's native service, after confirming the instance's access prerequisites, the corresponding registration shape is:

```bash
codex mcp add gitlab-read --url https://gitlab.com/api/v4/mcp
codex mcp login gitlab-read
```

The name `gitlab-read` is only a label; it does not enforce read-only behavior. Inspect and restrict the available tools and use appropriate permissions. Self-managed instances use their own host. GitLab's native service and zereight's community adapter have different authentication requirements. [GitLab official setup](https://docs.gitlab.com/user/model_context_protocol/mcp_server/)

**Check:** the report names the repository, current revision, and observed evidence. A passed check at another commit or an accepted request to rerun CI is insufficient.

## F. Reproduce a small browser issue

**Outcome:** a reproducible page sequence with observed before/after state.

Playwright MCP is an option when exploratory interaction and retained browser context help. Its maintainer example can be expressed with Codex's explicit command separator:

```bash
# Illustrative package run; review/pin a release for a durable setup.
codex mcp add playwright -- npx -y @playwright/mcp@latest
```

Use a local or designated test site. Ask the agent to inspect the page before acting and to verify the final state. **Check:** the resulting page state matches the expected behavior, and the reproduction can be repeated. **Alternative:** a short Playwright script or CLI plus skill may be simpler for a stable regression check. [Maintainer guidance](https://github.com/microsoft/playwright-mcp/blob/b8b4183e099f136cbec0388a6088d4aa2f6b9685/README.md)

[Index](README.md) · [Next: validation and troubleshooting](07-validation-and-troubleshooting.md)
