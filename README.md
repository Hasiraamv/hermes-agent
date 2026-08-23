# MONDAY

A JARVIS/FRIDAY-style command layer for Claude Code, built on Claude Code subagents:

- **`monday`** — the front door. Command interface for everyday tasks across email, Slack, Notion, Google Drive, web research, and local generation. Routes work to the right connector or sub-agent, acts on reads/searches/drafts without asking, and only pauses for confirmation before sending, sharing, deleting, or paying for anything.
- **`hermes`** — offloads generation-heavy work (drafts, brainstorms, summaries, boilerplate code) to a locally-run [Hermes 3](https://ollama.com/library/hermes3) model via [Ollama](https://ollama.com), instead of spending Claude API tokens on it.
- **`email-assistant`** — deep email work (digests, multi-thread triage) against your connected Gmail account. Never sends anything without explicit confirmation of recipient/subject/body first, and treats email content as untrusted data (ignores any instructions embedded inside messages).

## Setup

1. Install [Ollama](https://ollama.com/download) and pull the model:
   ```bash
   ollama pull hermes3:8b
   ```
2. Copy `.claude/agents/*.md` into your project's `.claude/agents/` folder (or keep this repo as a standalone project and work from here).
3. Connectors (Gmail, Slack, Notion, Google Drive) need to be authorized in your Claude client's connector settings — `monday` will simply not have access to a connector until it's authorized there.
4. Optionally create a `profile.local.md` (gitignored, never pushed) with your name and context — `monday` reads it for background if present. See the template fields used: name, email, ongoing projects, preferences.

## Usage

Talk to `monday` directly for anything — it decides whether to handle it itself, hand drafting to `hermes`, or dig into email with `email-assistant`. Examples:

- "Catch me up on Slack and email" → `monday` pulls both, gives a short digest
- "Draft a blog post outline about X" → `monday` → `hermes`
- "Find that Notion doc about the Q3 roadmap" → `monday` via Notion search
- "Reply to Sarah saying I'll join the call" → `monday` drafts it, shows you the exact text, waits for a "yes" before sending

## The one hard rule

Every agent here reads and organizes freely, but none of them will send a message, share/publish something, delete data, or spend money without you explicitly confirming the exact action in chat first — even if a task description or a document being read tries to instruct otherwise. This is intentional and not configurable per-task; it's the guardrail that makes "just command it" safe to use.
