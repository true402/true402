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
| `/v1/base/deployer-check` | $0.008 | `{ "token": string, "chain"?: string, "deep"?: boolean }` | Resolves the deployer of a Base token and profiles that wallet's reputation: age (tx-count), balance, and whether it is a FRESH throwaway spun up f… |
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
| `/v1/ethereum/deployer-check` | $0.008 | `{ "token": string, "chain"?: string, "deep"?: boolean }` | Resolves the deployer of a Ethereum token and profiles that wallet's reputation: age (tx-count), balance, and whether it is a FRESH throwaway spun… |
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

## Also on Agentic.Market (via FiatDock)

Twenty of the stalls are also sold through the [FiatDock](https://fiatdock.com) marketplace,
so they show up in Coinbase's x402 Bazaar and in Agentic.Market search. Same checks, same prices,
and the payment still goes straight to the true402 wallet (`0xFe257Ef6Dc89F5b688aC52Ef3eb8648C629fA9C4`).
Use whichever your agent already knows how to pay: each listing is a plain x402 endpoint (`POST`,
the tool's arguments as the JSON body, USDC on Base).

| Listing | Price | Body |
|---|---|---|
| [Token Safety](https://fiatdock.com/s/svc_712fa970-0f2a-4092-8042-e75946cead03) | $0.005 | `{ "token": string, "chain"?: "base" \| "ethereum" \| "bsc" }` |
| [Token Report](https://fiatdock.com/s/svc_2bbf8ea2-baed-4f72-abac-6295f880b67a) | $0.01 | `{ "token": string, "chain"?: "base" \| "ethereum" \| "bsc" }` |
| [Address Safety](https://fiatdock.com/s/svc_78c2e7d0-a1a6-4617-a45b-c0ab017037bc) | $0.005 | `{ "address": string, "chain"?: "base" \| "ethereum" \| "bsc" }` |
| [Deployer Check](https://fiatdock.com/s/svc_fc424a1d-8f50-42dc-8d65-d99ce85bee46) | $0.008 | `{ "token": string, "chain"?: "base" \| "ethereum" }` |
| [Tx Preflight](https://fiatdock.com/s/svc_b459b285-77ee-4882-88c4-b08f4a401b91) | $0.008 | `{ "from": string, "to": string, "data"?: string, "value"?: string }` (Base) |
| [Token Dossier](https://fiatdock.com/s/svc_eddced45-d48d-4857-a853-571760dde719) | $0.10 | `{ "token": string }` (Base) |
| [Solana Token Safety](https://fiatdock.com/s/svc_59058cbb-f8a1-451c-bd26-ae89ab7cc5df) | $0.005 | `{ "token": string }` (base58 mint) |
| [Liquidity History](https://fiatdock.com/s/svc_12587477-5940-4e4a-ac20-aefcb9a35baa) | $0.005 | `{ "token": string, "limit"?: number }` (Base) |
| [New Pairs](https://fiatdock.com/s/svc_a397f690-70c1-4e79-851a-9e1d436d03b8) | $0.003 | `{ "since"?: number, "limit"?: number, "dex"?: string }` (Base) |
| [Liquidity Pulls](https://fiatdock.com/s/svc_fbe25ea2-04b2-4983-ac92-891a4bb68479) | $0.003 | `{ "since"?: number, "limit"?: number, "dex"?: string, "minQuote"?: number }` (Base) |
| [Whale Swaps](https://fiatdock.com/s/svc_65609f66-a093-445d-ada7-9d13fed22953) | $0.005 | `{ "min"?: number, "dex"?: string, "since"?: number, "limit"?: number, "direction"?: string }` (Base) |
| [Prediction Markets](https://fiatdock.com/s/svc_600c8759-f2e1-4bd2-96fc-737e5255c3e9) | $0.005 | `{ "query": string, "limit"?: number }` |
| [DeFi Yields](https://fiatdock.com/s/svc_b5dd18c2-c82a-4f67-b287-b90598a670a5) | $0.005 | `{ "chain"?: string, "project"?: string, "asset"?: string, "stablecoinOnly"?: boolean, "minTvlUsd"?: number, "limit"?: number }` |
| [Quant](https://fiatdock.com/s/svc_64d3be32-5ced-4d2d-89fb-02132b0088dd) | $0.003 | `{ "function": string, "params": object }` |
| [Screenshot](https://fiatdock.com/s/svc_bbc969e8-c8dc-49f7-804e-f87d5d9044ad) | $0.01 | `{ "url": string, "fullPage"?: boolean, "width"?: number }` |
| [Web Extract](https://fiatdock.com/s/svc_8b5c724e-a162-4b80-9976-7975b58c1552) | $0.005 | `{ "url": string }` |
| [Link Preview](https://fiatdock.com/s/svc_f8d26c50-b1ed-49b5-b962-7bfcb6eb11f4) | $0.003 | `{ "url": string }` |
| [SEO + GEO Page Audit](https://fiatdock.com/s/svc_e60d1345-ee04-4f6f-9c85-915c6970976d) | $0.04 | `{ "url": string, "mode"?: "both" \| "seo" \| "geo" }` (one page) |
| [Robots Check](https://fiatdock.com/s/svc_5c3f119b-73f0-4ef0-b456-ddd188c2e701) | $0.003 | `{ "url": string }` |
| [Security Headers Check](https://fiatdock.com/s/svc_9b3a144e-bef3-4031-81b7-e40b51012564) | $0.003 | `{ "url": string }` |

Calling `true402.dev` directly gives you more: multi-page SEO audits, the keyword and backlink
stalls, and the free daily allowance (FiatDock calls are always paid).

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

### Changing a listing

Two ways, depending on what you control:

- **You control the domain:** update `/.well-known/x402-service.json` and register again. Free, and it
  also works if you have lost the payout wallet.
- **You control the payout wallet:** `POST /v1/services/{id}/update` with `{"manifest": { ...the new
  manifest... }}`. It costs **$0.01**, and the payment must be signed by the wallet the listing pays
  today (its `manifest.payment.address`, Base USDC). That signature is the proof that it is your
  listing, so this is also how you move a listing to a new payout address.

The payment must also be **bound to this exact change**: set the EIP-3009 `nonce` to

```
keccak256("true402:listing-update:v1:" + <id> + ":" + <the exact request body you send>)
```

so a $0.01 payment your wallet signed for anything else (another purchase, or a different update)
cannot be used to change your listing. A stock x402 client picks a random nonce, so sign this one
yourself, for example with viem:

```js
const body = JSON.stringify({ manifest });
const nonce = keccak256(toBytes(`true402:listing-update:v1:${id}:${body}`));
// signTypedData TransferWithAuthorization { from: payoutWallet, to: accepts[0].payTo,
//   value: accepts[0].amount, validAfter, validBefore, nonce } — then POST `body` unchanged
//   with the PAYMENT-SIGNATURE header.
```

```bash
# 1. unpaid -> 402 quoting $0.01 (accepts[0] gives payTo and amount)
curl -X POST https://true402.dev/api/v1/services/<id>/update \
  -H 'content-type: application/json' -d '{"manifest":{ ...new manifest... }}'
# 2. sign FROM the listing's current payout wallet with the bound nonce, retry the SAME body -> 200
```

You are only charged when the update is applied. Any refusal is a 4xx and costs nothing:

| Status | `type` | Meaning |
|---|---|---|
| 403 | `not_listing_owner` | The payment came from a different wallet. The message names the one that must sign. |
| 403 | `payment_not_bound` | The nonce is not bound to this update. The message gives the nonce expected for the body you sent. |
| 403 | `payer_unproven` | Not a Base USDC (EIP-3009) payment, so the paying wallet cannot be established. |
| 400 | `validation_error` / `invalid_url` | The new manifest is invalid, or its endpoint or facilitator is not a public address. |
| 404 | `not_found` | No such listing. |
| 409 | `unchanged` | The manifest is identical to the stored one. |

The listing keeps its `id`, `url` and history.

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
name `X-PAYMENT` is still accepted, because many clients — including our own npm packages released
before 30 September 2026 — send it.

| You send | Result |
|---|---|
| `PAYMENT-SIGNATURE` (v2, recommended) | ✅ |
| `X-PAYMENT` (v1 name) | ✅ same checks, same answer |
| Both, with the **same** value | ✅ one payment |
| Both, with **different** values | `400` — we never guess which one you meant |

The payload inside can take either layout:

```jsonc
// v2 (what @x402/fetch and our current clients send)
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

### Update notices for our clients

From `@true402.dev/mcp-server` 1.2.0, the npm SDKs 1.4.0 (`rugcheck` 1.3.0, `elizaos-plugin-true402`
1.5.0) and the Python packages 1.4.0, each client sends a User-Agent like `true402-langchain/1.4.0`.
When a newer release exists, the API answers with three headers:

| Header | Value |
|---|---|
| `X-True402-Client-Latest` | the current version, e.g. `1.5.0` |
| `X-True402-Client-Update` | `recommended`, or `required` when your version has a security fix pending |
| `X-True402-Client-Notice` | the same, as a sentence for a human reading headers |

The client then prints **one line, once per process**: a `console.warn` / `warnings.warn` in the SDKs,
and an extra text block in the first tool result for the MCP server (stdout is its protocol, and most
hosts hide stderr). Nothing is blocked — every payment is verified on our side whatever the client.

Two deliberate choices:

- **No call to npm or PyPI from your machine.** The check rides on requests you were already making to
  true402.
- **The client writes the message, not the server.** It prints its own package name and install
  command, and takes only a strictly validated `x.y.z` from the response. A client can be pointed at any
  seller, and a hostile one must not be able to make it tell you to install something else.

Versions before these send no identifying User-Agent and cannot be told — update them by hand:
`npm i <package>@latest` or `pip install -U <package>`.
