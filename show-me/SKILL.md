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

Start from these display patterns when they fit:

| Concern        | Best display pattern                                                                        |
| -------------- | ------------------------------------------------------------------------------------------- |
| Database       | Entities or tables, migration/backfill sequence, compatibility or rollback note when stated |
| API            | Endpoint/action rows with caller, behavior, and compatibility impact                        |
| Frontend       | Page and component changes grouped by the user journey they affect                          |
| Infrastructure | Service/configuration/deployment changes and operational dependency flow                    |
| Plan           | Ordered task with ownership or dependency only when supplied                          |
| Tests          | Test areas, scenario coverage, and intentionally absent coverage only when stated           |
| Progress       | Compact milestone timeline or status strip, using only source-grounded status               |

If these patterns distort the story, create an adaptive section with a specific title. Prefer a new, precise section over “Other” or “Miscellaneous.”

Use a relationship diagram only when a data flow, request flow, or infrastructure topology becomes clearer than it would be as a card. Use a small hand-built SVG or HTML diagram; do not add a diagram for decoration.

## 3. Keep the report glanceable and grounded

- Lead with an overview that answers purpose, scope, known status/progress, grounded counts, and prominent complex/risky work.
- Keep each section to roughly 3–7 visible, high-signal items. Group repetition and use a compact “+ N similar changes” summary when useful.
- Link outward to source material for detail rather than reproducing prose, a spec, or a diff.
- Omit unestablished facts. Mention “Not specified in the source” only when the absence itself materially matters.
- Use direct, plain-English wording. Explain technical nouns briefly when a non-specialist reader would need it.

Risk and complexity are visual signals, not judgments: use a distinct icon, accent color, bolder title, or callout treatment. Do not add an AI-sounding verdict, approval state, or speculative risk assessment.

## 4. Build the HTML artifact

Read [show-me-example.html](./show-me-example.html) before writing the report. Treat it as a visual guide, not a fixed schema: copy its semantic structure and the section pattern that fits, then remove unused sections and add better ones when needed.

Requirements:

- Write a standalone file to the operating system temporary directory named `show-me-<safe-slug>-<timestamp>.html`.
- Load Tailwind from `https://cdn.tailwindcss.com` and Lucide from a CDN. Do not add a build step or runtime dependency.
- Include semantic HTML and small inline base styles so the document remains readable if a CDN fails.
- Design desktop and print first; mobile should remain readable second.
- Use generous whitespace, restrained color, and icons/bold emphasis rather than verbose labels.
- Keep scripts limited to the CDN assets needed for styling/icons. The report otherwise remains static.
- Do not write reports into the repository unless the user asks for a specific location.

## 5. Handoff

Open the generated HTML file when the environment allows it. Then respond with its absolute path and one short sentence describing the briefing. Do not duplicate the report in chat.

There is no mandatory screenshot, browser, or print-validation loop. Make the artifact carefully, then open it.
