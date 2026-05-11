---
name: api-doc-review
description: "Review and improve OpenAPI 3.x YAML files and accompanying runtime messages for REST API documentation. Use when: reviewing API descriptions, summaries, parameter and response messages; checking consistency of error wording; editorial review of REST API docs; preparing API content for a documentation portal (Redoc, Zoomin, or other); annotating API doc changes; reviewing OpenAPI spec prose."
argument-hint: "Provide the YAML file path, a snippet, or the runtime message strings to review"
---

# API Documentation Review (OpenAPI 3.x)

Draft skill — under construction. Update this file with the finalized rules, style guide references, and output format.

## Intended scope (to be refined)

- Editorial review of OpenAPI 3.x YAML prose (summary, description, parameters, responses, schema descriptions)
- Review of accompanying runtime / UI error messages for consistency with the spec
- Structural / spec sanity checks (empty schemas, missing required fields, broken `$ref`s, unused components)
- Cross-file consistency (terminology, JWT phrasing, error templates, operationId conventions)

## Intended output format (to be refined)

Default: Actual / Revised blocks, grouped by section, with severity and one-line justification.

```
[#N] Location: <YAML path or message key>
Category: <Structure | Clarity | Grammar | Style | Consistency | Example>
Severity: <Critical | Major | Minor>

Actual:  <verbatim original>
Revised: <suggested replacement>

Why: <one-line justification>
```

## Style references (to be refined)

- Microsoft Writing Style Guide — voice, grammar, plain language
- Google Developer Documentation Style Guide — technical conventions
- OpenAPI 3.x Specification — structural rules
- Project conventions — JWT token phrasing, OpenEdge terminology, error message template

## Notes

- Renderer-agnostic by default (works for Redoc, Widdershins/Markdown, DITA pipelines, etc.).
- Add Redoc-specific or Zoomin-specific appendices as needed.
