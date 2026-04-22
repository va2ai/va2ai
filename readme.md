<h1 align="center">Hi, I'm Chris 👋</h1>

<p align="center">
  <b>Applied AI / LLM Engineer • Production RAG & Agentic Systems • Backend Architect</b>
</p>

<p align="center">
  I ship AI systems engineered like infrastructure: multi-agent pipelines, RAG, real-time voice AI, and LLM benchmarks — tested, observable, production-hardened, and <b>live in production</b>.
</p>

<p align="center">
  <a href="https://github.com/va2ai">GitHub</a> •
  <a href="https://www.linkedin.com/in/va2ai">LinkedIn</a> •
  <a href="https://vaclaims.net/resume">Resume</a> •
  <a href="mailto:ai@vaclaims.net">ai@vaclaims.net</a> •
  <a href="https://vaclaims.net">Portfolio</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Claude%20API-D97757?style=for-the-badge&logo=anthropic&logoColor=white" />
  <img src="https://img.shields.io/badge/LangGraph-1C3C3C?style=for-the-badge" />
  <img src="https://img.shields.io/badge/MCP-FF6F61?style=for-the-badge" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL%20%2B%20pgvector-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/GCP%20Cloud%20Run-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white" />
  <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
</p>

---

### 🚢 Currently Shipping

