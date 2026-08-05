# true402 API reference

Every endpoint below is live at `https://true402.dev/api`. There is no account, no API key and no signup — paid endpoints answer `402` with payment requirements, you sign a USDC authorization on Base and retry. Most also serve a small free daily allowance per IP, so you can evaluate them with no wallet at all.

**This file is generated from the live [OpenAPI spec](https://true402.dev/api/openapi.json), which is authoritative.**

## Free

| Endpoint | | What |
|---|---|---|
| `/` | GET | Service manifest |
| `/.well-known/ai-plugin.json` | GET | OpenAI plugin manifest |
| `/.well-known/mcp.json` | GET | MCP discovery manifest |
| `/.well-known/x402-manifest.json` | GET | x402 service catalog |
| `/health` | GET | Health check |
| `/health/detailed` | GET | Detailed health check |
| `/openapi.json` | GET | OpenAPI specification |
| `/v1/models` | GET | List available models |
| `/v1/models/{modelId}` | GET | Get model details |
| `/v1/services` | GET | List registered services |
| `/v1/services/register` | POST | Register an x402 service |

## Paid

| Endpoint | Price | Body | What |
|---|---|---|---|
| `/v1/base/address-safety` | $0.005 | `{ "address": string, "chain"?: string }` | Address Safety — structural profile + risk for any Base address |
| `/v1/base/deployer-check` | $0.008 | `{ "token": string, "chain"?: string, "deep"?: boolean }` | Deployer Reputation — who created a Base token + how established that wallet is |
| `/v1/base/liquidity-history` | $0.005 | `{ "token": string, "limit"?: number }` | Observed liquidity history for a Base token |
| `/v1/base/liquidity-pulls` | $0.003 | `{ "since"?: number, "limit"?: number, "dex"?: string, "minQuote"?: number }` | Liquidity-pull / rug alerts on Base |
| `/v1/base/new-pairs` | $0.003 | `{ "since"?: number, "limit"?: number, "dex"?: string, "withToken"?: boolean }` | Recently-created Base DEX pairs |
| `/v1/base/token-report` | $0.01 | `{ "token": string, "chain"?: string }` | Token Report — flagship "can I safely ape in?" composite |
| `/v1/base/tx-preflight` | $0.008 | `{ "from": string, "to": string, "data"?: string, "value"?: string }` | Preflight an unsigned Base transaction before signing |
| `/v1/base/whale-swaps` | $0.005 | `{ "min"?: number, "dex"?: string, "since"?: number, "limit"?: number, "direction"?: string }` | Whale swaps on Base — large ($-value) DEX Swaps |
| `/v1/bsc/address-safety` | $0.005 | `{ "address": string, "chain"?: string }` | Address Safety — structural profile + risk for any BNB Smart Chain (BSC) address |
| `/v1/bsc/token-report` | $0.01 | `{ "token": string, "chain"?: string }` | Token Report — flagship "can I safely ape in?" composite |
| `/v1/bsc/token-safety` | $0.005 | `{ "token": string, "chain"?: string }` | Token safety check |
| `/v1/chat/completions` | $0.0001–5.00 | `—` | Chat completions |
| `/v1/defi-yields` | $0.005 | `{ "chain"?: string, "project"?: string, "asset"?: string, "stablecoinOnly"?: boolean, "minTvlUsd"?: number, "includeOutliers"?: boolean, "limit"?: number }` | DeFi yield aggregator |
| `/v1/ethereum/address-safety` | $0.005 | `{ "address": string, "chain"?: string }` | Address Safety — structural profile + risk for any Ethereum address |
| `/v1/ethereum/deployer-check` | $0.008 | `{ "token": string, "chain"?: string, "deep"?: boolean }` | Deployer Reputation — who created a Ethereum token + how established that wallet is |
| `/v1/ethereum/token-report` | $0.01 | `{ "token": string, "chain"?: string }` | Token Report — flagship "can I safely ape in?" composite |
| `/v1/ethereum/token-safety` | $0.005 | `{ "token": string, "chain"?: string }` | Token safety check |
| `/v1/headers-check` | $0.003 | `{ "url": string }` | HTTP security-headers check |
| `/v1/link-preview` | $0.003 | `{ "url": string }` | Link preview |
| `/v1/prediction-markets` | $0.005 | `{ "query": string, "limit"?: number }` | Prediction markets |
| `/v1/quant` | $0.003 | `{ "function": string, "params": object }` | Quant/finance calculators |
| `/v1/robots-check` | $0.003 | `{ "url": string }` | Robots / AI-crawler check |
| `/v1/screenshot` | $0.01 | `{ "url": string, "fullPage"?: boolean, "width"?: number }` | Screenshot |
| `/v1/seo-audit` | $0.04–0.8 | `{ "url": string, "mode"?: string }` | SEO/GEO audit |
| `/v1/solana/token-safety` | $0.005 | `{ "token": string, "chain"?: string }` | Solana token safety — SPL / Token-2022 structural check |
| `/v1/token-safety` | $0.005 | `{ "token": string, "chain"?: string }` | Token safety check |
| `/v1/web-extract` | $0.005 | `{ "url": string }` | Web extract |

## Paying

```bash
# 1. unpaid -> 402 with the requirements
curl -X POST https://true402.dev/api/v1/base/token-safety \
  -H 'content-type: application/json' -d '{"token":"0x…"}'

# 2. sign an EIP-3009 USDC authorization for EXACTLY accepts[0].maxAmountRequired
# 3. retry with the X-PAYMENT header -> 200
```

The amount must be **exact**, not `>=`. Settlement submits the value you signed and there is no
refund path, so an overpayment would simply be swept — it is refused with `403` instead.

Ready-made clients: `@true402.dev/mcp-server`, `@true402.dev/langchain`, `@true402.dev/ai-sdk`,
`@true402.dev/agentkit`, `elizaos-plugin-true402`, `crewai-true402`, `game-true402`, and
`npx @true402.dev/rugcheck` for a terminal.
