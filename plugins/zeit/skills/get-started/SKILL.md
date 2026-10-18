---
name: get-started
description: Use when the user wants to create a Zeit account or verify their email to get started with Zeit.
---

Ask for the user's email if they have not supplied it. Call Zeit's `create_account`
tool with that email to send a verification code and display the verification form.

The user enters the code directly in the form. Never ask them to paste a password
or verification code into chat, and never read their inbox to retrieve the code.
If the host cannot display the form, show the browser link returned by the tool.

Wait for the form's verification result or the user's confirmation. Do not infer
success from sending a code. If they need another code, call `create_account` again
only when requested. Respect rate-limit errors; do not automatically retry.

Email verification establishes the Zeit account. It does not authorize the MCP
connection. This release supports account creation and email verification only;
account connection and authenticated tools are coming in a separate release.
