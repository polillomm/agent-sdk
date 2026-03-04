---
shortDescription: Assembles sub-agent prompts with task brief.
usedBy: [maestro]
version: 0.1.0
lastUpdated: 2026-03-04
---

## Purpose

Every sub-agent starts cold. It has no rules, no memory, and no awareness of the project it is about to work on. This skill defines how the Maestro assembles the initial prompt that boots a sub-agent into a ready state — loaded with the right rules, context, and a clear task to execute.

## Terminology

A **sub-agent** is a persona defined in this framework — nothing else. The terms "sub-agent" and "persona" are interchangeable throughout this skill. Sub-agents are **not** host-runtime features (IDE subprocesses, tool-provided agents, or built-in workers). The Maestro must never route work to a host-runtime agent when a framework persona exists for the job.

To discover available sub-agents, read:

- **`personas/README.md`** — lists every persona and its purpose.

This is the only registry. If a persona is not listed there, it does not exist. The Maestro must consult this registry during the Route step before dispatching.

## Procedure

1. **Load the agent.**
   - Read the persona file and extract its `modelTier` from the frontmatter. The tier is a floor — upgrade when the task is complex or high-stakes.
   - Strip the frontmatter and wrap the result in `<agent>` tags verbatim — do not summarize, paraphrase, or shorten the persona file. The full text must arrive exactly as written. Each dispatch targets exactly one agent — never multiple personas in a single prompt.
   - Dispatch using the host runtime's native mechanism for spawning an isolated subagent (e.g., a Task tool, agent subprocess, or equivalent).

2. **List the commandments (scoped).** Consult `rules/README.md` and select only commandments whose scope matches the task category. List their file paths in `<commandments>` tags — do not inline the file contents. The persona has file access and will read them directly. If no commandments match, omit the block entirely. When the task involves code changes — even if the persona does not write code (e.g. architect planning implementations) — include `coding`-scoped rules so the persona's output aligns with the conventions the coder will follow.

3. **List relevant skills.** Consult `skills/README.md` and identify skills that would help the persona complete the task — even if the persona's playbook does not reference them explicitly. List their file paths in `<skills>` tags — do not inline the file contents. If no extra skills are relevant, omit the block entirely.

4. **Write the task brief.** Translate the user's intent into actionable instructions, wrapped in `<task>` tags. The brief must contain:
   - **Intent** — what the user wants accomplished, in the Maestro's words.
   - **Entities** — key nouns: files, modules, endpoints, services.
   - **Constraints** — deadlines, tech stack limits, scope boundaries. Omit if none.
   - **Acceptance criteria** — what "done" looks like. If the user did not provide criteria, the Maestro defines them.

5. **Compose and dispatch.** Assemble the final prompt:

```markdown
<agent>
  [persona file without frontmatter]
</agent>

<commandments>
  [file paths to scoped commandments — omit block if no scope matches]
</commandments>

<skills>
  [file paths to relevant skills — omit block if none apply]
</skills>

<notes>
  - You are running non-interactively — there is no user on the other end to answer prompts. Never pause to wait for input. If you lack information that is critical to proceed, stop immediately and return a handoff explaining what is missing. A new run will be dispatched with the missing context.
</notes>

<task>
  [task brief]
</task>
```

## Guardrails

- Never dispatch without acceptance criteria. If the user was vague, that is the Maestro's problem to solve before dispatch, not the sub-agent's.
- Never copy-paste the user's raw message as the task brief. The Maestro's job is to interpret and structure, not relay.
