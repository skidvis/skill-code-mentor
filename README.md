# skill-code-mentor

Code Mentor is a standalone Agent Skill that turns raw project ideas into human-executable learning guides. It asks for the developer's experience level, explores the codebase when needed, forms a project contract, then writes phase guides with explanations, verification steps, and common pitfalls.

Repository: <https://github.com/skidvis/skill-code-mentor>

## Features

- Converts messy project ideas into structured mentor artifacts
- Calibrates guide depth for beginner, intermediate, senior, or mixed skill levels
- Uses confidence scoring before generating a contract
- Produces contracts, roadmaps, and phase guides under `docs/mentor/{project-name}/`
- Includes templates, examples, and a scoring rubric for consistent output

## Project structure

```text
.
├── SKILL.md
├── examples/
│   ├── contract-example.md
│   └── guide-example.md
└── references/
    ├── confidence-rubric.md
    ├── contract-template.md
    ├── guide-template.md
    └── skill-level-guide.md
```

This repository has no `package.json`, no package manifest, and no build step. Install it by cloning or copying the repository into the skills location used by your agent.

## Prerequisites

- Git, for cloning the repository
- Claude Code, Codex, or Pi coding agent
- A terminal and a text editor
- A target project where generated mentor guides can be written

The skill writes output artifacts into the target project, not into this skill repository, unless this repository is the current target project.

## Claude Code

Claude Code is Anthropic's terminal coding assistant. It can read project files, edit code, run commands, and use skills when they are installed in the Claude skills directory.

### Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Start Claude Code from a project directory:

```bash
claude
```

Official documentation: <https://docs.anthropic.com/en/docs/claude-code>

### Install this skill for Claude Code

Clone the repository into the Claude skills directory:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/skidvis/skill-code-mentor.git ~/.claude/skills/code-mentor
```

If Claude Code is already running, restart it so it can discover the skill.

### Use this skill with Claude Code

From the project you want to plan, start Claude Code and invoke the skill with a slash command when supported:

```text
/code-mentor I want to build a small habit tracker, but I need a human-readable plan I can follow myself.
```

If slash commands are not available in your Claude Code setup, use a plain prompt fallback:

```text
Use the code-mentor skill. I want to build a small habit tracker, but I need a human-readable plan I can follow myself.
```

## Codex

Codex is OpenAI's terminal-based coding agent. It can inspect files, propose edits, and help implement or document projects.

### Install Codex

```bash
npm install -g @openai/codex
```

Start Codex from a project directory:

```bash
codex
```

Project link: <https://github.com/openai/codex>

### Install this skill for Codex, if skills are supported

If your Codex setup supports reusable skills, clone this repository into a Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/skidvis/skill-code-mentor.git ~/.codex/skills/code-mentor
```

Then invoke the skill with a slash command when supported:

```text
/code-mentor I want to build a local-first recipe manager. Turn the idea into a contract and phase guides.
```

If slash commands are not available in your Codex setup, use a plain prompt fallback:

```text
Use the code-mentor skill to turn my project idea into a contract and phase guides.
```

### Fallback usage for Codex

If your Codex setup does not support reusable skills, keep this repository available locally and explicitly prompt Codex to read `SKILL.md`:

```text
Read ~/.codex/skills/code-mentor/SKILL.md and follow the code-mentor workflow for my project idea. Use the templates in references and the examples in examples.
```

If the repository is somewhere else, replace the path with your local clone path.

## Pi coding agent

Pi is a coding agent harness that supports skills, project instructions, tool access, and file edits through an agent interface.

### Install this skill for Pi

Preferred global skill path:

```bash
mkdir -p ~/.pi/agent/skills
git clone https://github.com/skidvis/skill-code-mentor.git ~/.pi/agent/skills/code-mentor
```

You can also reference the skill directly when starting Pi:

```bash
pi --skill ~/.pi/agent/skills/code-mentor
```

For a project-local install, clone the skill into the target project:

```bash
mkdir -p .pi/skills
git clone https://github.com/skidvis/skill-code-mentor.git .pi/skills/code-mentor
```

Using `pi install` is not recommended for this repository right now because it has no Pi package manifest and no top-level `skills/` directory.

### Use this skill with Pi

From the project you want to plan, invoke the skill with a slash command when supported:

```text
/code-mentor I want to build a CLI that organizes screenshots by date and project. Create a contract and phase guide.
```

If slash commands are not available in your Pi setup, use a plain prompt fallback:

```text
Use the code-mentor skill to turn this project idea into a contract and phase guide. Ask me for my experience level first.
```

## Output artifacts

The skill writes mentor artifacts into the target project under:

```text
docs/mentor/{project-name}/
```

Typical outputs:

```text
docs/mentor/{project-name}/contract.md
docs/mentor/{project-name}/guide-phase-1.md
docs/mentor/{project-name}/guide-phase-2.md
docs/mentor/{project-name}/guide.md
```

`guide.md` is used for small projects that do not need multiple phases.

## Workflow

1. Share a raw project idea.
2. The agent asks for your experience level.
3. The agent explores the codebase if one exists.
4. The agent scores confidence using `references/confidence-rubric.md`.
5. If confidence is below the threshold, the agent asks targeted clarification questions.
6. When confidence is high enough, the agent writes a project contract.
7. After contract approval, the agent writes a roadmap and one or more phase guides.
8. You follow the guides manually, checking each verification step as you go.

## Compatibility note

`SKILL.md` tells agents to use an `AskUserQuestion` tool for clarification and approval steps. `AskUserQuestion` is built into Claude. For Pi, it is available through the `pi-ask-user-question` npm package: <https://www.npmjs.com/package/pi-ask-user-question>.

If your agent runtime does not provide that exact tool, use the closest available interactive question mechanism. If no tool exists, the agent should ask the question in chat and wait for your answer before continuing.

## Authoring notes

- Keep generated guides human-readable, not agent task lists.
- Explain why before how, especially for beginner and intermediate developers.
- Add verification steps after meaningful actions.
- Use the templates in `references/` and match the examples in `examples/`.

## Acknowledgements

This skill is based on Nick Nisi's Ideation plugin: <https://github.com/nicknisi/claude-plugins/tree/main/plugins/ideation>.

## License

No license file is present in this repository.
