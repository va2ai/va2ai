# Hey, I'm Chris 👋

**Applied AI Engineer · Full-Stack Product Builder · Founder of [V2V Intelligence](https://vaclaims.net)**

> AI can generate code. The hard part is building systems that are traceable, useful, and safe enough for real users.

Applied AI and full-stack engineer with 15+ years building software and 3+ years shipping AI systems in production. I build production-grade AI products for document-heavy, high-stakes workflows — with a focus on retrieval, agents, evaluation, citation validation, and real SaaS infrastructure.

Building LLM applications since mid-2023, starting with LangChain/OpenAI retrieval QA prototypes and progressing into production multi-agent legal-intelligence systems with source-grounded retrieval, citation validation, observability, and SaaS deployment.

[GitHub](https://github.com/va2ai) · [LinkedIn](https://www.linkedin.com/in/va2ai) · [Resume](https://vaclaims.net/resume) · [V2V Intelligence](https://vaclaims.net) · [Email](mailto:ai@vaclaims.net)

---

## 🚀 What I Ship

- **Production AI systems** — multi-agent workflows, retrieval-grounded analysis, MCP tools, eval loops, and validation gates
- **Legal / GovTech intelligence products** — VA claims research, BVA/CAVC analysis, CFR/M21-1 retrieval, citation-backed synthesis
- **Full-stack SaaS platforms** — React, TypeScript, Node.js, Firebase, Stripe billing, usage gates, Cloud Run deploys
- **Agentic infrastructure** — tool calling, role-gated agents, streaming workflows, cost/latency controls, observability
- **Document automation** — upload parsing, structured extraction, official form prefill, PDF generation, and workflow routing

---

## 🏆 Highlighted Work

**[V2V Intelligence](https://vaclaims.net)** — production VA legal-intelligence SaaS for veterans, attorneys, and advocates. Built with React 19, TypeScript, Firebase, Stripe, Cloud Run, and AI research workflows across BVA decisions, CAVC appeals, 38 CFR, M21-1, KnowVA, and Federal Register sources.

**[decision-lens](https://github.com/va2ai/decision-lens)** — multi-agent document analysis pipeline turning dense administrative decisions into grounded, citation-checked reports. LangGraph orchestration, LiteLLM for model routing, Instructor for structured outputs, ChromaDB retrieval, FastAPI + React, with an adversarial critic in the synthesis loop.

**[bvaapi2](https://github.com/va2ai/bvaapi2)** — BVA Decision Search + KnowVA Knowledge Base API. The data plane behind V2V Intelligence — Cloud Run service exposing BVA, CAVC, CFR, M21-1, KnowVA, diagnostic code, and Federal Register tools for agent workflows.

**[crazy-caller](https://github.com/va2ai/crazy-caller)** — real-time AI phone assistant bridging Twilio Programmable Voice and Gemini Live API. Pure TypeScript audio codec, 30 voices, tool calling, live transcription dashboard, TDD throughout.

**[bva-citation-validator](https://github.com/va2ai/bva-citation-validator)** — post-generation citation validator for legal AI output. Detects hallucinated CFR citations, BVA docket numbers, and CAVC case references in LLM responses before they reach a user.

**[ai-agent-platform](https://github.com/va2ai/ai-agent-platform)** — multi-agent platform with Gemini 3.1, pgvector, custom runtime, and chat UI. Exploration surface for orchestration patterns that ended up in production V2V workflows.

**[rag-decision-analysis-system](https://github.com/va2ai/rag-decision-analysis-system)** — 100-decision validation harness for a graph-lite schema over BVA decisions backed by pgvector. Eval-driven schema design before scaling retrieval to 1.85M decisions.

**[edge-ai-search-validation-service](https://github.com/va2ai/edge-ai-search-validation-service)** — AI-powered deep research API on Cloudflare Workers using the OpenAI Responses API. Edge-deployed validation surface for research outputs.

**[llm-compaction-benchmark](https://github.com/va2ai/llm-compaction-benchmark)** — benchmarks LLM conversation compaction quality across 6 Gemini models (36 combinations). Built to pick a compaction model for long-running agent sessions instead of guessing.

---

## 🧠 Proof of Thinking

I do not treat "AI built this" as the signal. The signal is whether I can explain the architecture, trade-offs, failure modes, and validation strategy.

### V2V Intelligence

**What it does:**
Turns fragmented VA legal sources into a unified research and claims-intelligence platform. Users can search legal authorities, analyze decisions, inspect appellate patterns, generate issue breakdowns, and trace outputs back to sources.

**Why this architecture:**
VA claims analysis is not one retrieval call. Intake, authority mapping, evidence evaluation, conflict detection, citation validation, and synthesis all fail differently. Splitting the work into staged agents makes failures easier to isolate and repair.

**What can break:**
- Bad retrieval can poison downstream synthesis.
- BVA decisions are persuasive, not precedential.
- M21-1 policy and CFR authority require careful hierarchy handling.
- Citation-looking text can be legally irrelevant if not grounded in the retrieved record.

**What I learned:**
The most important AI reliability feature is not a bigger model. It is a validation loop that converts model uncertainty into explicit metadata instead of hiding it inside polished prose.

---

### MCP Research Platform (bvaapi2)

**What it does:**
Exposes legal research tools as a unified service surface so agents and humans can query BVA decisions, CFR sections, CAVC dockets, M21-1 content, diagnostic codes, and Federal Register material through one interface.

**Why this architecture:**
Agents perform better when tools are narrow, typed, and role-specific. A general "search everything" endpoint creates ambiguity. Separate tools create clearer contracts.

**What can break:**
- Live source APIs can change shape.
- Retrieval freshness and latency trade off against each other.
- Tool overload can degrade agent planning if every agent can call every tool.

**What I learned:**
Least-privilege agent tooling matters. Intake agents do not need the same tools as analysis agents. Role-gating reduces accidental misuse and improves debuggability.

---

### Citation Validation Loop

**What it does:**
Checks generated legal claims against retrieved authority before output is trusted.

**Why this architecture:**
LLMs can state a correct legal rule while attaching the wrong citation. That failure is hard for users to spot and dangerous in legal workflows.

**What can break:**
- Regex validation misses semantic overreach.
- Model critics can miss deterministic citation errors.
- Retrieved authority may be incomplete.

**What I learned:**
Validation needs both deterministic checks and model-based skepticism. Regex catches citation existence. A critic catches overstatement, unsupported inference, and misleading synthesis.

---

## 📝 Published / Public Work

- **[V2V Intelligence](https://vaclaims.net)** — live VA claims intelligence SaaS
- **[decision-lens](https://github.com/va2ai/decision-lens)** — multi-agent document analysis pipeline
- **[bvaapi2](https://github.com/va2ai/bvaapi2)** — BVA + KnowVA API and MCP research surface
- **[crazy-caller](https://github.com/va2ai/crazy-caller)** — Twilio + Gemini Live phone agent
- **[bva-citation-validator](https://github.com/va2ai/bva-citation-validator)** — hallucination control for legal AI
- **[GitHub Profile](https://github.com/va2ai)** — full repo history across AI, automation, and product experiments
- **[Resume](https://vaclaims.net/resume)** — applied AI, full-stack engineering, and product background

---

## 🛠 Tech Stack

**Core production stack**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=nextdotjs&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat&logo=vite&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat&logo=express&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat&logo=tailwindcss&logoColor=white)
![shadcn/ui](https://img.shields.io/badge/shadcn%2Fui-000000?style=flat&logo=shadcnui&logoColor=white)

**AI, agents, and retrieval**

![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat&logo=openai&logoColor=white)
![Claude](https://img.shields.io/badge/Claude-191919?style=flat&logo=anthropic&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=flat&logo=googlegemini&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat&logo=langchain&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?style=flat&logo=langchain&logoColor=white)
![CrewAI](https://img.shields.io/badge/CrewAI-Multi_Agent-4B5563?style=flat)
![LiteLLM](https://img.shields.io/badge/LiteLLM-4B5563?style=flat)
![Instructor](https://img.shields.io/badge/Instructor-Structured_Outputs-4B5563?style=flat)
![MCP](https://img.shields.io/badge/MCP-Model_Context_Protocol-000000?style=flat)
![RAG](https://img.shields.io/badge/RAG-Retrieval_Grounded_AI-4B5563?style=flat)
![ChromaDB](https://img.shields.io/badge/ChromaDB-Vector_Store-4B5563?style=flat)
![pgvector](https://img.shields.io/badge/pgvector-4169E1?style=flat&logo=postgresql&logoColor=white)
![Pinecone](https://img.shields.io/badge/Pinecone-000000?style=flat)
![Groq](https://img.shields.io/badge/Groq-F55036?style=flat&logo=groq&logoColor=white)

**Cloud, data, and product infrastructure**

![Google Cloud](https://img.shields.io/badge/Google_Cloud-4285F4?style=flat&logo=googlecloud&logoColor=white)
![Cloud Run](https://img.shields.io/badge/Cloud_Run-4285F4?style=flat&logo=googlecloud&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=firebase&logoColor=black)
![Cloudflare Workers](https://img.shields.io/badge/Cloudflare_Workers-F38020?style=flat&logo=cloudflareworkers&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat&logo=sqlite&logoColor=white)
![Cloudflare R2](https://img.shields.io/badge/Cloudflare_R2-F38020?style=flat&logo=cloudflare&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=flat&logo=stripe&logoColor=white)
![Twilio](https://img.shields.io/badge/Twilio-F22F46?style=flat&logo=twilio&logoColor=white)

**Testing, automation, and delivery**

![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![Vitest](https://img.shields.io/badge/Vitest-6E9F18?style=flat&logo=vitest&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=githubactions&logoColor=white)
![CI/CD](https://img.shields.io/badge/CI%2FCD-Automated_Delivery-4B5563?style=flat)
![SSE](https://img.shields.io/badge/SSE-Streaming_UI-4B5563?style=flat)
![WebSockets](https://img.shields.io/badge/WebSockets-Realtime-4B5563?style=flat)

**Extended repo history / experiments**

![Vue.js](https://img.shields.io/badge/Vue.js-4FC08D?style=flat&logo=vuedotjs&logoColor=white)
![Nuxt](https://img.shields.io/badge/Nuxt-00DC82?style=flat&logo=nuxt&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat&logo=vercel&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazonwebservices&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter_Notebook-F37626?style=flat&logo=jupyter&logoColor=white)
![Reddit API](https://img.shields.io/badge/Reddit_API-FF4500?style=flat&logo=reddit&logoColor=white)
![tastytrade](https://img.shields.io/badge/tastytrade-Options_API-4B5563?style=flat)
![yfinance](https://img.shields.io/badge/yfinance-Market_Data-4B5563?style=flat)

---

## ⚙️ Systems I Care About

- Retrieval-augmented generation that cites real sources
- Multi-agent workflows with clear handoff contracts
- AI output validation and repair loops
- SaaS billing, quota gates, and usage tracking
- Real-time streaming interfaces
- Legal / regulatory workflows where traceability matters
- Products that survive contact with real users

---

## 📫 Let's Connect

[![Website](https://img.shields.io/badge/vaclaims.net-000?style=for-the-badge&logo=safari&logoColor=white)](https://vaclaims.net)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/va2ai)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/va2ai)
[![Email](https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:ai@vaclaims.net)

---

💼 **Currently:** Building V2V Intelligence, running Generative AI Ohio, and looking for applied AI / backend / agent systems roles where production judgment matters.
