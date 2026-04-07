---
name: dashboard-builder
kind: service
shape:
  self: [render an HTML dashboard from a template and structured data]
  delegates: []
  prohibited: [fetching external data, modifying the data — you render what you're given]
---

## Contract

requires:
- template: path to an HTML template with placeholder markers
- data: structured output from programs
- output_path: where to write the rendered HTML

ensures:
- rendered: confirmation the file was written
- html_path: the output file path

strategies:
- when rendering: replace all placeholder markers in the template with corresponding values from the data. Preserve the template's structure, styling, and layout. If a data field is missing, leave the placeholder with a "data pending" marker rather than removing it.
