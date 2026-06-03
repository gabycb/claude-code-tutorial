---
name: iterative-plan-review
description: >
  Orchestrates a multi-agent plan review loop. The plan-creator agent builds a plan from the user's request; the plan-critic agent reviews it cold (zero original context); their feedback cycles back and forth until the critic approves or 3 rounds complete. Trigger with: /iterative-plan-review <user request>
---

# Iterative Plan Review Workflow

You are the **orchestrator**. Your job is to drive a structured back-and-forth between two isolated agents until a high-quality plan emerges.

## The Two Agents

- **plan-creator** — Has full context of the user request. Produces and refines the plan.
- **plan-critic** — Receives ONLY the plan document. No original request. No context. Reviews cold.

## Orchestration Loop

Follow these steps exactly:

### Step 1 — Receive the request
Take the user's request as your input. Do not modify or summarize it yet.

### Step 2 — First plan draft
Invoke **plan-creator** with:
```
USER REQUEST:
<paste the full user request verbatim>
```

Wait for the complete plan output before continuing.

### Step 3 — Cold review (Round 1)
Invoke **plan-critic** with ONLY the plan document. Do not include:
- The user's original request
- Any explanation of what the plan is for
- Any of your own commentary

Pass exactly:
```
<paste the full plan document from Step 2 verbatim>
```

### Step 4 — Check verdict

**If APPROVED** → Skip to Step 6.

**If NEEDS REVISION** → Continue to Step 5.

### Step 5 — Revision
Invoke **plan-creator** with both:
```
USER REQUEST:
<original user request verbatim>

CRITIC FEEDBACK (Round N):
<paste the full critique verbatim>
```

Then return to Step 3 for the next review round.
Increment the round counter. Maximum 3 rounds total.

### Step 6 — Deliver final plan
Present the final approved plan to the user with:
- The plan itself (clean, no agent scaffolding)
- A brief summary of what changed across rounds (if multiple rounds ran)
- Any **Open Questions** from the plan that require the user's input before implementation

## Round limit behavior

If Round 3 ends without APPROVED:
- Present the Round 3 plan anyway
- List the remaining unresolved critic issues clearly
- Ask the user whether to run another round or proceed with known gaps

## What NOT to do

- Do not summarize or paraphrase the user request when passing it to plan-creator — pass it verbatim
- Do not pass ANY original context to plan-critic — only the raw plan document
- Do not skip rounds because the plan "looks good" — always run at least one full critic pass
- Do not merge or blend the two agents' outputs — keep them strictly isolated per round
