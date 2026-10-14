# Using an Exported Plugin from a New Codex Chat

**Version:** 0.0.1 *(document metadata)* · **Revised:** 2026-10-15 · **Status:** Saved explanation

This is a saved explanation about using or installing an extracted Codex plugin. Its linked official documentation is retained; check those sources and the current CLI before following version-sensitive steps.

**Yes—you can extract the ZIP into a directory and ask a new Codex chat to use it.** There are two ways to do that, depending on whether you want to use its instructions immediately or have it appear as an installed plugin.

**For immediate use in that chat**, you can say:

> Read the plugin manifest, skill instructions, and relevant reference files in `./plugins/node-ops/`. Use them to answer my Node Operations questions in this chat.

Codex can read those accessible files and use their instructions and technical context. For an instruction-and-reference plugin, this is a practical way to use the extracted package. Reading the files does not itself register the plugin in the host, add it to the `@` menu, or connect any bundled MCP tools. Those are separate parts of plugin installation. [Official plugin structure documentation](https://developers.openai.com/plugins/build/plugins)

**For installation and selectable invocation**, you can also ask a Codex chat to set that up:

> Inspect the extracted plugin at `./plugins/node-ops/` and register it as a local plugin for this project. Preserve existing marketplace entries and configuration. Verify that it is discoverable, then tell me how to install and invoke it.

The supported local flow is:

1. Extract the complete plugin directory, including hidden files.
2. Add a local marketplace entry pointing to that directory.
3. Refresh or restart the app as needed, then install the plugin from that marketplace.
4. Start a new chat to use the installed plugin.

The extracted source folder can live in your chosen location; the marketplace points to it. Codex loads an installed copy from its plugin cache, so later source edits need a refresh or reinstall to take effect. [Official local installation documentation](https://developers.openai.com/plugins/build/plugins)

For your intended use, **you do not need to copy and extract it into every project**. A personal marketplace installation can make it available across your local projects. A repository marketplace is useful when you want a particular project to carry its own plugin setup. [Personal and repository marketplace documentation](https://developers.openai.com/plugins/build/plugins)

So your proposed workflow is possible: **extract it, open a Codex chat, and tell Codex either “use these instructions here” or “register this as a local plugin.”** The second is the route to persistent plugin discovery and invocation.
