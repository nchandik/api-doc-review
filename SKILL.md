---
name: api-doc-review
description: "Review and improve OpenAPI 3.x YAML files and accompanying runtime messages for REST API documentation. Use when: reviewing API descriptions, summaries, parameter and response messages; checking consistency of error wording; editorial review of REST API docs; preparing API content for a documentation portal (Redoc, Zoomin, or other); annotating API doc changes; reviewing OpenAPI spec prose; finding messages shared between UI and API surfaces."
argument-hint: "Provide the YAML file path, a snippet, runtime message strings, or both UI and API message sources to compare"
---

# API Documentation Review (OpenAPI 3.x)

Finalized skill for OpenAPI and API message reviews.

This skill must produce outputs that follow the repository workflow used for REST API reviews.

## Required output and file policy

- Always generate a Markdown deliverable file in the active workspace.
- Do not return chat-only review content when a file deliverable is expected.
- Keep one consolidated review report unless the user explicitly asks for per-message output.

## Required review section order

Use this exact order:

1. Source / Total / Reviewer
2. Review Scope
3. Style Guide References
4. Question Tags
5. Summary
6. Findings
7. Cross-cutting Questions

## Required finding format

Use heading format `### Finding #n` and keep numbering sequential.
Place exactly one `---` divider line between findings.

Every finding must include all fields below:

- Location
- Category
- Severity
- Actual
- Recommended
- Editorial note
- Questions to the SME
- Why

If no SME question exists, set:

- Questions to the SME: None.

Use only these severity values:

- Critical
- Major
- Minor

## Critical description-field rules

- Never summarize description-field content in findings.
- For description reviews, Actual must quote the full source block verbatim.
- For description reviews, Recommended must be a full replacement paragraph or block.
- Do not use bullet-only replacement text for description findings unless explicitly requested.

## Mandatory scope separation

Do not mix artifact types within one finding narrative.
Keep findings segregated by scope:

- OpenAPI YAML or spec text
- UI/i18n message maps
- Java/runtime constants

If input includes mixed artifacts, split findings by scope and label scope clearly.

## Mandatory messaging guidance checks

- For 4xx, 5xx, and 207 partial/failure response descriptions, include explicit caller next-step guidance.
- Keep guidance concise and actionable.
- Use question tags exactly as [Editorial] and [Technical].
- Keep terminology casing consistent, including JWT token.

## Core review scope

- Editorial review of OpenAPI 3.x YAML prose (summary, description, parameters, responses, schema descriptions)
- Review of accompanying runtime / UI error messages for consistency with the spec
- Structural / spec sanity checks (empty schemas, missing required fields, broken `$ref`s, unused components)
- Cross-file consistency (terminology, JWT phrasing, error templates, operationId conventions)

## Standard finding block

Default: detailed finding blocks grouped under Findings.

```
### Finding #N
Location: <YAML path or message key>
Category: <Structure | Clarity | Grammar | Style | Consistency | Example>
Severity: <Critical | Major | Minor>

Actual:  <verbatim original>
Recommended: <suggested replacement>

Editorial note: <editorial recommendation context>
Questions to the SME: <question or None.>

Why: <one-line justification>
```

## Clarification protocol

If required context is missing or wording intent is ambiguous, ask concise clarification
questions before producing final findings.

Trigger questions when any of the following is true:

- A placeholder is unclear or inconsistent (for example `dbName` vs `dbname`).
- A message can map to more than one operation, resource, or severity.
- The desired tone, audience, or policy constraints are not explicit.
- A proposed revision could change runtime meaning.

Question rules:

- Ask only what is required to unblock an accurate review.
- Prefer grouped questions (2-5) over many single questions.
- Use multiple-choice wording when possible to reduce back-and-forth.
- If no response is available, proceed with clearly labeled assumptions.

Default clarification set (adapt as needed):

1. Should user-facing messages avoid support-contact directives entirely?
2. What is the canonical placeholder style (for example `{dbName}`)?
3. Should action names mirror exact API values (for example `logout` vs `log out`)?
4. What is the approved numeric range policy for port validation?

## Style references

- Microsoft Writing Style Guide — voice, grammar, plain language
- Google Developer Documentation Style Guide — technical conventions
- OpenAPI 3.x Specification — structural rules
- Project conventions - JWT token phrasing, OpenEdge terminology, error message template

## Notes

- Renderer-agnostic by default (works for Redoc, Widdershins/Markdown, DITA pipelines, etc.).
- Add Redoc-specific or Zoomin-specific appendices as needed.

## Output quality bar

