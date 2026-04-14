---
name: oversight
kind: composite
version: 0.1.0
description: Actor executes, observer independently analyzes outcomes, arbiter decides whether to continue, adjust, or abort.
slots:
  - name: actor
    primary: true
    contract:
      requires: [task_brief, adjustment (if prior cycle was adjusted)]
      ensures: [execution outcome]
  - name: observer
    contract:
      requires: [task_brief, actor outcome]
      ensures: [independent analysis of the outcome]
  - name: arbiter
    contract:
      requires: [task_brief, observer report]
      ensures: [decision: continue, adjust, or abort]
config:
  task_brief:
    type: string
    default: null
    description: The task for the actor to perform
  max_cycles:
    type: integer
    default: 3
    description: Maximum oversight cycles
invariants:
  - The observer sees only the outcome, never the actor's reasoning or self-assessment
  - The actor does not know an observer is watching
  - The observer does not know an arbiter will act on its report
  - The arbiter decides solely from the observer's report, not from the actor
---

# Oversight

Actor acts, observer watches outcomes independently, arbiter decides next step based on the observer's report — not the actor's self-assessment.

## Shape

```
shape:
  self: [manage the cycle, route information between roles, enforce separation]
  delegates:
    actor: [execute the current plan]
    observer: [independently analyze the outcome]
    arbiter: [decide: continue, adjust, or abort]
  prohibited: none
```

## Contract

```
requires:
  - &compositeState exists at __compositeState with:
      actor: string         -- component name for the actor
      observer: string      -- component name for the observer
      arbiter: string       -- component name for the arbiter
      task_brief: string    -- the task to execute
      max_cycles: number    -- (optional, default 3)

ensures:
  - Actor receives the task brief (first cycle) or adjusted brief (subsequent cycles)
  - Observer receives ONLY the outcome — not the actor's reasoning or self-assessment
  - Arbiter receives the observer's report and decides: continue, adjust, or abort
  - If arbiter says adjust: the adjustment is passed to the actor's next cycle
  - If arbiter says abort: return immediately with the reason
  - If arbiter says continue: actor runs another cycle with the same brief
  - &compositeState.result contains the final outcome
  - &compositeState.cycles contains the number of cycles run
```

## Delegation Loop

```javascript
const { actor, observer, arbiter, task_brief, max_cycles = 3 } = __compositeState;
let currentBrief = task_brief;
let lastOutcome = null;

for (let cycle = 0; cycle < max_cycles; cycle++) {
  // Actor executes
  lastOutcome = await rlm(currentBrief, null, { use: actor });

  // Observer analyzes the outcome — no access to actor's reasoning
  const observerBrief = `Analyze this outcome independently.\n\nTask: ${task_brief}\nOutcome:\n${lastOutcome}`;
  const observation = await rlm(observerBrief, null, { use: observer });

  // Arbiter decides based on observer's report
  const arbiterBrief = `Based on this independent observation, decide: continue, adjust, or abort.\n\nTask: ${task_brief}\nObserver report:\n${observation}`;
  const decision = await rlm(arbiterBrief, null, { use: arbiter });

  const decisionStr = String(decision).toLowerCase();
  if (/abort/.test(decisionStr)) {
    __compositeState.result = lastOutcome;
    __compositeState.cycles = cycle + 1;
    __compositeState.abort_reason = decision;
    return(lastOutcome);
  }

  if (/adjust/.test(decisionStr)) {
    currentBrief = `${task_brief}\n\nAdjustment from oversight: ${decision}`;
  }
  // "continue" → loop with same or adjusted brief
}

__compositeState.result = lastOutcome;
__compositeState.cycles = max_cycles;
return(lastOutcome);
```

## Notes

The actor does not know an observer is watching. The observer does not know an arbiter will act on its report. The information firewall between actor and observer is the structural guarantee — the observer's assessment cannot be contaminated by the actor's rationalization.
