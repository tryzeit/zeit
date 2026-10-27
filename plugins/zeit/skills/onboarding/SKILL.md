---
name: onboarding
description: Connect to Zeit or create an account through its browser email-code flow. Use when the user wants to get started with Zeit, connect after installation, or continue onboarding in a new chat.
---

Guide the user through install → connect → use Zeit. Continue in the current
conversation whenever possible. Some hosts do not expose newly installed plugin
tools immediately in the same chat; do not claim this happens in every host.

## Connect after installation

Start connecting as part of onboarding; do not wait for a separate “connect to
Zeit” request. If `get_account` is available, call it first. Success confirms identity; then follow Workspace setup below. Otherwise, use the host's existing Zeit connection to start its
normal MCP OAuth flow. Missing tools do not prevent attempting a supported host
connection action. In Codex, prefer the host's connection action; if unavailable,
`codex mcp login zeit` is a fallback when the installed CLI recognizes that
existing server. Do not add a second server to make this command work.

If neither Zeit tools nor a supported connection action are available, try host
tool discovery and an available host-supported plugin or MCP refresh, then
rediscover once. If still unavailable, explain:

> Zeit is installed, but this chat hasn't loaded its tools or connection action.
> Some hosts only load newly installed plugins in a new chat. Start a new chat
> and send: **“Connect to Zeit using the installed plugin, then get my account.”**

Only say the plugin is installed after the host confirms installation. Do not
ask the user to reinstall. In the new chat, attempt the connection immediately.

## Complete browser sign-in

Open the authorization URL returned by the host. This flow handles both new and
existing accounts; no separate signup is needed. Do not collect email in chat.
The user enters their email in the browser, then the emailed code, and selects
**Verify and connect** to allow the displayed application and permissions.
Existing verified accounts can choose **Other sign-in options** and use their
password instead. Passwords must be entered only on the browser sign-in page, never in chat.

Never ask for passwords or verification codes in chat, read their inbox for a
code, or ask them to paste tokens. Do not construct an OAuth URL yourself or
replace its callback with a new-chat link. Wait for the host's connection result.
If cancelled or failed, report that connection is incomplete. Do not automatically
resend codes or repeatedly restart authorization.

## Use Zeit after connection

After confirmed OAuth success, suggest continuing in a new chat and provide a
ready-to-copy follow-up prompt in a fenced text block. Explain that some hosts
do not immediately load newly connected plugin tools in the current chat, so a
fresh chat can pick up the connection. Do not claim a new chat is always required
or that it is guaranteed to fix tool loading.

Use this handoff:

Zeit is connected. I suggest starting a new chat so it can load the connected
plugin. Copy and paste this prompt:

```text
Use the connected Zeit plugin to get my account and confirm which email is signed in. Zeit is already installed and OAuth completed; try get_account first. Only ask me to reconnect if the host reports that authentication is required. Then get my active workspace. Signup normally provisions an initial workspace named Personal.
```

If the user has a specific next task, include it after the account check in the
copyable prompt. Do not include tokens, codes, or other credentials. Do not open
a new chat automatically. If the user prefers to continue here, discover tools
and try `get_account`, using an available host-supported refresh once if needed.

A missing tool is not evidence that login failed. Do not restart OAuth solely
for missing tools. If a fresh chat still lacks tools, report the host-loading
problem instead of sending the user through more new chats or sign-ins.

Provide the relevant next-chat prompt when handing off; do not include credentials.
Open a new chat only when the user requests it and the host provides that capability.
Do not start a separate app-server process, edit unrelated settings, repeatedly
reinstall, create a duplicate MCP server, or modify internal credential stores.
Do not claim a refresh or connection succeeded without host confirmation.


## Workspace setup

After confirming identity, call `get_workspace`. A successful non-null result means
workspace setup is complete; reuse it. Do not create or rename during a read.

If the result is null, call `create_workspace` with the default name Personal to finish
initial provisioning. It reuses an existing membership and never creates a duplicate.
Personal is only a default name; use `update_workspace` when the user asks to rename it.

Users can belong to several workspaces. Use `list_workspaces` to discover memberships.
Use `switch_workspace` only when the user chooses a destination, and report its name.
Selection is global across the dashboard and all chats; other ordinary tools resolve it
on the server. Existing in-flight operations retain their original workspace.

Use `list_pending_invitations` to find invitations for the verified email. Use `respond_to_workspace_invitation` with its ID and status accept or deny after user
confirmation. The verified email must match; an ID alone gives no access. Acceptance
adds membership, preserves resources, and never changes active selection. Offer an explicit
switch to the joined workspace. Denial adds no membership and leaves the active workspace unchanged.

If workspace scopes are missing, explain that the connection needs the corresponding
workspace action permission; do not treat this as a missing workspace or create one under
another identity. Membership never grants access to teammates' private requests.

### Workspace names and slugs

Workspace `name` is a non-unique display name and defaults to `Personal`. The separate
`slug` is globally unique. Omit the slug on creation to generate one automatically.
Only change a slug when the user requests it; a name change leaves it unchanged.
Slugs use 3–63 lowercase letters, numbers, and single hyphens. If the API returns
`workspace_slug_taken`, ask the user to choose another slug and retry; keep their display name.
