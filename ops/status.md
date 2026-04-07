---
name: status
kind: program
services: [scanner, summarizer]
---

requires:
- runs_dir: (optional, default ".prose/runs/") path to the runs directory

ensures:
- summary: summary of recent runs showing run ID, program name, timestamp, duration, cost estimate, and pass/fail status

errors:
- no-runs: no run data found in the runs directory

strategies:
- scan the runs directory for run folders, sorted by timestamp descending
- for each run, read the execution log to extract program name, duration, cost estimate, and final status
- present as a table with the most recent runs first
