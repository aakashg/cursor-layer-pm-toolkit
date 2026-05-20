# Worked Example: Cursor Layer Audit for TaskFlow

TaskFlow is a fictional B2B project management tool with 50K users. They have an AI sidebar (Stage 2) that can summarize tasks, generate status updates, and answer questions about projects. Here's what their audit revealed.

## Context Switch Map (Top 5 by frequency x time cost)

| Exit Point | Destination | Freq/week | Time (min) | Cursor-Layer? | Priority |
|---|---|---|---|---|---|
| PM checks competitor's new feature | Chrome tab to competitor site | 4 | 6 | Yes | High |
| User asks AI to explain a Gantt dependency | Opens AI sidebar, describes what they're looking at | 12 | 2 | Yes | High |
| User searches Slack for stakeholder decision | Slack search | 8 | 4 | Yes | High |
| User copies task list to email for status update | Gmail compose | 3 | 8 | Yes | Medium |
| User checks docs for keyboard shortcut | Help center tab | 6 | 1.5 | Yes | Medium |

**What this revealed:** The highest-frequency exit (12x/week) was users leaving the Gantt view to describe what they're looking at to the AI sidebar. The AI is 40 pixels away and users still lose context because they have to describe what they see. That's a Stage 1 interaction hiding inside a Stage 2 product.

## Overhead Inventory (Top 5)

| Overhead Item | Category | Frequency | Time/instance | Removable? | Difficulty |
|---|---|---|---|---|---|
| Selecting "Project Context" before asking AI | Context Selection | Every AI query | 15 sec | Yes | Easy |
| Remembering which view has the data they need | Navigation Memory | Daily | 45 sec | Yes | Medium |
| Switching between "Ask AI" and "Generate" modes | Mode Selection | Every AI query | 10 sec | Yes | Easy |
| Re-explaining project context when starting new AI thread | Context Reset | 3x/day | 90 sec | Yes | Medium |
| Choosing between AI summary types | Format Selection | 2x/day | 20 sec | Yes | Easy |

**What this revealed:** Users are managing 5 forms of overhead on every AI interaction. Total: roughly 4 minutes per day of pure friction. The AI sidebar forces users to select context, select mode, and re-explain their situation. A cursor-layer approach would absorb all five automatically.

## Eight-Second Opportunity Log (Top 5)

| Micro-Interaction | Current Workflow | Time Cost | Eight-Second Version | Freq/day | Value |
|---|---|---|---|---|---|
| "What's blocking this task?" | Click task > read thread > click linked tasks > piece together | 3 min | Point at task, ask "what's blocking this?" | 6 | High |
| "Is this timeline realistic?" | Open resource view > cross-reference capacity > estimate | 4 min | Point at timeline, ask "is this realistic given current capacity?" | 3 | High |
| "Summarize this thread for my update" | Copy thread > paste in AI > wait > edit | 2 min | Look at thread, say "summarize for my stakeholder update" | 4 | High |
| "What did Sarah say about this feature?" | Search Slack > find thread > read context | 4 min | Ask "what did Sarah say about this?" while looking at the feature | 2 | Medium |
| "Add this to next sprint" | Navigate to backlog > create task > fill fields > assign | 3 min | Say "add this to next sprint, assign to Mike" | 3 | Medium |

**What this revealed:** The six daily "what's blocking this?" queries alone cost 18 minutes per user per day. That's 90 minutes per week of a PM's time spent on a question the system already has the answer to. The AI sidebar can answer it, but users have to leave the Gantt view, select the project, type the question, and navigate back. The cursor-layer version: point at the task and ask.

## Scoring Results

Score = Freq/week × Time saved (min) × Stage gap ÷ Effort (Low=1, Med=3, High=9)

| Feature | Current → Target | Gap | Freq/wk | Time saved | Effort | **Score** |
|---|---|---|---|---|---|---|
| Status update generator | Stage 1 → Stage 2 | 1 | 3 | 8 min | Low (1) | **24.0** |
| AI task summarizer | Stage 2 → Stage 3 | 1 | 8 | 2 min | Med (3) | **5.3** |
| Dependency explainer | Stage 1 → Stage 3 | 2 | 12 | 2 min | High (9) | **5.3** |
| Meeting action item extractor | Stage 1 → Stage 2 | 1 | 4 | 5 min | Med (3) | **6.7** |
| Competitive monitoring | Stage 1 → Stage 3 | 2 | 4 | 6 min | High (9) | **5.3** |

## The Recommendation

**This quarter: ship the status update generator.** Score 24.0 — it doesn't have the highest frequency, but it's a 1-sprint Stage 1 → Stage 2 move that frees up real time on every weekly cycle. Easy win, fast validation.

**Next quarter: ship the dependency explainer.** Score 5.3 looks low, but the raw user impact is the biggest in the list (24 minutes saved per user per week, 6 hours per week for a 15-PM team). The Effort=9 dragged the score down; treat that as a signal to break it into phases. Prototype it as Cowork first (`/prototypes/01-screen-aware-assistant.md`) to validate before committing to the native build.

**Park for now:** Competitive monitoring and AI task summarizer score the same as dependency explainer but with less raw impact. Revisit next planning cycle.

This is the difference the effort divisor makes. Without it, the dependency explainer's 12×/week frequency would win automatically and the team would commit to a six-quarter rewrite. With it, the audit surfaces the boring 1-sprint win that ships first.
