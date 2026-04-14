---
name: confident-writer
kind: program
description: Demonstrates Level 3 decorator syntax — a writer with stacked review and confidence measurement.
services:
  - writer:
      review: worker-critic(critic: reviewer, max_rounds: 2)
      confidence: stochastic-probe(analyst: variance-analyst, sample_size: 3)
---

requires:
- task_brief: a combined brief describing the topic and audience for the article

ensures:
- article: a polished article that has been quality-reviewed and confidence-scored

# Notes

This program desugars to two nested composite instantiations:

1. `worker-critic` wraps `writer` (primary slot) with `reviewer` as the critic
2. `stochastic-probe` wraps the reviewed writer, running the entire worker-critic loop 3 times to measure output variance

The confidence score in the output indicates whether the writer produces consistent quality (low variance) or inconsistent results (high variance) for this topic.
