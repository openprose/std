---
name: worker-critic
kind: composite
version: 0.1.0
description: Iteratively refines a worker's output by looping through critic evaluation until acceptance or budget exhaustion.
slots:
  - name: worker
    primary: true
    contract:
      requires: [task_brief, optional critique from previous attempt]
      ensures: [result text]
  - name: critic
    contract:
      requires: [worker result, original task_brief, criteria]
      ensures: [structured verdict with accept/reject, reasoning, issues, suggestions]
config:
  task_brief:
    type: string
    default: null
    description: The task to pass to the worker
  criteria:
    type: string
    default: null
    description: Acceptance criteria the critic evaluates against
  max_rounds:
    type: integer
    default: 3
    description: Maximum number of worker-critic cycles before returning best attempt
invariants:
  - Worker never sees the raw criteria — only the critique derived from them
  - Critic always receives the original task brief alongside the result
  - On accept the loop exits immediately and returns the worker's result
  - After max_rounds exhausted the final attempt is returned with its critique
---

# Worker-Critic

One agent works, another evaluates. Retry until the critic accepts or budget exhausts.

## Shape

```
shape:
  self: [manage retry loop, pass critique to worker, return accepted result]
  delegates:
    worker: [produce result from brief]
    critic: [evaluate result against criteria]
  prohibited: none
```

## Contract

```
requires:
  - &compositeState exists at __compositeState with:
      worker: string        -- component name to use as worker
      critic: string        -- component name to use as critic
      task_brief: string    -- the task to pass to the worker
      criteria: string      -- acceptance criteria for the critic
      max_rounds: number    -- (optional, default 3)

ensures:
  - Worker receives only the task brief (first attempt) or task brief + critique (retries)
  - Critic receives the worker's result AND the original task brief AND the criteria
  - Critic returns { verdict: "accept" | "reject", reasoning, issues, suggestions }
  - On reject: worker receives the critique as learning signal — not the raw task repeated
  - On accept: return the worker's result immediately
  - After max_rounds exhausted: return the best attempt with the final critique
  - &compositeState.result contains the final output
  - &compositeState.attempts contains the count
```

## Delegation Loop

```javascript
const { worker, critic, task_brief, criteria, max_rounds = 3 } = __compositeState;
let lastResult = null;
let lastCritique = null;

for (let attempt = 0; attempt < max_rounds; attempt++) {
  // Build worker brief
  let workerBrief = task_brief;
  if (lastCritique) {
    workerBrief += `\n\nPrevious attempt was rejected.\nCritique: ${lastCritique.reasoning}`;
    if (lastCritique.suggestions?.length) {
      workerBrief += `\nSuggestions: ${lastCritique.suggestions.join("; ")}`;
    }
  }

  lastResult = await rlm(workerBrief, null, { use: worker });

  // Build critic brief
  const criticBrief = `Evaluate this result against the criteria.\n\nOriginal task: ${task_brief}\nCriteria: ${criteria}\n\nResult to evaluate:\n${lastResult}`;
  const verdict = await rlm(criticBrief, null, { use: critic });

  // Parse verdict — critic should return structured assessment
  if (verdict?.verdict === "accept" || /accept/i.test(String(verdict))) {
    __compositeState.result = lastResult;
    __compositeState.attempts = attempt + 1;
    return(lastResult);
  }

  lastCritique = verdict;
}

__compositeState.result = lastResult;
__compositeState.attempts = max_rounds;
__compositeState.final_critique = lastCritique;
return(lastResult);
```

## Notes

This is a seed pattern in the standard library. The worker does not know it will be critiqued. The critic does not know its verdict triggers a retry. Multi-polarity is managed here, not in the children.
