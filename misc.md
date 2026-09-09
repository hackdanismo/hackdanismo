# Miscellaneous

## Glossary
+ **Incident management** in software engineering is the process of detecting, responding to, resolving, and learning from production problems that affect users, systems, or business operations.
+ **CI/CD** is the process teams use to automatically test, validate, and release software changes.
+ **Dynamic content delivery** - show different content to different users or changing the content shown based on context, data, or rules at the time the page, app, email, or experience is loaded. This is opposite to `static content`, where everyone sees the same fixed content until someone manually changes it.
+ **Technical SEO** - part of `Search Engine Optimisation (SEO)` with the focus on making websites easy for search engines like Google to `crawl`, `understand`, `index` and `rank`. It deals mainly with the website's technical foundation rather than the actual wording of pages or acquiring backlinks.
+ **Core Web Vitals** are `Google's` key metrics for measuring the real-world user experience of a webpage, especially how fast it loads, how responsive it feels, and how visually stable it is.

## AI

### Agents
In AI, an `agent` is a system that can do more than just answer a single prompt. It can take a goal, decide what steps are needed, use tools, inspect results, and continue working until the task is complete. A simple chatbot works roughly like:

```
User asks question
→ AI responds
```

An agent works more like:

```
User gives goal
→ Agent makes a plan
→ Uses tools
→ Checks results
→ Adjusts approach
→ Uses more tools if needed
→ Produces final result
```

For example, we might tell a coding agent: `"Fix the broken checkout flow."` The agent could then inspect the codebase, identify the relevant files, read logs, modify code, add or update tests, run the test suite, fix failures, and prepare the final changes. That is why tools like `Claude Code`, Codex-style coding agents, `Cursor agents`, and similar systems are called `agents`: they can operate across multiple steps and interact with an environment rather than only generating text. Agents often have access to tools such as a terminal, web browser, Git, files, databases, APIs, issue trackers, or cloud services.

A useful way to think about it is:

+ **LLM** = the brain
+ **Tools** = the hands
+ **Agent** = the brain + tools + decision-making loop

For software engineering, a coding agent might follow a loop like:

```
Goal: Add a new feature

1. Read AGENTS.md
2. Inspect the codebase
3. Identify relevant architecture
4. Edit files
5. Run unit tests
6. Run integration tests
7. Fix failures
8. Run build
9. Review diff
10. Finish
```

This connects directly to `AGENTS.md`: that file tells the agent how it should behave inside that specific repository. The key difference is autonomy. An AI assistant mainly says, "Here's how you could do it." An agent can often say, "I’ll do the steps required to achieve it."

### AGENTS.md and CLAUDE.md
`AGENTS.md` and `CLAUDE.md` are essentially instruction files for AI coding agents. You put them in a code repository so the AI understands how it should work on that specific project. Think of them as a `README.md` or `CONTRIBUTING.md` written specifically for AI assistants.

`CLAUDE.md` is the native project-instructions file for Claude Code. Claude Code automatically loads it when working in a repository, so teams use it to tell Claude about the codebase, conventions, commands, architectural rules, testing expectations, and things it should or shouldn't change.

```markdown
# CLAUDE.md

## Project
This is an Astro + TypeScript marketing website.

## Commands
npm run dev
npm run test
npm run build

## Coding standards
- Use TypeScript strict mode.
- Prefer server-side rendering where possible.
- Do not add dependencies without approval.
- Run tests before completing a task.

## CMS
Content comes from Sanity.
Do not hardcode marketing copy into components.

## Git
- Create focused commits.
- Do not modify unrelated files.
- Never push directly to main.
```

Now if you tell Claude Code: `Add a new pricing page.` Claude doesn't have to rediscover all those rules from scratch. It already knows that the site uses `Astro`, content belongs in `Sanity`, `TypeScript` rules apply, and tests/builds should run.

`AGENTS.md` serves much the same purpose, but it is designed as a more tool-independent convention. Various coding agents support it, including tools in the Codex/Cursor/Gemini ecosystem.

Many teams therefore don't maintain two independent versions, because they can easily drift apart. Instead, they might make `AGENTS.md` the source of truth and have `CLAUDE.md` reference it.

```markdown
# CLAUDE.md

@AGENTS.md
```

