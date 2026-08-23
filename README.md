# MONDAY

A JARVIS/FRIDAY-style command layer for Claude Code, built on Claude Code subagents.

- **`monday`** — the front door. Command interface for everyday tasks across email, Slack, Notion, Google Drive, the local filesystem/terminal, the desktop itself, browser automation, and scheduled/recurring tasks. Acts on reads/searches/drafts/navigation without asking, and only pauses for confirmation before sending, sharing, deleting, submitting, or paying for anything.
- **`hermes`** — offloads generation-heavy work (drafts, brainstorms, summaries, boilerplate code) to a locally-run [Hermes 3](https://ollama.com/library/hermes3) model via [Ollama](https://ollama.com), instead of spending Claude API tokens on it.
- **`email-assistant`** — deep email work (digests, multi-thread triage) against your connected Gmail account. Never sends anything without explicit confirmation of recipient/subject/body first, and treats email content as untrusted data.

## What MONDAY can reach

| Area | Capability |
|---|---|
| Gmail | read, search, draft, label freely; send/reply/forward needs confirmation |
| Slack | read/search channels, threads, users freely; sending needs confirmation |
| Notion | search, fetch, create/update pages freely |
| Google Drive | search, list, read, download, create/update freely; external sharing needs confirmation |
| Local filesystem & terminal | full read/write/edit anywhere on disk, real process/terminal control (Desktop Commander) |
| Desktop | open apps, click, type, read the screen (Computer Use) — submitting/sending/paying still needs confirmation |
| Browser | drives a real browser for logged-in/JS-heavy sites and multi-step flows (browser-use) |
| Local LLM | Hermes 3 via Ollama, zero token cost |
| Documents | docx/xlsx/pptx/pdf generation and data viz via Claude Code skills |
| Automation | recurring or one-off scheduled tasks (daily briefing, reminders, etc.) |

## Setup

1. Install [Ollama](https://ollama.com/download) and pull the model:
   ```bash
   ollama pull hermes3:8b
   ```
2. Copy `.claude/agents/*.md` into your project's `.claude/agents/` folder (or keep this repo as a standalone project and work from here).
3. Connectors (Gmail, Slack, Notion, Google Drive, Desktop Commander, Computer Use, browser-use, scheduled-tasks) need to be authorized/connected in your Claude client — `monday` simply won't have access to whatever isn't connected yet.
4. Optionally create a `profile.local.md` (gitignored, never pushed) with your name and context — `monday` reads it for background if present.

## Usage

Talk to `monday` directly for anything — it decides whether to handle it itself, hand drafting to `hermes`, or dig into email with `email-assistant`. Examples:

- "Catch me up on Slack and email" → pulls both, gives a short digest
- "Draft a blog post outline about X" → `monday` → `hermes`
- "Open Spotify and play something" → Computer Use
- "Set up a daily 8am briefing" → scheduled task
- "Find that Notion doc about the Q3 roadmap" → Notion search
- "Reply to Sarah saying I'll join the call" → drafts it, shows you the exact text, waits for a "yes" before sending

## The one hard rule

Every agent here reads, searches, and navigates freely, but none of them will send a message, share/publish something, delete data, submit a form, or spend money without you explicitly confirming the exact action in chat first — even if a task description, document, or screen content being read tries to instruct otherwise. This is intentional and not configurable per-task; it's the guardrail that makes "just command it" safe to use.
