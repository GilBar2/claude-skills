---
name: chat-title
description: MANDATORY trigger any time the literal string "project=" appears at or near the start of the user's message (e.g. "project=cute-cat", "project=my-app fix the bug", "project=node-writer: add dark mode"). This is a formatting convention the user has explicitly set up — they want every Claude response in this chat to be prefixed with the project name in brackets so they can find the conversation later. Even if the user's request looks trivial or you think you can answer without a skill, you MUST consult this skill any time "project=" appears in the message. Do not skip this skill because the question seems simple — the entire point is response formatting, not task complexity. Applies in Claude Code, Claude.ai, and Cowork.
---

# Chat Title — Project Naming Convention

The user has opted into a personal convention: messages beginning with `project=<name>` mean "this entire conversation is scoped to that project — please label every response accordingly so I can find it later in my chat history."

This is a **formatting rule**, not a task. It applies regardless of whether the user's question is trivial or complex. Always honor it when you see `project=` at the start of the message.

## Step 1 — Extract the project name

The project name is everything between `project=` and the first space, comma, colon, dash, or newline.

| User message | Project name | Actual task |
|---|---|---|
| `project=cute-cat` | `cute-cat` | *(no task — just setting context)* |
| `project=my-app fix the login bug` | `my-app` | `fix the login bug` |
| `project=node-writer, add dark mode` | `node-writer` | `add dark mode` |
| `project=api-svc: 500 on /login` | `api-svc` | `500 on /login` |

## Step 2 — Open your first response with a header

Begin every first response with this exact pattern on its own line, before anything else:

```
## [<project-name>]
```

Example for `project=cute-cat, help me with dark mode`:

```
## [cute-cat]

Here's how to add a dark mode toggle...
```

If the user only sent `project=cute-cat` with no task, respond with just:

```
## [cute-cat]

Project context set. What would you like to work on?
```

This header is critical — it's the part Claude Code uses to auto-generate the chat title, so it must be the very first content in your response.

## Step 3 — Prefix every follow-up response

For every subsequent message in this conversation, start with an inline tag so the project label persists:

```
**[cute-cat]** Sure — here's the updated component...
```

Keep this on every turn until the conversation ends or the user explicitly switches projects with a new `project=` message.

## What NOT to do

- Don't echo `project=cute-cat` literally — strip the prefix, just use the extracted name in brackets.
- Don't skip the header on the first response because the task seems too small.
- Don't drop the prefix on later messages — the user relies on it for searching their history.
- Don't ask "did you mean project X?" — just extract the name verbatim and proceed.

## Why this skill matters

Claude Code, Claude.ai, and Cowork all auto-generate chat titles from the first response. By putting `[project-name]` at the top, the title reliably reflects which project the chat belongs to. The follow-up `**[project-name]**` prefixes turn the chat into a searchable, labeled artifact — the user can grep their history for `[cute-cat]` and find every related session.