These files become particularly useful for agentic coding, because an AI agent may be doing much more than autocomplete. It might autonomously:

+ Read ticket
+ Explore repository
+ Design solution
+ Modify 8 files
+ Write tests
+ Run tests
+ Fix failures
+ Run build
+ Prepare PR

Without project instructions, the agent has to infer things like:

+ Which framework should I use?
+ Can I add dependencies?
+ Which tests should I run?
+ What's the architecture?
+ Should content come from Sanity?
+ Can I modify the database schema?
+ How should errors be handled?
+ What constitutes "done"?

`AGENTS.md` gives it those answers before it starts.

Now you could ask an AI agent: `Add an enterprise landing-page template with HubSpot form integration and GA4 conversion tracking.` Because of `AGENTS.md`, it already knows how that organisation expects the work to be implemented.

### MCP
`MCP` stands for `Model Context Protocol`. It is an open standard that lets AI applications connect to external tools, data sources, and services in a consistent way. MCP is like a standard plug/socket for AI tools. Without MCP, every AI application might need a custom integration for GitHub, Slack, databases, Google Drive, internal APIs, and so on. With MCP, those systems can expose capabilities through an MCP server, and compatible AI applications can connect to them using the same protocol.

AI tool examples can include `Claude Code`, `Cursor`, `VS Code`, or another agent application. Current MCP SDK documentation describes hosts such as Claude Code, VS Code, Cursor, and custom applications connecting to MCP servers. An MCP server exposes capabilities that the AI can access. For example, a GitHub-oriented MCP server might expose operations such as:

```
search_repository()
read_issue()
create_issue()
read_pull_request()
comment_on_pull_request()
```

The AI doesn't need to understand all of GitHub's REST API itself. It communicates with the MCP server through the common MCP interface.

Imagine you give Claude Code this task: `"Find the Jira ticket for the broken pricing page, fix the bug, and open a GitHub PR."`. Without integrations, Claude might only have access to your local code. With MCP, you could connect it to:

```
Claude Code
     │
     ├── Jira MCP server
     │       ↓
     │   Search tickets
     │
     ├── GitHub MCP server
     │       ↓
     │   Read/create PRs
     │
     └── Database MCP server
             ↓
         Query data
```

The agent could potentially:

```
1. Search Jira
2. Read the ticket
3. Inspect repository
4. Modify code
5. Run tests
6. Create GitHub PR
7. Link the PR to the Jira ticket
```

```
LLM
 │
 ▼
Agent
 │
 ▼
MCP
 │
 ├── GitHub
 ├── Slack
 ├── Jira
 ├── PostgreSQL
 ├── Google Drive
 └── Internal APIs
```

So MCP is one of the ways the agent gets access to its tools. Without MCP, though, you may end up building:

```
Claude → custom GitHub integration
Claude → custom Slack integration
Claude → custom database integration

Cursor → different GitHub integration
Cursor → different Slack integration
Cursor → different database integration
```

## Incident management
An incident could be anything from "the website is completely down" to "checkout conversions suddenly dropped because analytics stopped firing." Incident management means being able to help own production reliability alongside CI/CD, deployments, performance, uptime, and engineering standards.

A typical incident lifecycle looks like this:

+ **Detect** — monitoring, alerts, user reports, analytics anomalies
+ **Triage** — determine severity, scope, and likely cause
+ **Respond** — assign an incident owner, communicate status, mitigate impact
+ **Resolve** — fix, roll back, disable a feature, scale infrastructure, etc.
+ **Recover** — verify the service is healthy again
+ **Review** — perform a post-incident review / postmortem and prevent recurrence

The team might see an alert in `Datadog`, `Sentry`, or `New Relic`, for example as these are tools we can use to monitor for changes.

Sentry + Datadog → PagerDuty → Slack/incident.io → GitHub/Vercel for rollback → postmortem

A more website-specific example would be a `Sanity CMS` publishing incident. Marketing publishes new content, but pages begin failing because a content model change wasn't backwards compatible. The engineer might temporarily revert the schema or add defensive rendering, restore the site, then improve schema validation and preview/testing workflows.

