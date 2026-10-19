---
name: onboarding
description: Create and verify a Zeit account, then connect it to the AI host. Use when the user wants to get started with Zeit or continue onboarding.
---

Continue setup in the current conversation. Follow these steps in order.
Installing the plugin does not create an account. “Continue onboarding” does
not mean the user already has an account.

## 1. Determine where to start

Use explicit evidence from this conversation:
- Account verification succeeded: proceed to connection.
- A verification form is pending: help the user finish it.
- The user explicitly says they already have an account: proceed to connection.
- Otherwise: start account creation.

Do not call `get_account` to discover whether an unverified user has an account.
Do not start MCP OAuth, run an MCP login command, or open an authorization URL
before verification succeeds, unless the user explicitly confirms they already
have an account.

## 2. Create the account

Ask for the user's email only if they have not supplied it, then call
`create_account`. Do not stop at confirming installation or ask whether to
start already-requested setup. Only claim an email was sent when the tool
confirms it.

`create_account` is public and does not require MCP OAuth. If it is unavailable,
use host tool discovery and an available host-supported plugin or MCP refresh,
then rediscover once. Missing tools are a loading problem, not a reason to start
OAuth. Do not start a separate app-server process, edit unrelated settings,
repeatedly reinstall, or claim a refresh happened without confirmation. If tools
remain unavailable, explain the limitation and offer a new chat.

## 3. Complete email verification

Use the attached MCP Apps form with the prefilled, read-only email, code field,
and Verify email button. The user enters the code in the form, never in chat.
Never read their inbox to retrieve it or ask for a password in chat. Do not open
a browser verification page or invent a fallback link. If the form does not
render, report that UI problem.

Wait for the form's successful verification result or the user's explicit
confirmation that verification succeeded. Sending a code or displaying the
form does not establish a verified account. Resolve verification failures before
proceeding; do not start OAuth as a workaround. Request another code only when
the user asks, and do not automatically retry rate-limit errors.

## 4. Connect the verified account

The form may return `access_token`, `refresh_token`, `client_id`,
`token_endpoint`, `resource`, `scope`, and `expires_in`. These belong to Zeit's
onboarding OAuth client, not necessarily the host's registered OAuth client.
Import them only through a supported host mechanism that accepts that client
identity and resource. Never print tokens, place them in commands, or write
internal credential stores. A bearer-only setter does not install refresh
behavior.

If full credential import is unavailable or fails, use the normal MCP OAuth
connection flow. The account now exists, so the user can sign in. Do not retry a
consumed verification code to obtain credentials.

For a user who explicitly already has an account, call `get_account` first and
start OAuth only if authentication is required. The browser signs them in with
an email code and asks them to allow account:read. Do not create a duplicate
connection if the host already manages the plugin's MCP server.

## 5. Confirm completion

Call `get_account` using the connected MCP. If it requires authentication after
verification or credential import, use the normal OAuth flow, then retry it.
Confirm connection only when it returns the user's account, and confirm the
returned email. Distinguish “Email verified; account connection still needed”
from “Account connected and confirmed.” Never describe verification alone as a
connected account.

This preview supports onboarding and reading the connected account. Information
requests are not available yet.
