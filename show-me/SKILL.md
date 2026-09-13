---
name: show-me
description: Create a clean HTML briefing that makes a technical plan, spec, ticket, pull request, diff, or file easy for a human to scan.
---

# Show Me

Turn technical source material into a standalone, human-readable HTML briefing. The HTML report is the primary artifact; chat is only the handoff.

Use this skill when a user wants to understand, present, discuss, or review a plan, specification, ticket, pull request, diff, or technical file without reading every underlying detail.

## Scope

- Present source material in clear, plain English.
- Summarize what changes, why it matters, what is in scope, and what is known about progress and tests.
- Use visual emphasis to make complex or risky work easy to notice.
- Do **not** issue review verdicts, infer approval, claim readiness, or invent missing details.

The source does not determine a mode. A GitHub pull request, Jira ticket, Azure DevOps item, Markdown plan, and local diff all use the same briefing workflow.

## 1. Resolve the source

Read the material using the tools available in the current environment:

- local files and working-tree diffs;
- a URL in a browser;
- GitHub pull requests and issues through `gh` when available;
- connected ticketing tools when available.

If the source cannot be accessed, ask the user to provide it or grant access. Do not guess what it contains.

Preserve the source's own identity—its type, ID, title, and reference—when those details help orient the reader. Do not impose generic labels such as “Source”, “Reference”, or “ID”.

## 2. Select the story and sections

Write a one- or two-sentence plain-English purpose. Then select only the sections that materially clarify the source. Never create empty sections or “not affected” cards.

Use the example's section names, order, and display formats for concerns present in the source. Omit sections without material changes. A concern that fits an existing section belongs there, even when a more narrative heading could describe it.

| Concern        | Best display pattern                                                                        |
| -------------- | ------------------------------------------------------------------------------------------- |
| Database       | One entity article per table; column tables and migration/consistency notes                 |
| API            | Endpoint table with change icon, method badge, endpoint, and behavior/compatibility         |
| Frontend       | Route/page table; dependency trees for components, services, and libraries when established |
| Infrastructure | Resource table with before/after values and impact when established                        |
| State machine  | Small SVG or HTML diagram with states as nodes and labeled transitions as arrows             |
| Tasks          | Ordered task list with status icons; ownership/dependencies only when supplied              |
| Tests          | Table of test type, scenario, and source-grounded execution status                          |

Keep the example's order: Database, API, Frontend, Infrastructure, State machine, Tasks, Tests. Add a specifically titled section only for material that none of these sections can express. Build it from the example's heading, article, table, and notes patterns. Adapt table columns when necessary to avoid inventing details; retain the table format and styling.

Database entity tables describe columns: column, type, nullability/default, and notes. Put access policies, triggers, constraints, and migration behavior in the entity's notes rather than replacing its column table with a rules matrix. When only rules change, retrieve the relevant existing schema and show a compact set of columns labeled as unchanged context; mark only actual column changes as added/modified. If the schema cannot be retrieved, present the rule changes in notes without inventing columns.

When a State machine section is included, render it as a small hand-built SVG or HTML diagram with states as nodes and source-grounded transitions as labeled arrows, rather than a table. Put conditions and caveats in the example's notes structure beneath the diagram. For other concerns, use a relationship diagram only when a data flow, request flow, or infrastructure topology becomes clearer than it would be as a card.

## 3. Keep the report glanceable and grounded

- Lead with the example's compact metadata row, title, and short description. Fit known status/progress, grounded counts, and the most material caveat into concise metadata or prose; move detailed explanation into the applicable section rather than expanding the header into a notes panel.
- Keep each section to roughly 3–7 visible, high-signal items. Group repetition and use a compact “+ N similar changes” summary when useful.
- Link outward to source material for detail rather than reproducing prose, a spec, or a diff.
- Omit unestablished facts. Mention “Not specified in the source” only when the absence itself materially matters.
- Use direct, plain-English wording. Explain technical nouns briefly when a non-specialist reader would need it.

Risk and complexity are visual signals, not judgments: use a distinct icon, accent color, bolder title, or callout treatment. Do not add an AI-sounding verdict, approval state, or speculative risk assessment.

