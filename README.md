<p align="center">
  <img src="assets/banner.svg" width="100%" alt="Alex Sorokin — AI Engineer · agents, evals, RAG and MCP" />
</p>

<p align="center">
  <a href="https://www.linkedin.com/in/alex-sorokin-ai-engineer/"><img src="https://img.shields.io/badge/LinkedIn-connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
  <a href="#in-production"><img src="https://img.shields.io/badge/systems-in%20production-10b981?style=for-the-badge" alt="Systems in production" /></a>
  <a href="#architecture"><img src="https://img.shields.io/badge/architecture-diagram-8b5cf6?style=for-the-badge" alt="Architecture" /></a>
</p>

<p align="center">
  <img src="assets/metrics.svg" width="100%" alt="77/87 blind backtest · 75/82 RAG golden set · 10x faster SQL p95 · 2,600+ automated tests" />
</p>

I build AI systems that do real work in a business and run them in production: a Claude agent that records customer payments, a RAG assistant over eight years of company data, a voice receptionist on a live phone line, and the integration layer that opens a closed CRM to all of them.

Every system is evaluated on real historical data before launch, and every write to a system of record passes a deterministic check and, where it matters, a human. I set the requirements, guardrails, evals and go/no-go calls and direct the design; Claude Code writes the code to my specs and tests.

## In production

