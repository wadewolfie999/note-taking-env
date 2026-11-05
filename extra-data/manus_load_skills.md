**Send the same ZIP, with Manus-specific import instructions.** The `~/.agents/skills/engineering` destination in your selected message is for Codex. Manus provides its own skill library and supports importing skill ZIP files. [Manus skill-sharing guide](https://help.manus.im/en/articles/14753565-how-to-share-and-use-skills-in-manus)

The documented installation route is:

1. Download [engineering-v0.1.0.zip](/Users/vaheedgorgeen/Documents/Codex/2026-10-26/x20/outputs/engineering-v0.1.0.zip).
2. In Manus, open **Skills → + Add → Upload a skill**.
3. Upload the ZIP, keeping the complete bundle together.
4. In a chat, type `/` and select **engineering** from the skill list. [Manus import and invocation instructions](https://help.manus.im/en/articles/14753565-how-to-share-and-use-skills-in-manus)

If you want to attach the ZIP to a Manus conversation and send an instruction with it, use this revised message:

```text
Import the attached engineering-v0.1.0.zip as a reusable Manus
skill named engineering.

Inspect the archive first. Preserve the complete engineering
folder, including SKILL.md, all 11 reference files, the agents
directory, and the manifest.

Use Manus's native skill-import mechanism. If an engineering
skill already exists, report it before replacing it.

Verify that SKILL.md is valid, all internal references resolve,
and all 11 engineering modes are available.

Confirm whether the skill is registered in my Manus skill library
or has only been extracted for this conversation. If registration
requires a manual UI step, explain that step.

This request authorizes importing and verifying the skill only.
```

**I haven’t tested this bundle inside Manus.** Its `SKILL.md` and reference-file structure matches the format Manus documents, so it is a reasonable import candidate; successful registration and loading of its references remain to be checked there. [Manus Agent Skills format](https://manus.im/blog/manus-skills)
