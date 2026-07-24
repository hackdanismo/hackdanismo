# Agents
An `agent` is a system that uses a `large language model (LLM)` to not just generate text but to perform an action, or pursue a goal, by deciding what actions to take. For example, an agent maybe tasked with organising a team meeting by checking the calendar for everyone in the team, choosing the best slot, creating a calendar event, send invitations and report what the agent did.

`Guardrails` are used to restrict what agents can do and when human approvals are needed.

## Coding Agent
An example of an AI coding agent is: `OpenAI Codex`. An AI coding agent can be used to inspect a code repository, read files, modify the code, run commands and test the code changes. This can work through a terminal or an IDE, such as VSCode. Other coding agents include: `Claude Code`, `GitHub Copilot` and `Cursor`. 

A `CLAUDE.md` file provides persistent instructions and project context to `Claude Code`, Anthropic's AI coding agent. Claude Code automatically reads the file and uses its contents when working in the repository. It is similar to an `AGENTS.md` file, but it is specifically recognised by Claude Code.

## AGENTS.md
The `AGENTS.md` file is a Markdown instruction file to instruct AI coding agents how to work on a project. This is similar to a `README.md` file which is used to explain the project to a human, but `AGENTS.md` is for agents.

## DESIGN_SYSTEM.md
A design system can be specified within a `DESIGN_SYSTEM.md` file. AI coding tools will not automatically treat it as authoritative unless we point to this file. For example, we can add this to the `AGENTS.md` file to point to the file:

```
## UI and design work

Follow the rules in `DESIGN_SYSTEM.md` for all user-interface changes.
```

Link to the `DESIGN_SYSTEM.md` file from the `README.md` and `AGENTS.md` files.
