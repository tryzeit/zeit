# Zeit

Zeit's plugin for ChatGPT and Codex. The first release sends an email code and
opens a verification form. Enter the code in the form, never in chat.

## Install in Codex

After this package and its server release are published:

```sh
codex plugin marketplace add tryzeit/zeit
```

Open the Plugins Directory, choose **Zeit**, install the plugin, and ask:
“Help me create a Zeit account.” Hosts with MCP Apps display the form in chat;
other hosts can open the returned browser link.

GitHub distribution does not publish the plugin to the universal ChatGPT directory.

## Development and release

This repository is published automatically by Zeit. Source changes are maintained
in the team's monorepo and exported here; do not edit this mirror directly.

The package is in `plugins/zeit`; the marketplace catalog is in
`.agents/plugins/marketplace.json`. The hosted MCP endpoint is
`https://mcp.tryzeit.com/`. No local server or API key is needed to use it.

This release supports account creation and email verification. Verification does
not authorize an MCP session. OAuth connection and authenticated account tools
will follow in a later release.
