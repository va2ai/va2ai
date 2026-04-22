<h1 align="center">Hi, I'm Chris 👋</h1>

<p align="center">
  <b>Applied AI / LLM Engineer • Multi-Agent Systems • Backend</b>
</p>

<p align="center">
  I ship production AI for a niche I understand: VA disability claims. For every project below you'll see what it does, why I built it that way, where it's fragile, and what I learned — because in 2026, shipping isn't the signal. Thinking is.
</p>

<p align="center">
  <a href="https://github.com/va2ai">GitHub</a> •
  <a href="https://www.linkedin.com/in/va2ai">LinkedIn</a> •
  <a href="https://vaclaims.net/resume">Resume</a> •
  <a href="mailto:ai@vaclaims.net">ai@vaclaims.net</a> •
  <a href="https://vaclaims.net">vaclaims.net</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Claude%20API-D97757?style=for-the-badge&logo=anthropic&logoColor=white" />
  <img src="https://img.shields.io/badge/Gemini-4285F4?style=for-the-badge&logo=google&logoColor=white" />
  <img src="https://img.shields.io/badge/LangGraph-1C3C3C?style=for-the-badge" />
  <img src="https://img.shields.io/badge/MCP-FF6F61?style=for-the-badge" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Postgres%20%2B%20pgvector-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/GCP%20Cloud%20Run-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white" />
  <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
</p>

---

### 🚢 Currently Shipping

#### [**V2V Intelligence**](https://vaclaims.net) — Live at vaclaims.net

