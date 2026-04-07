# OpenProse Standard Library

Reusable programs for evaluation, operations, and memory management in OpenProse.

## Usage

```prose
use "std/evals/inspector"
use "std/ops/profiler"
use "std/memory/project-memory"
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

### memory/

Programs for persistent agent memory management.

| Program | Purpose |
|---------|---------|
| `user-memory` | Cross-project memory (user-scoped) |
| `project-memory` | Project-scoped memory |
