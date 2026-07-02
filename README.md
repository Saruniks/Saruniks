<h2> Hi, I'm Šarūnas 👋 </h2>

[![Linkedin Badge](https://img.shields.io/badge/-sarunasgincas-blue?style=flat-square&logo=Linkedin&logoColor=white&link=https://www.linkedin.com/in/sarunas-gincas-a91676211/)](https://www.linkedin.com/in/sarunas-gincas-a91676211/)

Trying out **serverless-first**: TypeScript end-to-end - SPA, Lambdas, and CDK
infra as one codebase. I collapse complexity rather than add it - retired a
Rust/DSQL stack and a Dockerized frontend for a leaner all-TypeScript, static-SPA
architecture. Currently building **real-money market-making tooling** for Polymarket.

- 🤖 **AI-powered** - Claude (via Amazon Bedrock) does natural-language market discovery and LLM-in-the-loop infra triage.
- 👁 **Observable** - CloudWatch dashboard + alarms, X-Ray/Lambda Insights, and an AI pipeline that summarizes health to Telegram.
- 🧱 **Serverless-first** - static SPA on S3/CloudFront; EventBridge-scheduled Lambdas over a single small RDS Postgres (the one always-on piece, migrated off Aurora DSQL).

### 🛠 Stack

`TypeScript` · `React + Vite (SPA)` · `AWS Lambda` · `RDS Postgres`
`S3 · CloudFront · Cognito` · `AWS CDK` · `Claude API`

---

### 📊 Dashboard - live earnings-market charts + alerts console

<img width="923" alt="Analytics dashboard with a grid of earnings-market price charts and a live alerts console" src="https://github.com/user-attachments/assets/bcc9c508-115b-46bd-91ac-e5a97fc96700" />

### 🤖 AI market discovery - build views from a prompt or a trader's wallet

<img width="242" height="413" alt="download" src="https://github.com/user-attachments/assets/f335ded3-b440-49f4-a791-6bd249f9fa57" />

### 💧 Liquidity Rewards - markets paying for liquidity provision

<img width="920" alt="Liquidity Rewards table showing per-market earnings, risk bands, min size, max spread and liquidity" src="https://github.com/user-attachments/assets/fc9deb48-ac36-4b33-9c7e-d7101b317f0d" />

### 📡 Market Monitor - real-time new-market feed by category

<img width="464" alt="Market Monitor feed streaming newly listed stock markets in real time" src="https://github.com/user-attachments/assets/89d5602a-d593-430d-8973-baf8f9d59ef5" />

---

<details>
<summary><b>Previously (Rust)</b></summary>

<br>

Frontend with **Yew** / **Tailwind**, backend in **Rust** (**gRPC**, REST, **clap-rs**), **PostgreSQL** + **diesel-rs**, infra on **AWS CDK**.

**Past projects**

- [Slop Rust Full Stack App + AWS CDK for CI/CD](https://github.com/Saruniks/cdk-rust-full-stack-app)
- [Distributed sccache HashiCorp Packer + CDK for Terraform IaC](https://github.com/Saruniks/sccache-dist-cdktf)

**Open source contributions**

- reqwest [#1295](https://github.com/seanmonstar/reqwest/issues/1295)
- sccache [#2161](https://github.com/mozilla/sccache/issues/2161) · ~~[#2164](https://github.com/mozilla/sccache/issues/2164)~~ · ~~[#2241](https://github.com/mozilla/sccache/pull/2241)~~

</details>
