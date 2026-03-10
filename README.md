# Agent Starter Kit

> You speak naturally, a Maestro agent breaks it into tasks and routes each one to a specialized AI model.

The scaffold for your ultra-personalized, **multi-model** AI harness in **pure natural language**. The smartest model orchestrates the workflow while cheaper/faster ones handle the less complicated bits, **extending your premium coding plan (such as Claude Code)** while fresh eyes enhance the output.

It's model-agnostic: orchestrate from Claude, plan on GLM, review on Qwen, or any combination — any CLI scoring 1300+ ELO on [GDPval-AA](https://artificialanalysis.ai/evaluations/gdpval-aa) can run the show.

## Setup

1. Clone into `.agents/` inside your project:

   ```bash
   cd /path/to/your/project
   git clone git@github.com:ntorga/agent-starter-kit.git .agents
   ```

2. Symlink the entry file to the project root:

   ```bash
   ln -s .agents/AGENTS.md AGENTS.md
   ```

3. Start the AI agent (e.g., `claude`, or whatever CLI you use).
4. Say **"Please comply with AGENTS.md."** — this boots the Maestro and loads the framework.
5. From there, speak naturally. The Maestro orchestrates everything. On first run, it automatically dispatches the Contextualizer to map the codebase.
6. Customize — add personas, rules, skills, and providers to fit your project (see Customization below).

## How It Works

The **Maestro** is the conductor. It receives user requests, decomposes them, and dispatches work to specialized personas:

- **Architect** — plans implementations, defines before/after states
- **Coder** — writes software following the plan
- **Reviewer** — checks work for correctness and quality
- **Contextualizer** — documents project structure for orientation

Each persona has an identity (who they are), a playbook (what they do), a handoff format (what they deliver), and red lines (what they must not do). Each persona also declares a `preferredModel` — the Maestro uses this to route work to the right provider automatically.

## Why This Over Other Harnesses?

Frameworks like GSD, HumanLayer, and OpenDev are software — they require a programming language, dependencies, and runtime integration. This kit is **pure natural language**. Every persona, rule, and skill is a Markdown file. There is no code to install, no SDK to learn, no build step to maintain.

That makes it extensible by anyone who can write a sentence. Add a persona by writing a `.md` file. Change a rule by editing a line. Swap a provider by updating a table row. The entire framework is readable, forkable, and understandable without knowing any programming language.

## Structure

```
personas/    Specialized AI roles (who does the work)
rules/       Constraints organized by authority level
skills/      Reusable procedures and protocols
```

## Rules Hierarchy

- **Commandments** — absolute, never bypassed
- **Edicts** — authoritative within scope, not bent
- **Counsel** — wise guidance, may be deviated from with justification

## Skills

Skills codify procedures that personas reference. They answer "how to do X" so personas can focus on "what to do."

- **boot** — session startup sequence
- **dispatch** — how the Maestro assembles and sends work to personas.
- **task-tracking** — file-based to-do for multi-step work

## Customization

- **Dispatch** — edit `skills/dispatch.md` to match your CLI agents. The Providers table and CLI Dispatch section are pre-configured for Claude Code and [opencode](https://github.com/anomalyco/opencode) (Qwen). Add rows for any other provider/model you use.
- Add new personas to `personas/` following the schema in `personas/README.md`
- Add rules to `rules/commandments/`, `rules/edicts/`, or `rules/counsel/`
- Add skills to `skills/` following the schema in `skills/README.md`
- Modify existing files to match your project's needs

Each directory has a README with the full schema definition.

## Best Practices

- **Keep premium models as the orchestrator.** The Maestro (Claude, Codex) makes routing decisions and manages context — these are short, high-leverage interactions worth the cost. Token-heavy roles like Architect and Reviewer can be delegated to capable but cheaper models (Qwen, Kimi) to reduce consumption without sacrificing quality.
