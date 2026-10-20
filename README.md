# GooseWorks plugin — review candidate

This package contains the current GooseWorks entry skills and its hosted MCP connection. The Hermes catalog candidate uses the portable Agent Plugins format. Full signed-in research, paid generation and final video delivery still need manual end-to-end testing.

## What is included

- Research and data work through `gooseworks`.
- Image ads through `goose-ads`.
- Product photos through `goose-product-photos`.
- Video planning and the existing execution handoff through `goose-video` and `make-custom-video`.

Skills come from the same source as the CLI. Their workflow bodies are unchanged; custom frontmatter fields are moved into standard string metadata for portable skill loaders. More detailed recipes are fetched from GooseWorks when needed. The installed entries follow the server's current instructions and stay on the same connection.

## Connect and use

Install with a client that supports the Agent Plugins 1.0.0 format, remote Streamable HTTP MCP and interactive sign-in. The host manages authorization; sign in to GooseWorks when prompted. A client that only supports native plugin formats needs a separately tested adapter.

Ask for a brand research task, image ads or product photos. Existing onboarding sets up or selects your brand. Paid work requires your approval of the displayed credit total; an existing GooseWorks credit balance is required. Read returned results and any reported failure before treating a job as complete.

## Video execution

Video follows the existing workflow and each answer's `next_step`. When it supplies a make command, its last line says who starts it. An agent with a terminal may run that exact command after the required approval. In a hosted chat without a terminal, the customer runs the supplied command locally. A command or approved plan alone is not a finished video.

Sandbox execution is not included in this package. Its availability and credit cost have not been established. Provider-specific manual tests must cover the full local handoff and the actual final output before claiming that workflow works there.

## Claude installation and review

The native Claude adapter uses `.claude-plugin/plugin.json`, `.mcp.json` and the same five skills. With this repository checked out locally, load it in Claude Code:

```sh
claude --plugin-dir /absolute/path/to/agent-plugin
```

Sign in to the GooseWorks MCP connection when Claude prompts. Claude manages its OAuth credentials. Installing the plugin does not authorize an account or approve paid work. For chat and Cowork, installation through the public directory depends on Anthropic accepting the plugin and its associated MCP connector.

Directory publication is pending. The current package includes AI image and video workflows, so its eligibility must be resolved against the [Software Directory Policy](https://support.claude.com/en/articles/13145358-anthropic-software-directory-policy) before publication. Local schema validation does not establish policy acceptance, sign-in or end-to-end workflow behavior.

## Hermes installation and approvals

Requires Hermes 0.22.2 or later. After the catalog entry is accepted, install with `hermes plugins install gooseworks`, then explicitly enable the plugin with `hermes plugins enable gooseworks`.

The Hermes extension in `plugin.json` marks the `gooseworks` MCP server as `untrusted`. Hermes asks before calls that lack `readOnlyHint: true` and refuses those calls in unattended runs where nobody can approve. GooseWorks also requires approval of the quoted credit amount before paid work. Neither approval replaces the other. The plugin does not override Hermes guards.

The package provides five namespaced skills and one remote MCP server. It registers no native Python tools, hooks, middleware or Desktop UI. No Python dependencies, local MCP subprocess or API keys are bundled.

## Data, network and execution disclosures

- **Network and account data:** the remote MCP client connects to `https://mcp.gooseworks.ai/mcp`. Requests and user-selected brand material go to GooseWorks. Depending on the requested task, GooseWorks reads saved brand knowledge, products, uploaded assets and connected advertising accounts, researches public web/social sources, or sends inputs to its generation providers. These services operate remotely; their current schemas and workflow instructions are supplied by GooseWorks rather than frozen in this package.
- **Sign-in and credentials:** a GooseWorks account and interactive sign-in are required. Hermes's remote MCP client manages OAuth and its token storage. The package contains no secrets and no code that reads `.env`, another application's login, browser profiles or credential stores. Connected third-party accounts are managed by GooseWorks. Paid work requires a GooseWorks credit balance.
- **Local commands and files:** research, image ads and product photos use the hosted MCP. Video workflows may return a production command for the user's computer. Review the exact returned command, paths and destinations before allowing Hermes to execute it. The command can download instructions/assets, read the selected source files, write local media and upload production outputs to GooseWorks; it runs with the local user's authority and can take a long time. Local execution is a separate approval boundary and is not a sandbox. No fixed production command or executable is shipped in this package.
- **Background jobs:** remote research and generation may continue after a tool response; the skills poll saved job IDs. This package starts no daemon or scheduled task on its own.
- **Updates and reporting:** the package has no self-updater or telemetry code. Hermes controls updates to catalog-pinned files. GooseWorks records account activity and credit usage as part of the hosted service; see its privacy policy. Installing the plugin does not enroll the user in optional telemetry.

## Review status

Passing package checks proves that the files and archive match the canonical sources. It does not prove installation, sign-in, brand onboarding, paid approvals, image delivery or video completion in a provider's app. Those require manual testing with the actual candidate.

Support: https://docs.gooseworks.ai/help-center/troubleshooting

Privacy: https://gooseworks.ai/privacy · Terms: https://gooseworks.ai/terms
