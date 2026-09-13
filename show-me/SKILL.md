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
| State machine  | Small SVG or HTML relationship diagram when transitions or flow materially clarify the work |
| Tasks          | Ordered task list with status icons; ownership/dependencies only when supplied              |
| Tests          | Table of test type, scenario, and source-grounded execution status                          |

Keep the example's order: Database, API, Frontend, Infrastructure, State machine, Tasks, Tests. Add a specifically titled section only for material that none of these sections can express. Build it from the example's heading, article, table, and notes patterns. Adapt table columns when necessary to avoid inventing details; retain the table format and styling.

Use a relationship diagram only when a data flow, request flow, or infrastructure topology becomes clearer than it would be as a card. Use a small hand-built SVG or HTML diagram; do not add a diagram for decoration.

## 3. Keep the report glanceable and grounded

- Lead with the example's metadata row, title, and description. Include known status/progress, grounded counts, and prominent complex/risky work as concise metadata or prose within that header.
- Keep each section to roughly 3–7 visible, high-signal items. Group repetition and use a compact “+ N similar changes” summary when useful.
- Link outward to source material for detail rather than reproducing prose, a spec, or a diff.
- Omit unestablished facts. Mention “Not specified in the source” only when the absence itself materially matters.
- Use direct, plain-English wording. Explain technical nouns briefly when a non-specialist reader would need it.

Risk and complexity are visual signals, not judgments: use a distinct icon, accent color, bolder title, or callout treatment. Do not add an AI-sounding verdict, approval state, or speculative risk assessment.

## 4. Build the HTML artifact

Read [show-me-example.html](./show-me-example.html) before writing the report. It is the required visual and structural template. Start by copying the example file, then replace its illustrative content and remove unused sections. Preserve its complete inline stylesheet, CSS variables, font stack, font sizes and weights, content width, spacing, borders, colors, icon sizing, and responsive/print rules. Reuse its HTML classes and section markup for the formats selected in step 2. Example facts are placeholders, not source evidence.

Limit styling additions to source-specific needs that the existing classes cannot express, such as a small relationship diagram or a grounded risk accent. Keep those additions consistent with the template. Preserve the single-column section layout, stacked entity articles, and compact tables; changing the report content does not authorize a typography or layout redesign. A user-requested design change overrides this template requirement.

Requirements:

- Write a standalone file to the operating system temporary directory named `show-me-<safe-slug>-<timestamp>.html`.
- Load Tailwind from `https://cdn.tailwindcss.com` and Lucide from a CDN. Do not add a build step or runtime dependency.
- Keep the example's semantic HTML and inline stylesheet so the document remains readable if a CDN fails. Load Tailwind before the inline stylesheet so the template's styles remain authoritative.
- Design desktop and print first; mobile should remain readable second.
- Use generous whitespace, restrained color, and icons/bold emphasis rather than verbose labels.
- Keep scripts limited to the CDN assets needed for styling/icons. The report otherwise remains static.
- Do not write reports into the repository unless the user asks for a specific location.

Before handoff, compare the generated file with the example: confirm the original stylesheet is retained, the header and section classes are reused, applicable sections follow the prescribed order and formats, and all illustrative content has been replaced or removed. Correct deviations before opening the report. This is a file-level check; it does not require a screenshot or browser-validation loop.

## 5. Handoff

Open the generated HTML file when the environment allows it. Then respond with its absolute path and one short sentence describing the briefing. Do not duplicate the report in chat.

There is no mandatory screenshot, browser, or print-validation loop. Make the artifact carefully, then open it.