Another example is `Core Web Vitals` suddenly deteriorating. Suppose an external marketing script causes LCP to jump from 1.8 seconds to 5 seconds. Monitoring or RUM data detects the regression. The web engineer identifies the third-party script, removes or lazy-loads it, verifies performance recovery, and then introduces performance budgets to prevent similar regressions.

## CI/CD
**CI = Continuous Integration**. Developers regularly merge code into a shared repository such as GitHub. Every change automatically runs checks to catch problems early—for example TypeScript compilation, linting, unit tests, integration tests, accessibility checks, or a production build.

A simple CI flow might look like:

Developer opens PR → GitHub Actions runs tests → TypeScript check passes → build succeeds → reviewer approves → code is merged

**CD = Continuous Delivery** or **Continuous Deployment**. Once the code passes CI, the deployment pipeline prepares and releases it to an environment such as staging or production.

There are two common meanings:

+ **Continuous Delivery**: the software is always ready to deploy, but a person may approve the production release.
+ **Continuous Deployment**: every change that passes the pipeline is automatically deployed to production.

A deployment pipeline is the full automated path from code change to running production software.

For a web application, it might look like:

GitHub → CI checks → build → preview deployment → staging → automated tests → production deployment → monitoring

For example, imagine you're working on an Astro + TypeScript website. You push a branch and open a pull request. GitHub Actions might run:

+ npm install
+ ESLint
+ TypeScript type checking
+ Unit tests
+ Astro production build
+ Lighthouse/Performance tests

If everything passes, Vercel might automatically create a preview deployment so the team can test the actual website before merging. Once the PR is approved and merged into `main`:

+ main branch
+ CI tests
+ Production build
+ Deploy to Vercel
+ Smoke tests
+ Monitor errors/performance

If something goes wrong, the team might roll back to the previous deployment.

Common **CI/CD** tools include `GitHub Actions`, `GitLab CI/CD`, `CircleCI`, `Jenkins`, `Azure DevOps`, `Bitbucket Pipelines`, and `Buildkite`. Deployment platforms commonly include `Vercel`, `Netlify`, `AWS`, `Cloudflare`, `Azure`, `Google Cloud`, and `Kubernetes-based infrastructure`.

A mature website pipeline could include:

+ Feature branch
+ Pull Request
+ Lint + TypeScript
+ Unit / integration tests
+ Astro / Next.js build
+ Sanity schema validation
+ Preview environment
+ Automated accessibility tests
+ Lighthouse / Core Web Vitals checks
+ Human PR review
+ Merge
+ Production environment
+ Smoke tests
+ Sentry / Datadog monitoring

Good CI/CD makes it safe and fast for engineers to ship changes. Expect automated quality checks on every pull request, reproducible builds, preview or staging environments where appropriate, controlled production deployments, and monitoring after release. The goal isn't simply automation—it's reducing deployment risk while allowing the team to release frequently.

The key distinction is: **CI asks "Is this change safe to merge?"**; **CD asks "Can we safely get this change into production?"**.

## Tests
**Unit tests** test a small piece of code in isolation, usually one function, component, or module. They answer: "Does this individual piece behave correctly?" For example, if you have:

```typescript
function calculateDiscount(price: number, discount: number) {
  return price - price * discount
}
```

A unit test might check that `calculateDiscount(100, 0.2)` returns `80`. Common tools include `Vitest`, `Jest`, `Mocha`, and for React components, `React Testing Library`.

**Integration tests** check that multiple parts of the system work together correctly. They answer: "Do these pieces interact properly?" For example, you might test that a website form submits data, calls an API, writes to a database or CRM, and returns the correct response. an integration test might verify:

+ Website form
+ API endpoint
+ HubSpot integration
+ Lead has been created successfully

Or it could test that an `Astro/Next.js` page correctly fetches content from `Sanity` and renders it. Common tools include `Playwright`, `Cypress`, `Vitest/Jest`, and API tools such as `Supertest`.

**Smoke tests** are quick checks performed after a deployment to make sure the most important parts of the application are basically working. They answer: "Is the system alive and usable?" They are deliberately shallow rather than exhaustive.

For a marketing website, smoke tests might check:

+ Homepage loads ✓
+ Pricing page loads ✓
+ Navigation works ✓
+ Signup form opens ✓
+ API responds ✓
+ No HTTP 500 errors ✓

