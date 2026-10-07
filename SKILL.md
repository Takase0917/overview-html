---
name: overview-html
description: Create concise, source-grounded single-file HTML overviews, with interactive diagrams and code walkthroughs for implementation or architecture explanations. Use when the user asks for 全体像, "overview", "big picture", "HTML", "visualize", "explain the flow", or stakeholder-friendly material about incidents, implementations, operational flows, or repo investigations.
---

# Overview HTML

Create a self-contained HTML page that helps the user quickly understand a complex topic. Optimize for source-grounded clarity, not decoration.

## Workflow

1. Define the central question.
   - Examples: "What happened?", "What does this fix solve?", "How does the data flow work?", "What should users do when it fails?"
   - Identify the audience: engineer, operator, product owner, customer, or mixed.

2. Gather evidence before designing.
   - Prefer local repo files, diffs, commits, docs, logs, and user-provided context.
   - For code work, inspect the real implementation paths first.
   - Record exact branch names, commit hashes, files, commands, and unresolved verification gaps when relevant.
   - Separate confirmed facts from assumptions or recommendations.

3. Choose a one-page structure.
   - Incident: Summary -> Root cause -> Data/flow diagram -> Fix -> User/ops behavior -> Risks -> Verification.
   - Implementation: Problem -> Old flow -> New flow -> Key controls -> Affected files -> Test status.
   - Architecture: Actors -> Data stores -> Request/job flow -> Failure modes -> Operational notes.
   - Decision aid: Options -> Tradeoffs -> Recommended path -> Residual risks -> Next steps.

4. Build a single-file HTML artifact.
   - Default path: `docs/<short-topic>-overview.html` unless the user specifies another path.
   - Keep CSS, markup, interaction data, and JavaScript in one `.html` file; avoid external dependencies.
   - Use Japanese text when the user is working in Japanese or the artifact is for Japanese stakeholders.
   - Use stable responsive layouts: grids, tables, flow lanes, state boxes, callouts.
   - Keep cards to individual repeated items. Do not nest cards inside cards.
   - Avoid generic marketing hero pages. This is an explanation artifact, not a landing page.

5. Include the minimum visuals that make the system understandable.
   - Show parent/child data relationships, queues/jobs, locks, status transitions, and user/ops paths.
   - Use CSS diagram blocks or Mermaid if the target renderer supports it; prefer pure HTML/CSS for file:// portability.
   - Use tables for tradeoffs, status handling, verification, and branch/commit inventories.
   - For code-related explanations, use the interactive patterns below by default wherever the corresponding code, operation, or data boundary exists. Adapt to the task; do not invent CRUD operations or proposed changes for unrelated topics.

6. Verify the output.
   - Run a basic file existence/grep check for title and key claims.
   - If a browser tool is available and the path is local or localhost, open the generated HTML and inspect the layout.
   - Exercise every diagram node and operation selector, step navigation, and comparison control that was added. Confirm that each selection updates the matching explanation and code, and that first/last-step controls behave correctly.
   - Check desktop and tablet widths, plus narrow widths when the document will be read there. Check clipped content inside panels as well as whole-page horizontal overflow, keyboard operation, source links, and browser errors.
   - If verification is blocked, state exactly what was not verified.

## Interactive Code Explanations

Help readers move from a visible operation to its implementation using these three patterns:

1. **Clickable diagram → responsible code and explanation.**
   - Draw the actual relationships between screens, components, state/operation logic, API functions, and external systems. Label arrows so render dependencies, user events, and data flow are distinguishable.
   - Make each relevant node selectable. Show its responsibility in plain language, file path, a short real source excerpt with line numbers, and a link to the verified source revision when available.
   - Select a useful node initially. Explain unfamiliar terms alongside the code rather than relying on filenames alone.

2. **Operation → step-by-step execution.**
   - Let readers select relevant operations such as loading, saving, editing, or deleting. Provide clickable steps and previous/next controls, with the active step visibly marked.
   - Tie each step to the actual function and source excerpt. Trace meaningful transitions such as validation, duplicate-submit prevention, HTTP requests, cache updates, and navigation where implemented.
   - Explain important failure and cancellation paths, including which input or state is retained. Keep these walkthroughs illustrative: interaction changes the document display and does not perform application writes or call live APIs.

3. **Data transformation and current implementation → proposed change.**
   - Show representative frontend values beside their API request/response shapes, with field mappings, validation boundaries, and the responsible conversion functions. Use synthetic sample data.
   - Where a change is under discussion, compare the current behavior with the proposed behavior and identify affected files or functions. Distinguish browser page URLs from API endpoints when both appear.
   - Label proposals as unimplemented unless verified in the inspected revision. If no change is proposed, explain the current data flow without inventing a before/after comparison.

Use native buttons with accessible names, selected-state semantics, visible focus, and keyboard support. Preserve focus when rebuilding step controls. Let code wrap or scroll within its panel without clipping surrounding explanations. Embed and escape source excerpts safely; avoid fetching repository code at viewing time so the artifact remains usable offline. If print styling is included, keep the selected explanation and code readable in print.

## Content Rules

- Lead with the practical conclusion.
- Make the "old flow vs new flow" explicit when explaining a fix.
- Explain terms that are local to the codebase, such as table/model names, queue names, or status values.
- Preserve operational nuance: "safe by default", "manual retry only", "blocked by DB auth", "staging only", etc.
- Include residual risks and what would still require a separate data repair or deploy action.
- Do not include secrets, credentials, raw production data dumps, or long logs.
- Do not claim tests passed if they were blocked or only syntax checks ran.

## Reusable Template

Use `assets/overview-template.html` as a starting point when creating a fresh artifact. Copy it to the target `docs/` path and replace the placeholder sections with evidence from the current task. Do not leave placeholder text in the final HTML.

The template supplies a static layout foundation. For code-related material, extend it with the interactive patterns above; a static file list alone does not provide the requested code walkthrough.

## Final Response

Return the generated file as a clickable absolute path and summarize:

- What the HTML covers
- Any verification performed
- Any known gaps or assumptions
