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
| `/v1/services/register` | GET | The registration contract — schema, worked example, common failures |
| `/v1/services/register` | POST | Register an x402 service (see **Listing a service** below) |

## Paid

| Endpoint | Price | Body | What |
|---|---|---|---|
| `/v1/backlinks` | $0.10 | `{ "target": string }` | A domain's backlink profile: referring domains and pages, the dofollow split that carries authority, authority rank, spam score, broken backlinks… |
| `/v1/base/address-safety` | $0.005 | `{ "address": string, "chain"?: string }` | Structural safety profile for ANY Base address — an EOA or an arbitrary contract — before an agent sends funds to it, approves it, or calls it. |
| `/v1/base/deployer-check` | $0.008 | `{ "token": string, "chain"?: string, "deep"?: boolean }` | Resolves the deployer of a Base token and profiles that wallet's reputation: age (tx-count), balance, contracts shipped, and whether it is a FRESH… |
| `/v1/base/dossier` | $0.10 | `{ "token": string, "chain"?: string }` | Everything we know about a Base ERC-20, in one call. |
| `/v1/base/liquidity-history` | $0.005 | `{ "token": string, "limit"?: number }` | Observed liquidity history for a Base token, from our own DEX archive. |
| `/v1/base/liquidity-pulls` | $0.003 | `{ "since"?: number, "limit"?: number, "dex"?: string, "minQuote"?: number }` | Liquidity-pull / rug alerts on Base — Burn (liquidity-removal) events on recently-launched DEX pools (the new-pairs watcher's set). |
| `/v1/base/new-pairs` | $0.003 | `{ "since"?: number, "limit"?: number, "dex"?: string, "withToken"?: boolean }` | Recently-created Base DEX pairs (Uniswap V3 + Aerodrome) from a background log-watcher — fresh token launches for trading agents/snipers. |
| `/v1/base/token-report` | $0.01 | `{ "token": string, "chain"?: string }` | The flagship composite for a Base ERC-20 — one call instead of five. |
| `/v1/base/tx-preflight` | $0.008 | `{ "from": string, "to": string, "data"?: string, "value"?: string }` | Check a Base transaction BEFORE signing it. |
| `/v1/base/whale-swaps` | $0.005 | `{ "min"?: number, "dex"?: string, "since"?: number, "limit"?: number, "direction"?: string }` | Recent large ($-value) DEX Swap events (Uniswap V3 + Aerodrome) on the Base pools the new-pairs watcher tracks — a whale-following / copy-trading s… |
| `/v1/bsc/address-safety` | $0.005 | `{ "address": string, "chain"?: string }` | Structural safety profile for ANY BNB Smart Chain (BSC) address — an EOA or an arbitrary contract — before an agent sends funds to it, approves it… |
| `/v1/bsc/token-report` | $0.01 | `{ "token": string, "chain"?: string }` | The flagship composite for a BNB Smart Chain (BSC) ERC-20 — one call instead of five. |
| `/v1/bsc/token-safety` | $0.005 | `{ "token": string, "chain"?: string }` | Rug/honeypot safety check for an ERC-20 token on BNB Smart Chain (BSC) (from on-chain reads — no API key): ERC-20 conformance, ownership renounce… |
| `/v1/chat/completions` | $0.0001–5.00 | — | OpenAI-compatible chat completions endpoint. |
| `/v1/defi-yields` | $0.005 | `{ "chain"?: string, "project"?: string, "asset"?: string, "stablecoinOnly"?: boolean, "minTvlUsd"?: number, "includeOutliers"?: boolean, "limit"?: number }` | Filter + rank live DeFi lending/staking pool APYs across every protocol/chain (DefiLlama, ~15k pools). |
| `/v1/ethereum/address-safety` | $0.005 | `{ "address": string, "chain"?: string }` | Structural safety profile for ANY Ethereum address — an EOA or an arbitrary contract — before an agent sends funds to it, approves it, or calls it. |
| `/v1/ethereum/deployer-check` | $0.008 | `{ "token": string, "chain"?: string, "deep"?: boolean }` | Resolves the deployer of a Ethereum token and profiles that wallet's reputation: age (tx-count), balance, contracts shipped, and whether it is a FR… |
| `/v1/ethereum/token-report` | $0.01 | `{ "token": string, "chain"?: string }` | The flagship composite for a Ethereum ERC-20 — one call instead of five. |
| `/v1/ethereum/token-safety` | $0.005 | `{ "token": string, "chain"?: string }` | Rug/honeypot safety check for an ERC-20 token on Ethereum (from on-chain reads — no API key): ERC-20 conformance, ownership renounce, mint-capabili… |
| `/v1/headers-check` | $0.003 | `{ "url": string }` | Fetch a URL and analyse its HTTP security headers (HSTS, CSP, X-Frame-Options, …) into present/missing + a 0–100 score. |
| `/v1/keyword-ideas` | $0.05 | `{ "seed": string, "limit"?: integer, "location"?: integer, "language"?: string }` | Related and long-tail keyword ideas for a seed term, each with search volume, CPC, competition and search intent. |
| `/v1/keyword-volume` | $0.15 | `{ "keywords": string[], "location"?: integer, "language"?: string }` | Monthly search volume, CPC and competition for a batch of keywords, with a 12-month trend per term. |
| `/v1/link-preview` | $0.003 | `{ "url": string }` | Fetch a URL and return its Open Graph card (title, description, image, site name, favicon, canonical). |
| `/v1/prediction-markets` | $0.005 | `{ "query": string, "limit"?: number }` | Keyword-search live prediction markets across Polymarket, Limitless, and Manifold; returns normalized markets (probability outcomes, USD volume, cl… |
| `/v1/quant` | $0.003 | `{ "function": string, "params": object }` | Deterministic finance calculators in one dispatch endpoint. |
| `/v1/ranked-keywords` | $0.05 | `{ "target": string, "limit"?: integer, "location"?: integer, "language"?: string }` | Which keywords a domain already ranks for in organic search, with position, search volume, CPC and competition. |
| `/v1/robots-check` | $0.003 | `{ "url": string }` | Fetch a site's robots.txt + llms.txt and report whether the major AI crawlers are allowed/blocked, plus sitemaps. |
| `/v1/screenshot` | $0.01 | `{ "url": string, "fullPage"?: boolean, "width"?: number }` | Render a web page in headless Chromium and return a PNG screenshot as base64 JSON. |
| `/v1/seo-audit` | $0.04–0.2 | `{ "url"?: string, "urls"?: string[], "mode"?: string }` | Audit web pages for SEO + GEO (generative-engine optimization). |
| `/v1/solana/token-safety` | $0.005 | `{ "token": string, "chain"?: string }` | Rug/trap safety check for a Solana SPL or Token-2022 token: mint authority (supply inflation), freeze authority (the Solana honeypot — the issuer c… |
| `/v1/token-safety` | $0.005 | `{ "token": string, "chain"?: string }` | Rug/honeypot safety check for an ERC-20 token on Base (from on-chain reads — no API key): ERC-20 conformance, ownership renounce, mint-capability… |
| `/v1/web-extract` | $0.005 | `{ "url": string }` | Fetch a web page and return clean readable text + light markdown + title/description/links. |

## Listing a service

Free. No payment, no approval, no account — the wallet in your manifest is your identity.

There are two ways in. Most registrations that fail do so because the caller used neither.

**1. Publish a manifest (recommended).** Serve this at `https://<your-domain>/.well-known/x402-service.json`:

```json
{
  "x402": "1.0",
  "name": "my-service",
  "description": "A useful x402 service",
  "capabilities": ["summarize"],
  "pricing": { "currency": "USDC", "base": "0.001", "unit": "request" },
  "payment": {
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "chain": "base",
    "facilitator": "https://pay.openfacilitator.io"
  },
  "endpoint": "https://my-service.example.com/v1/process"
}
```

Then register with just the origin — we fetch that document:

```bash
curl -X POST https://true402.dev/api/v1/services/register \
  -H 'content-type: application/json' \
  -d '{"url":"https://my-service.example.com"}'
```

**2. Send the manifest inline.** Use this when you cannot serve the well-known path:

```bash
curl -X POST https://true402.dev/api/v1/services/register \
  -H 'content-type: application/json' \
  -d '{"url":"https://my-service.example.com","manifest":{ ...the object above... }}'
```

### If it is refused

| Status | `type` | What to do |
|---|---|---|
| 422 | `manifest_unavailable` | We could not fetch `<url>/.well-known/x402-service.json` and you sent no `manifest`. Publish that document, or include `manifest` in the body. |
| 422 | `manifest_invalid` | The document was fetched but is not a valid manifest. The response names the failing field. |
| 400 | `validation_error` | `url` is missing or not a valid URL. |

`GET /v1/services/register` returns this same contract as JSON, including the worked example.

## Paying

```bash
# 1. unpaid -> 402 with the requirements (body + a base64 PAYMENT-REQUIRED header)
curl -X POST https://true402.dev/api/v1/base/token-safety \
  -H 'content-type: application/json' -d '{"token":"0x…"}'

# 2. sign an EIP-3009 USDC authorization for EXACTLY accepts[i].amount
# 3. retry with the PAYMENT-SIGNATURE header -> 200 + a PAYMENT-RESPONSE receipt
```

The amount must be **exact**, not `>=`. Settlement submits the value you signed and there is no
refund path, so an overpayment would simply be swept — it is refused with `402` instead.

### Payment headers: v2 and v1 are both accepted

true402 speaks **x402 v2**. Send the base64-encoded payment in `PAYMENT-SIGNATURE`. The v1 header
name `X-PAYMENT` is still accepted, because many clients — including our own npm packages up to
1.2.x — send it.

| You send | Result |
|---|---|
| `PAYMENT-SIGNATURE` (v2, recommended) | ✅ |
| `X-PAYMENT` (v1 name) | ✅ same checks, same answer |
| Both, with the **same** value | ✅ one payment |
| Both, with **different** values | `400` — we never guess which one you meant |

The payload inside can take either layout:

```jsonc
// v2 (what @x402/fetch and our clients from 1.3 send)
{ "x402Version": 2, "accepted": { /* the accepts[] entry you chose, verbatim */ }, "payload": { "signature": "0x…", "authorization": { … } } }

// v1 layout — scheme/network at the top level (still accepted)
{ "x402Version": 2, "scheme": "exact", "network": "eip155:8453", "payload": { … } }
```

**Why accepting both is safe:** the header name and the layout only tell us *where to look*. What is
verified is always the signed authorization, against **our own quote**: the chain, the USDC contract,
`payTo`, and the exact amount. So neither form can steer verification elsewhere:

- `network` must be the CAIP-2 id we quoted (`eip155:8453`). A v1 alias like `base`, a testnet or
  another chain is refused, not translated.
- A top-level `scheme`/`network` that contradicts `accepted` is refused.
- Each authorization is accepted **once**, keyed on the signed nonce, not on the header. Replaying
  it under the other header name or in the other layout is still a replay (`402`).

**Receipts** come back in both `PAYMENT-RESPONSE` (v2) and `X-PAYMENT-RESPONSE` (v1), with the same
value. A payment that fails verification or settlement answers `402` with a failure receipt; a
malformed payload answers `400`.

**A client built only for v1 cannot pay us.** Our 402 names networks in CAIP-2 form
(`eip155:8453`), which the v2 spec requires and directory crawlers validate, and v1-only clients
(e.g. `x402-fetch` 1.x) reject that before they sign anything. Use a v2 client — `@x402/fetch`,
the official Python `x402` package, the Go module, or one of ours below.

Ready-made clients: `@true402.dev/mcp-server`, `@true402.dev/langchain`, `@true402.dev/ai-sdk`,
`@true402.dev/agentkit`, `elizaos-plugin-true402`, `crewai-true402`, `game-true402`, and
`npx @true402.dev/rugcheck` for a terminal.
