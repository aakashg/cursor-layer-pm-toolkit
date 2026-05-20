# Prototype 1: Screen-Aware Assistant

A Cowork workflow that uses Claude's vision to answer questions about what's on your screen. This is the closest you can get to Clicky's core interaction using tools you already have.

## What It Does

You take a screenshot, paste it into Cowork, and ask a question about what you see. Claude reads the screenshot and answers in context, no describing required.

This is Stage 2.5: not full cursor-layer (Claude can't see your screen continuously), but it eliminates the "describe what you're looking at" step that kills most AI interactions.

## When to Use This

- Learning a new tool (DaVinci Resolve, Figma, Blender)
- Doing competitive teardowns (screenshot competitor page, ask questions)
- Debugging a UI issue (screenshot the bug, ask what's wrong)
- Reviewing a dashboard (screenshot the chart, ask for interpretation)

## Setup (5 minutes)

1. Open Cowork (or Claude.ai on desktop)
2. No special configuration needed. Claude's vision capability handles screenshots natively.

## The Workflow

### For tool learning (the DaVinci Resolve use case):

Take a screenshot of the interface you're stuck on. Paste it into Cowork with this prompt:

```
I'm looking at [TOOL NAME]. I want to [SPECIFIC TASK].

Look at this screenshot. Tell me exactly which button, menu, 
or control to click and in what order. Be specific about 
location (top-left, third icon from left, under the "Edit" 
menu, etc.).

If there are multiple steps, walk me through each one with 
the specific UI element to interact with at each step.
```

### For competitive teardowns:

Screenshot the competitor's page. Paste with:

```
This is [COMPETITOR]'s [PAGE TYPE: pricing page / feature page / 
landing page].

Answer these questions from what you see:
1. What is their primary value proposition on this page?
2. What pricing model are they using?
3. What features are they leading with?
4. What's missing that we offer?
5. What are they doing better than us visually or in copy?

Be specific. Reference exact text and layout elements you see.
```

### For dashboard interpretation:

Screenshot your dashboard. Paste with:

```
This is our [DASHBOARD NAME] showing [TIME PERIOD].

What are the 3 most important things I should notice?
What looks off or unexpected?
If I had to brief my VP in 30 seconds on this dashboard, 
what would I say?
```

## Before You Run This at Work

This workflow sends screenshots of whatever's on your screen to Claude. If those screenshots contain customer data, internal financials, unreleased UI, or anything else covered by your company's data policies, check with your security or legal team first. The pattern is sound; just pick non-sensitive surfaces for the prototype.

## Making It Faster

### macOS shortcut (recommended):
- `Cmd+Shift+4` to screenshot a region to a file, then paste into Cowork.
- `Cmd+Ctrl+Shift+4` to screenshot a region straight to clipboard. Paste directly with `Cmd+V`. This is the faster loop — no file to clean up afterwards.

### Windows shortcut:
- `Win+Shift+S` opens the snipping toolbar, captures to clipboard. Paste directly with `Ctrl+V`.

### Keyboard macro (power user):
Set up a macro that takes a screenshot and opens Cowork in one keystroke. Tools: Raycast, Alfred, or Keyboard Maestro.

### Recurring version:
For daily competitive monitoring, set up a Cowork Scheduled Task:

```
Every morning at 8 AM:
1. Open [COMPETITOR URL] in the browser
2. Take a screenshot of the page
3. Compare against yesterday's screenshot (saved in 
   ~/competitive-screenshots/)
4. If anything changed, write a one-paragraph summary 
   of what's different
5. Post to Slack channel [CHANNEL ID]
6. Save today's screenshot to ~/competitive-screenshots/
```

## Expected Output

Tool learning query on a DaVinci Resolve screenshot:

```
To add a keyframe at this point:

1. The playhead (red vertical line) is already at the right 
   position in your timeline
2. Look at the Inspector panel on the top-right. You should 
   see "Transform" with Position, Rotation, and Zoom controls
3. Click the small diamond icon to the right of "Zoom" to 
   add a keyframe at the current playhead position
4. Move the playhead to where you want the zoom to end
5. Change the Zoom value. A new keyframe is added automatically

The diamond icon is currently gray (no keyframe set). It will 
turn red once you click it.
```

## Limitations

- Not real-time. You take a screenshot, paste it, wait for response. This is a 30-second loop, not Clicky's 8-second loop.
- No pointing. Claude can describe where something is, but can't animate a triangle to it.
- No voice. You type the question. Add voice-to-text (macOS Dictation or Whisper) to get closer to the Clicky experience.
- Screenshot quality matters. Retina screenshots work best. Blurry or low-res screenshots produce worse answers.

## What This Teaches You

If this workflow saves you time (it will on competitive teardowns especially), that's signal. Your users are probably doing the same describe-then-switch loop with their own AI tools. The audit template helps you find where.
