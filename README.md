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

## Flagship work

Three systems built end to end, each readable in full. The test counts are what the suites
actually run, and CI runs them on every push.

<table>
<tr>
<td width="33%" valign="top">

### [QuantSwap](https://github.com/GeneralTradingSarl/QuantSwap)

[![CI](https://github.com/GeneralTradingSarl/QuantSwap/actions/workflows/ci.yml/badge.svg)](https://github.com/GeneralTradingSarl/QuantSwap/actions/workflows/ci.yml)

A constant product **decentralised exchange**: Solidity pair, router and sliding-window TWAP
oracle with flash swaps and EIP-2612 permit; an event indexer that survives chain
reorganisations; a Next.js and wagmi interface.

**43 tests**, including reentrancy attempted from inside a flash-swap callback, a flash swap
repaid 99.9% of what is owed, and randomised invariant runs asserting that k never decreases
and no liquidity provider is ever diluted.

`Solidity` `Hardhat` `viem` `Next.js` `wagmi`

</td>
<td width="33%" valign="top">

### [Cascade](https://github.com/GeneralTradingSarl/Cascade)

[![CI](https://github.com/GeneralTradingSarl/Cascade/actions/workflows/ci.yml/badge.svg)](https://github.com/GeneralTradingSarl/Cascade/actions/workflows/ci.yml)

An **Instagram to WhatsApp pipeline** on the official APIs: idempotent publishing with media
container polling, webhook lead capture and explainable scoring, and outreach gated by
consent and the WhatsApp 24 hour service window.

**64 tests**, none of which need credentials or a network. Three importable n8n workflows and
a read-only operations console.

`TypeScript` `Node 22` `Graph API` `WhatsApp Cloud API` `n8n`

</td>
<td width="33%" valign="top">

### [QuantSphere Terminal](https://github.com/GeneralTradingSarl/quantsphere-terminal)

An **institutional quantitative terminal**: C++20 numerical core with an API-identical NumPy
fallback, options pricing (Black-Scholes, Heston, Merton, Monte Carlo, Crank-Nicolson PDE
with PSOR for early exercise), Kalman filtering, GARCH forecasting, walk-forward optimisation.

**88 verification checks**: closed-form parity, Monte Carlo within standard error, native and
fallback agreeing to 1e-14, mechanical look-ahead guards.

`C++20` `pybind11` `Python` `Streamlit`

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
