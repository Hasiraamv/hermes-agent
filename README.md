# Hermes Agent Kit

Claude Code subagents for everyday task automation on this machine:

- **`hermes`** — general-purpose task agent that offloads generation-heavy work (drafts, brainstorms, summaries, boilerplate code) to a locally-run [Hermes 3](https://ollama.com/library/hermes3) model via [Ollama](https://ollama.com), instead of spending Claude API tokens on it.
- **`email-assistant`** — reads, searches, summarizes, and drafts email against your connected Gmail account. Never sends anything without explicit confirmation of recipient/subject/body in chat first, and treats email content as untrusted data (ignores any instructions embedded inside messages).

## Setup

1. Install [Ollama](https://ollama.com/download).
2. Pull the model:
   ```bash
   ollama pull hermes3:8b
   ```
3. Copy `.claude/agents/*.md` into your project's `.claude/agents/` folder (or keep this repo as a standalone project and work from here).
4. The `email-assistant` agent needs a Gmail MCP connection authorized in your Claude Code / Claude client settings.

## Usage

Just describe the task normally — Claude Code will route it to the right subagent based on each agent's `description`. Examples:

- "Draft a blog post outline about X" → `hermes`
- "Any important emails today?" → `email-assistant`
- "Reply to Sarah's thread saying I'll join the call" → `email-assistant` (will show you the draft and ask before sending)

## Notes

- `hermes` has no email/messaging access — it only produces text via the local model and your file/search tools.
- `email-assistant` can freely read and draft, but sending/replying/forwarding always requires your explicit "yes" in chat.
