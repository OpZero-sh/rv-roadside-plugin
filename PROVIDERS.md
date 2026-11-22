# Use RV Roadside with your assistant

Find US RV repair, parts, or campgrounds and identify the right manual or component information to bring to the shop.

## ChatGPT and Codex

[Download the ChatGPT and Codex package](https://raw.githubusercontent.com/OpZero-sh/rv-roadside-plugin/downloads/rv-roadside.zip). It includes the product skills, remote connection and product icon.

For a publisher or developer setup, open the OpenAI developer dashboard, choose **Upload plugin**, upload the one-connection ZIP and connect the declared MCP endpoint. People with an existing directory listing can install it directly in ChatGPT. For an OAuth-protected service select OAuth and complete its browser sign-in. Choose no authentication for a public service. Rebuild the archives from a checkout with `python3 scripts/build_openai.py ../plugin-zips`.

[OpenAI package guide](https://developers.openai.com/plugins/build/plugins) · [Upload and connection setup](https://developers.openai.com/plugins/deploy/submission)

## Claude

In Claude Code run `/plugin marketplace add OpZero-sh/plugins`, then `/plugin install rv-roadside@opzero`. In Claude chat or Cowork, add the OpZero marketplace if available to your account and connect the services in the installed plugin’s **Connectors** tab.

## Grok Build

From a checkout of this repository, run `grok --plugin-dir .`. Open `/plugins` or `/mcps` to check the loaded capabilities. Grok Build reads the Claude-compatible plugin, its skills and MCP configuration. [Grok Build plugins](https://docs.x.ai/build/features/skills-plugins-marketplaces)

## Grok API

Preview a request locally with Python 3. Add `XAI_API_KEY` in your environment and omit `--dry-run` to send it. Use `--model` to select a model enabled for your account. The example requests a focused set of product tools; its tool list is in [grok-servers.json](integrations/grok-servers.json).

```sh
python3 integrations/grok_example.py --dry-run 'Use RV Roadside to find RV parts and service'
```

For authenticated connections, obtain a current end-user OAuth access token for each named OpZero service in your application’s sign-in flow. Supply the unprefixed token in `RV_ROADSIDE_ACCESS_TOKEN`. Your application manages consent, refresh and storage. The example attaches each token only to its own MCP server.

[Remote MCP tools in the xAI API](https://docs.x.ai/developers/tools/remote-mcp)

## Muse Code

From the repository root, validate and install the native plugin. Its skills load with the plugin. Set up the signed-in connection below to use them.

```sh
muse plugins validate .
muse plugins install .
```

For signed-in connections, merge the entries from [the settings fragment](integrations/muse-settings.json) into your existing `$XDG_CONFIG_HOME/muse/settings.json` (normally `~/.config/muse/settings.json`). Preserve the rest of your settings and use `schema_version: 1`. Start a fresh process and complete browser sign-in for each connection.

```sh
muse mcp login rv-roadside
```

[How Muse handles OAuth](https://meta-models.github.io/muse-code-sdk/next/guides/extend/mcp-servers/)

[Muse Code plugins](https://meta-models.github.io/muse-code-sdk/next/guides/plugins/reference/manifest/)