Before returning final output, enforce a formatting and quality pass:

- Ensure every finding block includes all required fields: Location, Category, Severity,
   Actual, Recommended, Editorial note, Questions to the SME, Why.
- Ensure severity values are only: Critical, Major, Minor.
- Ensure terminology and placeholders are consistent throughout the output.
- Ensure no duplicated finding IDs and no missing numbering.
- Ensure grammar, punctuation, and spacing are clean and publication-ready.
- Ensure recommended text is actionable and preserves message intent.
- For every edited item, include an explicit rationale that states why text was removed,
  replaced, or reordered (for example: Clarity, Consistency, Brevity, Grammar,
  Technical fidelity).
- If the recommended text is unchanged, state why no change was needed.
- Ensure description findings contain verbatim Actual and full-block Recommended content.
- Ensure exactly one `---` divider appears between adjacent findings.
- Ensure declared total findings count matches actual findings.
- Ensure no cross-scope mixing occurs within a finding.

If the user requests file output, generate cleanly formatted `.md` content only.
Do not generate `.docx` output unless the user explicitly overrides this rule.

## Review framing

When you use this skill to review a guide or spec, keep the response structured around the
developer task, not just the wording:

1. State the use case clearly — who the guide is for and what problem it solves.
2. Select a small number of representative samples — usually 2 or 3 that cover the main
   issues, not every minor example.
3. Explain how the issue helps or hurts the developer experience — for example, faster
   onboarding, fewer support calls, or clearer endpoint selection.
4. Present the result cleanly — summarize the finding, show the sample, explain the impact,
   and then give the recommended revision.

## Developer-experience review

Beyond grammar and structure, review whether the docs actually help a developer succeed.
Most "this API is hard to integrate" complaints trace back to gaps in these areas, not to
missing reference content. Treat documentation as part of the product experience, not an
afterthought.

### What to check (per operation and per resource)

1. **Getting started** — Is there a minimal end-to-end example a new developer can copy,
   run, and get a successful response from? If a resource requires setup steps in another
   resource first, is that prerequisite stated and linked?
2. **Authentication clarity** — Is the auth mechanism (JWT, OAuth scopes, API key header)
   shown in at least one concrete request example, not only described in prose?
3. **Endpoint selection** — When two or more endpoints look similar (e.g., `POST /things`
   vs. `POST /things/bulk`, `GET /search` vs. `GET /things`), does the description state
   *when to use this one instead of the other*?
4. **Expected workflow** — For multi-step resources (create → poll → fetch result; upload →
   commit; reserve → confirm), is the sequence documented in one place, with example
   payloads for each step?
5. **Practical examples** — Each non-trivial operation should have:
   - A realistic request body (not `"string"` / `"foo"` placeholders)
   - The matching success response
   - At least one error response example with the actual error shape
6. **What "success" looks like** — Does the response example show the fields a caller will
   typically use next (IDs, status, links), so they know they integrated correctly?
7. **Field intent, not just type** — Descriptions should answer "what do I put here and
   why", not restate the type. Bad: "name: The name." Good: "name: Display name shown to
   end users; max 64 chars; not required to be unique."
8. **Failure modes the caller can act on** — For each documented error, can the developer
   tell whether to retry, fix input, re-authenticate, or follow the documented escalation path?

### Signals that examples are missing or weak

- Request bodies use placeholder strings (`"string"`, `"example"`, `0`, `true`) instead of
  realistic values.
- Only one example per operation, covering only the happy path.
- Examples present in `description` prose but not in `examples:` / `example:` fields, so
  they don't render in Try-It-Out tools.
- Enum values listed without explaining what each one means or when to use it.
- Schemas with `description: ""` or descriptions that just repeat the field name.

### How to report developer-experience findings

Use the standard finding block with `Category: Example` or `Category: Clarity`, and in
`Why:` state the developer task that is currently blocked. For example:

```
### Finding #N
Location: paths./reports.post
Category: Example
Severity: Major

Actual:  (single example with body { "name": "string", "type": "string" })
Recommended: Add two examples: "Create a scheduled report" and "Create an ad-hoc report",
         each with realistic field values and the matching 201 response body.

Editorial note: Add realistic examples to reduce trial-and-error integration.
Questions to the SME: None.

Why: Callers can't tell which fields are required for each report type without trial and
     error; this is the resource that drove the recent support-call spike.
```

Prefer recommending **one more well-placed example** over recommending more prose. A
concrete example often resolves confusion that additional explanation cannot.

### Error message actionability

