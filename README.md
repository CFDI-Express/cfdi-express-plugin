# CFDI Express Agent Plugin

Official [Agent Plugin](https://agent-plugins.org/plugin-authors/build-an-agent-plugin) for [CFDI Express](https://cfdi.express): a portable package (manifest, MCP server, and skill) so Cursor, Grok Bot, and other Agent Plugins clients can issue and manage Mexican CFDI 4.0 invoices from the user's CFDI Express account.

Plugin identifier: `cfdi-express`. License: MIT.

## What it does

The plugin connects to the hosted CFDI Express MCP at `https://api.cfdi.express/mcp` and adds a skill that tells the agent to preview before stamping and never invent fiscal data.

Authorized users can preview and stamp CFDI 4.0 invoices, credit notes, payment complements (REP), payroll receipts (nómina), and retenciones for digital platforms; search SAT catalogs; and retrieve document status and files.

## Install

Once the plugin is listed, install it from the [Cursor Marketplace](https://cursor.com/marketplace) or the [Grok Bot Marketplace](https://x.ai/bot/marketplace), then sign in with your CFDI Express account when asked (OAuth 2.1). The plugin does not ship API keys or secrets.

To try it locally in Cursor before it is listed, copy this directory to `~/.cursor/plugins/local/cfdi-express` and reload the window. See the [Cursor plugins docs](https://cursor.com/docs/plugins).

## Authentication and environments

- Production: `https://api.cfdi.express/mcp` — issues real tax invoices and uses account balance.
- SAT sandbox: `https://api.cfdi.express/mcp/test` — free, no fiscal validity.

OAuth uses the path to choose the mode. API keys (`sk_live_` / `sk_test_`) are only for private client configs; do not put them in this repository.

MCP guide: [https://cfdi.express/docs/api/mcp](https://cfdi.express/docs/api/mcp)

## Prices and preview

- Line item `unitPrice` includes transferred IVA/IEPS by default (`pricesIncludeTax: true`). Send `pricesIncludeTax: false` when the amount is before tax.
- Preview before stamping. Some MCP clients can show an interactive preview (for example a card with «Autorizar y timbrar»). Clients without interactive views, such as Grok Bot, get a text summary instead; the agent must wait for an explicit confirmation in chat, then call `create_invoice` (or the matching `create_*`) with the same `idempotency_key` from the preview.


## Package layout

```text
plugin.json      Agent Plugins 1.0.0 manifest
mcp.json         Remote Streamable HTTP MCP server
skills/cfdi-express/SKILL.md
assets/logo.png  600×600 PNG, solid background
LICENSE          MIT
README.md
```

## Legal and support

- Privacy notice: [https://cfdi.express/politicas-de-privacidad](https://cfdi.express/politicas-de-privacidad)
- Terms of service: [https://cfdi.express/terminos-del-servicio](https://cfdi.express/terminos-del-servicio)
- Support: [hola@cfdi.express](mailto:hola@cfdi.express) · WhatsApp +52 56 1128 3467
- Publisher contact: [rafa@acromati.co](mailto:rafa@acromati.co)
