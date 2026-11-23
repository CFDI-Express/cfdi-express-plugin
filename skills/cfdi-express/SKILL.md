---
name: cfdi-express
description: Issue and manage Mexican CFDI 4.0 invoices, credit notes, payment complements, payroll receipts (nómina), and retenciones through the CFDI Express MCP. Use when the user wants to preview, stamp, cancel, or look up CFDIs or SAT catalog codes.
---

# CFDI Express

Use the CFDI Express MCP tools to work in the signed-in user's account. Do not invent fiscal data.

## Rules

- Never invent an RFC, legal name, tax regime, or postal code. If a required fiscal field is missing, ask the user.
- Call the matching `preview_*` tool before `create_*`. Do not stamp until the user authorizes it. Pass the `idempotency_key` returned by the preview.
- `unitPrice` is tax-inclusive by default (`pricesIncludeTax: true`): it already includes transferred IVA/IEPS. To send prices before tax, pass `pricesIncludeTax: false`.
- Some clients may show an interactive preview (for example a card with «Autorizar y timbrar»). Do not rely on it. If the client has no interactive views (for example Grok Bot), summarize the preview in text and wait for an explicit confirmation in the chat before calling `create_*` with the same `idempotency_key`.
- Search SAT catalogs (`search_product_codes`, `search_unit_codes`) instead of guessing keys.
- Stamping or canceling in production consumes a timbre and is irreversible.
- Certificates (CSD) and new issuers are managed in the [dashboard](https://dash.cfdi.express), not through MCP.
- Production (`https://api.cfdi.express/mcp`) issues real tax invoices. The SAT sandbox is `https://api.cfdi.express/mcp/test`.

## Typical flow

1. `list_merchants` and `get_balance`.
2. `search_product_codes` (and unit codes if needed).
3. `preview_invoice` (or `preview_credit_note`, `preview_payment`, `preview_nomina`).
4. Show the preview to the user (an interactive card if the client renders one; otherwise a text summary). After they explicitly confirm in chat, call `create_*` with the preview `idempotency_key`.
5. `get_*` for the official PDF and XML links.

Docs: [https://cfdi.express/docs/api/mcp](https://cfdi.express/docs/api/mcp)
