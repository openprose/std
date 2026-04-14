---
name: composed-reviewer
kind: program
description: Demonstrates Level 1 composite instantiation — a writer with worker-critic review.
services:
  - name: reviewed-draft
    compose: std/composites/worker-critic
    with:
      worker: writer
      critic: reviewer
      max_rounds: 2
---

requires:
- task_brief: a combined brief describing the topic and audience for the article

ensures:
- article: a polished article that has passed quality review
