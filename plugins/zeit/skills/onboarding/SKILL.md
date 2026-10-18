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
If the host cannot display the form, show the browser link returned by the tool
and ask the user to return to this chat after verification.

Wait for the form's verification result or the user's confirmation. Sending a
code is not verification. After success, briefly confirm their account is ready.
If they need another code, call `create_account` again only when requested.
Respect rate-limit errors; do not automatically retry.

This preview supports account creation and email verification only. Verification
does not authorize an MCP connection. Do not attempt OAuth or authenticated
account tools until that functionality is available.
