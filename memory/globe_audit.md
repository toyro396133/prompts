# Globe Audit & Improvement Log
## Iteration 1: Physical CSS Properties
- **Critique**: Globe marked the frontend chunks (`app`, `components`) as complete in `STATE.md`, but numerous physical CSS classes (`pl-`, `pr-`, `ml-`, `mr-`, `left-`, `right-`) remained in the codebase. Globe is not validating its own work effectively.
- **Improvement**: Updated Globe's prompt to explicitly enforce running verification commands (e.g., `grep -rE "pl-|pr-|ml-|mr-|left-|right-"`) before marking a chunk as complete.
## Iteration 2: API Validation & Hidden Texts
- **Critique**: Globe left raw English strings in `NextResponse.json` (e.g., `Invalid domain format`, `Billing checkout gateway configuration error.`). This causes standard UI toasts to display untranslated server errors.
- **Improvement**: Globe must enforce that ALL string literals inside `NextResponse.json` (especially `error` and `message` properties) are wrapped in `t()` hooks (if server-side i18n is available) or replaced with error translation keys.
## Iteration 3: Bi-Directional (BDI) Text
- **Critique**: Globe failed to explicitly wrap dynamic variables containing English text (e.g. domain names, emails, IPs) or numbers within Hebrew text using `<bdi>` elements. This causes punctuation (like dots or colons) to render on the wrong side of the string in RTL mode.
- **Improvement**: Updated Globe's prompt to strictly enforce wrapping dynamic variables in `<bdi>` when injected into translated strings, especially in dashboards or logs.
## Iteration 4: Auto-Validation
- **Critique**: Globe prematurely marked chunks as complete in `STATE.md` without verifying if TypeScript builds passed or if any localization changes broke the layout logic.
- **Improvement**: Updated the Globe pipeline to explicitly run `npm run build` or equivalent verification commands before committing and changing `STATE.md`.