| System | What it does | Measured |
|---|---|---|
| **Payments agent**<br/><sub>Claude Managed Agents · 54 MCP tools</sub> | Matches check photos to the right job and invoice; a deterministic gate writes only on proof or a manager's "yes" | Blind backtest on 87 past payments: **77 correct, 9 escalated, 1 wrong** |
| **RAG over 8 years of CRM data**<br/><sub>[emg-rag-public ↗](https://github.com/SanyaBoroda4/emg-rag-public)</sub> | Routed text-to-SQL (validated, read-only) + hybrid BM25/vector search with reranking; cited answers | **75 / 82** on a hand-verified golden set · recall@10 **0.28 → 0.82** · SQL p95 **1.96 s → 0.20 s** |
| **AI voice receptionist**<br/><sub>Retell · Twilio · n8n</sub> | Recognizes callers from the CRM before the first word, qualifies leads, logs every call back to the CRM | Latency **1.7–2.3 s → ~1.4 s** median |
| **CRM gateway**<br/><sub>[moraware-bridge ↗](https://github.com/SanyaBoroda4/moraware-bridge)</sub> | C#/.NET Web API over a closed vendor SDK with no public API | **95 routes · 230 tests** |
| **MCP server**<br/><sub>[emg-mcp-portfolio ↗](https://github.com/SanyaBoroda4/emg-mcp-portfolio)</sub> | Gives Claude the CRM: dry-run writes, READ-ONLY reads, explicit "not readable" map | **40 tools** · tests in CI |
| **Ops console**<br/><sub>[emg-console-portfolio ↗](https://github.com/SanyaBoroda4/emg-console-portfolio)</sub> | Mobile PWA where staff act on AI-read payments and slab scans through push decision cards | **105 tests** gate every deploy |
| **Thrift Agent**<br/><sub>[thrift-agent ↗](https://github.com/SanyaBoroda4/thrift-agent)</sub> | Phone photos → live listings on Poshmark, Depop and Vinted; Opus groups, Sonnet extracts, Haiku checks | **~2,100 tests** · 3-OS CI |
| **Proof of Turnover** · Hackathon 2026<br/><sub>[proof-of-turnover ↗](https://github.com/SanyaBoroda4/proof-of-turnover)</sub> | Proves vacation-rental cleanings from the cleaner's photos and voice notes: an item counts as done only when two independent vision checks and SAM 3.1 agree on one photo; gaps become re-photo requests in her language | **186 tests** · Meta models only (Muse Spark, SAM 3.1, voice) |

## Architecture

```mermaid
flowchart LR
  call([Phone call]) --> voice[Voice agent<br/>Retell + Twilio]
  photo([Check photo]) --> agent[Claude agent<br/>Managed Agents]
  ask([Office question]) --> rag{RAG router}

  voice --> n8n[n8n workflows]
  agent -- read tools --> mcp[MCP server<br/>40 tools]
  agent --> gate{{Deterministic gate}}
  gate -- proof or manager's yes --> qb[(QuickBooks)]

  mcp --> bridge[C#/.NET gateway<br/>95 routes]
  n8n --> bridge
  bridge --> crm[(CRM<br/>closed SDK)]
  n8n <-. decision cards .-> console[Ops console PWA]

  rag --> sql[Text-to-SQL<br/>validated · read-only]
  rag --> hyb[BM25 + vectors<br/>+ rerank]
  sql --> pg[(Postgres + pgvector)]
  hyb --> pg
  bridge -. export .-> pg
  rag -. traces .-> lf[Langfuse]
```

## Engineering notes

<details>
<summary><b>An agent that can't write without proof</b> · payments</summary>
<br/>

The agent investigates each payment like a person would, through 54 tools, and proposes a match in plain language. It never writes directly: a deterministic gate re-checks the evidence in code and records the payment only on proof or a manager's explicit "yes". Before launch it was run blind against 87 historical payments (77 correct, 9 escalated to a human, 1 wrong), and the rollout was gated by 1,635 automated checks and 17 adversarial reviews.
</details>

<details>
<summary><b>The slow query that wasn't where everyone looked</b> · RAG</summary>
<br/>

Median SQL time was 0.03 s; p95 was 1.96 s. Per-call tracing showed every slow call hit one view. `EXPLAIN ANALYZE` named a correlated regex subquery running 3,399 times, not the `ILIKE` everyone suspected. A 184 kB partial index plus skipping a redundant re-count took p95 to 0.20 s, with output verified byte-identical by checksum.
</details>

<details>
<summary><b>Why fusion alone made ranking worse</b> · retrieval</summary>
<br/>

Each retrieval layer was measured against the same golden set. Reciprocal-rank fusion raised recall@10 from 0.28 (vectors alone) to 0.50, but MRR fell from 0.33 to 0.20: fusion finds more and orders it worse. Adding a reranker took recall@10 to 0.82 and MRR to 0.66. Measuring recall alone would have hidden the regression.
</details>

## How I work

- **Evals before launch.** Golden sets, blind historical backtests, a judge model that differs from the generator, known failures kept visible.
- **Deterministic gates on writes.** Models propose; code checks; people approve what matters.
- **Measure, don't assert.** Every number on this page comes from a command or a run file in the linked repo.
- **Spec-driven work with coding agents.** Written work orders with verification steps, scoped permissions and a required "what got worse" section.

## Stack

<p>
<img src="https://img.shields.io/badge/Claude-191919?style=flat-square&logo=anthropic&logoColor=white" alt="Claude" />
<img src="https://img.shields.io/badge/MCP-191919?style=flat-square" alt="MCP" />
<img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python" />
<img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript" />
<img src="https://img.shields.io/badge/C%23%20%2F%20.NET-512BD4?style=flat-square&logo=dotnet&logoColor=white" alt="C# / .NET" />
<img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI" />
<img src="https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB" alt="React" />
<img src="https://img.shields.io/badge/PostgreSQL%20%2B%20pgvector-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL" />
<img src="https://img.shields.io/badge/Langfuse-191919?style=flat-square" alt="Langfuse" />
<img src="https://img.shields.io/badge/n8n-EA4B71?style=flat-square&logo=n8n&logoColor=white" alt="n8n" />
<img src="https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white" alt="Playwright" />
<img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker" />
<img src="https://img.shields.io/badge/Azure-0078D4?style=flat-square&logo=microsoftazure&logoColor=white" alt="Azure" />
<img src="https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white" alt="GitHub Actions" />
<img src="https://img.shields.io/badge/Twilio-F22F46?style=flat-square&logo=twilio&logoColor=white" alt="Twilio" />
<img src="https://img.shields.io/badge/Retell%20AI-191919?style=flat-square" alt="Retell AI" />
</p>

<p align="center"><sub>Open to AI Engineer · AI Solutions / Deployment Engineer roles — remote, or hybrid in Charleston, SC.</sub></p>
