# 02 — `codex mcp`, Step by Step

**Version:** 0.0.1 · **Previous version:** new artifact · **Revised:** 2026-10-15 · **Status:** Official documentation plus local CLI help checks; configuration exercises not executed

`codex mcp` manages the servers Codex knows about. You run it in a normal terminal. You then open Codex and ask it to use the registered tools.

The commands below were checked against the installed **`codex-cli 0.162.1`** help and the [official CLI reference](https://learn.chatgpt.com/docs/developer-commands?surface=cli). Use your installed command's help when an older tutorial differs.

## 1. Recognize the three places you type

| Where | What belongs there | Example |
|---|---|---|
| macOS Terminal, iTerm, or an editor terminal at a shell prompt | Shell commands | `codex mcp list` |
| Inside the interactive Codex terminal interface | Slash commands and requests | `/mcp` or “Search the documentation…” |
| A configuration file | TOML settings | `[mcp_servers.openai-docs]` |

Do not type the shell prompt symbol, such as `$` or `%`, when copying a command. In a multiline shell example, a final backslash joins the next line. Quoted text stays together as one argument.

## 2. Start with inspection

These commands inspect the executable and its command syntax:

```bash
codex --version
codex mcp --help
codex mcp add --help
```

Then inspect your configured servers:

```bash
codex mcp list
```

`list` reports registrations. It is not proof that every server has started successfully or that a real tool call can complete. Avoid sharing raw configuration output publicly if it contains private paths, server addresses, or inline credentials.

## 3. First exercise: a documentation server

This command changes Codex's saved MCP configuration by registering OpenAI's public documentation endpoint:

```bash
codex mcp add openai-docs --url https://developers.openai.com/mcp
```

Read it from left to right:

| Piece | Meaning |
|---|---|
| `codex` | Run the Codex executable |
| `mcp` | Use its MCP management commands |
| `add` | Save a server registration |
| `openai-docs` | Your local name for this registration |
| `--url` | This server uses Streamable HTTP |
| `https://developers.openai.com/mcp` | The actual MCP endpoint |

The server is a read-only OpenAI documentation service; it does not execute OpenAI API requests for you. OpenAI documents this endpoint and the registration flow in [Docs MCP](https://developers.openai.com/learn/docs-mcp).

Inspect the entry:

```bash
codex mcp get openai-docs
codex mcp list
```

Open a new Codex session in your intended working directory:

```bash
codex
```

Inside Codex, enter:

```text
/mcp
```

Then ask:

```text
Use the openai-docs MCP server to find the official explanation of
Codex MCP configuration. Read the relevant page and give me its link.
Identify which server tool you actually called.
```

Success means you can identify a tool call and inspect its result. A fluent answer without a call is not evidence that this connection worked. The official [Codex MCP guide](https://learn.chatgpt.com/docs/extend/mcp?surface=cli) documents `/mcp` for inspecting active servers.

## 4. A local process uses `--`, not `--url`

For illustration, this registers the Context7 process and may cause package download/execution when Codex starts it:

```bash
codex mcp add context7 -- npx -y @upstash/context7-mcp
```

Everything after the standalone `--` is the server's launch command:

```text
Codex arguments                 Server command and arguments
codex mcp add context7  --      npx -y @upstash/context7-mcp
```

`npx` runs a Node package. `-y` accepts its package-install prompt. The example follows [OpenAI's stdio registration example](https://learn.chatgpt.com/docs/extend/mcp?surface=cli); Context7's [maintainer documentation](https://github.com/upstash/context7) describes current authentication and hosted options.

For a server you wrote, use its environment's Python executable and an absolute script path:

```bash
# Example paths: replace both before running.
codex mcp add notes-demo -- \
  /absolute/path/to/lab/.venv/bin/python \
  /absolute/path/to/lab/notes_demo.py
```

This avoids ambiguity about which Python has the dependencies. A stdio server usually waits quietly if started by hand: it expects protocol messages on stdin. That behavior is explained in the [SDK running guide](https://py.sdk.modelcontextprotocol.io/run/).

## 5. Know the small command vocabulary

| Command | Purpose | Effect |
|---|---|---|
| `codex mcp list` | List saved registrations | Inspection |
| `codex mcp list --json` | Return them as JSON | Inspection |
| `codex mcp get NAME` | Inspect one entry | Inspection |
| `codex mcp get NAME --json` | Inspect it as JSON | Inspection |
| `codex mcp add NAME --url URL` | Register an HTTP endpoint | Writes configuration; may initiate auth |
| `codex mcp add NAME -- COMMAND ARGS` | Register a local process | Writes configuration |
| `codex mcp login NAME` | Authenticate with an OAuth-capable server | Starts authorization; stores credentials |
| `codex mcp logout NAME` | Remove saved OAuth credentials for that server | Changes authentication state |
| `codex mcp remove NAME` | Remove its registration | Writes configuration |

`NAME`, `URL`, `COMMAND`, and `ARGS` are placeholders. Do not type angle brackets around them. For the complete options supported by your installation, use `codex mcp SUBCOMMAND --help`. This table follows the local 0.162.1 command tree; the [official reference](https://learn.chatgpt.com/docs/developer-commands?surface=cli) is the online counterpart.

There is no `codex mcp call` or `codex mcp tools` subcommand in that checked installation. Call tools through Codex, an MCP client, or an inspection tool. `codex login` signs into Codex itself; it is separate from `codex mcp login NAME`.

## 6. Authentication has two common shapes

For an OAuth-capable remote server, after registering it:

```bash
# Example only: the server must support this authorization flow.
codex mcp login my-service
```

Follow the authorization page and verify the account and requested access. OAuth support depends on the server. A public unauthenticated server does not need this step.

For an HTTP server expecting a bearer token already available in the launching environment:

```bash
# SERVICE_MCP_TOKEN is an environment-variable name, not a token value.
codex mcp add my-service --url https://mcp.example.com/mcp \
  --bearer-token-env-var SERVICE_MCP_TOKEN
```

The URL is illustrative. The CLI stores the variable's name here. It does not create the token or guarantee that a desktop app inherits your terminal's environment. See [Codex HTTP configuration](https://learn.chatgpt.com/docs/extend/mcp?surface=cli).

For local stdio, `--env KEY=VALUE` passes an environment value to the child process. Passing a secret this way can place it in command history and saved configuration. Prefer an existing protected credential mechanism or `env_vars` forwarding where appropriate; keep secret values out of repository examples.

## 7. Understand where configuration lives

Codex documents user configuration at `~/.codex/config.toml` and trusted project configuration at `.codex/config.toml`. A server entry uses the table name `mcp_servers`. Do not paste another client's `mcpServers` JSON directly into this TOML file. [Configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference)

An illustrative local-server entry:

```toml
[mcp_servers.notes-demo]
command = "/absolute/path/to/lab/.venv/bin/python"
args = ["/absolute/path/to/lab/notes_demo.py"]
startup_timeout_sec = 20
tool_timeout_sec = 30
enabled_tools = ["read_demo_note"]
```

Use either the CLI registration or manual editing for the same entry. Review and merge settings; do not replace an existing configuration file with a short example. `enabled = false` can disable an entry while preserving it. Timeout units in these examples are seconds. [Configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference)

The checked `add --help` has no `--scope project` flag. Do not transfer Claude Code's flags to Codex. Use the documented project file when you need project scope, and verify effective configuration in the intended session.

## 8. Disconnect deliberately

To remove the first exercise's registration:

```bash
codex mcp remove openai-docs
codex mcp list
```

Removal does not uninstall a downloaded package or delete the service's data. Logout concerns saved OAuth credentials. For a change to an already running session, restart/reconnect as the host requires and inspect `/mcp` again.

You have learned the core cycle when you can explain **registration → active tools → actual result → removal**, and identify which of those four states you have verified.

[Index](README.md) · [Next: Ollama integration patterns](03-ollama-integration-patterns.md)
