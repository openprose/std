# OpenProse Standard Library

Reusable programs, patterns, and roles for evaluation, operations, and memory management in OpenProse.

## Usage

```prose
use "std/evals/inspector"
use "std/ops/profiler"
use "std/memory/project-memory"
use "std/composites/worker-critic"
use "std/controls/pipeline"
use "std/roles/critic"
```

Install with `prose install`. See [deps.md](https://github.com/openprose/prose/blob/main/skills/open-prose/deps.md) for details.

## Programs

### evals/

Programs for evaluating and improving prose runs.

| Program | Purpose |
|---------|---------|
| `inspector` | Post-run analysis — runtime fidelity and task effectiveness |
| `calibrator` | Validates light evals against deep evals for reliability |
| `vm-improver` | Analyzes inspections, proposes VM improvements |
| `program-improver` | Analyzes inspections, proposes program improvements |

### ops/

Programs for profiling and debugging.

| Program | Purpose |
|---------|---------|
| `profiler` | Cost, token usage, and time profiling for completed runs |
| `cost-analyzer` | Token usage and cost pattern analysis across runs |
| `error-forensics` | Root cause analysis for failed runs |
| `lint` | Validate structure, schema, shapes, and contract consistency for a program and its service tree |
| `preflight` | Check that all runtime dependencies are satisfied before executing a program |
| `status` | Summarize recent runs from .prose/runs/ |
| `wire` | Run Forme wiring to produce an execution manifest |

### delivery/

Services for delivering program outputs to humans and external systems.

| Service | Purpose |
|---------|---------|
| `human-gate` | Present output for human review, block until approved or rejected |
| `slack-notifier` | Format and deliver content to Slack via webhook or API |
| `email-notifier` | Send an HTML email via a configured email provider (Resend, SendGrid, Postmark, SES, SMTP) |
| `email-renderer` | Render structured report data into a branded, email-safe HTML string |
| `html-renderer` | Render an HTML document from a template and structured data |
| `webhook-notifier` | Deliver content to an HTTP endpoint via webhook |
| `file-writer` | Write content to a local, S3, or GCS destination |

### composites/

Named multi-agent topology patterns for RLM programs.

| Pattern | Purpose |
|---------|---------|
| `observer-actor-arbiter` | Three-role OAA composite: observer tracks state, actor proposes actions, arbiter decides |
| `ensemble-synthesizer` | N-agent ensemble that independently solves then synthesizes into a consensus answer |
| `worker-critic` | Two-agent loop: worker produces, critic evaluates, repeat until accepted |
| `ratchet` | Iterative improvement loop with regression prevention |
| `witness` | Meta-observer composite for recording and reflecting on agent behavior |
| `dialectic` | Two-agent debate structure producing a synthesized conclusion |
| `proposer-adversary` | Adversarial proposal testing: proposer generates, adversary stress-tests |
| `assumption-miner` | Surfaces implicit assumptions in a solution for explicit evaluation |
| `blind-review` | Independent evaluation without exposure to other agents' assessments |
| `contrastive-probe` | Generates contrasting solutions to expose decision boundaries |
| `drift-detector` | Monitors agent behavior over iterations to detect drift from objectives |
| `stochastic-probe` | Introduces controlled randomness to test solution robustness |
| `synchronization-probe` | Tests coordination quality between collaborating agents |

### controls/

Flow control patterns for structuring RLM program execution.

| Pattern | Purpose |
|---------|---------|
| `pipeline` | Sequential stage pipeline: output of each stage feeds the next |
| `map-reduce` | Parallel fan-out across inputs followed by aggregation |
| `gate` | Conditional branching: proceed only if a quality or validity threshold is met |
| `progressive-refinement` | Iterative solution improvement with expanding scope each round |
| `retry-with-learning` | Retry loop that accumulates failure context to guide subsequent attempts |

### roles/

Single-agent role guides for use in RLM programs and composites.

| Role | Purpose |
|---------|---------|
| `classifier` | Classifies inputs into a fixed set of categories; returns structured label |
| `critic` | Evaluates a proposed solution against criteria; returns scored feedback |
| `extractor` | Extracts structured data from unstructured text; returns JSON |
| `summarizer` | Condenses long content into a concise representation |
| `verifier` | Checks a solution for correctness against known constraints or test cases |

### memory/

Programs for persistent agent memory management.

| Program | Purpose |
|---------|---------|
| `user-memory` | Cross-project memory (user-scoped) |
| `project-memory` | Project-scoped memory |
