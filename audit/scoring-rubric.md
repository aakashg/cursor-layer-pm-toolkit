# Cursor Layer Scoring Rubric

Score every AI feature on your roadmap. Place it at Stage 1, 2, or 3. The gap between current stage and target stage is your opportunity.

**Higher stages are not always better.** Some features belong at Stage 1 — deep research, long-form drafting, anything where the user wants a dedicated environment and a slow, thoughtful answer. The rubric helps you find features stuck at the *wrong* stage, not push everything to Stage 3. Set a target stage feature-by-feature, not as a blanket goal.

## Stage 1: Destination

The user leaves their work to go to the AI.

Diagnostic questions (if 2+ are yes, you're at Stage 1):
- Does the user open a separate tab, window, or app to use this feature?
- Does the user have to describe what they're looking at to the AI?
- Does the user lose their place when they come back?
- Does the user have to re-establish context every session?

Examples: ChatGPT in a browser tab, Claude chat for a code question, a standalone AI app

What it costs: Context lost every time. Users save questions for later instead of asking now. Frequency stays low because the switching cost is high.

## Stage 2: Embedded

The AI lives inside the product but the user still manages the interaction.

Diagnostic questions (if 2+ are yes, you're at Stage 2):
- Is the AI accessible without leaving the product (sidebar, panel, inline)?
- Does the user still have to select context, mode, or agent type?
- Does the user still type or paste input rather than the AI reading the screen?
- Does the AI respond in a separate panel rather than inline where the user is working?

Examples: Copilot in VS Code, Notion AI, GitHub Copilot suggestions, AI sidebars

What it costs: Better than Stage 1, but still has friction. Users still manage model selection, context, and thread state. The AI is closer but the user is still the router.

## Stage 3: Cursor Layer

The AI sees what the user sees. The user speaks or gestures. The AI responds where the user already is.

Diagnostic questions (if 3+ are yes, you're at Stage 3):
- Does the AI see the user's screen in real time?
- Can the user interact by voice or gesture without typing?
- Does the AI respond inline (pointing, highlighting, acting) rather than in a separate panel?
- Does the system route to the right agent without the user selecting one?
- Can the AI hand off from guiding to executing without a mode switch?

Examples: Clicky, Google Magic Pointer, (hypothetical) a Figma plugin that watches your design and answers spatial questions

What it costs: Zero context switching. Frequency increases because the interaction is eight seconds, not two minutes. Users ask questions they would have skipped before.

## How to Score

For each AI feature on your roadmap:

1. Answer the diagnostic questions for each stage
2. Place the feature at the highest stage where 2+ diagnostics are "yes"
3. Decide the target stage (where should this feature be in 6 months?)
4. Calculate the gap (target minus current)
5. Estimate effort (Low: 1 sprint, Medium: 1-2 quarters, High: 3+ quarters)

## Prioritization

Score = (Frequency per user per week) × (Time saved per interaction in minutes) × (Stage gap) ÷ Effort

Where Effort is: **1** (Low, 1 sprint), **3** (Medium, 1-2 quarters), **9** (High, 3+ quarters).

The effort divisor matters. Without it, the formula will always pick the highest-frequency item even when it's a six-quarter rebuild. With it, a Stage 1→2 sidebar shipped this sprint can outrank a Stage 1→3 rewrite that ships next year.

Sort descending. The top 3 items are your cursor-layer roadmap for the next two quarters.

### Worked example

A dependency explainer used 12×/week, saving 2 minutes per interaction, moving Stage 1 → Stage 3 (gap 2), High effort:
`12 × 2 × 2 ÷ 9 = 5.3`

A status update generator used 3×/week, saving 4 minutes, moving Stage 1 → Stage 2 (gap 1), Low effort:
`3 × 4 × 1 ÷ 1 = 12.0`

The status generator outranks the dependency explainer this quarter — same idea, ships sooner. Revisit the dependency rewrite next planning cycle.

## Template

| Feature | Current Stage | Target Stage | Gap | Freq/week | Time saved (min) | Effort (1/3/9) | Score |
|---|---|---|---|---|---|---|---|
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |
