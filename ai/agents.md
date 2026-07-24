# Agents
An `agent` is a system that uses a `large language model (LLM)` to not just generate text but to perform an action, or pursue a goal, by deciding what actions to take. For example, an agent maybe tasked with organising a team meeting by checking the calendar for everyone in the team, choosing the best slot, creating a calendar event, send invitations and report what the agent did.

`Guardrails` are used to restrict what agents can do and when human approvals are needed.

## Claude

### Claude Code
To start `Claude Code`, use the command: `claude` in the terminal.

## Google Gemini
`Antigravity CLI` is the replacement for `Gemini CLI`. Starting **June 18, 2026**, `Gemini CLI` stopped serving requests for the Gemini Code Assist for individuals, Google AI Pro, and Google AI Ultra tiers, and affected users should migrate to `Antigravity CLI`. `Antigravity CLI` is a newer implementation and part of Google's broader Antigravity agent platform.

[https://antigravity.google/product/antigravity-cli](https://antigravity.google/product/antigravity-cli)

### Install Antigravity CLI

```shell
# macOS & Linux
$ curl -fsSL https://antigravity.google/cli/install.sh | bash

# Windows Powershell
$ irm https://antigravity.google/cli/install.ps1 | iex

# Windows CMD
$ curl -fsSL https://antigravity.google/cli/install.cmd -o install.cmd && install.cmd && del install.cmd
```

Once installed, run `agy` to start the CLI. Follow the setup steps. A valid Google account will be needed.

<img width="579" height="481" alt="Screenshot 2026-07-24 at 14 07 22" src="https://github.com/user-attachments/assets/8adefe22-0951-4422-828a-ec6e6170e3da" />

Find the `Antigravity CLI` install files:

```shell
$ open ~/.gemini
```

Antigravity supports the `Model Context Protocol (MCP)`, an open standard that lets AI agents and editors securely connect to local developer tools, databases, file parsers, and external remote APIs.

#### Antigravity and Webflow
Begin by creating a folder/directory and inside this folder add a directory named `.agents`. This will contain the `mcp_config.json` file.

```shell
$ mkdir your-project
```

We may want to also set the `Node` version using `nvm` and using an `.nvmrc` file:

```
24.18.0
```

Which can then be used to set the Node version:

```shell
$ nvm use
```

The file structure:

```
your-project/
├── .agents/
│   └── mcp_config.json
├── src/
├── .nvmrc
├── package.json
└── README.md
```

The `mcp_config.json` file:

```json
{
  "mcpServers": {
    "webflow": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.webflow.com/mcp"
      ]
    }
  }
}
```

Run the following command to open `Antigravity CLI`:

```shell
$ agy

# Check the MCP that have been installed:
$ /mcp
```

This will list out the MCP listed and will also show if Webflow is connected/setup once authorisation has been provided.

`mcp-remote` currently pulls a version of undici that expects the global File API available in newer Node releases. Node 19 does not provide the required environment, producing ReferenceError: File is not defined. Node 19 is also end-of-life; use Node 22 or Node 24 LTS instead.

<img width="1548" height="777" alt="Screenshot 2026-07-24 at 14 35 24" src="https://github.com/user-attachments/assets/e7964ffe-9bc6-4113-9721-74aba9c9b11a" />

I haven't yet been able to get Antigravity CLI to work with Webflow MCP.

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
