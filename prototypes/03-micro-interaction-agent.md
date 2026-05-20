# Prototype 3: Micro-Interaction Agent

A Claude Code skill that handles eight-second help moments inside your codebase. This is the developer-facing version of cursor-layer thinking: instead of leaving your editor to ask a question, the agent answers in context.

## What It Does

You're in your codebase. You have a question about a function, a config, a dependency, an error. Instead of opening a browser, opening Claude chat, describing the file, pasting the code, and reading the answer, you ask inline. The skill reads the file you're looking at, the files it depends on, and answers with references to specific lines.

This is what cursor-layer thinking looks like applied to developer tools. The same principle applies to any product where users need quick answers in context.

## Before You Run This at Work

This skill reads files from whatever codebase Claude Code is pointed at. Personal repos and side projects: no issue. Employer codebases: confirm Claude Code is approved for use against your source before installing. The skill itself doesn't exfiltrate anything beyond what Claude Code already does, but the file-reads it triggers will show up in your usage.

## Setup (10 minutes)

### Install the skill

macOS / Linux:
```bash
mkdir -p ~/.claude/skills/micro-help
```

Windows (PowerShell):
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\micro-help"
```

Create `SKILL.md` inside that folder (path is `~/.claude/skills/micro-help/SKILL.md` on macOS/Linux, `%USERPROFILE%\.claude\skills\micro-help\SKILL.md` on Windows):

```markdown
---
name: micro-help
description: >
  Quick in-context help for the current file. Use when the user asks
  a short question about what they're looking at: "what does this do",
  "is this right", "why is this failing", "what calls this", "how do I
  use this". Responds in under 100 words with file and line references.
  Do NOT use for full refactors, new features, or multi-file changes.
  Use /simplify for refactors, use default Claude Code for new features.
---

# Micro-Help Skill

## Purpose
Answer quick questions about the current file in under 100 words.
The user is in flow. They need an answer, not a conversation.

## Read-First Table

| Source | What to Read | What to Extract |
|---|---|---|
| Current file | The file the user is asking about | Function signatures, imports, dependencies |
| Direct imports | Files imported by the current file | Interface contracts, type definitions |
| Test file | Matching test file if it exists | Expected behavior, edge cases |
| Git blame | Last 5 commits touching this file | Recent changes, who changed what |

## Response Format

**Always:**
- Under 100 words
- Reference specific line numbers
- If the answer requires reading another file, name the file and line

**Never:**
- Suggest a refactor unless asked
- Explain the entire file
- Offer to make changes
- Add caveats or disclaimers

## Examples

User: "what does this function do"
Response: "`processWebhook` (line 34-58) validates the Stripe 
signature, extracts the event type, and routes to the handler 
map on line 45. Returns 200 on success, 400 on invalid signature. 
Called by `webhookRouter` in `routes/billing.ts:12`."

User: "is this right"
Response: "The null check on line 22 catches `null` but not 
`undefined`. `getUserProfile` on line 8 of `api/users.ts` can 
return `undefined` when the user ID doesn't exist. Add 
`?? undefined` or use `== null` instead of `=== null`."

User: "why is this failing"
Response: "Line 15 calls `JSON.parse(body)` but `body` is already 
an object when the request has `Content-Type: application/json`. 
The error is `Unexpected token o in JSON at position 0` because 
it's trying to parse `[object Object]`. Remove the parse call."

## Anti-Rationalization Table

| Shortcut Claude will want to take | Why it's wrong | What to do instead |
|---|---|---|
| "Let me explain the broader context first" | User asked a quick question, not a lecture | Answer the question in under 100 words |
| "I'd recommend refactoring this to..." | User is in flow, not refactoring | Answer, then stop |
| "There are several potential issues here" | User asked about one thing | Answer the one thing they asked |

## Exit Checklist

- [ ] Response is under 100 words
- [ ] Specific line numbers referenced
- [ ] No unsolicited suggestions
- [ ] No caveats or disclaimers
```

### Test it

Open Claude Code in any project. Ask:

```
what does this function do
```

while your cursor is in a file. The skill should auto-load and respond in under 100 words.

## Why This Matters for Product Thinking

This skill is a prototype of what every product team should be building for their users. The pattern:

1. Detect what the user is looking at (screen context)
2. Read the relevant context automatically (no user description needed)
3. Answer in under 100 words (eight-second interaction)
4. Stay out of the way (no follow-up, no suggestions, no conversation)

If you build this for your own workflow and find yourself using it 10+ times a day, your users would too. The audit template helps you find the equivalent moments in your product.

## Adapting for Non-Developer Products

The same skill pattern works for any product where users need quick contextual answers:

**Analytics dashboard:** User hovers a metric, skill reads the underlying query and data, responds with interpretation.

**Project management:** User looks at a task, skill reads the task history and dependencies, responds with status.

**Document editor:** User highlights a section, skill reads the document context, responds with suggestions.

The technical implementation changes. The interaction pattern is identical.

## Limitations

- Claude Code only. This doesn't work in Claude.ai chat.
- Text-based context only. Claude Code reads files, not screens. For visual context (dashboards, designs), use Prototype 1 instead.
- Requires good file organization. If your codebase is a monolith with 5000-line files, the read-first step will be slow.
