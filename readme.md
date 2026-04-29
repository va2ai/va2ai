# Hey, I'm Chris 👋

**Applied AI Engineer · Full-Stack Product Builder · Founder of [V2V Intelligence](https://vaclaims.net)**

> AI can generate code. The hard part is building systems that are traceable, useful, and safe enough for real users.

Applied AI and full-stack engineer with 15+ years building software and 3+ years shipping AI systems in production. I build production-grade AI products for document-heavy, high-stakes workflows — with a focus on retrieval, agents, evaluation, citation validation, and real SaaS infrastructure.

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

**[VA Claims Intelligence Repo](https://github.com/va2ai/va-claims-intel)** — full-stack AI platform showing how I build: product UI, authentication, subscription gating, usage tracking, document workflows, living knowledge base patterns, attorney research mode, and production-oriented AI interfaces.

**MCP Research Platform** — 25+ tool research surface deployed on GCP Cloud Run, exposing BVA, CAVC, CFR, M21-1, KnowVA, diagnostic code, Federal Register, and RAG-style research tools for agent workflows.

**Post-Generation Citation Validator** — hallucination-control system for legal AI output: grounded generation, structured claim extraction, deterministic citation checking, adversarial critic review, and repair feedback loops.

**Twilio + Gemini Live Voice Agent** — real-time phone intake agent using Twilio Programmable Voice and Gemini Live API for conversational customer-facing workflows.

---

## 🧠 Proof of Thinking

I do not treat “AI built this” as the signal. The signal is whether I can explain the architecture, trade-offs, failure modes, and validation strategy.

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

### MCP Research Platform

**What it does:**  
Exposes legal research tools as a unified service surface so agents and humans can query BVA decisions, CFR sections, CAVC dockets, M21-1 content, diagnostic codes, and Federal Register material through one interface.

**Why this architecture:**  
Agents perform better when tools are narrow, typed, and role-specific. A general “search everything” endpoint creates ambiguity. Separate tools create clearer contracts.

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
- **[VA Claims Intelligence](https://github.com/va2ai/va-claims-intel)** — public product repository and implementation surface
- **[GitHub Profile](https://github.com/va2ai)** — AI systems, automation, legal intelligence, trading tools, and product experiments
- **[Resume](https://vaclaims.net/resume)** — applied AI, full-stack engineering, and product background

---

## 🛠 Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=firebase&logoColor=black)
![Google Cloud](https://img.shields.io/badge/Google_Cloud-4285F4?style=flat&logo=googlecloud&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=flat&logo=stripe&logoColor=white)
![Twilio](https://img.shields.io/badge/Twilio-F22F46?style=flat&logo=twilio&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat&logo=openai&logoColor=white)
![Claude](https://img.shields.io/badge/Claude-191919?style=flat&logo=anthropic&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=flat&logo=googlegemini&logoColor=white)

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
