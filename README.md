# true402

**The machine-native marketplace.** Agents discover, call, and pay for services over [x402](https://x402.org) — USDC on Base, per call. No accounts, no API keys, no KYC. The wallet is the identity.

**Live at [true402.dev](https://true402.dev)** — ~13 pay-per-call stalls:

- **Token safety** — rug check + a real buy/sell **honeypot simulation** (state-override `eth_call`: proves a token is *sellable* instead of reading static flags), full token report, address safety, deployer reputation
- **DeFi data** — new pairs, whale swaps on Base
- **Web** — SEO / GEO (AI-citability) audits
- **LLM inference** — OpenAI-compatible, multi-provider

First few safety checks each day are **free, no wallet needed** → try it at [true402.dev/check](https://true402.dev/check).

## Integrate in one minute

```bash
# CLI — rug-check a Base token (free trial, no wallet)
npx @true402.dev/rugcheck 0xTOKEN

# MCP — every stall as an agent tool (Claude, Cursor, any MCP client)
npx @true402.dev/mcp-server

# Raw HTTP — returns 402 with payment terms, pay in USDC, retry
curl -X POST https://true402.dev/api/v1/base/token-report \
  -H 'Content-Type: application/json' -d '{"token":"0x..."}'
```

- OpenAPI: [`true402.dev/openapi.json`](https://true402.dev/openapi.json)
- Catalog: [`true402.dev/api/v1/services`](https://true402.dev/api/v1/services)
- For LLMs: [`true402.dev/llms.txt`](https://true402.dev/llms.txt)
- Guides: [true402.dev/guides](https://true402.dev/guides) · Glossary: [true402.dev/glossary](https://true402.dev/glossary)

## Repos

| Repo | What |
|------|------|
| [`mcp-server`](https://github.com/true402/mcp-server) | MCP server — auto-discovers all paid stalls from the live OpenAPI |
| [`elizaos-plugin-true402`](https://github.com/true402/elizaos-plugin-true402) | ElizaOS plugin — pre-trade rug/honeypot guard for Base trading agents |

*Free to list, ranked by settlement history. The bazaar doesn't own the stalls — it is the place where trade happens.*
