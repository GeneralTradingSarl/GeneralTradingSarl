<div align="center">

[![QuantSphere Terminal — live implied volatility surface, Kalman filtering, Markowitz frontier](assets/hero.gif)](https://github.com/GeneralTradingSarl/quantsphere-terminal)

<sub>Real frames from <a href="https://github.com/GeneralTradingSarl/quantsphere-terminal">QuantSphere Terminal</a>. Every figure shown is read off the running app.</sub>

https://github.com/user-attachments/assets/6df2443f-786e-4214-8a00-23146b106845

<sub>Press play: 47 seconds on who I am, what I build, and what I am open to.</sub>

# Ismaël LADJOHOUNLOU

**I build systems that run unattended.**

Execution engines, market makers, automation pipelines: software where a retry that
duplicates an order, a webhook processed twice, or a token that expired at 3am costs real
money.

[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://ismael-portfolio-liard.vercel.app/en)
[![Upwork](https://img.shields.io/badge/Upwork-6FDA44?style=for-the-badge&logo=upwork&logoColor=white)](https://www.upwork.com/freelancers/~01498331f7c7800fc0)
[![Email](https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:general.trading.bot.2025@gmail.com)

</div>

---

## Ask me anything, literally

Two ways to get an answer without waiting for me to wake up:

[![Ask on GitHub](https://img.shields.io/badge/Ask%20here-open%20an%20issue-24292F?style=for-the-badge&logo=github&logoColor=white)](https://github.com/GeneralTradingSarl/GeneralTradingSarl/issues/new?title=Question&body=Ask%20anything%20about%20the%20work%20listed%20on%20this%20profile.)
[![Ask on the site](https://img.shields.io/badge/Or%20on%20the%20site-live%20assistant-D4AF37?style=for-the-badge&logo=vercel&logoColor=white)](https://ismael-portfolio-liard.vercel.app/en)

Open an issue and an assistant answers in the thread, in your language. It is
handed this README as its only source of facts and is not allowed past it: ask
for an hourly rate, a named client or a trading performance figure and it will
tell you it does not have that rather than produce one. Clients under a
confidentiality agreement stay unnamed. I read every thread myself.

The same assistant sits on the portfolio, where it also knows the private
client work, the figures behind each build and the starting prices.

---

## Flagship work

Five systems built end to end, each readable in full. The test counts are what the suites
actually run, and CI runs them on every push.

<table>
<tr>
<td width="33%" valign="top">

### [QuantSwap](https://github.com/GeneralTradingSarl/QuantSwap)

[![CI](https://github.com/GeneralTradingSarl/QuantSwap/actions/workflows/ci.yml/badge.svg)](https://github.com/GeneralTradingSarl/QuantSwap/actions/workflows/ci.yml)

A constant product **decentralised exchange**: Solidity pair, router and sliding-window TWAP
oracle with flash swaps and EIP-2612 permit; an event indexer that survives chain
reorganisations; a Next.js and wagmi interface.

**43 tests**, including reentrancy attempted from inside a flash-swap callback and randomised
invariant runs asserting that k never decreases.

`Solidity` `Hardhat` `viem` `Next.js` `wagmi`

</td>
<td width="33%" valign="top">

### [Sluice](https://github.com/GeneralTradingSarl/sluice)

[![CI](https://github.com/GeneralTradingSarl/sluice/actions/workflows/ci.yml/badge.svg)](https://github.com/GeneralTradingSarl/sluice/actions/workflows/ci.yml)

A **streaming data pipeline** written from the log up: segmented append-only files with
CRC-checked records and crash recovery, credit-based backpressure, idempotent producers, and
exactly-once materialisation into a time-series store.

**35 tests** over the failure modes, and a benchmark that publishes the durability trade-off
in numbers rather than adjectives.

`TypeScript` `Node 22` `SQLite`

</td>
<td width="33%" valign="top">

### [quant-mcp](https://github.com/GeneralTradingSarl/quant-mcp)

[![CI](https://github.com/GeneralTradingSarl/quant-mcp/actions/workflows/ci.yml/badge.svg)](https://github.com/GeneralTradingSarl/quant-mcp/actions/workflows/ci.yml)

A **Model Context Protocol server** giving an agent typed market data, indicators,
backtesting and position sizing. Bounded responses, errors a model can recover from, and
execution assumptions returned with every backtest.

**24 tests**, including a real MCP client driving a real handshake. Read-only by design.

`MCP` `TypeScript` `Zod`

</td>
</tr>
<tr>
<td width="33%" valign="top">

### [Cascade](https://github.com/GeneralTradingSarl/Cascade)

[![CI](https://github.com/GeneralTradingSarl/Cascade/actions/workflows/ci.yml/badge.svg)](https://github.com/GeneralTradingSarl/Cascade/actions/workflows/ci.yml)

An **Instagram to WhatsApp pipeline** on the official APIs: idempotent publishing with media
container polling, webhook lead capture and scoring, outreach gated by consent and the
WhatsApp 24 hour service window.

**64 tests**, none of which need credentials. Three importable n8n workflows.

`TypeScript` `Graph API` `WhatsApp Cloud API` `n8n`

</td>
<td width="33%" valign="top">

### [QuantSphere Terminal](https://github.com/GeneralTradingSarl/quantsphere-terminal)

An **institutional quantitative terminal**: C++20 numerical core with an API-identical NumPy
fallback, options pricing (Black-Scholes, Heston, Merton, Monte Carlo, Crank-Nicolson PDE
with PSOR), Kalman filtering, GARCH forecasting, walk-forward optimisation.

**88 verification checks**: closed-form parity, Monte Carlo within standard error, native and
fallback agreeing to 1e-14.

`C++20` `pybind11` `Python` `Streamlit`

</td>
<td width="33%" valign="top">

### What they have in common

External systems that fail, retries that must not duplicate, and state that has to survive a
crash. Different domains, one discipline.

Every one of them runs from a clean clone with `npm install` or `pip install -r`, and every
test suite finishes in seconds without a container, a cloud account or an API key.

That is deliberate: a reviewer who has to provision infrastructure to see your work does not
see your work.

</td>
</tr>
</table>

---

## Where the difficulty actually is

Not in the feature. In the state around it. A sample of decisions from the code above, each
one a thing that breaks in production long before it breaks in a demo:

| The problem | Where | What it does about it |
|---|---|---|
| The chain changes its mind after you indexed it | [QuantSwap indexer](https://github.com/GeneralTradingSarl/QuantSwap/blob/main/indexer/src/indexer.js) | Every indexed block header is stored; the tip is compared against the chain on each poll, a fork deletes exactly the rows above it, and reserves are rebuilt from the last surviving `Sync` |
| A retry publishes the same post twice | [Cascade publisher](https://github.com/GeneralTradingSarl/Cascade/blob/main/src/instagram/publisher.ts) | An idempotency key per post and a persisted container id, so a retry resumes the publish it already started instead of beginning a second one |
| Spot price is manipulable inside a single block | [QuantSwap oracle](https://github.com/GeneralTradingSarl/QuantSwap/blob/main/contracts/oracle/QuantSwapOracle.sol) | Sliding-window TWAP. One test has a whale collapse spot by more than half while the oracle moves under 0.5%; the next shows it does converge once the skew is actually held, because a TWAP is a cost and not immunity |
| Messaging somebody who never opted in | [Cascade consent gate](https://github.com/GeneralTradingSarl/Cascade/blob/main/src/whatsapp/outreach.ts) | One function that every send passes: consent, service window, opt-out list, audit entry. A new workflow cannot route around it |
| Meta answers HTTP 200 with an error body | [Graph client](https://github.com/GeneralTradingSarl/Cascade/blob/main/src/core/graph.ts) | Retries classified by error code: back off on 4 / 80007 / 429, never retry 190 or 100, because retrying a dead token only delays the fix |
| The consumer is slower than the producer | [Sluice broker](https://github.com/GeneralTradingSarl/sluice/blob/main/src/broker/broker.ts) | Writes are refused with a retry hint once the slowest group falls behind. Buffering without bound turns a slow consumer into an out-of-memory kill that loses what was buffered |
| The same records are delivered twice | [Sluice sink](https://github.com/GeneralTradingSarl/sluice/blob/main/src/sink/timeseries.ts) | Rows, incremental rollups and the consumer offset move in one transaction, so a full replay applies nothing and every aggregate is unchanged |
| A tool answers with five thousand rows | [quant-mcp guards](https://github.com/GeneralTradingSarl/quant-mcp/blob/main/src/guards.ts) | Responses are truncated with the total stated. A tool that floods the context window pushes the user's own question out of it |
| Prompt injection reaches a dangerous tool | [quant-mcp threat model](https://github.com/GeneralTradingSarl/quant-mcp/blob/main/docs/SECURITY.md) | There is no dangerous tool: no writes, no network, no child processes. Cheaper than making a model immune to persuasion |

---

## Shipped for clients

The repositories above show how I build. This is what has reached production, with numbers a
reviewer can re-count and, where the product is public, a link that opens.

| What | State | The part that was hard |
|---|---|---|
| [**Praxis Academy**](https://praxisacademy.xyz), a white-label trading academy | Live, paywall taking real payments | 184 written lessons, 72 narrated cinematic stages, 17 playable exercise types, bilingual throughout, and a mobile money paywall running in production |
| [**Aptus**](https://aptus-tableau.vercel.app), a blackboard an AI reads | Live | The mentor never sees a photograph. It reads the board's structured state, every fraction, exponent and solid edge, and answers in brass in the margin. Exact 3D solids by convex hull, with net unfolding |
| **Mercatis**, a B2B marketplace from China to West Africa | Private repository, 841 automated checks | One product page carrying two competing offers, a single delivery run when two carts share a courier, escrow, lifetime affiliate tracking, and 817 hardcoded French strings brought down to zero |
| **paiements-bj**, three mobile money payment APIs | Private, sandbox proven end to end | MTN over REST/OAuth2, Moov over SOAP, Celtiis through the QoSIC gateway, in a module with no runtime dependencies. 251 tests, emulators that reproduce the operators' failures rather than only their happy path, and 24 documented gotchas, seven of which appear in no official documentation |
| **Real-time coordination platform** with contradiction detection | Delivered, in client testing | FastAPI, Postgres with pgvector, Redis, WebSocket. The arbiter stopped inventing conflicts once each decision carried the event it belongs to, not only the subject it is about: a reconnaissance date and an execution date had been two competing answers to one question |
| **Deriv options bot**, Telegram-driven | Delivered | The recovery ladder is sized from the payout the broker quotes on the contract about to be bought, not from an assumed 100%. Measured on a demo account: 15 recovery ladders closed, each at exactly +0.82, and a session won on a 36% win rate |

Clients under a confidentiality agreement are described without being named, with no
screenshot and no link.

---

## What I work on

<table>
<tr>
<td width="50%" valign="top">

**Automation & integration**

Workflow and API automation, n8n orchestration, webhook infrastructure with signature
verification and idempotency, durable job queues with dead letters, Docker deployment, CI/CD.

</td>
<td width="50%" valign="top">

**AI & intelligent agents**

LLM integration and tool use, multi-agent and computer-use systems, retrieval and research
pipelines, natural language automation wired into real systems rather than into demos.

</td>
</tr>
<tr>
<td width="50%" valign="top">

**Data engineering & analytics**

ETL and on-chain event indexing, structured extraction, time-series storage, dashboards and
performance analytics, SQL and NoSQL schema design, real-time monitoring.

</td>
<td width="50%" valign="top">

**Quantitative & algorithmic trading**

Expert Advisors and indicators for MetaTrader 4/5, Smart Money Concepts, derivative pricing,
volatility modelling, backtesting with honest execution assumptions, risk management.

</td>
</tr>
</table>

## Stack

<div align="center">

<img src="https://skillicons.dev/icons?i=py,cpp,ts,js,solidity,react,nextjs,nodejs,postgres,sqlite,docker,githubactions,vercel,linux,bash,git" />

<sub>Also MQL4 / MQL5 · Pine Script · Hardhat · viem and wagmi · Supabase · n8n · Streamlit · Playwright</sub>

</div>

## Writing

### [The Money Path](https://github.com/GeneralTradingSarl/the-money-path)

A 126 page handbook on designing, securing and auditing a Stripe integration that deserves to run in production. 18 chapters, a reference implementation in TypeScript and PostgreSQL, and a 72 point grid for reviewing an integration you did not write. Free PDF.

`Stripe` `PCI DSS 4.0.1` `SCA and 3D Secure` `webhooks` `idempotency` `disputes` `Connect`

### [Le Fil d'Exécution](https://github.com/GeneralTradingSarl/le-fil-d-execution)

A second handbook, 104 pages, in French: designing, operating and governing automated workflows with n8n, from a laptop prototype to a system you can invoice a client for. Twenty-three chapters and six case studies. An English edition is in preparation.

`n8n` `idempotence` `queue mode` `observability` `agents` `governance`

## Other repositories

| Repository | What it is |
|---|---|
| [ismael-portfolio](https://ismael-portfolio-liard.vercel.app/en) | Bilingual Next.js portfolio: marketplace, blog, live market widgets, full structured data |
| [Smart-Money-Concepts](https://github.com/GeneralTradingSarl/Smart-Money-Concepts) | MetaTrader 5 indicator: order blocks, liquidity zones, market structure, fair value gaps |
| [computer_Agent](https://github.com/GeneralTradingSarl/computer_Agent) | Computer-use automation experiments on the Agent S framework, with MetaTrader integration |
| [claude-code-vs-codex-cli-guide](https://github.com/GeneralTradingSarl/claude-code-vs-codex-cli-guide) | Bilingual field guide comparing two AI coding command line tools |
| [mql4_experts](https://github.com/GeneralTradingSarl/mql4_experts) · [expert-mt5](https://github.com/GeneralTradingSarl/expert-mt5) · [mql5 indicators](https://github.com/GeneralTradingSarl?tab=repositories&q=mql) | Curated archives of open-source MetaTrader code, kept for reference. Third-party work, not mine |

<div align="center">

<sub>5+ years shipping production software · Benin · remote worldwide · French and English</sub>

</div>

## Working together

Available for **automation, integration, data and trading systems** work.

The fastest way to judge whether I am the right hire: pick any file linked in the table above
and ask me about it on a call.

[Upwork](https://www.upwork.com/freelancers/~01498331f7c7800fc0) ·
[Portfolio](https://ismael-portfolio-liard.vercel.app/en) ·
[Email](mailto:general.trading.bot.2025@gmail.com)

<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0072FF,100:00C6FF&height=120&section=footer" width="100%"/>
</div>
