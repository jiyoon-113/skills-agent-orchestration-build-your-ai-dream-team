# Project Pulse implementation plan

## Summary
Project Pulse is a lightweight static dashboard for contributors that surfaces active projects, owners, statuses, recent activity, priority or risk, and a contributor-friendly summary. The implementation should produce a polished frontend that loads project data from `app/project-data.json`, renders a card-based UI from `app/index.html`, applies styling in `app/styles.css`, and provides a VS Code launch configuration in `.vscode/launch.json` that opens the dashboard in a browser.

## Ordered implementation steps

### 1. Confirm scope and assets
- Review the dashboard brief, existing repository context, and the custom agent definitions.
- Validate the expected deliverables: `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`.
- Capture the required user-visible behaviors: title, project cards, status badges, readable layout, and a working preview via the Run Project Pulse Dashboard launch configuration.

### 2. Define the data contract
- Create the top-level `projects` array in `app/project-data.json`.
- Ensure each project entry includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Keep the JSON structure simple and predictable so the HTML can render the data without extra transformation logic.

### 3. Build the dashboard structure
- Create `app/index.html` to host the dashboard shell and render project cards from the data file.
- Use proper semantic structure, accessible labels, and distinct sections for the dashboard title, project cards, and supporting metadata.
- Reference `styles.css` and `project-data.json` so the page can load the stylesheet and display project content when opened.

### 4. Add polished styling
- Create `app/styles.css` with a `.dashboard` container and `.project-card` styles.
- Apply spacing, color, borders, border radius, box shadow, and responsive layout rules.
- Ensure readability and contrast for project metadata, badges, and summaries.

### 5. Add launch configuration
- Create `.vscode/launch.json` with a `Run Project Pulse Dashboard` configuration.
- Configure it to run `python3 -m http.server 5500` from the `app/` directory.
- Add `serverReadyAction` so the browser opens `http://localhost:%s/index.html` and presents the dashboard instead of a directory listing.

### 6. Validate the end-to-end experience
- Run the launch configuration and open the dashboard in a browser.
- Confirm project cards render correctly, all data values appear, and styling is applied.
- Verify that the app starts from the intended directory and opens `index.html` rather than the filesystem tree.

## File assignments

### `app/index.html`
- Primary responsibility: dashboard structure and rendered content.
- The Designer should provide the content hierarchy, card arrangement, and accessibility decisions.
- The Coder should implement the page markup, fallback handling, and data-driven rendering.

### `app/styles.css`
- Primary responsibility: visual design, layout, spacing, and responsive behavior.
- The Designer should define the visual system, card treatment, typography, badges, and color usage.
- The Coder should implement the stylesheet to match the approved design and ensure the selectors like `.dashboard` and `.project-card` are present.

### `app/project-data.json`
- Primary responsibility: source-of-truth project data.
- The Coder should create the top-level `projects` array and populate each project with the required keys.
- The Designer should validate that the data supports the intended card layout and that the status and priority fields map cleanly to visible UI.

### `.vscode/launch.json`
- Primary responsibility: local preview configuration.
- The Coder should create the strict JSON configuration required for the dashboard preview.
- The Designer should confirm that the launch setup opens the intended `index.html` and not a directory listing.

## Designer responsibilities
- Define the visual hierarchy for the dashboard, including the project card layout, readable spacing, badges, and emphasis for status and priority.
- Guide accessibility choices such as semantic HTML, clear contrast, readable text, and consistent labeling.
- Review the final UI for polish and contributor-friendly readability.

## Coder responsibilities
- Implement the static frontend files with the agreed structure and data contracts.
- Build the rendering logic in `app/index.html` so project cards are generated from `app/project-data.json`.
- Write `app/styles.css` to match the approved design and ensure responsive presentation.
- Create `.vscode/launch.json` and verify the preview opens the expected page.

## Dependencies
- `app/project-data.json` must exist before `app/index.html` can reliably render all projects.
- `app/styles.css` depends on the final HTML structure and class names so the intended selectors are implemented correctly.
- `.vscode/launch.json` depends on the final app file locations (`app/` and `index.html`) and should be configured after the HTML and CSS structure are aligned.
- Validation depends on all four deliverables being present and consistent.

## Parallel work decisions
- Parallel work can begin after the data contract is agreed:
  - The Designer can draft the card layout and visual styling rules while the Coder prepares the data in `app/project-data.json`.
  - The Coder can build the HTML shell and stylesheet in parallel once the data shape and class names are known.
- The launch configuration can be drafted in parallel with the frontend implementation, as long as the final `app/` directory and `index.html` target are already known.
- Work should remain coordinated around the exact file names and selectors so the HTML, CSS, and launch configuration remain aligned.

## Sequential requirements
- Confirm the app scope and required content before creating files.
- Complete the data structure first so the HTML and CSS can be implemented against a stable contract.
- Finish the HTML structure before the styling pass, to avoid rework on selectors and layout assumptions.
- Validate the launch configuration after the app files exist, since the preview depends on the correct file paths and serving behavior.

## Edge cases to handle
- Missing or invalid `project-data.json` values should be treated as a validation issue during manual testing.
- The dashboard should not rely on a server directory listing; the launch configuration must explicitly open `index.html`.
- The page should still be readable if a project has a long `recentActivity` entry or an unusual status value.
- The CSS should remain responsive across common browser widths without breaking card alignment.

## Validation expectations
- `app/index.html` loads without showing a directory listing and displays a visible `Project Pulse` dashboard title.
- The page renders project cards from the `projects` array in `app/project-data.json`.
- Each card displays `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `app/styles.css` includes `.dashboard` and `.project-card` selectors and applies polished styling such as border radius and box shadow.
- `.vscode/launch.json` exists, uses strict JSON, and includes the `Run Project Pulse Dashboard` configuration.
- Running the launch configuration opens `http://localhost:%s/index.html` and shows the dashboard in the browser.
- Manual review confirms the dashboard is visually polished, readable, and contributor-friendly.

## Open questions
- Should the dashboard include any filtering or sorting beyond the base static layout?
- Should the `priority` field be represented as text only, or should it also include a visual indicator such as a badge or risk color?
- Does the team want project summaries to be rendered from the same data or added as a separate property later?