## 4. Build the HTML artifact

Read [show-me-example.html](./show-me-example.html) before writing the report. It is the authoritative visual and structural template, not design inspiration. Unless the user explicitly requests a design change, the example prevails over framework defaults, CDN styles, utility classes, browser resets introduced by the report, and agent styling preferences. Example facts are placeholders, not source evidence.

Start by copying the example file, then replace its illustrative content and remove unused sections. Preserve its complete inline stylesheet, CSS variables, content width, spacing, borders, colors, icon sizing, and responsive/print rules. Adapt the existing section markup rather than rebuilding it from generic HTML helpers.

### Typography and markup fidelity

- Treat the example as the single source of truth for typography. Preserve the exact font stacks, font sizes, weights, line heights, and letter spacing for `body`, `h1`, `h2`, `h3`, `.description`, `.meta`, tables, notes, and code. Keep the title as `h1`, section headers as `h2`, and entity headers as `h3`; retain the title's responsive `clamp()` expression rather than substituting a fixed size.
- Preserve typography-bearing markup as well as CSS: use `<code>` for database identifiers/types, every literal route and endpoint (including those in links or notes), paths, and code identifiers. Keep the example's monospace treatment and relative sizing; place prose in the normal font. Route and endpoint cells also inherit the example's explicit monospace font-family rules. Show actual routes when established by source material; keep page/surface labels separate rather than substituting them for routes.
- Reuse the example's header, `.section-heading`, `.entity-heading`, table, and notes structures. Retain `.endpoints-table` on API endpoint tables and `.routes-table` on frontend tables; place dependency trees in the page cell, as in the example.
- Keep API method cells compact: one method/endpoint operation per row, using the example's row classes and badge styles. For additional methods, add narrowly scoped classes consistent with that treatment rather than inline style overrides. Group repetitive operations through a concise summary outside the table when necessary.
- Account for the template's non-wrapping second table column. Put short methods, types, or routes there; keep verbose surface descriptions in wrapping prose cells. Shorten or regroup content before changing the template's layout.

Limit styling additions to source-specific needs that the existing classes cannot express, such as a small relationship diagram or a grounded risk accent. Keep those additions consistent with the template. Preserve the single-column section layout, stacked entity articles, and compact tables; changing the report content does not authorize a typography or layout redesign. A user-requested design change overrides this template requirement.

Requirements:

- Write a standalone file to the operating system temporary directory named `show-me-<safe-slug>-<timestamp>.html`.
- Use the example's asset-loading strategy, including its Lucide CDN icons. Keep its semantic HTML and inline stylesheet so the document remains readable if a CDN fails. Do not add a build step or runtime dependency.
- Add no CSS framework, external font, reset, or utility stylesheet absent from the example. In particular, Tailwind is not required: its global reset can change rendering even when the original inline stylesheet is retained. If a user explicitly requests a framework, isolate or disable its global reset and verify that the example's typography and layout still prevail. Script-tag order alone does not prove CSS precedence, especially for dynamically injected styles.
- Design desktop and print first; mobile should remain readable second.
- Use generous whitespace, restrained color, and icons/bold emphasis rather than verbose labels.
- Keep scripts limited to the CDN assets needed for styling/icons. The report otherwise remains static.
- Do not write reports into the repository unless the user asks for a specific location.

### Fidelity gate before handoff

Compare the generated file with the example: confirm the original stylesheet is retained, typography-bearing tags and applicable section/table classes are reused, sections follow the prescribed order and formats, and all illustrative content has been replaced or removed. Inspect header density, method-cell width, wrapping, and dependency placement; matching CSS text alone is insufficient.

This is a file-level comparison only. Do not use browser tools, screenshots, or computed-style checks for this workflow. Preserve typography directly from the example's CSS and markup; no rendered validation is required.

## 5. Handoff

Open the generated HTML file when the environment allows it. Then respond with its absolute path and one short sentence describing the briefing. Do not duplicate the report in chat.

Complete the file-level fidelity gate, then open the artifact. No browser or print-validation loop is required.
