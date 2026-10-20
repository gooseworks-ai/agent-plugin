# GooseWorks for Gemini CLI

This extension connects Gemini CLI to GooseWorks and includes the five skills in this repository. Sign in to your GooseWorks account when prompted. Paid work requires approval of the displayed credit total and an existing credit balance.

## Install

After this change is merged into the repository's default branch:

```sh
gemini extensions install https://github.com/gooseworks-ai/agent-plugin
```

For a preview of this pull request:

```sh
gemini extensions install https://github.com/gooseworks-ai/agent-plugin --ref codex/gemini-gallery
```

Restart Gemini CLI, then use `/extensions list` to check that `gooseworks` is enabled. Run `/mcp auth gooseworks` to sign in before asking GooseWorks to research a brand, create image ads or make product photos.

If your Gemini settings already define an MCP server named `gooseworks`, that configuration takes precedence over the extension's connection. Check it when troubleshooting sign-in or connection problems.

Video uses the existing execution handoff described in [README.md](README.md#video-execution). Follow the returned instructions and review the final output before treating a video as complete.

## Gallery publishing

The [official release guide](https://geminicli.com/docs/extensions/releasing/#list-your-extension-in-the-gallery) requires a public GitHub repository, a root `gemini-extension.json`, and the `gemini-cli-extension` repository topic. After the manifest is merged and the extension is approved for publication, add that topic in the repository's About section. The gallery crawls daily and lists extensions that pass its validation.

## Keep the adapter current

The root manifest is an adapter for the existing package. Its name, version and description come from `plugin.json`. Each Streamable HTTP connection in `mcp.json` maps to Gemini's `mcpServers.<name>.httpUrl`. Regenerate these values together whenever a new package is exported from the canonical `apps/plugin` source; keep all five generated skills unchanged.

The original `source-lock.json` describes the portable package. This adapter and this guide are additional files; they do not change that sealed package or extend its recorded hashes. Successful installation does not establish account sign-in, paid generation or finished-video behavior; those need separate checks in a signed-in client.
