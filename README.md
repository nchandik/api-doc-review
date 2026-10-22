# api-doc-review

A Copilot skill for reviewing OpenAPI 3.x YAML specs and the runtime/UI error messages that accompany them.

## What it does

- Editorial review of OpenAPI 3.x prose: `summary`, `description`, parameters, responses, schema descriptions
- Review of runtime / UI error messages for consistency with the spec
- Structural sanity checks: empty schemas, missing required fields, broken `$ref`s, unused components
- Cross-file consistency: terminology, JWT phrasing, error templates, `operationId` conventions

## Output format

Reviews are delivered as one consolidated Markdown report with these sections:

1. Source / Total / Reviewer
2. Review Scope
3. Message Type Summary
4. Style Guide References
5. Question Tags
6. Summary
7. Findings
8. Cross-cutting Questions

Findings use sequential `### Finding #n` headings, with exactly one `---` divider between
findings. Each includes the message type, severity, verbatim original, revised wording,
editorial note, SME questions, and rationale. The labels
`**Message type:**`, `**Actual:**`, and `**Revised:**` use bold Markdown formatting.

Example finding:

```
### Finding #N
Location: <YAML path or message key>
**Message type:** <type from the classification table>
Category: <Structure | Clarity | Grammar | Style | Consistency | Example>
Severity: <Critical | Major | Minor>

**Actual:** <verbatim original>
**Revised:** <suggested replacement>

Editorial note: <editorial recommendation context>
Questions to the SME: <question or None.>

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

Finalized. The skill defines review coverage, message classification, finding format,
severity levels, and output requirements.

## License

No license specified yet.