If those fail immediately after a production deployment, the team might stop the release or roll back.

A unit test checks that the email-validation function rejects an invalid email. An integration test checks that submitting the form sends the correct payload to HubSpot. A smoke test checks that after deployment, the form page loads and a basic submission succeeds.

In a CI/CD pipeline, you might therefore have:

+ Pull Request
+ Unit tests
+ Integration tests
+ Build
+ Deploy
+ Smoke tests
+ Production considered healthy

## Technical SEO
Typical `technical SEO` work includes:

+ **Crawlability**: ensuring search engines can access important pages.
+ **Indexing**: making sure the right pages appear in Google's index.
+ **Site speed**: improving loading performance and Core Web Vitals.
+ **Mobile friendliness**: ensuring the site works properly on phones and tablets.
+ **Site architecture**: creating a logical structure and strong internal linking.
+ **HTTPS/security**: using secure connections correctly.
+ **XML sitemaps**: helping search engines discover pages.
+ **robots.txt**: controlling which areas search engines can crawl.
+ **Canonical tags**: preventing duplicate-content problems.
+ **Redirects and broken links**: fixing 404s, redirect chains, and incorrect redirects.
+ **Structured data/schema**: helping search engines understand things like products, reviews, articles, and organizations.
+ **JavaScript SEO**: ensuring content generated by JavaScript can still be discovered and indexed.

### robots.txt
A `robots.txt` file is a small text file on a website that gives instructions to search-engine crawlers about which parts of the site they should or shouldn't crawl. It usually lives at: `https://example.com/robots.txt`. The `robots.txt` file controls crawling, not necessarily indexing. A page blocked in `robots.txt` can sometimes still appear in search results if Google discovers the URL elsewhere. 

Don't use the `robots.txt` to protect private or sensitive information—it is publicly accessible.

A simple example:

```
User-agent: *
Disallow: /admin/
Allow: /
```

+ `User-agent: *` = applies to all crawlers
+ `Disallow: /admin/` = don’t crawl the /admin/ section
+ `Allow: /` = crawling is allowed elsewhere

It can also point search engines to the `XML sitemap`:

```
Sitemap: https://example.com/sitemap.xml
```

Use `noindex` when we want search engines to crawl a page but not include it in search results. That means: don't index this page, but you can still follow its links.

```html
<meta name="robots" content="noindex, follow">
```

Examples of when to use `noindex`:

+ Thank-you / confirmation pages after a form submission or purchase
+ Internal search results pages
+ Login, account, cart, or checkout pages
+ Staging or test pages that are accidentally public
+ Low-value utility pages that users need but that shouldn’t rank
+ Duplicate or near-duplicate pages where you do not want that specific version indexed
+ Temporary campaign pages that no longer need organic visibility

### Canonical tag
A `canonical tag` tells search engines which URL you consider the main or preferred version of a page when multiple URLs contain the same or very similar content. For example, these URLs might all show essentially the same product page:

+ `example.com/shoes`
+ `example.com/shoes?color=black`
+ `example.com/shoes?utm_source=google`

So the canonical tag would be added to the `<head>` section of the page to signal to `Google` that `https://example.com/shoes` is the primary URL:

```html
<link rel="canonical" href="https://example.com/shoes">
```

## Core Web Vitals
`Core Web Vitals` are Google's key metrics for measuring the real-world user experience of a webpage, especially how fast it loads, how responsive it feels, and how visually stable it is. The three main Core Web Vitals are:

+ `LCP — Largest Contentful Paint`: measures loading performance. It tracks how long the main visible content takes to appear.
Good: 2.5 seconds or less
+ `INP — Interaction to Next Paint`: measures responsiveness. It tracks how quickly the page reacts when a user clicks, taps, or types.
Good: 200 ms or less
+ `CLS — Cumulative Layout Shift`: measures visual stability. It checks whether elements unexpectedly move around while the page is loading.
Good: 0.1 or less

For example, if a page loads its main image slowly, buttons feel delayed when clicked, or text jumps down because an ad suddenly appears, its Core Web Vitals may be poor. They matter because they affect user experience and are also part of Google’s page-experience signals used in Search.
