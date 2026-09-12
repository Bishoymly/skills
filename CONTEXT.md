# Public Skills

This repository publishes reusable agent skills. Its first context is the creation of human-readable briefings from technical source material.

## Language

**Show-me report**:
A standalone HTML briefing that makes a technical plan, specification, ticket, or change set easy for a human to scan and understand. It is a presentation artifact, not an approval or validation result.
_Avoid_: Review verdict, audit

**Source material**:
The user-provided or agent-retrieved plan, specification, ticket, pull request, diff, or file from which a show-me report is derived. Its origin does not determine the report's behavior.
_Avoid_: Mode, report type

**Report section**:
A focused visual block that summarizes one concern, such as database, API, UI, infrastructure, tasks, tests, or progress. Sections are selected from the source material and may be extended when needed.
_Avoid_: Fixed schema, mandatory category

**Risk marker**:
A visual treatment—icon, color, or emphasis—that signals an item is complex or carries meaningful implementation risk, without judging correctness, readiness, or approval.
_Avoid_: Review finding, verdict

**Overview**:
The opening, executive-briefing portion of a show-me report. It gives the source identity, plain-English purpose, known status or progress, scope, grounded counts, and prominent risk markers before the detailed sections.
_Avoid_: Dashboard, executive summary document

**Adaptive section**:
A report section invented for a source-specific concern when the standard section ideas do not express it well. It is preferred over forcing content into an irrelevant category.
_Avoid_: Other, miscellaneous

**Relationship diagram**:
A small optional visual used only when a data, request, or infrastructure relationship becomes clearer than it would be as a card or prose.
_Avoid_: Required diagram, decorative graphic

**Section template**:
A tailored visual pattern for a familiar concern, such as a migration sequence for database work or an endpoint matrix for API work. It is guidance for choosing a clear layout, not a required data model.
_Avoid_: Report schema, fixed card

**Source-linked claim**:
A report detail that is grounded in source material and links back to it when a usable reference exists. Details not established by a source are omitted unless their absence is itself materially important.
_Avoid_: Inference, assumed detail

**Exemplar**:
The single, polished sample HTML file that demonstrates the show-me visual system and section templates.
_Avoid_: Application, renderer

**Artifact handoff**:
The completion experience in which the agent opens a generated temporary report when possible, then gives the user its absolute path and a concise description.
_Avoid_: Chat-based report

**Report density guardrail**:
The rule that each section exposes only a small, grouped set of high-signal items, using concise wording and links rather than repeating a specification or diff. Repeated work may be summarized as a count.
_Avoid_: Exhaustive rendering, source reproduction

**Source identity**:
The source's own item type, identifier, title, and reference, shown without mandatory generic field labels. The report uses only the identity details that materially help the reader orient themselves.
_Avoid_: Fixed header metadata, labelled source fields

**Temporary report name**:
A safe, readable name that distinguishes one report artifact from another.
_Avoid_: Preview.html, fixed output name

**Skill package**:
A self-contained, installable unit that describes one skill and carries the assets it needs.
_Avoid_: Application package, generated package

**Source resolution**:
Establishing access to report source material before presenting it. When access is unavailable, the report does not guess at its contents.
_Avoid_: Source-specific mode, assumed access

**Readable degradation**:
The requirement that a report remains understandable when its preferred visual presentation is unavailable.
_Avoid_: Offline asset bundle, runtime dependency

**Exemplar scenario**:
A coherent fictional change used by the exemplar to demonstrate every relevant section pattern without representing a real project or using filler content.
_Avoid_: Dummy data, real case study

**Public release**:
A published, installable version of a skill package and its public documentation.
_Avoid_: Local-only initialization, unpublished draft
