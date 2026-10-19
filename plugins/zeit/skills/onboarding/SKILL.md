---
name: onboarding
description: Set up a Zeit account with email verification. Use when the user wants to get started with Zeit, create an account, or finish verification.
---

Continue setup in the current conversation after installation. Ask for the user's
email only if they have not already supplied it, then call Zeit's `create_account`
tool to send a code and display the verification form. Do not stop at confirming
that the plugin was installed or ask whether to start already-requested setup.

Discover the installed Zeit tools through the host's tool discovery if needed.
If tools are missing after installation, use an available host-supported plugin
or MCP configuration refresh, then rediscover once and continue in this chat.
Do not start a separate app-server process, edit unrelated settings, repeatedly
reinstall, or claim a refresh happened without confirmation. If refresh is
unavailable or tools remain missing, explain the limitation and offer a new chat.

The user enters the code directly in the form. Never ask them to paste a password
or verification code into chat, and never read their inbox to retrieve the code.
Use the attached MCP Apps form with the prefilled, read-only email, code field,
and Verify email button. Do not open a browser verification page or invent a
fallback link. If the form does not render, report that UI problem rather than
claiming that verification succeeded.

Wait for the form's verification result or the user's confirmation. Sending a
code is not verification. After success, the form may provide
`access_token`, `refresh_token`, `client_id`,
`token_endpoint`, `resource`, `scope`, and `expires_in`. These belong to Zeit's
onboarding OAuth client, not necessarily the host's registered OAuth client.
Try importing credentials only if the host exposes a supported mechanism that
accepts that client identity and resource. Do not write internal OAuth stores,
print tokens, place them in commands, or silently create a duplicate MCP server.
A bearer-only setter does not install refresh behavior. If full credential import
is unavailable, use the normal OAuth connection flow. Do not retry a consumed
verification code to obtain credentials. Call `get_account` to confirm connection;
a failed import or authentication error also falls back to normal OAuth.
If they need another code, call `create_account` again only when requested.
Respect rate-limit errors; do not automatically retry.

When `get_account` requires authentication, use the host's connection flow. The
browser signs the user in with an email code and asks them to allow account:read.
After the host finishes OAuth, call `get_account` again and confirm the returned
account email. Do not ask for tokens or codes in chat, or describe verification
alone as a connected account. If the user already has an account, begin with
`get_account` and connect it rather than restarting account creation.

This preview supports onboarding and reading the connected account. Information
requests are not available yet. Do not create a second connection manually if the
host already manages the plugin's MCP server.
