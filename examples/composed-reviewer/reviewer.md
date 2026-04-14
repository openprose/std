---
name: reviewer
kind: service
version: 0.1.0
description: Evaluate a piece of writing against quality criteria and return a structured verdict.
---

requires:
- output: the article to review
- criteria: quality standards to evaluate against (optional — uses editorial best practices if not provided)

ensures:
- verdict: "accept" or "reject"
- reasoning: why the verdict was reached
- suggestions: specific, actionable improvements (empty list if accepted)

strategies:
- evaluate clarity, specificity, and audience fit
- flag vague claims, missing examples, or unsupported assertions
- accept if the article is publishable as-is; reject if substantive improvements are needed
