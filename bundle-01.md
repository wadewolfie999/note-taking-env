**Send your friend the ZIP, then have Codex install the extracted `engineering` folder on their laptop.** The bundle includes all 11 workflows and needs no additional packages, API keys, or original skills.

1. Download and transfer this file:

   **[engineering-v0.1.0.zip](/Users/vaheedgorgeen/Documents/Codex/2026-10-26/x20/outputs/engineering-v0.1.0.zip)**

   Your friend can save it in their **Downloads** folder.

2. Have them open a local chat in Codex and paste this:

   ```text
   Install the local engineering skill from
   engineering-v0.1.0.zip in my Downloads folder.

   Inspect the archive first. Install the complete engineering
   folder, including its references, agents directory, and manifest,
   into my user-level skills directory.

   Use ~/.agents/skills/engineering unless this Codex installation
   uses a different configured location. Do not overwrite an
   existing engineering skill; report it if one exists.

   Verify that SKILL.md is valid and all internal references resolve.
   This request authorizes installing the skill only.
   ```

   Current OpenAI documentation lists **`~/.agents/skills`** as the user-level location for skills available across projects. Your Mac’s existing installation uses `~/.codex/skills`; the prompt allows their Codex to account for its own setup. [Official local-skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)

3. Check that Codex recognizes it. In a new message, they can write:

   ```text
   Find the installed $engineering skill, read its entry point,
   and list its available modes. Do not start engineering work yet.
   ```

   It should identify the **11 modes**. Codex normally detects newly installed skills automatically; if it does not appear, restart Codex and try again. [Official installation guidance](https://learn.chatgpt.com/docs/build-skills#install-curated-skills-for-local-use)

4. Start using it:

   ```text
   Use $engineering in red-team mode to review this plan:
   [paste the plan]
   ```

Keep the entire extracted folder together. The expected layout is:

```text
engineering/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   └── [11 mode files]
└── bundle-manifest.json
```

I rechecked the **v0.1.0 ZIP**: its integrity check passed and all 14 package files are present. Installation and discovery on your friend’s laptop still need to be verified there.