#### [**V2V Intelligence**](https://vaclaims.net) — Live SaaS (deployed March 30, 2026)
*A two-layer VA disability claims intelligence product used by veterans and attorneys.*
*   **Frontend ([vaintel](https://github.com/va2ai/vaintel)):** React 19 + Firebase Auth/Firestore + Stripe. Powers BVA case search, Nexus Scout, Decision Deconstructor, VA Math Calculator, and embedded AI chat.
*   **Multi-agent intelligence backend:** 12-agent production system on GCP Cloud Run with PostgreSQL/pgvector, LangGraph, and the Anthropic Claude API. Handles claim analysis, Claim Fix Packet document generation, automated daily executive briefs, and content pipeline publishing to Cloudflare R2.
*   **Engineering rigor:** Ablation-tested pipeline configurations (E2A/E2B/E3A/E3B) to empirically compare multi-agent vs. single-prompt architectures — architectural decisions driven by measured output quality, not vibes.

---

### 🧠 Featured Projects

#### [**BVA Research API & MCP Platform**](https://github.com/va2ai/bvaapi2)
*FastAPI service + MCP server exposing 25+ tools across BVA decisions, 38 CFR, CAVC case law, M21-1, and the Federal Register — usable by any MCP-compatible client.*
*   **The Impact:** Turns unstructured regulatory corpus into structured, queryable research infrastructure for AI agents — the retrieval layer powering V2V Intelligence.
*   **Key Features:** USA.gov search integration, section-aware regex preset search (9 built-in presets for CFR citations, nexus opinions, diagnostic codes), batch operations, rate-limited concurrent request handling, Dockerized Cloud Run deployment.

#### [**Post-Generation Citation Validator**](https://github.com/va2ai/bva-citation-validator)
*Production hallucination detection pipeline — sentinel-tagged context, grounded generation, structured extraction, cross-reference validation.*
*   **The Impact:** Reduced citation hallucination from ~15% to under 1.5% of sessions in a legal intelligence platform where fabricated citations directly harm veterans' claims.
*   **Key Features:** Node.js + Anthropic API, dual-mode demo (grounded vs. ungrounded), live BVA API verification, web GUI with model selection and customizable system prompts.

#### [**LLM Compaction Benchmark**](https://github.com/va2ai/llm-compaction-benchmark)
*Benchmarking framework measuring conversation compaction quality across 6 Gemini models in a 36-combination matrix.*
*   **The Impact:** Discovered smaller/faster models (gemini-2.5-flash-lite at 2s) outperform larger ones (gemini-2.5-pro at 14s) for compaction, with the best combination achieving 132% quality retention. Prompt engineering lifted scores from 70% to 97%+.
*   **Key Features:** Fact recall, task continuation, and hallucination detection tests; interactive Chart.js dashboards; 100% fact recall and zero hallucination in top model pairs.

#### [**Crazy Caller**](https://github.com/va2ai/crazy-caller)
*Real-time AI phone assistant — makes outbound calls via Twilio, conducts natural conversation using Gemini Live API, streams live transcripts to a web dashboard.*
*   **The Impact:** End-to-end real-time voice AI — two concurrent WebSocket streams bridged, G.711 mulaw ↔ PCM16 resampling, barge-in interruption, mid-conversation tool calling.
*   **Key Features:** Twilio Programmable Voice + Gemini 3.1 Flash Live, 30 voice options, personality presets (Professional, Assertive, Southern Charm, etc.), pure TypeScript audio conversion (no ffmpeg / native deps), live WebSocket dashboard.

#### [**GCP MCP Deployer**](https://github.com/va2ai/gcp-mcp-deployer)
*A Claude Skill that takes any MCP server GitHub repo and produces a production-ready Cloud Run deployment — Dockerfile generation, Artifact Registry, Secret Manager, dedicated service account, health checks, and Claude Desktop config included.*
*   **The Impact:** Collapses multi-hour MCP deployment workflows into a single conversation with Claude. Works with any MCP server — Node.js, Python, TypeScript, FastMCP, or custom.
*   **Key Features:** Automatic transport/port/secret detection, generated end-to-end `deploy.sh` with preflight checks, API enablement, and health verification.

#### [**Multi-Agent Ideation Pipeline**](https://github.com/va2ai/nexus-deep-research)
*5-phase AI system where specialized agents collaborate to discover, generate, validate, and rank ideas autonomously.*
*   **The Impact:** Orchestrates 5 specialized agents (Trend Hunter, Ideator, Validator, Scorer, Synthesizer) through a pub/sub event bus, producing ranked startup ideas from a single industry prompt.
*   **Key Features:** FastAPI + SSE streaming, live agent status dashboard, Gemini Deep Research integration, LLM-powered narrative report generation, full observability metrics, and 200+ tests.

#### [**AI Agent Orchestration Platform**](https://github.com/va2ai/ai-agent-orchestration-platform)
*A "roundtable refinement" multi-agent system designed for high-accuracy reasoning.*
*   **The Impact:** Implemented parallel critic loops and convergence conditions that significantly accelerated refinement cycles compared to sequential chains.
*   **Key Features:** Structured outputs for iterative stability, run-metadata capture for debugging, and traceable history for audit logs.

#### [**AI Agent Platform**](https://github.com/va2ai/ai-agent-platform)
*Multi-agent AI platform with pluggable tools, parallel + compositional function calling, agent delegation, and a built-in research system.*
*   **The Impact:** Reference implementation of modern agent patterns — parallel tool calls, agent-to-agent delegation, and multi-depth research loops — as a reusable chassis.
*   **Key Features:** Interactive chat UI with model selector, agent picker, tool mode, research depth slider, system prompt editor, and live tool activity display; Gemini-powered.

#### [**BVA Decision Intelligence**](https://github.com/va2ai/bva-decision-intelligence)
*Multi-agent research platform for deep BVA decision analysis — hybrid search, outcome classification, automated report generation.*
*   **The Impact:** Turns natural-language research objectives into comprehensive, citation-verified reports with PDF/DOCX export.
*   **Key Features:** Full-text + Qdrant vector hybrid search, automatic Granted/Denied/Remanded/Mixed outcome classification, token/cost tracking, OpenRouter integration.

#### [**CAVC Tracker**](https://vaclaims.net)
*Legal-tech product for veterans-law attorneys tracking Court of Appeals for Veterans Claims dockets.*
*   **The Impact:** Purpose-built docket-monitoring layer over CAVC filings — alerts, case timelines, and decision tracking for a niche underserved by general legal-tech.
*   **Key Features:** Landing page live at [vaclaims.net](https://vaclaims.net); ingestion and timeline views built on the BVA research infrastructure.

#### [**Edge Deep Research & Validation API**](https://github.com/va2ai/edge-ai-search-validation-service)
*Edge-deployed research agent that performs multi-round search and returns citation-backed synthesis.*
*   **The Impact:** Developed robust source-quality scoring and conflict detection to mitigate hallucinations under strict edge latency constraints.
*   **Key Features:** OpenAI Responses API on Cloudflare Workers, designed for low-latency validation and high-recall research against live web and internal data sources.

#### [**RAG Decision Analysis System**](https://github.com/va2ai/rag-decision-analysis-system)
*Extraction + hybrid retrieval pipeline for processing large corpuses of regulated legal decisions.*
*   **The Impact:** Achieved high retrieval recall by framing development around evaluation rigor — focusing on reproducible extraction and defensible output chains.
*   **Key Features:** Hybrid search optimization, entity extraction at scale, and similarity search for high-stakes decision matching, validated on a 100-decision test set.

#### [**DevPortAI RAG Fact-Check**](https://github.com/va2ai/devportai)
*Production-ready RAG fact-checking monorepo — FastAPI backend, React frontend, PostgreSQL + pgvector.*
*   **The Impact:** Reference architecture stress-testing grounding reliability in RAG pipelines via multi-stage verification layers.
*   **Key Features:** Full-stack monorepo, pgvector-backed semantic retrieval, Dockerized for repeatable deployment.

---

### 🎯 What I Bring
*   **Ships to production:** Not a prototype-only engineer. I've deployed a multi-agent SaaS on GCP that real users depend on, with daily automated content pipelines and monetization live.
*   **Operational Efficiency:** Drastically reduced manual analyst prep and review time for complex case work through grounded automation and validation layers.
*   **Audit-Ready Reliability:** Specialized in output consistency and traceability, achieving high-density citation coverage and minimal false-citation rates via custom evaluation frameworks.
*   **Performance Optimization:** Built production-grade workflows optimized for cost-efficiency and low-latency response times in interactive, multi-turn AI sessions.

---

### 🛠 Core Competencies
*   **Agent Orchestration:** Multi-agent coordination (LangGraph), routing, memory management, tool-use guardrails, MCP server authoring and deployment.
*   **Retrieval Engineering:** Advanced chunking strategies, embedding optimization, hybrid (keyword + semantic) search, pgvector and Qdrant at scale.
*   **Real-Time Systems:** Voice AI, WebSocket bridging, SSE streaming, sub-second latency under concurrent load.
*   **Evals & Governance:** Building golden datasets, regression testing, ablation studies, and production telemetry.
*   **Data Intelligence:** Extracting structured, actionable insights from high-volume, "messy" legal and policy documentation.

---

### 🤝 Let's Connect
📍 **Location:** Miamisburg / Dayton / Cincinnati (Remote OK)
📧 **Email:** [ai@vaclaims.net](mailto:ai@vaclaims.net)
📝 **Note to Recruiters:** I specialize in the bridge between "AI hype" and "Applied Reality." If you need an engineer who has shipped production multi-agent systems and understands evaluation rigor, grounding, and operational cost — let's talk.

<p align="center">
  <img height="170" src="https://github-readme-stats.vercel.app/api?username=va2ai&show_icons=true&theme=dark&hide_border=true" />
  <img height="170" src="https://github-readme-stats.vercel.app/api/top-langs/?username=va2ai&layout=compact&theme=dark&hide_border=true" />
</p>
