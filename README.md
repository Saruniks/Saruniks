<h2> Hi, I'm Šarūnas 👋 </h2>

[![Linkedin Badge](https://img.shields.io/badge/-sarunasgincas-blue?style=flat-square&logo=Linkedin&logoColor=white&link=https://www.linkedin.com/in/sarunas-gincas-a91676211/)](https://www.linkedin.com/in/sarunas-gincas-a91676211/)

Trying out **serverless-first**: TypeScript end-to-end - SPA, Lambdas, and CDK
infra as one codebase.

I collapse complexity rather than add it. Recently retired a Rust/DSQL stack and a
Dockerized frontend in favour of a leaner all-TypeScript, static-SPA architecture.
Currently building **real-money market-making tooling** for Polymarket, with
Claude-powered market discovery.

### 🛠 Stack

`TypeScript` · `React + Vite (SPA)` · `AWS Lambda` · `RDS Postgres`
`S3 · CloudFront · Cognito` · `AWS CDK` · `Claude API`

---

### 📊 Dashboard - live earnings-market charts + alerts console

<img width="923" alt="Analytics dashboard with a grid of earnings-market price charts and a live alerts console" src="https://github.com/user-attachments/assets/bcc9c508-115b-46bd-91ac-e5a97fc96700" />

### 💧 Liquidity Rewards - markets paying for liquidity provision

<img width="920" alt="Liquidity Rewards table showing per-market earnings, risk bands, min size, max spread and liquidity" src="https://github.com/user-attachments/assets/fc9deb48-ac36-4b33-9c7e-d7101b317f0d" />

### 📡 Market Monitor - real-time new-market feed by category

<img width="464" alt="Market Monitor feed streaming newly listed stock markets in real time" src="https://github.com/user-attachments/assets/89d5602a-d593-430d-8973-baf8f9d59ef5" />

---

### 🧱 Compute & scheduling

Serverless-first: the frontend is a **static SPA** on S3 + CloudFront (no app
server), and every backend job is an **AWS Lambda**. There is exactly one
persistent, stateful piece by design:

- **Persistent state** — a single `db.t4g.micro` **RDS Postgres 16** instance
  (single-AZ, 20 GB, IAM-auth). This replaced the previous **Aurora DSQL**
  setup: the "0-to-1" of always-on compute, kept deliberately small while
  everything around it stays serverless. Each Lambda owns its own schema
  (idempotent `CREATE TABLE IF NOT EXISTS` on connect — no migration runner).
- **Background processes** are **EventBridge-scheduled** Lambdas, each on its own
  cadence:

  | Cadence | Jobs |
  |---|---|
  | every 1 min | alert checker |
  | every 2 min | trade pollers |
  | every 5 min | market ingestor · liquidity poller · market monitor · user monitor · orderbook-history poller · earnings-cache refresher |
  | every 10–15 min | AI health sweep · indicators refresher |
  | hourly / 6h | indicators scan · earnings-history refresher · AI health digest |

  On-demand work (order placement, market discovery, user views) runs through
  **HTTP-API-fronted Lambdas** instead of a schedule.

### 👁 Observability

A single CloudWatch layer plus an **AI-assisted alerting pipeline**, all wired in CDK:

- **CloudWatch dashboard + alarms** — per-Lambda invocations, errors, throttles
  and p50/p95/p99 duration, plus application-level EMF metrics for the ingestor
  (run outcomes, rows loaded/failed/skipped, Gamma latency, and a
  **data-freshness** alarm on `MAX(market_created_at)` so a "successful" run
  that writes stale data still pages).
- **X-Ray** service map + **Lambda Insights** enhanced metrics, linked from the dashboard.
- **Logs Insights saved query** — one-click "trace one run by `run_id`" drill-in.
- Alarms fan out to an **SNS topic** → Telegram (see below).

### 🤖 AI-powered features

<img width="242" height="413" alt="image" src="https://github.com/user-attachments/assets/6b09e2fa-4739-4047-a613-4682172c7293" />

Everything runs on **Claude via Amazon Bedrock** — no `ANTHROPIC_API_KEY`; each
Lambda authenticates with its **IAM role** (`bedrock:InvokeModel`). Cheap
**Haiku 4.5** is the default for the high-frequency jobs; triage uses **Sonnet**.
Model is overridable per-Lambda via `BEDROCK_MODEL_ID`.

**Market discovery** (`ai-api` Lambda) — the "New view" modal takes a
natural-language prompt ("Iran", "tech earnings"), Claude expands it into 3–5
keywords, and each keyword hits Polymarket's Gamma search in parallel; results
are deduped and dropped into a dashboard view. `POST /markets/find-by-prompt`.

**AI observability pipeline** (`obs-*` Lambdas) — LLM-in-the-loop monitoring on
top of the raw CloudWatch alarms:

| Lambda | Trigger | What it does |
|---|---|---|
| `obs-forwarder` | SNS alarm | Dumb, dependency-free relay — posts the raw alarm to Telegram so you *always* hear about it, even if the AI path fails. |
| `obs-triage` | SNS alarm | Gathers recent logs + alarm reason, asks Claude (Sonnet) for likely root cause + fix, posts to Telegram. Best-effort. |
| `obs-sweep` | every 10 min | Pulls the metric picture (checker heartbeat, RDS storage/conns/CPU), asks Claude for a health verdict + trend forecast; pings Telegram only when something's off. |
| `obs-digest` | scheduled (6h) | Always-on heartbeat: infra health + alert activity + trend forecast + log anomalies, summarized by Claude into a compact Telegram digest. |

**Setup** — Bedrock access comes from the Lambda IAM roles at deploy time (no
key to manage). Telegram delivery is optional: set `TELEGRAM_BOT_TOKEN` /
`TELEGRAM_CHAT_ID` (passed through CDK context) and the `obs-*` Lambdas start
posting; leave them unset and the pipeline runs silently against CloudWatch only.

---

<details>
<summary><b>Previously (Rust)</b></summary>

<br>

Frontend with **Yew** / **Tailwind**, backend in **Rust** (**gRPC**, REST, **clap-rs**), **PostgreSQL** + **diesel-rs**, infra on **AWS CDK**.

**Past projects**

- [Rust Full Stack App](https://github.com/Saruniks/cdk-rust-full-stack-app)
- [Distributed sccache IaC](https://github.com/Saruniks/sccache-dist-cdktf)

**Open source contributions**

- reqwest [#1295](https://github.com/seanmonstar/reqwest/issues/1295)
- sccache [#2161](https://github.com/mozilla/sccache/issues/2161) · ~~[#2164](https://github.com/mozilla/sccache/issues/2164)~~ · ~~[#2241](https://github.com/mozilla/sccache/pull/2241)~~

</details>
