# Project Pulse Dashboard Implementation Plan

## Goal

Create a lightweight, contributor-friendly Project Pulse dashboard that lets Mona's team quickly see which projects are active, who owns them, their current status, recent activity, and priority or risk level. The result should be a polished, readable static app with project cards, status badges, consistent spacing, and a short summary for each project.

The implementation will use the repository's orchestration flow rather than one undifferentiated prompt. The app is currently empty, so the work starts by establishing the four required implementation surfaces:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

The existing `.vscode/tasks.json` must remain unchanged. The launch configuration must serve the `app/` directory and open `index.html`, so users see the dashboard instead of a server directory listing.

## Agent responsibilities

### Orchestrator

- Own the overall sequence, delegate work to the appropriate agents, and keep file ownership explicit.
- Resolve conflicts between the design, data, and implementation decisions before coding begins.
- Ensure the final result satisfies the brief, remains static, and does not overwrite the existing `.vscode/tasks.json`.
- Coordinate integration and run the final validation checklist.

### Planner

- Maintain this implementation plan and identify dependencies, phases, risks, and open decisions.
- Define the minimum information architecture for the dashboard and the data contract for project records.
- Confirm that every required file has an owner and that the launch behavior is part of the acceptance criteria.

### Designer

- Define the visual hierarchy and responsive layout for project cards, including how status and priority are made scannable.
- Specify accessible color usage, readable spacing, typography, focus states, and behavior at narrow and wide viewport sizes.
- Provide the Coder with a concise visual and accessibility direction without implementing application files.

### Coder

- Implement the static dashboard in the assigned app files according to the agreed design and data contract.
- Keep the dashboard dependency-free unless the Orchestrator explicitly approves a change; the initial implementation should work as a small static app.
- Wire data loading and rendering so the UI handles the complete required project record shape and reports data-loading failures visibly.
- Create the launch configuration as assigned and preserve the existing tasks configuration.

## File assignments

| File | Owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder, guided by Designer | Create the semantic page shell, page heading and summary area, project-list container, accessible status/priority presentation hooks, and references to the stylesheet and dashboard behavior. Keep markup suitable for a static app and avoid embedding project records directly when they belong in JSON. |
| `app/styles.css` | Coder, guided by Designer | Define the visual system, responsive project-card layout, readable spacing, status and priority treatments, typography, focus visibility, and accessible contrast. Keep layout usable on mobile and desktop without external build tooling. |
| `app/project-data.json` | Coder, with the Planner's data contract | Provide a top-level `projects` array. Every project object must include `name`, `owner`, `status`, `recentActivity`, and `priority`; values should be realistic, concise, and useful for demonstrating active projects, differing statuses, and priority/risk levels. |
| `.vscode/launch.json` | Coder, reviewed by Orchestrator | Add a deterministic **Run Project Pulse Dashboard** launch configuration that serves from `app/` and opens `index.html`, never the directory root. Use a suitable existing or standard local static-server approach supported by the repository environment, and keep `.vscode/tasks.json` intact. |

## Phased implementation

### Phase 1: Confirm contract and ownership

1. The Planner reviews the brief and this plan, confirms the required fields and acceptance criteria, and records any unresolved launch-server assumption.
2. The Orchestrator assigns the four files and confirms that `app/` is empty while `.vscode/tasks.json` is pre-existing and out of scope.
3. The Designer defines the card information hierarchy and accessibility requirements before visual implementation.

**Output:** an agreed data shape, page structure, design direction, and file ownership.

### Phase 2: Prepare independent inputs

1. The Designer can work in parallel with the Planner's data examples because layout decisions and sample record content are initially independent.
2. The Planner specifies representative values for status and priority so the UI can demonstrate visual variation without changing the required schema.
3. The Orchestrator checks that no agent expands the scope into a framework, build pipeline, or unrelated repository files.

**Output:** design guidance and a stable `projects` data contract.

### Phase 3: Implement the static surfaces

1. The Coder creates `app/project-data.json` first, because the required record shape drives rendering and visible labels.
2. The Coder creates `app/index.html` and `app/styles.css` against the agreed structure and design direction. These tasks can be developed in parallel after the page structure and class names are agreed, but integration requires both.
3. The Coder creates `.vscode/launch.json` using the repository's supported launch conventions, with `app/` as the server root and `index.html` as the entry point.

