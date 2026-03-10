---
shortDescription: Session startup — gitignore, auto-update, rules, orient, and greet.
usedBy: [maestro]
version: 0.2.0
lastUpdated: 2026-03-10
---

## Purpose

Every session starts cold. The Maestro needs to ensure the project is wired correctly, the framework is up to date, load the project's rules, and understand the codebase before it can dispatch work effectively. This skill defines the boot sequence that brings the Maestro from zero to ready.

## Procedure

1. **Gitignore.** Ensure `.agents/` and `.memory/` are in the project's `.gitignore`. Run:

   ```bash
   touch .gitignore
   grep -qxF '.agents/' .gitignore || echo '.agents/' >> .gitignore
   grep -qxF '.memory/' .gitignore || echo '.memory/' >> .gitignore
   ```

2. **Framework pull.** Run:

   ```bash
   git -C .agents pull
   ```

   - If the pull brought changes:
     - Read the `CHANGELOG.md` in `.agents` to understand what changed.
     - Stop and reboot — re-read the Maestro persona from the top so updated instructions take effect.
   - If already up to date, continue.

3. **Load the rules.** Read and internalize all files under `rules/commandments/` and `rules/edicts/`. Counsel (`rules/counsel/`) is optional — read it if the task involves user-facing communication.

4. **Orient.** Read relevant source files, documentation, and any existing `.context.md` files to understand the project's current state. If the project has architecture-related skills or documentation, read them — every dispatched task must respect the project's conventions.

5. **First-run check.** If the `.memory/` directory does not exist, this is the first session on this project. Dispatch the **Contextualizer** to walk the codebase and produce `.context.md` orientation files before proceeding.

6. **Greet.** Greet the user and wait for instructions.

## Guardrails

- Never skip rule loading. Dispatching without rules means dispatching without constraints.
- Never skip the framework pull. An outdated `.agents` directory means outdated instructions.
- Never proceed past orientation if the project structure is unclear — ask the user for guidance.
