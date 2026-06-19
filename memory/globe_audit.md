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
## Iteration 5: Directional Icons (SVG Flipping)
- **Critique**: Globe focused heavily on layout classes (like padding/margin) but completely ignored directional visual cues. Icons like `ArrowRight` or `ChevronRight` point in the wrong direction (LTR) when the layout flips to RTL.
- **Improvement**: Updated Globe's prompt to aggressively hunt for directional icons (`grep -rE "Arrow|Chevron"`) and append `rtl:-scale-x-100` class to flip them automatically.
## Iteration 6: Accessibility (ARIA) & Hidden Props
- **Critique**: Globe primarily extracted visible React node text (e.g. `<div>Text</div>`), leaving `aria-label`, `title`, and `placeholder` attributes populated with hardcoded English strings. This breaks screen readers for localized users.
- **Improvement**: Updated Globe's prompt to mandate auditing HTML attributes explicitly using `grep -rE "aria-label=|placeholder=|title="`.
## Iteration 7: Vanilla JS Dialogs (Alert/Confirm)
- **Critique**: Globe successfully handled standard JSX text but missed raw vanilla JS dialog functions like `alert("...")` and `confirm("...")` containing hardcoded English text.
- **Improvement**: Updated Globe's prompt to explicitly check `alert(` and `confirm(` functions for raw string literals that must be localized.
## Iteration 8: Absolute Raw Text Extraction
- **Critique**: Despite previous instructions, Globe left hundreds of basic hardcoded strings in JSX nodes (e.g., `>Asset Management<`, `>Workspace Settings<`). It seems Globe assumed another chunk covered them or failed to run a deep regex validation.
- **Improvement**: Updated Globe's prompt to provide a Python script/regex method to mathematically prove all raw JSX text is gone. Globe MUST run `grep -rE '>\s*[A-Za-z]+[^<]*[A-Za-z]+\s*<' <dir>` and pipe it out to ensure zero untranslated visible elements remain.
## Iteration 9: Language Switcher Injection
- **Critique**: Globe successfully created the `LanguageSwitcher.tsx` component but failed to actually import and render it anywhere in the application (like `TopNav.tsx` or `layout.tsx`). The feature was orphaned.
- **Improvement**: Updated Globe's prompt to not only verify/create the switcher, but STRICTLY mandate injecting it into the main layout header (`TopNav.tsx` or `app/layout.tsx`) so it is visibly accessible to the user.