For error response descriptions, always check whether a developer reading the message can
determine what action to take. If the required action is unclear, include an [Editorial]
question to SME asking whether the message should include:
- Recovery guidance (e.g., "Verify your permissions" or "Check field format")
- Reference to response fields that provide details (e.g., "See notFoundRoleURNs")
- Link to troubleshooting or escalation path

Common cases requiring SME questions:
- `403` / `401` errors that don't explain whether to retry, re-authenticate, or escalate
- `404` / `400` errors that don't indicate which input caused the failure or how to fix it
- Generic messages like "Request failed" that don't help developers differentiate causes

## UI ↔ API message overlap

Use this mode when the user asks which runtime messages appear on **both** the UI and the API
(e.g., the same error string is hardcoded in a UI component and also returned in an API response
or documented in OpenAPI `description`/`example`).

### Inputs to ask for (only if not provided)

1. **UI message sources** — paths or globs. Common locations:
   - i18n bundles: `**/locales/**/*.{json,yaml,properties}`, `**/i18n/**`, `messages.*.json`
   - Hardcoded UI strings: `**/*.{ts,tsx,js,jsx,vue,svelte}`
2. **API message sources** — paths or globs. Common locations:
   - OpenAPI specs: `**/*.{yaml,yml}` (extract `summary`, `description`, `example`, `examples.*.value`, `enum` text)
   - Server-side message bundles or constants
3. **Match mode** (default: normalized + template-aware).
4. **Desired output** (default: overlap + UI-only + API-only + wording inconsistencies).

### Extraction rules

- **UI hardcoded strings**: collect string literals of length ≥ 8 chars containing a space or
  ending in `.`/`!`/`?`. Skip imports, log tags, CSS class names, test fixtures, and translation
  keys (heuristic: keys are usually `dot.case` or `snake_case` with no spaces).
- **UI i18n bundles**: collect leaf string values; remember the key for traceability.
- **OpenAPI**: collect text from `info.title`, `info.description`, `paths.*.*.summary`,
  `paths.*.*.description`, `parameters[].description`, `responses.*.description`,
  `components.schemas.*.description`, `examples.*.value` (string), `enum` items (string).
- Ignore code fences, URLs, and pure markdown punctuation.

### Normalization (default)

Apply to both sides before comparing:

1. Trim whitespace; collapse internal whitespace to a single space.
2. Lowercase.
3. Strip surrounding quotes and trailing terminal punctuation (`. ! ?`).
4. Replace template placeholders with a single sentinel `«VAR»`:
   - `{name}`, `{0}`, `{{name}}`, `${name}`, `%s`, `%d`, `%(name)s`, `:param`
5. Collapse consecutive sentinels.

Two messages are considered the **same** if their normalized forms are equal.

### Comparison procedure

1. Build `UI = { normalizedText -> [ {source, originalText, key?} ] }`.
2. Build `API = { normalizedText -> [ {source, originalText, path} ] }`.
3. `OVERLAP = keys(UI) ∩ keys(API)`.
4. `UI_ONLY = keys(UI) - keys(API)`; `API_ONLY = keys(API) - keys(UI)`.
5. **Wording inconsistencies**: for each normalized key in `OVERLAP`, if the set of original
   strings (case/punctuation/placeholder-style preserved) has more than one distinct form,
   flag it.

### Output format

Produce four sections. Omit any that are empty.

```
# Messages on both UI and API (N)

[1] Normalized: <normalized text>
    UI:  <file:line>  "<original>"   (key: <i18n key if any>)
    API: <file:jsonpath>  "<original>"
    Status: <Identical | Wording differs>
    Suggestion (if differs): <unified wording>

# UI-only messages (N)
- <file:line>  "<original>"

# API-only messages (N)
- <file:jsonpath>  "<original>"

# Wording inconsistencies (N)
[1] Normalized: <normalized text>
    Variants:
      - UI  "<v1>"
      - API "<v2>"
    Recommended: "<single canonical wording>"
    Why: <one-line justification>
```

### Severity guidance for inconsistencies

- **Critical**: same error condition, materially different meaning between UI and API.
- **Major**: same meaning, different terminology (e.g., "user" vs "account").
- **Minor**: punctuation, casing, or placeholder-style differences only.

### Caveats to surface to the user

- Hardcoded-string extraction is heuristic; false positives (log lines, dev strings) are expected.
  Show counts and a sample so the user can refine globs.
- If UI strings are loaded dynamically (server-rendered, CMS), file scanning will miss them; ask
  for an exported message catalog instead.
- Translated bundles (non-English) should be excluded from overlap matching unless the user
  explicitly wants per-locale comparison.