**Output:** a runnable static dashboard and launch configuration.

### Phase 4: Integrate and validate sequentially

1. The Orchestrator verifies that HTML selectors, CSS hooks, and JSON fields agree.
2. Start the configured launch target and confirm that the served URL renders `index.html` directly rather than listing files.
3. Check the dashboard at narrow and wide viewport sizes, inspect keyboard focus, and verify that all required project information is legible.
4. Fix integration defects before considering the work complete; do not mask missing data or launch failures with silent defaults.

**Output:** a validated implementation ready for learner use.

## Dependencies and sequencing decisions

- The brief and data contract are the source of truth. The Planner must settle the top-level `projects` array and its five required fields before the Coder finalizes rendering.
- Designer and Planner work may proceed in parallel after the goal is understood: design needs the information categories, while data planning does not depend on final CSS.
- HTML structure, CSS hooks, and JSON field usage must be agreed before implementation is considered parallel-safe. The Coder may build the data file and initial page shell in parallel, but rendering integration is sequential.
- Launch configuration depends on the final app entry point and server-root requirement, so it is reviewed after `app/index.html` exists.
- Final browser or served-page validation is sequential after all four assigned files are present. This is necessary to catch path, MIME, fetch, and directory-listing issues that static file inspection cannot prove.
- No dependency installation or framework setup is planned. If the chosen launch mechanism requires a tool that is not already available, the Orchestrator must surface that dependency and choose a repository-compatible alternative rather than silently adding unrelated tooling.

## Validation expectations

- Confirm exactly the intended files are added or changed: the four assigned files, with `.vscode/tasks.json` preserved.
- Parse `app/project-data.json` and verify it contains a top-level array named `projects`; verify every record has non-empty `name`, `owner`, `status`, `recentActivity`, and `priority` fields.
- Inspect `app/index.html` for a semantic, usable page structure and valid references to the local stylesheet and data/rendering assets.
- Verify the page presents project cards, owner, status, recent activity, priority/risk, and contributor-friendly summary content without requiring a build step.
- Serve through the configured launch command and confirm the root response opens `index.html` rather than a directory listing; confirm JSON and CSS load from the expected relative paths.
- Check responsive layout, readable contrast, visible keyboard focus, meaningful text alternatives/labels where applicable, and status/priority communication that is not color-only.
- Run available repository validation appropriate to a static app. Report any unavailable browser or launch tooling explicitly rather than treating file existence as runtime validation.

## Risks and edge cases

- **Directory listing instead of the app:** an incorrect server root or missing entry path can expose files. The launch configuration must use `app/` and explicitly open `index.html`.
- **Data contract drift:** renamed fields, a nested array, or missing values will break rendering. Keep the exact required field names and validate the JSON before visual testing.
- **Empty or malformed data:** the UI should show an explicit, user-visible loading/error state rather than silently displaying an empty successful dashboard.
- **Unexpected status or priority values:** styles and labels should remain readable for values beyond the initial examples, with a sensible fallback treatment that preserves the text.
- **Long content:** long project names, owner names, or recent-activity text must wrap or truncate accessibly without breaking card layout.
- **Small screens and zoom:** cards must reflow without horizontal scrolling, and text must remain usable at increased zoom.
- **Accessibility:** color alone must not communicate status or risk; focus indicators, semantic headings, labels, and sufficient contrast are required.
- **Static-file restrictions:** relative paths and browser-compatible JSON loading must work when served locally; avoid assumptions about a backend or build-time environment.
- **Existing VS Code setup:** `.vscode/tasks.json` is already used for the exercise and must not be replaced, reformatted, or coupled to the new launch configuration.

## Open questions

- Which local static-server/debugger type is guaranteed in the learner's VS Code environment, and should `.vscode/launch.json` use an installed extension or a repository-provided command?
- Should the dashboard include a separate aggregate count/summary bar, or is the per-project contributor-friendly summary sufficient for the first implementation?
- What exact status and priority vocabulary should be standardized for the sample records (for example, whether priority uses `High/Medium/Low` or includes a risk label)?
- Should failed data loading expose a retry control, or is a clear error message sufficient for this exercise?
- Is browser-based accessibility testing available in the validation environment, or should the Orchestrator rely on manual keyboard/viewport checks plus static inspection?
