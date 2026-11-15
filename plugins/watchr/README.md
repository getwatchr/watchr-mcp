# Watchr for Claude

Watchr brings your competitive monitoring into Claude. Ask about recent competitor changes, inspect the original sources behind a signal, and read existing reports. The included onboarding skill helps you start a watchlist or improve an existing one through conversation.

## Connect and use

Add the Watchr plugin in Claude, then connect the included Watchr connector to your own Watchr account through browser OAuth. If you are new to Watchr, the connection flow can take you through account creation. Installing this plugin alone does not create an account, grant access, start a watchlist, or send emails.

Ask Claude to set up Watchr, or invoke the `watchr-onboarding` skill. It reads your existing configuration first, asks only for missing details, previews proposed changes, and waits for your approval before applying them. You can also ask Claude to summarize recent insights and reports with their sources and dates.

## Data and permissions

The plugin contains instructions and a remote MCP connection. It installs no local executable. After you authorize the connector, Claude sends account-scoped Watchr tool requests to `https://mcp.watchr.ai/mcp`; Watchr returns data scoped to your account and role. Authorized administrators can configure monitoring through preview and apply tools. Readers can consult their permitted data. Watchr checks permissions on every request. The plugin files store no account data or credentials.

See the [Watchr MCP documentation](https://github.com/getwatchr/watchr-mcp), [privacy and legal information](https://www.watchr.ai/legal), and [support](mailto:contact@watchr.ai). The plugin is MIT licensed.
