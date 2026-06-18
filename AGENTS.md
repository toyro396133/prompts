# JULES AGENT ARCHITECTURE GUIDELINES

This document stores the foundational memory, workflow architecture, and structural requirements for all AI Agents in this repository.

## 1. Directory Structure
The repository acts as a registry for AI agent prompts, storing markdown files in directories like `hebrew_prompts/` and `סקילים לז'וליס/`.
The project's `README.md` maintains a registry of agents using `<details>` blocks with specific `<summary>` tags: `🇬🇧 English (Original)` and `🇮🇱 עברית (Hebrew)`.

## 2. Communication Rules
- Communicate with the user exclusively in **Hebrew** for all chat messages, explanations, questions, and feedback.
- Code, bash commands, and git commits should remain in **English**.
- For operations that require direct access to repositories, use the provided GitHub token.

## 3. Creating a New Agent Prompt
When creating a new AI agent or updating existing prompts, the text file MUST include specific structural sections to enforce state awareness and prevent redundant or destructive actions.
Every agent prompt must include:
1. `# 🎯 מהות הסוכן` (Agent Essence) - A clear description of the agent's purpose.
2. `You are...` - Identity definition.
3. `Sample Commands` - Specific CLI/Bash commands the agent can run.
4. `Universal Full-Spectrum Coding Standards` - Good and Bad code examples.
5. `Core Directives` - Rules mandating Idempotency, checking state tracking files (`STATE.md`, `TODO.md`), and verifying git history before acting.

## 4. The Scale-Proof Execution Pipeline (Continuous Autonomous Loop)
AI agent prompts must define a "Fire and Forget" execution loop. Agents must handle large repositories by dividing work into chunks and resolving PRs autonomously.
The workflow section must enforce:
- **Target Selection & Isolation:** Read `STATE.md` to identify the next unprocessed logical chunk.
- **Fresh Sync & Branch:** Run `git pull origin main --rebase` to ensure no conflicts occur with other agents. Create a local feature branch.
- **Execute Chunk:** Apply the specific logic for that chunk.
- **Self-Challenge Loop:** The agent MUST stop and critique its own work before continuing. (e.g., "What did I miss?").
- **Bulletproof Auto-Merge:**
  - *Local Build Enforcement:* Run local build and tests. Never merge broken code.
  - *Smart CI Polling:* Check if external CI is configured (`.github/workflows`). If it exists, await CI approval. If it doesn't, safely perform a self-reviewed Auto-Merge to `main`.
- **Repeat:** Loop endlessly until 100% of the repository is completed.

## 5. Specific Agent Knowledge

### Localization Agents (e.g., Globe 🌐)
Localization agents must execute deep architectural i18n, not just string swapping:
- **Hidden Text Zero Tolerance:** Extract hidden texts like `placeholder`, tooltips, validation schemas (Zod/Yup), and toasts.
- **Server-Side Errors:** Convert raw English backend errors into translatable Error Codes handled by the frontend.
- **Database Enums:** Translate database enums dynamically before UI rendering (e.g., `t('status.' + status)`).
- **RTL/LTR Structural Mirroring:** Convert all physical CSS properties (e.g., `ml-`, `left-0`) to logical properties (`ms-`, `start-0`).
- **Pluralization & Gender:** Utilize the translation engine's built-in pluralization (`_one`, `_two`) and context features, avoiding raw slash-formats.
- **Bi-Directional Text:** Use `<bdi>` tags for mixed Hebrew/English elements.

## 6. Simulation & Testing Guidelines
When simulating or testing an agent's prompt against external repositories:
1. Clone the target repository locally using the available GitHub token.
2. Apply the simulated workflow and prove the logic.
3. Clean up the repository (e.g., `rm -rf test_repo`) before finalizing the changes to avoid polluting the main project.
