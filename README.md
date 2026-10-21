# Personal Email to Company Resolver MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-personal-email-to-company-resolver)](https://smithery.ai/servers/mambabuilt/mcp-personal-email-to-company-resolver) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-personal-email-to-company-resolver/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-personal-email-to-company-resolver) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-personal-email-to-company-resolver%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-personal-email-to-company-resolver&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-personal-email-to-company-resolver)](https://www.npmjs.com/package/@mambalabsdev/mcp-personal-email-to-company-resolver) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-personal-email-to-company-resolver)](https://www.npmjs.com/package/@mambalabsdev/mcp-personal-email-to-company-resolver) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-personal-email-to-company-resolver)](https://github.com/mambalabsdev/mcp-personal-email-to-company-resolver/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-personal-email-to-company-resolver)

Give it a free mail address (Gmail, Outlook, iCloud, Yahoo, and about 140 more consumer domains). Each run reads the public Gravatar profile for the address, the GitHub account whose public commits are signed with it, and, for a name with an employer, Mamba Labs Person LinkedIn URL Resolver. It returns one row per address with the person's name, likely employer, employer domain, and a confidence. It never contacts the mailbox and never opens LinkedIn.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

- `match_status` is `resolved` (employer at 0.75 or more, billed), `weak`, `name_only`, `corporate_email`, `not_found`, `invalid_email`, or `blocked`, and only `resolved` is billed.
- `employer_source` says where the employer came from: `gravatar`, `github`, `web_result`, `linkedin_headline`, or `email_domain`. `evidence_urls` lists the public pages.
- The LinkedIn step raises the confidence when a LinkedIn result shows the employer as current and lowers it when it shows it as former.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-personal-email-to-company-resolver": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-personal-email-to-company-resolver"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `resolve_personal_email_employer` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "These ten signups used Gmail addresses. Which companies do they work at?"
- "Find the employer behind this Outlook address and show me the evidence."

## Tool

`resolve_personal_email_employer`. One tool. Example arguments:

```json
{ "emails": ["sample.signup@gmail.com"] }
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults.

| Input | Type | Description |
|---|---|---|
| `email` | string | One address |
| `emails` | array | Up to 300 addresses |
| `github_token` | string | Optional, your own free GitHub token |
| `search_web` | string | true adds one exact Google search per address left without an employer. Default false |
| `use_linkedin_resolver` | string | true (default) runs Person LinkedIn URL Resolver from your account, billed at its own price |
| `country` | string | Google country code. Default us |
| `max_charge_usd` | string | Optional cost cap for the run |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `email-resolved`, per address with an employer at 0.75 or more | $0.025 | $0.02375 | $0.0225 | $0.02125 |
| `apify-actor-start`, per run start | $0.0005 | $0.0005 | $0.0005 | $0.0005 |

Every other row is free. With `use_linkedin_resolver` on, Person LinkedIn URL Resolver bills its own events on top: $0.035 per person sent to it, found or not, plus $0.0005 per run on the free plan.

## Your keys

The actor holds no key. `github_token` is optional and is your own, used for the run only. Mamba Labs pays the platform costs of the run, proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/personal-email-to-company-resolver](https://apify.com/mambalabs/personal-email-to-company-resolver)

## License

MIT. See [LICENSE](LICENSE).
