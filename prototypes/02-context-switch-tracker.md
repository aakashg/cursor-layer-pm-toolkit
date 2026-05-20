# Prototype 2: Context-Switch Tracker

A Claude Routine that monitors public signals (support tickets, forum posts, user feedback) to find where your users leave your product to get help elsewhere. This fills out the Context Switch Map in the audit template automatically.

## What It Does

Every Monday morning, this Routine searches Reddit, community forums, and support channels for moments where your users describe leaving your product to use another tool for a quick answer. It categorizes those exits and posts a ranked report to Slack.

This is how you discover your Stage 1 moments from user language instead of guessing.

## Before You Run This at Work

This Routine only reads public posts (Reddit, X, Product Hunt, G2) and writes a summary to a Slack channel you choose. No internal data leaves your machine. Safe to run on a corporate Claude account. The only judgment call: which Slack channel receives the report. Pick a private PM channel rather than #general — early-week reports can read like criticism of your own product.

## Setup (15 minutes)

### Prerequisites
- Claude Pro, Max, Team, or Enterprise (for Routines)
- Slack connected in Connectors

### Create the Routine

Open Claude on the web and find Routines under the Automations area of your account (exact menu path varies as Claude's UI evolves; search "Routines" in the help menu if you don't see it). If Routines isn't available on your tier yet, you can still run this as a manual weekly prompt — just set a calendar reminder.

Name: "Context Switch Tracker"
Trigger: Schedule, weekly on Monday at 7:00 AM
Connectors: Keep Slack. Remove everything else.

Paste this into the description box:

```
You are tracking where users of [YOUR PRODUCT] leave the product 
to get help or complete a task elsewhere.

Every time you run, do the following in order:

1. Search Reddit, Product Hunt, G2, and X for posts from the last 
   7 days where users mention [YOUR PRODUCT] alongside another tool 
   they switched to. Look for language like:
   - "I had to open [other tool] to..."
   - "I ended up using [other tool] because [YOUR PRODUCT] couldn't..."
   - "My workflow is [YOUR PRODUCT] then [other tool] then back"
   - "I wish [YOUR PRODUCT] could just..."
   - "copy paste into ChatGPT/Claude/Gemini"
   - "switched to [tool] for this part"
   
   Focus on posts with 5+ upvotes or 3+ replies.

2. For each context switch you find, extract:
   - What the user was doing in [YOUR PRODUCT]
   - Where they went (which tool/app)
   - Why they left (what was missing)
   - How long the switch likely took (estimate from their description)
   - Exact quote from the user

3. Categorize each switch into one of:
   - AI_QUERY: left to ask an AI something
   - TOOL_GAP: left because a feature doesn't exist
   - DATA_LOOKUP: left to find information
   - COMMUNICATION: left to message someone about what they found
   - EXPORT: left to move data to another format

4. Rank by frequency (how many users described the same switch).

5. Post to Slack channel [CHANNEL ID]:

   **Context Switch Report - Week of [date]**
   
   **Top Exits (ranked by frequency)**
   1. [What they were doing] -> [Where they went] | [Category]
      Why: [one sentence]
      Count: [N mentions]
      Quote: "[best user quote]"
   
   (all exits with 2+ mentions)
   
   **New This Week**
   [Any exit pattern not seen in previous reports]
   
   **Platforms scanned:** Reddit, Product Hunt, G2, X
   **Posts analyzed:** [N]

6. If fewer than 3 context switches are found, post:
   "Low signal this week - [N] context switches found. Consider 
   broadening search to adjacent product categories."
```

### Run It

Click Run Now. Check the Slack output. Adjust the product name and search terms if results are too broad or too narrow.

## Expected Output (Week 3)

```
Context Switch Report - Week of May 19

Top Exits (ranked by frequency)

1. Reviewing analytics dashboard -> ChatGPT | AI_QUERY
   Why: Users screenshot their dashboard and ask ChatGPT 
   to interpret the data because in-app AI can't see charts
   Count: 7 mentions
   Quote: "I literally screenshot my TaskFlow dashboard every 
   morning and paste it into Claude to ask what changed"

2. Writing status update -> Google Docs | EXPORT
   Why: Users copy task lists into a doc to write prose updates
   Count: 5 mentions
   Quote: "my weekly update workflow is: export tasks, open doc, 
   write update, copy back. why can't this just happen in TaskFlow"

3. Checking competitor feature -> Chrome tabs | DATA_LOOKUP
   Why: Users leave mid-planning to check what competitors offer
   Count: 4 mentions
   Quote: "every sprint planning I have 6 competitor tabs open 
   next to TaskFlow trying to figure out what they shipped"

New This Week:
Users describing "clipboard relay" pattern: copying data from 
TaskFlow to ChatGPT to Slack in a 3-step chain. Not seen in 
prior weeks. Possible emerging frustration.

Platforms scanned: Reddit, Product Hunt, G2, X
Posts analyzed: 142
```

## What to Do With the Output

Each exit in the report maps directly to a row in the Context Switch Map tab of the audit template. After 4 weeks of reports, you have a data-driven map of where your users are being failed by your current AI integration.

The "AI_QUERY" category is your highest-signal finding. Every time a user screenshots your product and pastes it into a separate AI tool, that's a cursor-layer opportunity sitting in plain sight.

## Limitations

- Only finds publicly discussed context switches. Internal frustration (Slack complaints, support tickets) won't appear. Pair with Managed Agent Use Case 1 (support ticket analysis) from the Automation Layer post for internal signal.
- Dependent on your product having enough public discussion to generate weekly signal. If your product is early-stage, run monthly instead of weekly.
- Reddit and forum search quality varies. Look at 4-week trends, not single-week results.
