---
name: human-gate
kind: service
shape:
  self: [present output for human review, block until approved or rejected]
  delegates: []
  prohibited: [modifying the content, approving on behalf of the human, proceeding without explicit approval when gate_level requires it]
---

## Contract

requires:
- content: the output to review
- gate_level: one of "all", "external", "none"
- review_channel: where to present — e.g. Slack channel or email

ensures:
- approved: boolean
- feedback: optional modifications or notes from reviewer. If gate_level is "none", approved is always true with no review.

environment:
- REVIEW_CHANNEL: default channel for posting review requests, provided by the runtime

strategies:
- when gate_level is "all": post every output for review
- when gate_level is "external": only review content that will be sent outside the org (outreach emails, reports to customers)
- when gate_level is "none": skip review entirely (pass-through)
