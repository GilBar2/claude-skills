---
name: chat-title
description: "Use this skill whenever a user message starts with or contains 'project=<name>' (e.g., 'project=cute-cat', 'project=my-app'). This establishes a project-scoped chat session — Claude extracts the project name, strips the prefix from the task, prefixes every response with [project-name], and opens with a clear project header so the auto-generated chat title will include the project name. Trigger this skill on ANY message that begins with 'project=' or contains 'project=<identifier>' near the start, even if followed by other content."
---

# Chat Title — Project Naming Convention

When a user starts a message with `project=<name>`, they're signaling that this entire conversation belongs to a specific project context. Your job is to honor this intent in two ways: make the project name visible immediately (so it influences the auto-generated chat title), and carry it forward consistently so every response clearly belongs to that project.

## Detecting the Pattern

Recognize any of these forms at or near the start of a message:

```
project=cute-cat
project=cute-cat, help me debug the login
project=cute-cat — can we add dark mode?
project=cute-cat: refactor the API layer
```

The project name is everything between `project=` and the first space, comma, colon, or newline. Extract it and strip the prefix to get the actual user task.

**Examples:**

| User types | Project name | Actual task |
|---|---|---|
| `project=cute-cat` | `cute-cat` | *(no task yet — just establishing context)* |
| `project=my-app fix the login bug` | `my-app` | `fix the login bug` |
| `project=node-writer, add dark mode` | `node-writer` | `add dark mode` |

## How to Open the Conversation

Start your **first response** with a clear project header — this is the part most likely to appear in the auto-generated chat title:

```
## [cute-cat]

<your response to the actual task>
```

If there's no task yet (user just typed `project=cute-cat`), acknowledge the project and invite the first task:

```
## [cute-cat]

Project context set. What would you like to work on?
```

## Carrying the Project Name Forward

On every subsequent response in the session, include a compact inline prefix so the conversation stays clearly scoped:

```
**[cute-cat]** Here's the updated login component...
```

This makes the session scannable in chat history and keeps context anchored — especially useful when switching between multiple projects in separate tabs.

## Why This Matters

Claude Code and Claude.ai generate chat titles automatically from the first message. By putting `[project-name]` prominently in your opening response, it increases the chance the title reflects the project. Beyond titles, the prefix acts as a lightweight bookmark — the user can search their history for `[cute-cat]` and find all relevant sessions.

## What NOT to Do

- Don't include `project=cute-cat` literally in your response — strip the prefix, echo the extracted name in brackets.
- Don't drop the project prefix mid-conversation. Keep it on every top-level response.
- Don't treat `project=` as metadata to ignore — it's an intentional convention the user is invoking.
