# Changelog

**Version:** 0.1.0 · **Previous version:** 0.0.1 · **Revised:** 2026-10-15

## 0.1.0 — 2026-10-15

Added the [Codex, MCP, and Ollama practical field guide](topics/mcp/codex-ollama/README.md) under `topics/mcp/codex-ollama/`. The eleven new Markdown files cover the mental model, `codex mcp` commands, Ollama architectures, public implementations, focused server design, practical recipes, validation, an adoption sequence, research evidence, and a primary-source register with pinned repository revisions.

The research includes official OpenAI/MCP/Ollama documentation, sixteen GitHub repository snapshots, and a GitLab-hosted learning project. It identifies archived or dated examples and separates source inspection from runtime verification. A small teaching server and companion check were exercised in memory and over stdio using Python 3.12.14 and `mcp==2.3.0`; live Ollama inference and third-party integrations remain untested.

The root index and this changelog advance from 0.0.1 to 0.1.0 for a substantial new reference collection. Each new guide document starts at 0.0.1. Existing note paths, saved conversations, prompts, and attachments retain their content and versions.

## 0.0.1 — 2026-10-15

Established the repository as an organized personal library of reference notes, reusable prompts, and material awaiting review. Replaced the root README with a grouped index and added a reading-list entry point. Applied light editorial cleanup and added context, status, and document-version metadata while preserving saved prompt instructions, authority rules, technical material, and citations.

### Path changes

| Previous path | New path |
|---|---|
| `Decomposing_ExtHiggs_MT.md` | `topics/research/exthiggs-scan-decomposition.md` |
| `Mynyra_Project_Lifecycle_and_Research_Workflow.pdf` | `topics/research/mynyra-project-lifecycle-and-research-workflow.pdf` |
| `src/ai-instruct/Decision_To_Execution_Method.md` | `topics/ai-workflows/decision-to-execution-method.md` |
| `src/ai-instruct/Pragmatic-Coder-AI-Instruction.md` | `prompts/pragmatic-coder.md` |
| `essential-docs-TDDS/local-Codex-plugin-7fold.md` | `prompts/phenora-local-engineering-plugin.md` |
| `topics/codex-tips-01` | `topics/developer-tools/codex-plugin-usage.md` |
| `topics/MCP/GPT-note-initial-temp.md` | `topics/mcp/learning-notes.md` |
| `topics/MCP/GPT-learning-roadmap.png` | `topics/mcp/learning-roadmap.png` |
| `topics/vpn-global-access.md` | `topics/connectivity/internet-access-during-restrictions.md` |
| `to-be-read/AI-Work-System-DTES-01.md` | `to-be-read/ai-work-system/01-thread-boundaries.md` |
| `to-be-read/AI-Work-System-DTES-02.md` | `to-be-read/ai-work-system/02-project-authority-and-working-copies.md` |
| `to-be-read/AI-Work-System-DTES-03.md` | `to-be-read/ai-work-system/03-decision-finalization.md` |
| `to-be-read/AI-Work-System-DTES-04.md` | `to-be-read/ai-work-system/04-work-lifecycle.md` |

The moves are clean: the old paths are removed without forwarding files. Git history retains the prior contents. The PDF and PNG were moved by retaining their original Git blob IDs, and the root LICENSE remains unchanged.

The version number describes the documentation set, not versions quoted inside saved content. For example, the Phenora prompt's embedded brief version 0.0.1 remains part of that prompt.