*Two-layer VA disability claims intelligence product for veterans, attorneys, and accredited advocates. A React 19 + TypeScript + Firebase front-end ([va-claims-intel](https://github.com/va2ai/va-claims-intel)) with a Stripe 4-tier billing and a Twilio + Gemini Live API phone agent, riding on a FastAPI multi-agent backend ([v2v-monorepo](https://github.com/va2ai/v2v-monorepo)) on GCP Cloud Run — LangGraph, Postgres + pgvector, Anthropic Claude. Handles unified search across 1.85M+ BVA decisions / 38 CFR / M21-1 / CAVC dockets, decision-letter analysis, claim-theory pressure-testing, and auto-generated daily intelligence briefs.*

Single-prompt LLM calls don't reliably decompose claim analysis — intake, retrieval, citation extraction, analysis, and refinement are genuinely different jobs — so the backend splits work across 6 specialized agents (`intake`, `planner`, `retrieval`, `analysis`, `cite`, `refinement`) with a coordinator + message-bus layer that can fan out into task teams. Keeping the consumer surface on React/Firebase/Stripe lets product iteration stay fast while the intelligence layer evolves independently on Cloud Run. The two stacks talk through a stable contract, not a shared codebase.

My biggest single dependency is Claude API pricing and rate limits. BVA source-format changes would ripple through retrieval. Observability is split across Firebase and Cloud Run, which makes incident response slower than I'd like. The win I'm proudest of is splitting the UX into **Veteran mode** (simplified denial-analysis wizard) vs. **Professional mode** (full research workspace with CAVC Tracker, Precedent Finder, and the Research Orchestrator): same backend, two completely different surface areas. The lesson was that audience fit beats feature parity. Pros hated the hand-holding; veterans drowned in the firehose. One product could not be one UI.

---

### 🧠 Featured Projects

#### [**BVA Research API & MCP Platform**](https://github.com/va2ai/bvaapi2)

*FastAPI service + MCP server exposing 27 tools across BVA decisions, 38 CFR, CAVC case law, KnowVA, diagnostic codes, Federal Register, and a RAG layer. Deployed on Cloud Run with a separate `Dockerfile.mcp`, usable by any MCP-compatible client.*

I chose to scrape USA.gov's BVA search live rather than maintain my own full-text index — their index is more complete and costs nothing to keep fresh. No database in v1; everything is computed on request. That saved three weeks of build time with no measurable quality loss. The fragile parts are USA.gov availability, BVA page HTML shifts that would break the parser, and no caching layer — sustained concurrent load degrades quickly. The lesson: "no database" was the right call for v1. The instinct to persist everything is usually wrong when freshness matters more than latency, and when someone else's index is already better than the one I'd build.

#### [**Post-Generation Citation Validator**](https://github.com/va2ai/bva-citation-validator)

*Five-layer hallucination detector for CFR citations, BVA docket numbers, and CAVC case references. Sentinel-tagged context, grounding constraint, structured claim extraction, live cross-reference, and an adversarial critic pass.*

Grounded generation alone only gets you so far — models still fabricate citation numbers that *sound right*. A cheap post-hoc check catches what generation-time grounding misses and is much easier to tune than fighting the hallucination upstream. The fragile points: live BVA API downtime blocks verification, new CFR citation formats not in the extractor silently pass through, and over-strict rules produce false positives that destroy trust faster than hallucinations do. The key insight was that **average hallucination rate is the wrong metric for this domain** — one bad citation in a claim packet is catastrophic for a veteran, so I optimized for *sessions with zero hallucinations* rather than a lower average. Got from ~15% of sessions with hallucinations down to under 1.5%.

#### [**LLM Compaction Benchmark**](https://github.com/va2ai/llm-compaction-benchmark)

*Full 6×6 matrix (36 combinations) across 6 Gemini models, measuring how well each preserves conversation context after compaction. Fact recall, task continuation, and hallucination tests with Chart.js dashboards.*

I assumed bigger models would compact better because "they understand more" and wanted to test the assumption instead of paying for it. Caveats: results don't transfer cleanly to Claude or GPT families, and prompt engineering explained more variance than model choice — which means a model-centric reading can mislead anyone optimizing for the wrong variable. The finding flipped my prior: cheap/fast models beat expensive ones for compaction. **gemini-2.5-flash-lite at ~2s (99.9%) outperformed gemini-2.5-pro at ~14s (96.7%)**, with the best compactor/evaluator combination hitting 132.3% retention (compaction actually *improved* downstream responses). Prompt engineering alone lifted scores from 70% to 97%+. On narrow tasks, prompt design beats model size.

#### [**Crazy Caller**](https://github.com/va2ai/crazy-caller)

*Real-time AI phone assistant. Dials real numbers via Twilio, runs Gemini 3.1 Flash Live for the conversation, streams transcripts to a dashboard. 30 voices, 70 tests across 9 files, TDD throughout.*

Most "AI voice" projects paper over audio pipeline issues with ffmpeg or native deps that break at scale. I wrote pure TypeScript G.711 mulaw↔PCM16 encode/decode and a linear-interpolation resampler so the thing deploys cleanly — real-time voice is an audio-streaming problem first and an AI problem second. The fragile parts: Gemini Live API contract changes, Twilio media format drift, packet loss during resampling, and VAD tuning for barge-in sensitivity. The lesson that surprised me: **barge-in is the hardest part of voice UX, not latency**. Users will tolerate a pause, but they will not tolerate being talked over. I ended up exposing `START_OF_ACTIVITY_INTERRUPTS` so the AI actually stops when the human speaks.

#### [**GCP MCP Deployer**](https://github.com/va2ai/gcp-mcp-deployer)

*A Claude Skill that takes any MCP server repo and generates a production-ready Cloud Run deployment — Dockerfile, Artifact Registry, dedicated service account, Secret Manager, health checks, and the Claude Desktop / Claude.ai connection config.*

I'd deployed a handful of MCP servers by hand and realized the work was 80% repetitive boilerplate, so I codified my own reps into a skill instead of re-typing them. The fragile parts are the MCP transport standard evolving, the GCP API surface shifting, and secrets handling if users accidentally commit keys — I push on the latter in the generated docs but can't enforce it. The surprise: building the skill forced me to write down the deployment checklist explicitly, and **the checklist turned out to be more valuable than the generator itself**. I reach for it manually more than I reach for the tool.

#### [**Edge Deep Research & Validation API**](https://github.com/va2ai/edge-ai-search-validation-service)

*Production research agent on Cloudflare Workers ([live](https://deep-research-agent.vetapp.workers.dev/docs)). Multi-round web research with source-quality scoring and conflict detection, returning citation-backed synthesis via the OpenAI Responses API.*

Edge deployment keeps validation latency sub-second globally, which matters for interactive UX. I used the OpenAI Responses API so search + tool orchestration happens inside one provider contract rather than stitched across three. The ceiling is the Workers 30-second execution cap, which bounds recursion depth; OpenAI quota is the other dependency. The surprise was that **source-quality scoring matters more than search depth** — a couple of low-quality sources poison the synthesis worse than missing a good one. I now return a fact table with per-source confidence rather than a prose-only answer.

#### [**AI Agent Orchestration Platform**](https://github.com/va2ai/ai-agent-orchestration-platform)

*"Roundtable refinement" system — parallel critic agents with convergence conditions on iterative outputs. FastAPI + WebSocket pub/sub, multi-provider LLM abstraction over OpenAI / Gemini / Anthropic.*

Sequential refinement plateaus quickly because each pass sees the same critic. Parallel critics produce more diverse feedback, so outputs converge on better answers faster. The hard part is tuning convergence detection — too tight and it never stops, too loose and you lose the refinement benefit. I added a hard max-iteration ceiling as a safety net because **convergence conditions are trickier to tune than they sound in a design doc**.

#### [**AI Agent Platform**](https://github.com/va2ai/ai-agent-platform)

*Reusable chassis for agent patterns — parallel + compositional tool calls, agent delegation (Research ↔ Reddit), multi-depth research loops. Chat UI with model, agent, tool mode, and depth controls. 9 built-in tools (Tavily, Exa, Reddit, etc.).*

I kept re-implementing tool routing on every new agent project, so I built the base I wished I'd had. It's tightly coupled to the Gemini SDK, which means moving to Claude or another model family would require rewriting the tool-call layer — a real cost I accepted for v1 so I could ship. Lesson learned: **"pluggable" is aspirational**. The more pluggable I tried to make the contracts, the more complex they got. Concrete beats configurable in v1.

#### [**BVA Decision Intelligence**](https://github.com/va2ai/bva-decision-intelligence)

*Multi-agent research platform that turns natural-language research objectives into citation-verified reports with PDF/DOCX export. Full-text + Qdrant hybrid search, automatic Granted/Denied/Remanded/Mixed outcome classification.*

Attorneys want structured outputs, not chat. Outcome classification is cheap to run and makes the result library filterable, which is what users actually came for. Qdrant is tightly coupled into retrieval, so migrating to pgvector later would be painful; OpenRouter sitting between me and Gemini adds a failure point I don't control. The surprise: **the outcome classifier ended up more useful than the fancy multi-agent reasoning**. Simple features win when they solve the workflow problem.

#### [**RAG Decision Analysis System**](https://github.com/va2ai/rag-decision-analysis-system)

*Extraction + hybrid retrieval pipeline for BVA decisions, validated on an outcome-balanced 100-decision test set. Graph-lite schema on PostgreSQL + pgvector.*

You can't improve retrieval without a test set, so I built the evaluation harness before the retriever. Fragile parts: the test set goes stale as the BVA corpus evolves, and the hybrid weighting doesn't transfer cleanly to new domains. The takeaway: **I spent more time on the eval harness than on the retriever, and it was the right call**. Eval quality is the ceiling on pipeline quality — you cannot out-engineer a bad benchmark.

#### [**DevPortAI RAG Fact-Check**](https://github.com/va2ai/devportai)

*Reference RAG fact-check monorepo — FastAPI backend, React frontend, PostgreSQL + pgvector, Docker Compose.*

I wanted a realistic-complexity template to experiment with grounding techniques without rebuilding the plumbing each time. Monorepo tooling is the main operational friction, and pgvector index tuning gets tricky past a few million rows. Lesson: multi-stage verification adds latency linearly — there's a Pareto frontier between speed and grounding confidence, and you have to pick a point rather than try to win both.

---

### 🛠 How I Work

*   **Comprehension before shipping.** I can explain every architectural choice in every project above — what I considered, what I rejected, and where the fragile points are. Nothing I ship is a magic box to me.
*   **Evaluation before optimization.** Test harness first, then the pipeline. If I can't measure it, I can't improve it.
*   **Plain English over marketing language.** "Reduced hallucination" is cheap. "15% of sessions to under 1.5%, measured on a scored test set" is a claim.
*   **Override the model deliberately.** I keep a running note of where I throw out AI output and where I keep it. That's where the real decisions live.
*   **Ship the learning with the code.** What I learned is as much the deliverable as the repo itself.

---

### 🤝 Let's Connect

📍 **Location:** Miamisburg / Dayton / Cincinnati • Remote OK
📧 **Email:** [ai@vaclaims.net](mailto:ai@vaclaims.net)

If you're hiring for applied AI / LLM engineering roles — especially ones where grounding, evaluation, and operational cost matter — I'd like to talk. I shipped a multi-agent SaaS on GCP in the last year. I can walk you through every trade-off I made.

<p align="center">
  <img height="170" src="https://github-readme-stats.vercel.app/api?username=va2ai&show_icons=true&theme=dark&hide_border=true" />
  <img height="170" src="https://github-readme-stats.vercel.app/api/top-langs/?username=va2ai&layout=compact&theme=dark&hide_border=true" />
</p>
