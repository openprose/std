---
name: proposer-adversary
kind: composite
version: 0.1.0
description: One agent proposes, another attacks the proposal, and the unresolved tension is returned for the parent to judge.
slots:
  - name: proposer
    primary: true
    contract:
      requires: [task_brief]
      ensures: [proposal text]
  - name: adversary
    contract:
      requires: [proposal, original task_brief]
      ensures: [attack identifying flaws, edge cases, and counterexamples]
config:
  task_brief:
    type: string
    default: null
    description: What the proposer should propose on
invariants:
  - The composite does not resolve the tension — it returns both proposal and attack
  - Proposer is unaware it will be attacked
  - Adversary is unaware its output will be weighed against the proposal
---

# Proposer-Adversary

One proposes, another attacks. The composite returns both — the parent decides.

## Shape

```
shape:
  self: [delegate to proposer, delegate to adversary, return both outputs]
  delegates:
    proposer: [produce a proposal given the brief]
    adversary: [find flaws in the proposal]
  prohibited: none
```

## Contract

```
requires:
  - &compositeState exists at __compositeState with:
      proposer: string      -- component name for the proposer
      adversary: string     -- component name for the adversary
      task_brief: string    -- what to propose

ensures:
  - Proposer receives only the task brief
  - Adversary receives the proposal and the task brief — its job is to find flaws,
    edge cases, and counterexamples
  - This composite does NOT resolve the tension
  - Returns { proposal, attack } — the parent reasons about the disagreement
  - &compositeState.result contains { proposal, attack }
```

## Delegation

```javascript
const { proposer, adversary, task_brief } = __compositeState;

const proposal = await rlm(task_brief, null, { use: proposer });

const adversaryBrief = `Find flaws, edge cases, or counterexamples in this proposal.\n\nOriginal task: ${task_brief}\n\nProposal:\n${proposal}`;
const attack = await rlm(adversaryBrief, null, { use: adversary });

const result = { proposal, attack };
__compositeState.result = result;
return(result);
```

## Notes

This is a seed pattern. The proposer does not know it will be attacked. The adversary does not know its output will be weighed against the proposal. The parent composes both perspectives and makes the final judgment.
