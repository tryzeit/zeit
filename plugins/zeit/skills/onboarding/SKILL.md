---
name: onboarding
description: Set up a Zeit account with email verification. Use when the user wants to get started with Zeit, create an account, or finish verification.
---

Continue setup in the current conversation after installation. Ask for the user's
email only if they have not already supplied it, then call Zeit's `create_account`
tool to send a code and display the verification form. Do not stop at confirming
that the plugin was installed or ask whether to start already-requested setup.

Discover the installed Zeit tools through the host's tool discovery if needed.
Do not claim a new chat or restart is required merely because tools were absent
before installation. If discovery still cannot find them, explain the actual
blocker instead of claiming signup succeeded.

The user enters the code directly in the form. Never ask them to paste a password
or verification code into chat, and never read their inbox to retrieve the code.
Use the attached MCP Apps form with the prefilled, read-only email, code field,
and Verify email button. Do not open a browser verification page or invent a
fallback link. If the form does not render, report that UI problem rather than
claiming that verification succeeded.

Wait for the form's verification result or the user's confirmation. Sending a
code is not verification. After success, call `get_account` to check the account connection.
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
