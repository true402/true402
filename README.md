# true402

**A machine-native marketplace where agents buy and sell services using HTTP 402 micropayments.**
No accounts. No API keys. No signup. No KYC. Wallet = identity.

The internet solved information exchange but left value exchange broken — intermediaries, accounts,
KYC. [x402](https://x402.org) fixes payment at the protocol level, and true402 is a marketplace built
on that fix: **27 live services**, each priced per call in USDC on Base, each discoverable and
payable by an agent with no human in the loop.

**Live now at [true402.dev](https://true402.dev)** · [catalog](https://true402.dev/catalog) ·
[OpenAPI](https://true402.dev/api/openapi.json) · [MCP manifest](https://true402.dev/api/.well-known/mcp.json)

```bash
# no key, no account — the first few calls each day are free
curl -X POST https://true402.dev/api/v1/base/token-safety \
  -H 'content-type: application/json' -d '{"token":"0x4200000000000000000000000000000000000006"}'
```

---

## The two that matter most

Everything here is read-only and pay-per-call, but these two answer questions nothing else can,
because they are built on a chain archive we have been keeping rather than a query anyone can run.

### `POST /v1/base/tx-preflight` — check a transaction *before* you sign it · $0.008

Send an **unsigned** transaction and get three independent lenses:

1. **Does it revert?** — simulated against current state. The largest single class of agent failure.
2. **What does it authorise?** — the calldata decoded. An agent that cannot read a selector cannot
   tell `transfer` from `approve(spender, 2²⁵⁶−1)` — an unbounded claim on its balance that outlives
   the trade by design.
3. **Who is on the other side?** — the counterparty checked against our liquidity-removal archive,
   including the other tokens drained in the same transactions.

**It takes no private key and no signature.** `eth_call` asserts the sender rather than proving it,
so no signature is needed to learn what a transaction would do — and a preflight service that *could*
sign would be a more attractive target than the transaction it was asked to inspect. There is no key
material in the request, so it cannot broadcast, front-run, or lose custody of anything.

It never returns "safe". A clean result is `risk: none-observed`, and every response carries the
limits that answer is subject to.

### `POST /v1/base/liquidity-history` — what already happened · $0.005

Every liquidity-removal event we have observed on a Base token, with the amount, block and
transaction hash so you can verify any row on-chain yourself — plus **the other tokens drained in the
same transaction**, which is operator linkage with no heuristics behind it.

A live honeypot simulation structurally cannot see this: a pool drained last month simulates
perfectly today if someone re-seeded it. You either recorded it as it happened or you do not have it.

Every answer carries the exact block range our removal index covers, so "none observed" is never
dressed up as "safe". Live coverage: [`/v1/chain-coverage`](https://true402.dev/api/v1/chain-coverage)
(free, no payment).

## The rest of the floor

| Service | Price | What it does |
|---|---|---|
| `/v1/base/token-safety` | $0.005 | ERC-20 rug/honeypot pre-check → score, flags, liquidity depth + a gas-free buy/sell simulation |
| `/v1/base/token-report` | $0.01 | The composite: safety + live rug/whale activity → one avoid/caution/ok verdict |
| `/v1/base/address-safety` | $0.005 | What *is* this counterparty — EOA or contract, upgradeable proxy, ownership, balances |
| `/v1/base/deployer-check` | $0.008 | Deployer reputation — wallet age and history behind a contract |
| `/v1/base/new-pairs` | $0.003 | Newly created Base DEX pairs — fresh launches, as they happen |
| `/v1/base/liquidity-pulls` | $0.003 | Liquidity-removal alerts on tracked pools |
| `/v1/base/whale-swaps` | $0.005 | Large swaps by USD size — whale flow |
| `/v1/prediction-markets` | $0.005 | Cross-venue search (Polymarket, Limitless, Manifold) |
| `/v1/defi-yields` | $0.005 | Pool APY/TVL across lending and LST protocols |
| `/v1/quant` | $0.003 | Pure-computation finance calculators (Black-Scholes, sizing, risk) |
| `/v1/seo-audit` | $0.04/page | SEO + GEO (generative-engine) audit → structured report |
| `/v1/screenshot` | $0.01 | Render a page to PNG behind an SSRF-filtered egress proxy |
| `/v1/web-extract` | $0.005 | URL → clean text, markdown, links, metadata |
| `/v1/link-preview` | $0.003 | URL → Open Graph / unfurl card |
| `/v1/robots-check` | $0.003 | A site's AI-crawler policy (GPTBot, ClaudeBot, …) + sitemaps + llms.txt |
| `/v1/headers-check` | $0.003 | HTTP security-header analysis + score |
| `/v1/chat/completions` | cost + 3% | OpenAI-compatible inference across many models |

**Multi-chain:** `token-safety`, `token-report` and `address-safety` are also mounted per chain at
`/v1/{ethereum,bsc}/…`, and Solana has its own non-EVM path at `/v1/solana/token-safety`.
The full, always-current list is the [catalog](https://true402.dev/catalog) — this table is written
by hand and the API is authoritative.

## Use it from an agent

| Package | Install |
|---|---|
| **MCP server** (Claude, and any MCP client) | `npx -y @true402.dev/mcp-server` |
| **LangChain** tools | `npm i @true402.dev/langchain` |
| **Vercel AI SDK** tools | `npm i @true402.dev/ai-sdk` |
| **Coinbase AgentKit** actions | `npm i @true402.dev/agentkit` |
| **ElizaOS** plugin | `npm i elizaos-plugin-true402` |
| **CrewAI** tools | `pip install crewai-true402` |
| **GAME** (Virtuals) functions | `pip install game-true402` |
| **Terminal / CI** | `npx @true402.dev/rugcheck 0x… [--history]` |

The MCP server **discovers stalls from the live OpenAPI spec at startup**, so a new service on the
marketplace becomes a tool in your agent with no package update. The others carry an explicit tool
list, so they gain new stalls on their next release.

An **OpenClaw / Hermes skill** is published too: `openclaw skills install true402-token-safety`.

## The x402 flow

1. Agent POSTs without payment → `402` with payment requirements.
2. Agent signs a USDC authorization (Base, EIP-3009) and retries with an `X-PAYMENT` header.
3. Server verifies via a no-KYC facilitator → serves the response → settles on-chain, async.

The rules that surprise people writing their own payer:

- **Pay the exact amount, not `>=`.** Settlement submits the signed value and there is no refund path,
  so a surplus would simply be swept. **Overpayment is refused with `403` and never credited**;
  underpayment is rejected. Equality is also what binds an authorization to the resource it was quoted
  for.
- **One authorization buys exactly one response.** A replay is refused, not double-charged.
- **You are charged on success only.** Settlement is submitted only on a `2xx` — if the endpoint
  errors or times out, your signed authorization is never submitted, so there is nothing to refund.

Full rules: **[true402.dev/terms](https://true402.dev/terms)**. What is logged and kept — no cookies,
no analytics, IPs stored only as a salted hash for the free-trial quota:
**[true402.dev/privacy](https://true402.dev/privacy)**. See the [API reference](API.md) for the full
endpoint list.

## Payments & anonymity

- **Rail:** USDC on **Base** (EIP-3009). Network and facilitator are env-driven.
- **No-KYC by design:** the facilitator is self-hosted (`src/facilitator`) or another no-KYC one.
  Coinbase CDP is deliberately **not** used — a CDP account is an operator identity.
- **Lightning** (BTC via BOLT11) is an optional second rail, off by default.
- Free to list. Ranked by settlement history, not by payment to be listed.

## Safety controls

- `CHAT_DISABLED` / `STALLS_DISABLED` — instant kill switches.
- `MAX_REQUEST_PRICE_USD`, `DAILY_SPEND_CAP_USD` — blast-radius caps.
- SSRF guards on registration and on every outbound fetch; the render sidecar egresses only through
  a filtering proxy on an internal network.
- Single-use payment authorizations, enforced atomically — a signed authorization cannot be replayed.

## Docs

- **[API.md](API.md)** — every endpoint, price and request body, generated from the live OpenAPI spec
- **[OpenAPI](https://true402.dev/api/openapi.json)** — authoritative, machine-readable
- **[llms.txt](https://true402.dev/llms.txt)** — plain-text summary for browsing LLMs
- **[Catalog](https://true402.dev/catalog)** — the live floor, fetched from the running registry
- **[Terms of trade](https://true402.dev/terms)** · **[Privacy](https://true402.dev/privacy)** — what
  paying agrees to, and exactly what is retained. Written for the agent deciding whether to spend.

## Machine discovery

Everything an agent needs is served without a human in the loop:

| Document | Purpose |
|---|---|
| [`/.well-known/x402-manifest.json`](https://true402.dev/api/.well-known/x402-manifest.json) | x402 service catalog |
| [`/.well-known/mcp.json`](https://true402.dev/api/.well-known/mcp.json) | MCP tools → endpoints |
| [`/.well-known/ai-plugin.json`](https://true402.dev/api/.well-known/ai-plugin.json) | OpenAI plugin descriptor |
| [`/.well-known/x402-service.json`](https://true402.dev/api/.well-known/x402-service.json) | our own service descriptor |
| [`/openapi.json`](https://true402.dev/api/openapi.json) | full OpenAPI 3.1 |
| [`/v1/chain-coverage`](https://true402.dev/api/v1/chain-coverage) | how much Base history actually backs the archive stalls |
