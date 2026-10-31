<div align="center">

<h1>Alex Sorokin</h1>

<p><b>AI Engineer</b> · production LLM systems: agents, evals, RAG and MCP</p>

<img src="https://readme-typing-svg.demolab.com?font=Inter&weight=500&size=18&duration=3200&pause=900&color=2F9FB4&center=true&vCenter=true&width=620&lines=Claude+agents+behind+deterministic+guardrails;RAG+%2B+text-to-SQL%2C+measured+on+golden+sets;MCP+servers+that+open+legacy+systems+to+AI;Voice+agents+wired+into+a+live+CRM" alt="What I build" />

<p>
<a href="https://www.linkedin.com/in/alex-sorokin-ai-engineer/"><img src="https://img.shields.io/badge/LinkedIn-alex--sorokin--ai--engineer-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
<img src="https://img.shields.io/badge/Charleston%2C%20SC-remote%20or%20hybrid-1f2937?style=flat-square" alt="Location" />
</p>

</div>

---

### About

I build AI systems that do real work in a business and run them in production: a Claude agent that records customer payments, a RAG assistant over eight years of company data, a voice receptionist on a live phone line, and the integration layer that makes a closed CRM usable by all of them.

Every system is evaluated on real historical data before launch, and every write to a system of record goes through a deterministic check and, where it matters, a human. I set the requirements, guardrails, evals and go/no-go calls and direct the design; Claude Code writes the code to my specs and tests.

### In production

| System | What it does | Measured |
|---|---|---|
| **Payments agent** · Claude Managed Agents, 54 MCP tools | Matches check photos to the right job and invoice; a deterministic gate writes only on proof or a manager's "yes" | Blind backtest on 87 past payments: 77 correct, 9 escalated, 1 wrong |
| **RAG over 8 years of CRM data** · [emg-rag-public](https://github.com/SanyaBoroda4/emg-rag-public) | Hybrid keyword + vector search with reranking, validated read-only text-to-SQL, cited answers | 75 / 82 on a hand-verified golden set · recall@10 0.28 → 0.82 · SQL p95 1.96 s → 0.20 s |
| **AI voice receptionist** · Retell, Twilio, n8n | Recognizes callers from the CRM before the first word, qualifies leads, logs every call back to the CRM | Latency 1.7–2.3 s → ~1.4 s median |
| **CRM gateway** · [moraware-bridge](https://github.com/SanyaBoroda4/moraware-bridge) | C#/.NET Web API over a closed vendor SDK that has no public API | 95 routes · 230 tests |
| **MCP server** · [emg-mcp-portfolio](https://github.com/SanyaBoroda4/emg-mcp-portfolio) | 40 tools that give Claude the CRM; dry-run writes, READ-ONLY reads | 40 tools · tests in CI |
| **Ops console** · [emg-console-portfolio](https://github.com/SanyaBoroda4/emg-console-portfolio) | Mobile PWA where staff act on AI-read payments and slab scans via push decision cards | 105 tests gate every deploy |
| **Thrift Agent** · [thrift-agent](https://github.com/SanyaBoroda4/thrift-agent) | Phone photos to live listings on Poshmark, Depop and Vinted with a multi-model pipeline | ~2,100 tests · 3-OS CI |

### How I work

- **Evals before launch.** Golden sets, blind historical backtests, a judge model that differs from the generator, known failures kept visible.
- **Deterministic gates on writes.** Models propose; code checks; people approve what matters.
- **Measure, don't assert.** Every number above comes from a command or a run file in the linked repo.
- **Spec-driven work with coding agents.** Written work orders with verification steps, scoped permissions, and a required "what got worse" section.

### Stack

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

---

<div align="center">
<sub>Open to AI Engineer · AI Solutions / Deployment Engineer roles — remote, or hybrid in Charleston, SC.</sub>
</div>
