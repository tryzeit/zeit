# Zeit

Request information from clients, teammates, and collaborators—and bring it back
into your AI workflow.

Your AI has the conversation. Other people have the missing information. Zeit helps
you turn that gap into a request, collect what you need, and
keep working with the answers in chat.

## How it works

- Turn your chat context into questions for the right people.
- Collect answers and files through a shared link, without requiring an account.
- Track responses in one place and bring them back into your AI workflow.

From client intake to gathering information for a confidential information
memorandum, the goal is the same: spend less time chasing information and more
time using it.

## Install in Codex

Choose **Install → Codex** on [tryzeit.com](https://tryzeit.com). It opens the Codex
desktop app with a setup prompt; press Send to install and begin onboarding.
Installation may require confirmation.

Alternatively, add the marketplace:

```sh
codex plugin marketplace add tryzeit/zeit
```

Open the Plugins Directory, choose **Zeit**, install the plugin, and ask:
“Get started with Zeit.” The package registers `skills/onboarding/SKILL.md` as
its setup skill. Onboarding starts the connection automatically when the host
supports it. If the current chat has not loaded the plugin, start a new chat with
“Connect to Zeit using the installed plugin, then get my account.”
Connect through the browser: enter your email, enter the emailed code, and select
**Verify and connect**. This creates an account or signs you in, then connects
the AI host through OAuth. Codes and tokens never need to be pasted into chat.

After sign-in, onboarding suggests a new chat and supplies a copyable prompt to
use the connected Zeit plugin and get your account. Some hosts do not immediately
load newly connected tools in the current chat. A missing tool does not mean you
need to reinstall or repeat sign-in.

GitHub distribution does not publish the plugin to the universal ChatGPT directory.

## Development and release

This repository is published automatically by Zeit. Source changes are maintained
in the team's monorepo and exported here; do not edit this mirror directly.

The package is in `plugins/zeit`; the marketplace catalog is in
`.agents/plugins/marketplace.json`. The hosted MCP endpoint is
`https://mcp.tryzeit.com/`. No local server or API key is needed to use it.
