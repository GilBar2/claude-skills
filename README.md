# Claude Skills

A collection of custom skills for [Claude Code](https://claude.ai/code), Claude.ai chat, and Cowork.

## Installing a skill

Copy the skill folder into `~/.claude/skills/` — Claude will pick it up automatically on the next session.

---

## Skills

### `chat-title` — Project naming convention

Start any message with `project=<name>` (e.g. `project=cute-cat`) and Claude will:
- Open every response with a `## [cute-cat]` header, so the auto-generated chat title reflects the project
- Prefix all follow-up responses with `**[cute-cat]**` to keep the session clearly scoped
- Strip the `project=` prefix before answering your actual question

Works in Claude Code, Claude.ai, and Cowork.

---

### `skip-busy-localhost-ports` — Auto-skip occupied dev server ports

Before starting any local development server, Claude checks whether the target port is already in use and skips to the next available one — so you never get a "port already in use" error mid-task.

- Default port busy? Tries 3007 → 3008 → 3009 → ...
- Always reports the actual URL it ends up using
- Never kills an existing process without being asked

Works with Next.js, Vite, Flask, Express, and any other local server.
