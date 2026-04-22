<h1 align="center">Hi, I'm Chris 👋</h1>

<p align="center">
  <b>Applied AI / LLM Engineer • Multi-Agent Systems • Backend</b>
</p>

<p align="center">
  I ship production AI systems. For every project below, you'll see what it does, why I built it the way I did, where it's fragile, and what I learned — because in 2026, shipping isn't the signal. Thinking is.
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

*A two-layer VA disability claims intelligence product for veterans and attorneys: a React/Firebase web app ([vaintel](https://github.com/va2ai/vaintel)) and a 12-agent backend on GCP Cloud Run (PostgreSQL/pgvector, LangGraph, Anthropic Claude API) that handles claim analysis, Claim Fix Packet generation, daily executive briefs, and content publishing to Cloudflare R2.*

Single-prompt LLM calls don't reliably decompose claim analysis — nexus evaluation, citation gathering, and remedy drafting are genuinely distinct tasks — so the backend splits the work across 12 specialized agents. Keeping the user surface on React/Firebase/Stripe lets product iteration stay fast while the intelligence layer evolves independently on Cloud Run.

My biggest single dependency is Claude API pricing and rate limits. BVA source format changes would ripple through the retrieval layer, and observability split across Firebase and Cloud Run makes incident response slower than I'd like. The win I'm proudest of is the ablation work: I ran E2A/E2B/E3A/E3B configurations to compare multi-agent vs. single-prompt pipelines empirically, and found multi-agent wins on complex decomposition but loses on simple lookups where the latency tax isn't worth it. Architecture choice has to be per-task, not per-product.

---

### 🧠 Featured Projects

#### [**BVA Research API & MCP Platform**](https://github.com/va2ai/bvaapi2)

*FastAPI service + MCP server exposing 25+ tools for BVA decisions, 38 CFR, CAVC case law, M21-1, and the Federal Register. Deployed on Cloud Run, usable by any MCP-compatible client.*

I chose to scrape USA.gov's BVA search instead of maintaining my own vector index — their index is more complete and costs nothing to maintain. No database in v1; everything is computed live. That saved three weeks of build time with no measurable quality loss. The fragile parts are USA.gov availability, BVA page HTML changes that would break the parser, and the lack of a caching layer — sustained concurrent load degrades quickly. The lesson: "no database" was the right call for v1. The instinct to persist everything is usually wrong when freshness matters more than speed.

#### [**Post-Generation Citation Validator**](https://github.com/va2ai/bva-citation-validator)

*A second-pass hallucination detector for CFR citations. Extracts cited regulations after generation, cross-references them against the live BVA API, flags fabrications.*

Grounded generation alone only gets you so far — models still hallucinate citation numbers that "sound right." A cheap post-hoc check catches what generation-time grounding misses, and it's much easier to tune than fighting the hallucination upstream. The fragile points: live BVA API downtime blocks verification, new CFR citation formats not in the extractor silently pass through, and over-strict rules produce false positives that hurt trust. The key insight was that average hallucination rate is the wrong metric for this domain — one bad citation in a claim packet is catastrophic for the veteran, so I optimized for *sessions with zero hallucinations* rather than a lower average. Got from ~15% of sessions with hallucinations down to under 1.5%.

#### [**LLM Compaction Benchmark**](https://github.com/va2ai/llm-compaction-benchmark)

*Benchmarking framework running 36 combinations across 6 Gemini models, measuring how well each preserves conversation context after compaction. Fact recall, task continuation, and hallucination tests with Chart.js dashboards.*

I assumed bigger models would compact better because "they understand more" and wanted to test the assumption instead of paying for it. Caveats: results don't transfer to Claude or GPT families, and prompt engineering explained more variance than model choice — which means a model-centric reading of the results can mislead people optimizing for the wrong variable. The finding flipped my prior: cheap/fast models beat expensive ones for compaction. Gemini-2.5-flash-lite at 2s outperformed gemini-2.5-pro at 14s, with the best combination hitting 132% quality retention. Prompt engineering alone lifted scores from 70% to 97%+. On narrow tasks, prompt design beats model size.

#### [**Crazy Caller**](https://github.com/va2ai/crazy-caller)

*Real-time AI phone assistant. Dials real numbers via Twilio, runs Gemini Live API for the conversation, streams transcripts to a web dashboard.*

Most "AI voice" products paper over audio pipeline issues with ffmpeg or native deps that break at scale. I wrote pure TypeScript mulaw↔PCM16 resampling so the thing deploys cleanly — real-time voice is an audio streaming problem first and an AI problem second. The fragile parts are Gemini Live API contract changes, Twilio media format drift, and packet loss during resampling. The lesson that surprised me: barge-in (the user interrupting the AI mid-sentence) is the hardest part of voice UX. Interrupt responsiveness matters more than model latency — users will tolerate a pause but not being talked over.

#### [**GCP MCP Deployer**](https://github.com/va2ai/gcp-mcp-deployer)

*A Claude Skill that takes any MCP server repo and generates a production-ready Cloud Run deployment — Dockerfile, Artifact Registry, Secret Manager, service account, health checks, and Claude Desktop connection settings.*

I'd deployed a handful of MCP servers manually and noticed the work was 80% repetitive boilerplate, so I codified my own reps into a skill. The fragile parts are MCP transport standard evolution, GCP API surface changes, and secrets handling if users accidentally commit keys — I push on the latter in the generated docs but can't enforce it. The surprise: building the skill forced me to write down the deployment checklist explicitly, and the checklist turned out to be more valuable than the generator itself. I reach for it manually more than I reach for the tool.

#### [**Multi-Agent Ideation Pipeline**](https://github.com/va2ai/nexus-deep-research)

*5-phase pipeline — Trend Hunter → Ideator → Validator → Scorer → Synthesizer — running through a pub/sub event bus with SSE streaming to a live dashboard. 200+ tests.*

Sequential chains fail on research tasks because early-phase errors compound, so I used pub/sub to let phases fail or retry independently. Fragile points: event bus contention under parallelism, and schema drift between agents silently breaking downstream consumers. Honest self-critique: pub/sub was overkill for 5 phases. A simpler state machine would have done the job — I over-engineered for the pattern, not the problem.

#### [**AI Agent Orchestration Platform**](https://github.com/va2ai/ai-agent-orchestration-platform)

*"Roundtable refinement" system — parallel critic agents with convergence conditions on iterative outputs.*

Sequential refinement plateaus quickly because each pass sees the same critic. Parallel critics produce more diverse feedback, so the outputs converge on better answers faster. The hard part is tuning convergence detection — too tight and it never stops, too loose and you lose the refinement benefit. I ended up adding a hard max-iteration ceiling as a safety net because convergence conditions are trickier to tune than I expected.

#### [**AI Agent Platform**](https://github.com/va2ai/ai-agent-platform)

*Reusable chassis for agent patterns — parallel and compositional tool calls, agent delegation, multi-depth research loops. Chat UI with model, agent, tool, and depth controls.*

I kept re-implementing tool routing on every new agent project, so I built the base I wished I'd had. It's tightly coupled to the Gemini SDK, which means switching to Claude or another model family would require rewriting the tool-call layer — a real cost I accepted for v1. Lesson learned: "pluggable" is aspirational. The more pluggable I tried to make the contracts, the more complex they got. Concrete beats configurable in v1.

#### [**BVA Decision Intelligence**](https://github.com/va2ai/bva-decision-intelligence)

*Multi-agent research platform that turns natural-language research objectives into citation-verified reports with PDF/DOCX export. Full-text + Qdrant hybrid search, automatic Granted/Denied/Remanded/Mixed outcome classification.*

Attorneys need structured outputs, not chat. Outcome classification is cheap to run and makes the result library filterable, which is what users actually came for. Qdrant is tightly coupled into retrieval, so migrating to pgvector later would be painful, and OpenRouter sitting between me and the model adds a failure point I don't control. The surprise: the outcome classifier ended up more useful to users than the fancy multi-agent reasoning. Simple features win when they solve the workflow problem.

#### [**CAVC Tracker**](https://vaclaims.net)

*Docket tracker for veterans-law attorneys following Court of Appeals for Veterans Claims cases — alerts, timelines, decision tracking.*

General legal-tech doesn't cover CAVC well; attorneys were copy-pasting court RSS feeds into spreadsheets. Niche, but underserved. The fragile parts are CAVC site structure changes breaking ingestion, and alert-volume tuning (too noisy and users unsubscribe). Learned to do landing page first, product second — pre-signup validation told me whether the ingestion pipeline was worth building at all before I invested in it.

#### [**Edge Deep Research & Validation API**](https://github.com/va2ai/edge-ai-search-validation-service)

*Research agent deployed on Cloudflare Workers. Multi-round search with source-quality scoring and conflict detection, returning citation-backed synthesis.*

Edge deployment keeps validation latency sub-second globally, which matters for interactive UX. I used the OpenAI Responses API for search-with-tools orchestration. The ceiling is Workers' 30-second execution limit, which bounds how deep I can recurse; OpenAI quota is the other dependency. The surprise was that source-quality scoring matters more than search depth — a few bad sources poison the synthesis worse than missing a good one.

#### [**RAG Decision Analysis System**](https://github.com/va2ai/rag-decision-analysis-system)

*Extraction + hybrid retrieval pipeline for BVA decisions, validated on a 100-decision test set.*

You can't improve retrieval without a test set, so I built the harness before the retriever. Fragile parts: the test set goes stale as the BVA corpus evolves, and the hybrid weighting doesn't transfer cleanly to new domains. The takeaway: I spent more time on the eval harness than on the retriever and it was the right call. Eval quality is a ceiling on pipeline quality.

#### [**DevPortAI RAG Fact-Check**](https://github.com/va2ai/devportai)

*Reference RAG fact-check monorepo — FastAPI backend, React frontend, PostgreSQL + pgvector.*

Wanted a realistic-complexity template to experiment with grounding techniques without rebuilding the plumbing each time. Monorepo tooling is the main operational friction, and pgvector index tuning gets tricky past a few million rows. Lesson: multi-stage verification adds latency linearly — there's a Pareto frontier between speed and grounding confidence, and you have to pick a point rather than try to win both.

---

### 🛠 How I Work

*   **Comprehension before shipping.** I can explain every architectural choice in every project above — what I considered, what I rejected, and where the fragile points are. Nothing I ship is a magic box to me.
*   **Evaluation before optimization.** Test harness first, then the pipeline. If I can't measure it, I can't improve it.
*   **Plain English over marketing language.** "Reduced hallucination" is cheap. "15% of sessions to under 1.5%, measured on X test set" is a claim.
*   **Override the model deliberately.** I keep a running note of where I throw out AI output and where I keep it. That's where the real decisions live.
*   **Ship learnings with the work.** What I learned is as much the deliverable as the code.

---

### 🤝 Let's Connect

📍 **Location:** Miamisburg / Dayton / Cincinnati • Remote OK
📧 **Email:** [ai@vaclaims.net](mailto:ai@vaclaims.net)

If you're hiring for applied AI / LLM engineering roles — especially ones where grounding, evaluation, and operational cost matter — I'd like to talk. I shipped a multi-agent SaaS on GCP in the last year. I can walk you through every trade-off I made.

<p align="center">
  <img height="170" src="https://github-readme-stats.vercel.app/api?username=va2ai&show_icons=true&theme=dark&hide_border=true" />
  <img height="170" src="https://github-readme-stats.vercel.app/api/top-langs/?username=va2ai&layout=compact&theme=dark&hide_border=true" />
</p>
