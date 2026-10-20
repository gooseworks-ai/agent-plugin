# GooseWorks plugin

GooseWorks for Claude Code and other agents: brand research, image ads, product photos and video ad planning, through five skills and the hosted GooseWorks MCP connection. Adapters are included for Claude, Gemini CLI and Hermes.

## Install in Claude Code

Add the GooseWorks marketplace once, then install the plugin:

```sh
claude plugin marketplace add gooseworks-ai/gooseworks
claude plugin install gooseworks@gooseworks
```

Sign in to GooseWorks when Claude prompts. To get new versions, run `claude plugin update gooseworks@gooseworks`, or turn on auto-update for the `gooseworks` marketplace.

On claude.ai, Claude Desktop or Cowork, add `https://mcp.gooseworks.ai/mcp` as a custom connector to use the GooseWorks tools. The skills in this package are not added that way.

**Before you start:** the plugin can read and store personal data supplied for a task. Approved uploads and local video work can send files outside the MCP connection. Saved data can be kept for longer than 30 days. The plugin is for adult business users. Details are under [Data, network and execution disclosures](#data-network-and-execution-disclosures).

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

## Claude adapter

The native Claude adapter uses `.claude-plugin/plugin.json`, `.mcp.json` and the same five skills. The marketplace entry in [gooseworks-ai/gooseworks](https://github.com/gooseworks-ai/gooseworks) points at this repository, so the install commands above always fetch it from here.

To try a local checkout without the marketplace, load it for one session:

```sh
claude --plugin-dir /absolute/path/to/agent-plugin
```

Claude manages the OAuth credentials for the MCP connection. Installing the plugin does not authorize an account or approve paid work.

This plugin is not listed in Anthropic's directory. The marketplace above is the supported way to install it.

## Hermes installation and approvals

Requires Hermes 0.22.2 or later. After the catalog entry is accepted, install with `hermes plugins install gooseworks`, then explicitly enable the plugin with `hermes plugins enable gooseworks`.

The Hermes extension in `plugin.json` marks the `gooseworks` MCP server as `untrusted`. Hermes asks before calls that lack `readOnlyHint: true` and refuses those calls in unattended runs where nobody can approve. GooseWorks also requires approval of the quoted credit amount before paid work. Neither approval replaces the other. The plugin does not override Hermes guards.

The package provides five namespaced skills and one remote MCP server. It registers no native Python tools, hooks, middleware or Desktop UI. No Python dependencies, local MCP subprocess or API keys are bundled.

## Data, network and execution disclosures

### What the plugin reads and stores

GooseWorks reads and stores account details such as names and email addresses. Depending on the requested task, it also processes brand knowledge, research about people and companies, prospect/contact information, selected files and images, prompts, project records, generated media and connected advertising data. These inputs can contain personal data, including names, email addresses and identifiable people in images or video.

The skills can save brand facts, research, uploaded files and project outputs in the GooseWorks workspace. They can read saved data and connected accounts for the requested work. Claude sends the task inputs used in tool calls; this package does not automatically export the user's full Claude conversation history or read unrelated local files.

### Services and destinations

The declared connector is `https://mcp.gooseworks.ai/mcp`. It receives tool arguments and selected task material and returns results and current workflow instructions. Other transfers can occur during an approved workflow:

| Service or destination | How it is used | Data involved |
| --- | --- | --- |
| GooseWorks API at `https://api.gooseworks.ai` and skill hub at `https://skills.gooseworks.ai` | Optional terminal workflows fetch recipes, run API requests and report production progress outside the MCP transport | Requested queries, project identifiers, selected inputs, production metadata and results; authentication is sent to the relevant GooseWorks service |
| GooseWorks-managed Amazon S3 storage, through the exact signed upload/download URL returned for the operation | A client or approved local video command can transfer file bytes directly to storage, outside the MCP transport | User-selected source files, images, audio, video and completed outputs; the video client sends the upload's specified headers rather than its GooseWorks login token |
| Source websites and media hosts named in the requested workflow | Research and local production may retrieve source pages, assets or generated media | Requested URLs, search queries and ordinary request metadata; local source files are not automatically uploaded to every source website |

GooseWorks also forwards task inputs to downstream services when the requested work needs them. Integrations include fal (`queue.fal.run`, `rest.fal.ai`) for image/video generation and asset storage, ElevenLabs (`api.elevenlabs.io`) for audio, and Higgsfield (`platform.higgsfield.ai`) for supported media tasks. Research and data calls can use managed providers such as Apify and ScrapeCreators or a provider selected through the GooseWorks API catalog. Depending on the operation, these providers receive prompts, reference media, URLs, search terms or lookup fields. Not every task uses every provider. These are transfers by the GooseWorks service or its managed proxy, separate from a skill making a direct vendor call.

Recipes are fetched at runtime, so this package is not a frozen inventory of every possible provider or source host. Review the actual recipe, returned command and transfer destinations before approving local execution or a new task-specific transfer. The installed skills direct provider calls through GooseWorks; they do not ask for the user's provider keys. See the [privacy policy](https://gooseworks.ai/privacy) for service-provider processing and the limits on use of private workspace content.

### Retention and deletion

Data saved to a GooseWorks workspace can remain for **longer than 30 days**. There is no blanket promise that all inputs received from Claude are discarded immediately or deleted within 30 days. Individual temporary records can have shorter lifetimes without changing the retention of saved workspace content.

Under the [published privacy policy](https://gooseworks.ai/privacy), a workspace owner can close an account in **Settings → Account → Close account**. Closure disables the workspace immediately. Data remains recoverable during a full 30-day grace period, and permanent deletion is scheduled for the first of the month after that period expires. Deletion can therefore occur later than day 30. Billing/tax or other legally required records may remain, and backups expire through the backup rotation rather than immediately.

Disconnecting an advertising account deletes its access credential and stops syncing, but previously imported metrics remain for historical reports until separately erased. See the [data deletion instructions](https://gooseworks.ai/data-deletion), or contact `help@gooseworks.ai` for deletion or restoration requests. These statements describe GooseWorks retention; they do not set a retention period for a third-party provider or for files the user keeps locally.

### Intended audience

This plugin is intended for **adult business users** working on brand research, marketing, sales and creative production. It is **not intended for users under 18**. The general GooseWorks privacy policy separately states that the service is not intended for children under 16; that statement is not a claim that this plugin targets 16–17-year-olds. This package adds no age-verification mechanism.

### Credentials, local execution and background work

- **Sign-in and credentials:** a GooseWorks account and interactive sign-in are required. The host's MCP client manages OAuth and its token storage. The package contains no secrets and no code that reads `.env`, another application's login, browser profiles or credential stores. Connected third-party accounts are managed by GooseWorks. Paid work requires a GooseWorks credit balance.
- **Local commands and files:** research, image ads and product photos use the hosted MCP, with direct file transfers where an upload is requested. Video workflows may return a production command for the user's computer. Review the exact command, paths and destinations before allowing the host to execute it. It can download instructions/assets, read selected source files, write local media and upload production outputs; it runs with the local user's authority and can take a long time. Local execution is a separate approval boundary and is not a sandbox. No fixed production command or executable is shipped in this package.
- **Background jobs:** remote research and generation may continue after a tool response; the skills poll saved job IDs. This package starts no daemon or scheduled task on its own.
- **Updates and reporting:** the package has no self-updater or telemetry code. The host controls plugin updates. GooseWorks records account activity and credit usage as part of its hosted service; see its privacy policy. Installing the plugin does not enroll the user in optional telemetry.

## Review status

Passing package checks proves that the files and archive match the canonical sources. It does not prove installation, sign-in, brand onboarding, paid approvals, image delivery or video completion in a provider's app. Those require manual testing with the actual candidate.

Support: https://docs.gooseworks.ai/help-center/troubleshooting

Privacy: https://gooseworks.ai/privacy · Terms: https://gooseworks.ai/terms
