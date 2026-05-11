# api-doc-review

A Copilot skill for reviewing OpenAPI 3.x YAML specs and the runtime/UI error messages that accompany them.

## What it does

- Editorial review of OpenAPI 3.x prose: `summary`, `description`, parameters, responses, schema descriptions
- Review of runtime / UI error messages for consistency with the spec
- Structural sanity checks: empty schemas, missing required fields, broken `$ref`s, unused components
- Cross-file consistency: terminology, JWT phrasing, error templates, `operationId` conventions

## Output format

Findings are returned as numbered blocks:

```
[#N] Location: <YAML path or message key>
Category: <Structure | Clarity | Grammar | Style | Consistency | Example>
Severity: <Critical | Major | Minor>

Actual:  <verbatim original>
Revised: <suggested replacement>

Why: <one-line justification>
```

## Style references

- [Microsoft Writing Style Guide](https://learn.microsoft.com/style-guide/welcome/)
- [Google Developer Documentation Style Guide](https://developers.google.com/style)
- [OpenAPI 3.x Specification](https://spec.openapis.org/oas/latest.html)
- Project-specific conventions (JWT phrasing, OpenEdge terminology, error message templates)

## Installation

Clone into your VS Code Copilot skills folder:

```bash
git clone https://github.com/nchandik/api-doc-review.git \
  ~/.copilot/skills/api-doc-review
```

On Windows:

```powershell
git clone https://github.com/nchandik/api-doc-review.git `
  "$env:USERPROFILE\.copilot\skills\api-doc-review"
```

## Usage

In a Copilot Chat session, ask to review an OpenAPI file or message strings. The skill is invoked automatically based on the description in [SKILL.md](SKILL.md).

Example prompts:

- "Review this OpenAPI YAML for clarity and consistency."
- "Check these error messages against the spec for consistent wording."

## Status

Draft — under construction. Rules, severity thresholds, and the output format are not yet finalized.

## License

No license specified yet.
