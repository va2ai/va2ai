<h1 align="center">Hi, I'm Chris 👋</h1>

<p align="center">
  <b>Applied AI / LLM Engineer • Multi-Agent Systems • Backend</b>
</p>

<p align="center">
  I ship production AI systems. Each project below answers four questions — what it is, why I built it this way, what could break, what I learned — so you can see I understood what I shipped, not just that I shipped it.
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
*   **What it is:** A VA disability claims intelligence product for veterans and attorneys. Two layers — a React/Firebase web app ([vaintel](https://github.com/va2ai/vaintel)) and a 12-agent backend on GCP Cloud Run (PostgreSQL/pgvector, LangGraph, Anthropic Claude API). The backend handles claim analysis, Claim Fix Packet generation, daily executive briefs, and content publishing to Cloudflare R2.
*   **Why this way:** Single-prompt LLM calls don't reliably decompose claim analysis (nexus evaluation, citation gathering, remedy drafting are genuinely distinct tasks). Split frontend/backend so the React side stays on Firebase/Stripe (fast iteration on the user surface) while the intelligence layer evolves independently on Cloud Run.
*   **What could break:** Claude API pricing or rate-limit changes are my biggest single dependency. BVA source data format changes would ripple through the retrieval layer. The Firebase↔Cloud Run split means observability lives in two places, which makes incident response slower than I'd like.
*   **What I learned:** I ran ablation studies (E2A/E2B/E3A/E3B configurations) to compare multi-agent vs. single-prompt pipelines empirically. Finding: multi-agent wins on complex decomposition tasks but loses on simple lookups where the latency tax isn't worth it. Architecture choice has to be per-task, not per-product.

---

### 🧠 Featured Projects

#### [**BVA Research API & MCP Platform**](https://github.com/va2ai/bvaapi2)
*   **What it is:** FastAPI service + MCP server exposing 25+ tools for BVA decisions, 38 CFR, CAVC case law, M21-1, and the Federal Register. Deployed on Cloud Run; usable by any MCP-compatible client.
*   **Why this way:** Chose to scrape USA.gov's BVA search instead of maintaining my own vector index — their index is more complete and has zero maintenance cost. No database in v1; everything is computed live. Saved three weeks of build time with no measurable quality loss.
*   **What could break:** USA.gov search availability or rate limits. BVA page HTML changes would break the parser. No caching layer means sustained concurrent load degrades quickly.
*   **What I learned:** "No database" was the right call for v1. The instinct to persist everything is usually wrong when freshness matters more than speed.

#### [**Post-Generation Citation Validator**](https://github.com/va2ai/bva-citation-validator)
*   **What it is:** A second-pass hallucination detector for CFR citations. Runs after generation, extracts cited regulations, cross-references them against the live BVA API, flags fabrications.
*   **Why this way:** Grounded generation alone only gets you so far — models still hallucinate citation numbers that "sound right." A cheap post-hoc check catches what generation-time grounding misses, and it's much easier to tune than trying to prevent the hallucination upstream.
*   **What could break:** Live BVA API downtime makes the validator unable to verify. New CFR citation formats not in the extractor pattern would silently pass through. Over-strict validation produces false positives that hurt trust.
*   **What I learned:** Average hallucination rate is the wrong metric for this domain. One bad citation in a claim packet is catastrophic for the veteran, so I optimized for *sessions with zero hallucinations* rather than a lower average. Got from ~15% sessions-with-hallucinations to under 1.5%.

#### [**LLM Compaction Benchmark**](https://github.com/va2ai/llm-compaction-benchmark)
*   **What it is:** Benchmarking framework running 36 combinations across 6 Gemini models, measuring how well each preserves conversation context after compaction. Fact recall, task continuation, and hallucination tests with Chart.js dashboards.
*   **Why this way:** Assumed bigger models would compact better because "they understand more." Wanted to test the assumption instead of paying for it.
*   **What could break:** Results don't transfer to Claude or GPT model families. Prompt engineering explained more variance than model choice — the model-centric framing could mislead people optimizing for the wrong variable.
*   **What I learned:** Cheap/fast models beat expensive ones for compaction: gemini-2.5-flash-lite at 2s outperformed gemini-2.5-pro at 14s, with the best combination hitting 132% quality retention. Prompt quality lifted scores from 70% to 97%+. On narrow tasks, prompt design beats model size.

#### [**Crazy Caller**](https://github.com/va2ai/crazy-caller)
*   **What it is:** Real-time AI phone assistant. Dials real numbers via Twilio, runs Gemini Live API for the conversation, streams transcripts to a web dashboard.
*   **Why this way:** Most "AI voice" products paper over audio pipeline issues with ffmpeg or native deps that break at scale. Wrote pure TypeScript mulaw↔PCM16 resampling so the thing actually deploys cleanly. Real-time voice is an audio streaming problem first, an AI problem second.
*   **What could break:** Gemini Live API contract changes. Twilio media format drift. Packet loss during resampling degrades audio noticeably.
*   **What I learned:** Barge-in (user interrupting the AI mid-sentence) is the hardest part of voice UX. Interrupt responsiveness matters more than model latency — users will tolerate a pause, but not being talked over.

#### [**GCP MCP Deployer**](https://github.com/va2ai/gcp-mcp-deployer)
*   **What it is:** A Claude Skill that takes any MCP server repo and generates a production-ready Cloud Run deployment — Dockerfile, Artifact Registry setup, Secret Manager config, service account, health checks, and Claude Desktop connection settings.
*   **Why this way:** I'd deployed a handful of MCP servers manually and noticed the work was 80% repetitive boilerplate. Codified my own reps into a skill so I (and anyone else) can skip the boilerplate.
*   **What could break:** MCP transport standard evolution. GCP surface changes. Secrets handling if users accidentally commit keys — I push on this in the generated docs but can't enforce it.
*   **What I learned:** Building the skill forced me to write down the deployment checklist explicitly. The checklist ended up being more valuable than the generator — I reach for it manually more than I reach for the tool.

#### [**Multi-Agent Ideation Pipeline**](https://github.com/va2ai/nexus-deep-research)
*   **What it is:** 5-phase pipeline — Trend Hunter → Ideator → Validator → Scorer → Synthesizer — running through a pub/sub event bus with SSE streaming to a live dashboard. 200+ tests.
*   **Why this way:** Sequential chains fail on research tasks because early-phase errors compound. Pub/sub lets phases fail or retry independently.
*   **What could break:** Event bus contention under parallelism. Schema drift between agents breaks downstream consumers silently.
*   **What I learned:** Pub/sub was overkill for 5 phases — a simpler state machine would have done the job. I over-engineered for the pattern, not the problem.

#### [**AI Agent Orchestration Platform**](https://github.com/va2ai/ai-agent-orchestration-platform)
*   **What it is:** "Roundtable refinement" system — parallel critic agents with convergence conditions on iterative outputs.
*   **Why this way:** Sequential refinement plateaus quickly because each pass sees the same critic. Parallel critics produce more diverse feedback.
*   **What could break:** Convergence detection tuning — too tight and it never stops, too loose and you don't get the refinement benefit.
*   **What I learned:** Convergence conditions are harder to tune than I expected. Ended up with a hard max-iteration ceiling as a safety net.

#### [**AI Agent Platform**](https://github.com/va2ai/ai-agent-platform)
*   **What it is:** Reusable chassis for agent patterns — parallel/compositional tool calls, agent delegation, multi-depth research loops. Chat UI with model/agent/tool/depth controls.
*   **Why this way:** Kept re-implementing tool routing on every new agent project. Built the base I wish I'd had.
*   **What could break:** Tightly coupled to the Gemini SDK. Switching to Claude or other models would require rewriting the tool-call layer.
*   **What I learned:** "Pluggable" is aspirational. The more pluggable I made the contracts, the more complex they got. Concrete beats configurable in v1.

#### [**BVA Decision Intelligence**](https://github.com/va2ai/bva-decision-intelligence)
*   **What it is:** Multi-agent research platform that turns natural-language research objectives into citation-verified reports with PDF/DOCX export. Full-text + Qdrant hybrid search, automatic Granted/Denied/Remanded/Mixed outcome classification.
*   **Why this way:** Attorneys need structured outputs, not chat. Outcome classification is cheap to run and makes the result library filterable, which is what users actually came for.
*   **What could break:** Qdrant → pgvector migration would be painful given how tightly coupled retrieval is. OpenRouter sits between me and the model, which adds a failure point I don't control.
*   **What I learned:** The outcome classifier was more useful to users than the fancy multi-agent reasoning. Simple features win when they solve the workflow problem.

#### [**CAVC Tracker**](https://vaclaims.net)
*   **What it is:** Docket tracker for veterans-law attorneys following Court of Appeals for Veterans Claims cases — alerts, timelines, decision tracking.
*   **Why this way:** General legal-tech doesn't cover CAVC well; attorneys were copy-pasting court RSS feeds into spreadsheets. Niche product for an underserved workflow.
*   **What could break:** CAVC site structure changes breaking ingestion. Alert volume tuning — too noisy and users unsubscribe.
*   **What I learned:** Landing page first, product second. Pre-signup validation filtered whether the ingestion pipeline was worth building at all.

#### [**Edge Deep Research & Validation API**](https://github.com/va2ai/edge-ai-search-validation-service)
*   **What it is:** Research agent deployed on Cloudflare Workers. Multi-round search with source-quality scoring and conflict detection, returning citation-backed synthesis.
*   **Why this way:** Edge deployment keeps validation latency sub-second globally, which matters for interactive UX. OpenAI Responses API handles the search-with-tools orchestration cleanly.
*   **What could break:** Workers' 30-second execution ceiling bounds how deep I can recurse. OpenAI quota. Conflict detection heuristics are hand-tuned.
*   **What I learned:** Source quality scoring matters more than search depth. A few bad sources poison the synthesis worse than missing a good one.

#### [**RAG Decision Analysis System**](https://github.com/va2ai/rag-decision-analysis-system)
*   **What it is:** Extraction + hybrid retrieval pipeline for BVA decisions, validated on a 100-decision test set.
*   **Why this way:** You can't improve retrieval without a test set. Built the harness first, then the retriever.
*   **What could break:** Test set staleness as the BVA corpus evolves. Hybrid weighting doesn't transfer cleanly to new domains.
*   **What I learned:** I spent more time on the eval harness than the retriever and it was the right call. Eval quality is a ceiling on pipeline quality.

#### [**DevPortAI RAG Fact-Check**](https://github.com/va2ai/devportai)
*   **What it is:** Reference RAG fact-check monorepo — FastAPI backend, React frontend, PostgreSQL + pgvector.
*   **Why this way:** Wanted a realistic-complexity template to experiment with grounding techniques without rebuilding the plumbing each time.
*   **What could break:** Monorepo tooling. pgvector index tuning gets tricky past a few million rows.
*   **What I learned:** Multi-stage verification adds latency linearly. There's a Pareto frontier between speed and grounding confidence — pick a point, don't try to win both.

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
