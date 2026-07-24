# Agents
An `agent` is a system that uses a `large language model (LLM)` to not just generate text but to perform an action, or pursue a goal, by deciding what actions to take. For example, an agent maybe tasked with organising a team meeting by checking the calendar for everyone in the team, choosing the best slot, creating a calendar event, send invitations and report what the agent did.

`Guardrails` are used to restrict what agents can do and when human approvals are needed.

## Claude

### Claude Code
To start `Claude Code`, use the command: `claude` in the terminal.

The fil structure to add a skill using a `SKILL.md` file:

```
your-project/
└── .claude/
    └── skills/
        └── webflow-copy-update/
            └── SKILL.md
```

A `SKILL.md` file would look like this:

```
---
name: webflow-copy-update
description: Create a Webflow page branch and safely update approved page copy.
version: 1.0.0
webflow_mcp_version: 2.0.1
required_mcp_servers:
  - webflow
---

# Webflow Copy Update

Use this skill when updating text content on an existing Webflow page. Do not use it for layout, styling, component, or CMS-schema changes.

## Objective

Make copy changes on a page branch without modifying the production page directly.

## Required inputs

- Webflow site name or site ID
- Page name, slug, or page ID
- Existing copy to locate
- Replacement copy
- Locale, when applicable

## Workflow

1. Confirm the Webflow MCP server is connected and authenticated.
2. List accessible sites and resolve the requested site.
3. Resolve the requested page.
4. Check whether the page supports branching.
5. Check whether an appropriate branch already exists.
6. Create a new page branch when necessary.
7. Switch the working context to the branch.
8. Read the current page content before changing anything.
9. Match the target copy exactly or identify the element containing it.
10. Update only the specified text.
11. Read the page content again and verify:
    - the replacement text is present;
    - unrelated content is unchanged;
    - the production page was not modified.
12. Return:
    - site name;
    - page name;
    - branch name or ID;
    - old copy;
    - new copy;
    - verification status.

## Safety rules

- Never edit the primary page when page branching is available.
- Never publish, merge, or delete a branch without explicit approval.
- Never rewrite surrounding copy unless explicitly requested.
- Stop if more than one element matches the requested text.
- Stop if the requested page cannot be identified confidently.
- Preserve links, formatting, element structure, and component bindings.
- For localized pages, confirm the locale before editing.
- Read before writing and verify after writing.

## Failure handling

If branching is unavailable:

1. Do not modify the production page.
2. Explain whether the limitation is caused by the Webflow plan,
   permissions, page type, or MCP capability.
3. Ask for explicit approval before using a non-branch workflow.

## Example request

Update the homepage hero heading from:

"Build better customer experiences"

to:

"Create customer experiences that convert"

Make the change on a new branch called:

`copy/homepage-hero-conversion`
```

This is metadata:

```
---
name: webflow-copy-update
description: Create a Webflow page branch and safely update the approved page copy.
version: 1.0.0
webflow_mcp_version: 2.0.1
required_mcp_servers:
  - webflow
---
```

+ `name`: this is the skill's display name in the skills listing, would be invoked by: `/webflow-copy-update`.
+ `description`: explains what the skill does and helps `Claude` decide when to load it automatically. This is the most important metadata line.
+ `version`: a custom version number for your own release management. It can help maintainers understand which revision they have installed, but Claude Code will not use it to select, update, or validate the skill.
+ `webflow_mcp_version`: another custom metadata field, intended to record the Webflow MCP version against which the instructions were written.

To make it operational, add a body instruction requiring `Claude` to check compatibility, or include that requirement in plugin documentation.

+ `required_mcp_servers`: introduces the list. `- webflow` adds one item named `webflow`.

However, `required_mcp_servers` is not a documented Claude Code skill field. By itself, it will not establish or require the connection.

#### Required Inputs
The `Required Inputs` introduces the information `Claude` needs before executing the workflow. `"Required"` implies `Claude` should obtain or resolve these values before changing anything. You should also specify what `Claude` must do when an input is missing: `ask the user`, `search Webflow`, or `stop`.

This is a further improved `SKILL.md` file for Claude:

```
---
name: webflow-copy-update
description: Create a Webflow page branch and safely update the approved page copy.
version: 1.0.0
webflow_mcp_version: 2.0.1
required_mcp_servers:
  - webflow
---

# Webflow Copy Update

Use this skill when tasked with updating copy on an existing Webflow page.

## Objective

Make copy changes on a page branch without modifying the production page directly.

## Required inputs
- Webflow site name or site ID
- Page name, slug, or page ID
- Existing copy to locate
- Replacement copy
- Locale, when applicable

## Gathering inputs

Review the user's request and the existing conversation before taking any action.

Extract any required inputs that the user has already provided.

If any required input is missing or ambiguous, ask the user for the missing information before creating a branch or modifying Webflow content.

For the target page, accept any of the following:

- page name;
- page URL;
- page slug;
- page ID.

Do not ask for a page ID when the user has already supplied a unique page name, URL, or slug.

Do not ask for information that the user has already provided.

Do not create a branch or modify Webflow content until:

- the Webflow site has been identified confidently;
- the target page has been identified confidently;
- the existing copy has been confirmed;
- the replacement copy has been confirmed;
- the locale has been confirmed when localization applies.

## Workflow

1. Review the user's request and the existing conversation.
2. Gather any missing or ambiguous required inputs.
3. Confirm the Webflow MCP server is connected and authenticated.
4. List accessible sites and resolve the requested site.
5. Check whether the page supports branching.
6. Check whether an appropriate branch already exists.
7. Create a new page branch when necessary.
8. Switch the working content to the branch.
9. Read the current page content before changing anything.
10. Match the target copy exactly or identify the element containing it.
11. Read the page content again and vetify:
  - the replacement text is present;
  - unrelated content is unchanged;
  - the production page was not modified.
12. Return:
  - site name;
  - page name;
  - branch name or ID;
  - old copy;
  - new copy;
  - verification status.

## Safety Rules

- Never edit the primary page when page branching is available.
- Never publish, merge, or delete a branch without explicit approval.
- Never rewrite surrounding copy unless explicitly requested.
- Stop if more than one element matches the requested text.
- Stop if the requested page cannot be identified confidently.
- Preserve links, formatting, element structure, and component bindings.
- For localized pages, confirm the locale before editing.
- Read before writing and verify after writing.

## Failure handling

If branching is unavailable:

1. Do not modify the production page.
2. Explain whether the limitation is caused by the Webflow plan, permissions, page type, or MCP capability.
3. Ask for explicit approval before using a non-branch workflow.
```

So a test prompt to run the SKILL.md would be:

```
webflow-copy-update On the Extension Demo site, update the homepage Heading Jumbo from "Grow your business" to "Updated with Claude Code"
```

+ `.mcp.json` connects Claude Code to Webflow.
+ `.claude/skills/webflow-copy-update/SKILL.md` tells Claude how to perform the copy-update workflow.

To setup the `Webflow MCP` with `Claude` that will generate the `.mcp.json` file in the project folder:

```shell
$ claude mcp add --transport http webflow https://mcp.webflow.com/mcp -s project
```

<img width="578" height="408" alt="Screenshot 2026-07-24 at 16 33 02" src="https://github.com/user-attachments/assets/62e0cb7b-3c75-42e4-85a9-243ce8260468" />

Open `Claude Code` and then view the `MCP servers` listed:

```shell
$ claude
$ mcp/
```

```
your-project/
└── .claude/
    settings.local.json
    └── skills/
        └── webflow-copy-update/
            └── SKILL.md
    .mcp.json
    .nvmrc
```

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
