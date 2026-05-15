---
name: chat-title
description: MANDATORY trigger any time the literal string "project=" appears at or near the start of the user's message (e.g. "project=cute-cat", "project=my-app fix the bug", "project=node-writer: add dark mode"). This is a formatting convention the user has explicitly set up — they want the sidebar chat title to START with the project name in brackets so they can scan their history. To achieve that, you MUST begin your very first response with the literal text "[project-name]" as the first characters of your reply, no markdown header, no preamble. Even if the user's request looks trivial or you think you can answer without a skill, you MUST consult this skill any time "project=" appears in the message. Applies in Claude Code, Claude.ai, and Cowork.
---

# Chat Title — Project Naming Convention

The user has opted into a personal convention: when a message begins with `project=<name>`, they want the **sidebar chat title to start with `[<name>]`** so they can find the conversation later.

Claude Code's title generator builds the chat title from the first user message + first assistant response. To bias it strongly toward the project name, your reply must **lead with `[<name>]` as the very first characters** — no `##` heading, no greeting, no preamble. The bracketed name must be the first thing the title generator sees.

This is a **formatting rule**, not a task. It applies regardless of whether the user's question is trivial or complex.

## Step 1 — Extract the project name

The project name is everything between `project=` and the first space, comma, colon, dash, or newline.

| User message | Project name | Actual task |
|---|---|---|
| `project=cute-cat` | `cute-cat` | *(no task — just setting context)* |
| `project=my-app fix the login bug` | `my-app` | `fix the login bug` |
| `project=node-writer, add dark mode` | `node-writer` | `add dark mode` |
| `project=api-svc: 500 on /login` | `api-svc` | `500 on /login` |

## Step 2 — Open the response with the bracketed name as plain text

The very first characters of your response **must** be `[<name>]` — no `#`, no `##`, no bolding, no emoji. Plain brackets, plain name. Then a blank line. Then the actual response content.

**Correct (for `project=cute-cat, help me with dark mode`):**

```
[cute-cat]

Here's how to add a dark mode toggle...
```

**Incorrect (do NOT do these):**

```
## [cute-cat]            ← markdown header dilutes the title signal
**[cute-cat]** Here's... ← bold + inline mixes label into prose
Sure! [cute-cat] ...     ← any text before the bracket breaks the title bias
```

If the user only sent `project=<name>` with no task, your entire response should be:

```
[cute-cat]

Project context set. What would you like to work on?
```

That bare format gives the title generator almost nothing to work with except `[cute-cat]`, which is exactly what the user wants in the sidebar.

## Step 3 — Prefix every follow-up response

For every subsequent message in this conversation, still start with `[<name>]` on its own line, then a blank line, then the response. Keep this on every turn until the conversation ends or the user explicitly switches projects with a new `project=` message.

```
[cute-cat]

Sure — here's the updated component...
```

## What NOT to do

- Don't use `## [name]` or `# [name]` — markdown headers weaken the title signal.
- Don't write anything before the `[name]` — not "Sure!", not "Hi", not the user's name. The bracket must be character #1.
- Don't echo `project=cute-cat` literally — strip the prefix, just put the extracted name in brackets.
- Don't drop the prefix on later messages — the user relies on it for searching their history.
- Don't ask "did you mean project X?" — just extract the name verbatim and proceed.

## Why this matters

Claude Code's sidebar title is auto-generated from the first exchange. When your reply begins with `[cute-cat]` as raw text, the title generator has the bracketed name as the strongest, earliest signal — and the resulting sidebar title typically becomes `[cute-cat]` or `[cute-cat] <short summary>`. Any other content before the bracket (greetings, "sure!", headers) dilutes that signal and the generator falls back to summarizing the user's question instead.
