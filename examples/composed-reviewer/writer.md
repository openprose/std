---
name: writer
kind: service
version: 0.1.0
description: Produce a well-structured article on a given topic for a specified audience.
---

requires:
- task_brief: a combined brief describing the topic and audience for the article

ensures:
- output: a clear, well-structured article based on the brief

strategies:
- open with a hook that connects the topic to the audience's concerns
- use specific examples and named references rather than generic statements
- keep paragraphs short and scannable
