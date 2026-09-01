# Legalize — the MCP connector for consolidated legislation

**What a law said on any past date, with the citation and the git commit behind it. Your assistant answers from the corpus instead of from memory.**

[![MCP](https://img.shields.io/badge/MCP-Streamable_HTTP-1f6feb)](https://modelcontextprotocol.io)
[![Corpus read-only](https://img.shields.io/badge/corpus-read--only-2ea043)](#the-tools)
[![Auth](https://img.shields.io/badge/auth-sign--in_required-f0883e)](#authentication)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)
[![Web](https://img.shields.io/badge/web-legalize.dev-111)](https://legalize.dev)

Connect your AI client (Claude, ChatGPT, Cursor, VS Code, Gemini CLI…) to [**Legalize**](https://legalize.dev) and ask about legislation in plain language. Every answer comes back with three things a model cannot invent:

- 📜 **The text itself** — the whole law or one whole article, never a truncated fragment.
- 🕰️ **A date** — not only the current wording, but the version that was on the books on the day you name.
- 🔗 **A citation and a git SHA** — the commit in the public corpus that holds those exact bytes, so the other side can check the quote.

> **One remote endpoint:**
> ```
> https://legalize.dev/mcp
> ```
> Sign-in required. See it live at [legalize.dev/mcp](https://legalize.dev/mcp), which always shows the current coverage and allowance.

---

## Quick connect

It is a **remote** server (Streamable HTTP, stateless). Nothing is installed: you hand the URL to your client, and the client walks you through the sign-in the first time it calls a tool.

**One click, if yours is one of these:**

[![Add to Claude](https://img.shields.io/badge/Add_to_Claude-D97757?style=for-the-badge)](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Legalize&connectorUrl=https%3A%2F%2Flegalize.dev%2Fmcp) [![Add to Cursor](https://img.shields.io/badge/Add_to_Cursor-000000?style=for-the-badge)](https://cursor.com/en/install-mcp?name=legalize&config=eyJ1cmwiOiJodHRwczovL2xlZ2FsaXplLmRldi9tY3AifQ%3D%3D) [![Add to VS Code](https://img.shields.io/badge/Add_to_VS_Code-0098FF?style=for-the-badge)](https://vscode.dev/redirect/mcp/install?name=legalize&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Flegalize.dev%2Fmcp%22%7D) [![Add to ChatGPT](https://img.shields.io/badge/Add_to_ChatGPT-10A37F?style=for-the-badge)](https://chatgpt.com/plugins#settings/Connectors?create-connector=true&redirectAfter=%2Fplugins)

The first three pre-fill the name and the address, then ask you to confirm it and sign in. **ChatGPT opens its create-connector form** — it takes no parameter for the server, so paste `https://legalize.dev/mcp` in yourself, with developer mode on under Advanced.

### Claude.ai · Claude Desktop (Connectors)

Settings → **Connectors** → **Add custom connector** → paste `https://legalize.dev/mcp`.

### Claude Code (CLI)
```bash
claude mcp add --transport http legalize https://legalize.dev/mcp
```

### Cursor
Use the button above, or in `~/.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "legalize": {
      "url": "https://legalize.dev/mcp"
    }
  }
}
```

### VS Code (GitHub Copilot)
Use the button above, or in `.vscode/mcp.json`:
```json
{
  "servers": {
    "legalize": {
      "type": "http",
      "url": "https://legalize.dev/mcp"
    }
  }
}
```

### ChatGPT
Settings → **Connectors** → **Advanced** → turn on **developer mode**, then create a connector and paste `https://legalize.dev/mcp`. There is no install link for this one: ChatGPT only deep-links to apps listed in its own directory, and custom connectors sit behind developer mode, on Plus or Pro.

### Gemini CLI and other clients
```json
{
  "mcpServers": {
    "legalize": {
      "httpUrl": "https://legalize.dev/mcp"
    }
  }
}
```

Any client that speaks remote MCP works. If yours only speaks stdio, bridge it:
```bash
npx mcp-remote https://legalize.dev/mcp
```

---

## The tools

**Nothing here can change a law.** The corpus is read-only to the connector: no tool alters a text, a version or a history, because those come from the git repositories and this server has no way in. The full descriptions the model reads, with every argument and its type, are published live by the running server at [`/mcp/tools.json`](https://legalize.dev/mcp/tools.json).

| Tool | What it answers |
|---|---|
| **`list_countries`** | Which countries are in the corpus, and how many laws each one holds. |
| **`search_laws`** | Find a norm by words in its title or by its official number. Returns the `id` every other tool takes. |
| **`get_law`** | Read what a norm says today — the whole text, or one article of it. |
| **`law_at_date`** | What the norm said on a given day, with the git SHA behind that version. |
| **`diff_law`** | What changed between two dates, as a unified diff of the two texts. |
| **`reform_history`** | Which norms amended this one, when, and what each says it touched. |
| **`law_stats`** | How large a corpus is and how much it moves, before drilling into it. |

The rest manage **your own** subscriptions — the one question reading the corpus cannot answer, which is *tell me when this changes*. They are the only tools that write anything, what they write is your account's own subscription, and they need a paid plan.

One rule, two ways to receive it. A webhook posts every change to a server you run; a digest lists a day of them in your inbox:

| Tool | What it does |
|---|---|
| **`preview_webhook`** | Try a subscription rule against the last 30 days without creating anything: how many events it would have delivered, and which. Works for either delivery — it tries the rule, not the destination. |
| **`create_webhook`** ✎ | Subscribe an HTTPS endpoint of yours to law changes. Narrow it to specific laws, to words in the title and subject headings, or to countries. The signing secret is **not** returned: it can forge a delivery, and anything a tool returns stays in the transcript. Collect and rotate it in the dashboard. |
| **`list_webhooks`** | The endpoints this account has, and the `id` to delete one by. Secrets are never returned. |
| **`delete_webhook`** ✎ | Remove one. Deliveries stop, and its history goes with it. |
| **`create_email_digest`** ✎ | The same rule, delivered as one email a day instead — the version you can receive without running anything. `lang` picks the language of the mail, not of the laws. |
| **`list_email_digests`** | The digests this account gets, with the rule and language of each, and the `id` to delete one by. |
| **`delete_email_digest`** ✎ | Stop one. The daily mail ends immediately. |

**The digest goes to the account's own address and there is no argument for another one.** Not an omission: a free-text recipient would turn a connector into a way to send mail to somebody else, signed by our domain, on a model's say-so.

Narrow the subscription or you will stop reading it. The filters compose as AND, a rule that could never match — an unknown country, a law id that is not in the corpus — is refused rather than stored, and a text filter reaches **one language**: laws are written in their own, so *proteccion de datos* finds the Spanish act and not the Portuguese *protecao de dados*.

The ones marked ✎ are declared to clients with `readOnlyHint: false`, so a client that confirms before a write will confirm before these. On a plan without them they all answer a structured `feature_not_available` error and create nothing. Neither delivery is instant: webhooks are signed and batched daily ([the format and the verifier](https://legalize.dev/docs/webhooks)), and a digest is one mail a day — on a day when nothing matched, no mail at all.

Every tool returns the same envelope — `data`, `citation`, `url`, `source`, `note` — and every result links back to its page on [legalize.dev](https://legalize.dev). A tool-level failure is a *successful* call whose `data` is a structured error naming what to do next, so the model can correct itself instead of guessing.

## Examples (ask your assistant)

- *"What did article 348 bis of the Spanish Companies Act say in March 2023?"*
- *"Has the French Code du travail changed on this point since 2020? Show me the diff."*
- *"Which norms have amended this law, and when?"*
- *"How much of Latvian legislation does Legalize actually hold?"*
- *"Email me every morning when anything about data protection changes in Spain — in Spanish."*

## Authentication

**Sign-in required; there is no anonymous access.** Every call is tied to an account, and an unauthenticated request gets `401` with the `WWW-Authenticate` challenge that starts the OAuth flow.

Your client discovers the authorization server from `https://legalize.dev/.well-known/oauth-protected-resource`, registers itself dynamically, and sends you to sign in once in a browser. After that it holds the token and you never see the handshake again. No API key is pasted anywhere.

## Limits

There is a free monthly allowance and a per-minute burst guard. Neither number is written here on purpose — they would go stale the day either changes, and a README that quietly lies about a limit is worse than one that points at it. Both are stated, live, on [legalize.dev/mcp](https://legalize.dev/mcp) and [legalize.dev/pricing](https://legalize.dev/pricing).

What does not change: past the monthly allowance the connector answers `quota_exceeded` and stops until the month rolls over — nothing is charged and nothing is cut off silently. **Every country is included in every tier.**

## Before you quote it

Most of the corpus is **consolidated**: amendments are folded into the text, so what you read is the law as it stands. Some of it is not. A text published **as enacted** comes back flagged, with the norms that amend it named, and never as the law in force — but the flag is only useful if you read it before pasting the quote into a brief.

Three more limits worth knowing:

- A date resolves to what was **published** on or before it, not to what was **in force**.
- **Article boundaries are derived** by matching headings in the text, not published as structure by the source. Quote the anchor, and check the boundary before relying on it.
- Counts are of what Legalize has ingested, not of what the official gazette has published. A gap in the corpus is not evidence that a norm does not exist.

A body is never truncated: a law too large to send whole comes back as its table of contents with instructions for asking again, because half an article reads exactly like a whole one.

## For developers

Transport is **Streamable HTTP**, **stateless**. Every JSON-RPC method needs a token, `initialize` and `tools/list` included. So the first thing to check is the catalogue, which is served beside the endpoint as a plain GET and needs nothing:

```bash
curl -s https://legalize.dev/mcp/tools.json | jq '.tools[] | {name, readOnly}'
```

That is what a directory reads, and it is where `readOnly` per tool is published. The JSON-RPC endpoint itself answers 401, and that 401 **is** the handshake — its `WWW-Authenticate` header is what tells a client where the authorization server is:

```bash
curl -si -X POST https://legalize.dev/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"list_countries","arguments":{}}}' | head -20
# 401 + WWW-Authenticate: Bearer ... resource_metadata="..."   ← correct: that is the handshake
```

Once signed in, the same call answers with the tool's payload.

## What is behind the data

Legalize builds one public git repository per country: every law is a Markdown file, and **every reform is a dated commit**. That is where the SHA in an answer comes from — it is a commit in a public repo, not an identifier minted for the reply.

Ask what the Spanish Companies Act said in March 2023 and the answer carries commit
`3b9aea8d5` of [`legalize-dev/legalize-es`](https://github.com/legalize-dev/legalize-es):

```
git show 3b9aea8d5:es/BOE-A-2010-10544.md
```

returns the same bytes. The connector reaches **every country Legalize publishes** — the live list, with the size of each corpus, is on [legalize.dev](https://legalize.dev) and from the `list_countries` tool.

## More

- 🌐 **Web:** [legalize.dev](https://legalize.dev) — browse the corpus, free, no account.
- 🔌 **This connector, online:** [legalize.dev/mcp](https://legalize.dev/mcp)
- 🧾 **REST API:** [legalize.dev/api](https://legalize.dev/api) — the same data over HTTP.
- 💶 **Pricing:** [legalize.dev/pricing](https://legalize.dev/pricing)
- 🛠️ **Pipeline and corpus:** [github.com/legalize-dev/legalize](https://github.com/legalize-dev/legalize) — open source.

## About Legalize

[Legalize](https://legalize.dev) turns official gazettes into git: consolidated legislation as Markdown, one commit per reform, one repository per country. This repository documents its **MCP connector** — the surface an AI assistant talks to.

[legalize.dev](https://legalize.dev) · [MIT](LICENSE) licence
