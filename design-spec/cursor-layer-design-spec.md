# The Cursor Layer Design Spec

A framework for building AI features that live where the user works instead of somewhere they navigate to.

## The Four Components

Every cursor-layer interaction has four components. You can ship with fewer, but the interaction gets better as you add more. Clicky has all four. Most AI products have zero.

```
                    SCREEN CONTEXT
                  (AI sees the work)
                          |
                          v
       VOICE INPUT --> [ USER ] <-- SPATIAL RESPONSE
       (speaks ask)    at work     (answer appears here)
                          |
                          v
                    AGENT HANDOFF
              (guide escalates to doer)
```

The user stays in the center. The four components surround the workspace instead of pulling the user out of it. The chatbox interface moves the user to the AI; the cursor layer moves the AI to the user.

### 1. Screen Context

**Principle:** The AI sees what the user sees. The user never describes their screen.

**What Clicky does:** Takes a screenshot every time the user holds the hotkey. Pairs it with the voice transcript. Both go to the model simultaneously.

**What Google does:** Magic Pointer combines cursor position with screen content. The model knows what's under the cursor and what's around it.

**Implementation pattern for your product:**
- Capture: screenshot of the active view, or structured data from the component the user is interacting with
- Frequency: on-demand (user triggers) or continuous (background monitoring)
- Scope: full screen, active panel, or element-level (what's under the cursor)

**The design question:** What does the user currently describe to the AI that the system could just see?

### 2. Voice Input

**Principle:** Speaking a question is faster than typing it. Voice removes the translation step between thought and prompt.

**What Clicky does:** Hold Ctrl+Option, speak. No wake word. No dictation mode. The hotkey IS the interface.

**What Google does:** Cursor position plus speech. "Fix this" and "Move that here" work because the system knows what "this" and "that" refer to from cursor position.

**Implementation pattern for your product:**
- Activation: hotkey, button hold, or always-listening (with appropriate permissions)
- Deictic reference: "this", "that", "here" resolve to whatever the user is pointing at or looking at
- Fallback: text input for contexts where voice is inappropriate (open office, public space)

**The design question:** Where do your users type a question that would be faster to speak?

### 3. Spatial Response

**Principle:** The AI responds where the user is working, not in a separate panel.

**What Clicky does:** Blue animated triangle flies to the exact button, slider, or menu item using bezier arc animations. The response is spatial, not textual.

**What Google does:** Highlights, annotations, and actions happen at the cursor position. No sidebar. No panel switch.

**Implementation pattern for your product:**
- Inline: response appears at or near the element the user asked about
- Pointing: visual indicator highlights the relevant UI element
- Overlay: minimal floating response that doesn't obscure the workspace
- Action: the AI performs the action directly instead of describing it

**The design question:** Where does your AI currently respond in a panel that could respond inline?

### 4. Agent Handoff

**Principle:** The AI starts as a guide and escalates to a doer. The transition should feel like one interaction, not a mode switch.

**What Clicky does:** Voice interaction for guidance. "Clicky agent" for delegation. Same interface. The user decides the escalation level.

**What Google does:** Point and speak for queries. Drag and speak for actions. The gesture determines whether the AI explains or executes.

**Implementation pattern for your product:**
- Trigger: explicit command ("do it"), gesture (drag), or confidence threshold
- Scope: single action, multi-step workflow, or background task
- Feedback: the user sees what the agent is doing and can interrupt
- Completion: result appears in the user's workspace, not in a log

**The design question:** Where does your AI explain how to do something that it could just do?

## How the Four Components Map to the Three Stages

| Component | Stage 1 (Destination) | Stage 2 (Embedded) | Stage 3 (Cursor Layer) |
|---|---|---|---|
| Screen Context | User describes screen to AI | AI can read some product data | AI sees what user sees in real time |
| Voice Input | N/A (text only) | N/A (text only) | User speaks naturally, deictic reference works |
| Spatial Response | Response in separate window | Response in sidebar or panel | Response at the point of interaction |
| Agent Handoff | Copy AI output, do it manually | AI suggests, user confirms | AI acts directly, user supervises |

## Minimum Viable Cursor Layer

You don't need all four components to ship something useful. Here's the minimum by use case:

**Help/guidance:** Screen Context + Spatial Response. The AI sees what the user sees and responds inline. No voice needed. This is the highest-ROI starting point for most products.

**Task execution:** Screen Context + Agent Handoff. The AI sees the context and acts on it. Response can be a notification rather than spatial.

**Learning/tutorial:** All four. The full Clicky experience. Only worth building if your product has a steep learning curve (complex creative tools, enterprise software with 200+ features).

## Using This Spec

1. For each AI feature on your roadmap, check which of the four components it currently has
2. For each component it's missing, ask the corresponding design question
3. The gap between current components and target components is your cursor-layer opportunity
4. Pair with the audit template to prioritize by user impact
