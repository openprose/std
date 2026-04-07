---
name: lint
kind: program
services: [resolver, validator, checker]
---

requires:
- target: path to the program .md file to lint

ensures:
- report: structured lint report with file-level validation results, contract compatibility checks, shape consistency checks, and warnings

errors:
- not-found: target file does not exist
- not-program: target file is not a valid program (missing or invalid `kind`)

strategies:
- recursively resolve all services declared in the program's `services:` list, following nested service trees
- validate each file's frontmatter against the Prose schema — valid `kind`, valid contract sections, valid shape structure
- check that all services referenced in `services:` lists exist as files
- verify that shape `delegates` entries reference known services
- attempt basic contract matching — does each service's `requires` have a plausible match in another service's `ensures`
