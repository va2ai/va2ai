<h1 align="center">Hi, I'm Chris 👋</h1>

<p align="center">
  <b>Applied AI / LLM Engineer • Multi-Agent Systems • Backend</b>
</p>

<p align="center">
  I ship production AI for a niche I understand: VA disability claims. For every project below I walk through the AI-logic strategy — how the prompts, state machines, retrieval fan-outs, and validation passes actually work, with direct links to the code. Shipping isn't the signal in 2026. Thinking is.
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

*Two-layer VA disability claims intelligence product. React 19 + TypeScript + Firebase front-end ([va-claims-intel](https://github.com/va2ai/va-claims-intel)) with Stripe 4-tier billing and a Twilio + Gemini Live API phone agent. FastAPI multi-agent backend ([v2v-monorepo](https://github.com/va2ai/v2v-monorepo)) on GCP Cloud Run — LangGraph, Postgres + pgvector, Anthropic Claude. Unified search across 1.85M+ BVA decisions / 38 CFR / M21-1 / CAVC dockets, decision-letter analysis, claim-theory pressure-testing, daily intelligence briefs.*

**Agent architecture** ([`apps/api/app/teams/coordinator.py`](https://github.com/va2ai/v2v-monorepo/blob/main/apps/api/app/teams/coordinator.py)) — six specialized agents (`intake`, `planner`, `retrieval`, `analysis`, `cite`, `refinement`) run under a `TeamCoordinator` that owns a `TaskBoard`, a `MessageBus`, and per-agent `AgentWorker`s. Task dependencies use `$N` placeholders resolved to real IDs at creation time — that's how an LLM-generated plan can be handed to the coordinator without the LLM having to know task IDs it hasn't created yet. The run loop picks `get_ready_tasks()`, finds idle agents, and fires them in parallel with `asyncio.gather()`. Each agent gets its **own** `LLMClient(model)` instance, so the intake agent can run on a fast/cheap model while the analysis agent runs on Sonnet — the coordinator doesn't force a uniform tier. Tools are **role-gated**: `AGENT_TOOLS.get(role, ALL_TOOLS)` — least privilege so the intake agent can't accidentally call the BVA write path.

**Retrieval strategy** ([`apps/api/app/agents/retrieval_agent.py`](https://github.com/va2ai/v2v-monorepo/blob/main/apps/api/app/agents/retrieval_agent.py)) — retrieval fans out asymmetrically across sources rather than uniformly, because latency budgets aren't equal. BVA `search_extract` runs on the full query list with keyword-aware passage extraction. CFR and KnowVA run on only the **first two** queries. CAVC appellate precedent runs on the **first query only** — appellate lookups are the slowest and the marginal case usually isn't worth the wait. Keywords are extracted with a custom stopword filter (the standard NLP stopword lists leak VA-specific short tokens like "SC" that actually matter).

**Why not one prompt.** Single-prompt Claude calls don't reliably decompose claim analysis because intake, retrieval, citation extraction, analysis, and refinement have different failure modes — and a mistake in intake poisons everything downstream silently. Splitting lets me put a schema contract between every stage, so each handoff is either valid or fails loudly.

**Biggest dependencies.** Claude API pricing and rate limits. BVA source-format changes would ripple through retrieval. Observability is split across Firebase and Cloud Run, which slows incident response. The win I'm proudest of is splitting the UX into **Veteran mode** (simplified denial-analysis wizard) and **Professional mode** (full research workspace with CAVC Tracker, Precedent Finder, and Research Orchestrator). Same backend, two surface areas. Audience fit beats feature parity — pros hated hand-holding; veterans drowned in the firehose.

---

### 🧠 Featured Projects

#### [**Post-Generation Citation Validator**](https://github.com/va2ai/bva-citation-validator)

*Five-stage hallucination detector for CFR citations, BVA docket numbers, and CAVC case references. Grounded generation → structured extraction → live cross-reference → adversarial critic → prompt-advisor loop.*

**Stage 1 — Grounded generation** with `claude-sonnet-4-6`. Context is wrapped in explicit boundary markers: `[SOURCE_START: id] ... [SOURCE_END: id]` ([`lib/context.js`](https://github.com/va2ai/bva-citation-validator/blob/main/lib/context.js)). This alone nearly eliminates "context boundary attribution" — where the model reads correct data but assigns it to the wrong adjacent record. The [system prompt](https://github.com/va2ai/bva-citation-validator/blob/main/system-prompt.txt) then hard-constrains every citation to appear *verbatim* inside a `SOURCE_*` tag, with explicit sub-rules for domain-specific traps: reasonable-doubt grants must be flagged as discretionary (not precedent), outdated decisions must carry temporal disclaimers, and "PTSD secondary to MST" must be rephrased as "PTSD claimed based on MST as in-service stressor" (because MST is a stressor event, not a service-connected condition).

**Stage 2 — Structured extraction** ([`critic.js`](https://github.com/va2ai/bva-citation-validator/blob/main/critic.js)). A second LLM pass extracts every verifiable claim into a typed JSON array using Claude's `output_config: { format: { type: "json_schema" } }` — the schema guarantees valid output instead of regex-parsing model prose.

**Stage 3 — Cross-reference** against the live BVA API. Each claim resolves to one of five statuses: `VERIFIED`, `OUTDATED`, `NOT_IN_SOURCES`, `HALLUCINATED`, `UNGROUNDED`. Separating "not in this context" from "doesn't exist anywhere" is important — it lets the UI tell the user *why* a claim failed instead of just rejecting it.

**Stage 4 — Adversarial critic**. `claude-haiku-4-5` audits for five classes of failure that stage-3 regex validation can't catch: overstated claims, unsupported inferences, misleading context use, temporal assumptions, and cross-source aggregation errors. Running this on Haiku instead of Sonnet keeps the per-session cost under control — **the critic doesn't need to be smarter than the generator, just skeptical**.

**Stage 5 — Prompt advisor** loops critic findings + validation failures back into system-prompt suggestions. Over time, the prompt evolves rules that target the failures the critic kept flagging. That's how the giant list of "distinguish reasonable-doubt grants from precedent" rules grew — they're *learned* additions, not bright ideas.

**The aggregation-hallucination fix is the one I'm proudest of.** Models invent rankings when asked to compare cases — "these three are the strongest" — because ranking is *generative*. The fix ([`fixes.js`](https://github.com/va2ai/bva-citation-validator/blob/main/fixes.js)) is to pre-compute the ranking server-side and inject it as a `[PRECOMPUTED_RESULT: ...] ... [END_PRECOMPUTED_RESULT]` block with the explicit instruction "do NOT re-rank or add cases." Deterministic work happens in deterministic code; the model only narrates.

**Outcome.** Sessions-with-any-hallucination dropped from ~15% to under 1.5%. *Average* hallucination rate was the wrong metric — one bad citation per packet is catastrophic, so I optimized for **sessions with zero hallucinations**.

---

#### [**BVA Research API & MCP Platform**](https://github.com/va2ai/bvaapi2)

*FastAPI service + MCP server exposing 27 tools across BVA decisions, 38 CFR, CAVC case law, KnowVA, diagnostic codes, Federal Register, and a RAG layer. Deployed on Cloud Run with a separate `Dockerfile.mcp`, usable by any MCP-compatible client.*

**Why no database.** The 27 tools ([`mcp_server.py`](https://github.com/va2ai/bvaapi2/blob/main/mcp_server.py)) wrap USA.gov's BVA search index, the KnowVA knowledge base, the eCFR API, and the Federal Register API directly. I **scrape USA.gov live instead of maintaining my own full-text index** because their index is more complete than anything I can build, and BVA decisions are frequently updated — freshness beats cached latency when wrongness is the worst outcome. No persistence layer means no staleness bugs, no reindexing jobs, no vector-drift detection. Everything is computed on request.

**Why a separate `Dockerfile.mcp`.** The same FastAPI service serves two interfaces: the raw REST API for web clients and an MCP server for LLM tool-use. Splitting the Dockerfile keeps the MCP attack surface narrow (smaller image, fewer dependencies) and avoids coupling them at deploy time.

**Section-aware regex search** is one of the more useful tools — instead of returning whole decisions, it searches *within* named sections (Findings, Reasons, Conclusions) with nine built-in presets for CFR citations, nexus opinions, diagnostic codes, etc. Section filtering gives the retriever much higher precision than whole-document matching because legal boilerplate overwhelms the signal at the document level.

**Fragile parts.** USA.gov availability, BVA page HTML shifts, no caching layer — sustained concurrent load degrades quickly. **The lesson: "no database" was the right v1 call.** The instinct to persist everything is usually wrong when freshness matters more than latency, and someone else's index is already better than the one I'd build.

---

#### [**LLM Compaction Benchmark**](https://github.com/va2ai/llm-compaction-benchmark)

*Full 6×6 matrix (36 combinations) across six Gemini models, measuring how well each preserves conversation context after compaction. Fact recall, task continuation, and hallucination tests with Chart.js dashboards.*

**Design choice.** Every model in the matrix acts as both **compactor** (writes the summary) and **evaluator** (scores whether the summary preserved enough to keep the downstream task on track) — cells on the diagonal are self-evaluation. That asymmetry is what surfaces the counterintuitive result: a model can compact well without being the best judge of its own output, and vice versa. ([`model_comparison.py`](https://github.com/va2ai/llm-compaction-benchmark/blob/main/src/model_comparison.py), [`quality_test.py`](https://github.com/va2ai/llm-compaction-benchmark/blob/main/src/quality_test.py)).

**The prompt work matters more than the model.** [`improved_summary_prompt.py`](https://github.com/va2ai/llm-compaction-benchmark/blob/main/src/improved_summary_prompt.py) is the version that lifts scores from ~70% to 97%+ on the same model — it specifies exactly which structured fields to preserve (pending decisions, open questions, user-stated preferences, active file references) instead of asking for "a good summary." A prompt that names its invariants beats a generic prompt on a bigger model.

**Finding that flipped my prior.** I expected bigger models to compact better. **gemini-2.5-flash-lite at ~2s (99.9% retention) outperformed gemini-2.5-pro at ~14s (96.7%)**, and the best compactor/evaluator pair hit 132.3% — compaction actually *improved* downstream responses by cleaning up noise. Caveats: the results don't transfer cleanly to Claude or GPT, and because prompt engineering explained more variance than model choice, a model-centric reading of this benchmark can mislead someone optimizing the wrong variable.

---

#### [**Crazy Caller**](https://github.com/va2ai/crazy-caller)

*Real-time AI phone assistant. Dials real numbers via Twilio, runs Gemini 3.1 Flash Live for the conversation, streams transcripts to a dashboard. 30 voices, 70 tests across 9 files, TDD throughout.*

**Core problem: sample-rate mismatch.** Twilio sends 8 kHz G.711 mulaw. Gemini wants 16 kHz PCM16 in and emits 24 kHz PCM16 out. Every audio packet has to be decoded, resampled, and re-encoded twice — inbound and outbound — with no ffmpeg and no native dependencies, because native dependencies break on Cloud Run / Docker in ways that are miserable to debug at call time.

**ITU G.711 codec in pure TypeScript** ([`src/bridge/audio-converter.ts`](https://github.com/va2ai/crazy-caller/blob/main/src/bridge/audio-converter.ts)) — `mulawDecode()` and `pcmToMulaw()` do the bit-twiddling directly against the spec (BIAS = 0x84, complemented bytes, segment/mantissa unpacking). I used linear-interpolation resampling rather than polyphase or sinc — explicit trade-off: simpler code and zero deps vs. theoretically cleaner frequency response. For 8kHz↔16kHz voice band, linear is audibly fine; the phone network is the bottleneck, not the resampler.

**Two full pipelines:** `mulawToPcm16k(buffer)` for inbound (phone → Gemini) and `pcm24kToMulaw8k(buffer)` for outbound (Gemini → phone). The asymmetric 16k-in / 24k-out is Gemini's choice — their output ships at higher fidelity for downstream processing, but the PSTN throws it away.

**Barge-in** is the hardest part of voice UX, not latency. Users tolerate a pause; they do not tolerate being talked over. The server sets Gemini Live's VAD to `START_OF_ACTIVITY_INTERRUPTS` with high sensitivity — the model stops mid-sentence the moment the user starts speaking. This needs aggressive tuning: too sensitive and the model interrupts itself on background noise; too loose and it talks over the user. I ended up exposing VAD sensitivity as a config knob rather than baking in one answer.

**Event-driven architecture.** `CallManager`, `GeminiSession`, and `TwilioStream` are all `EventEmitter`s — composable, trivially testable, and the audio bridge is just `twilio.on('audio', session.send)` plus its mirror. TDD throughout kept the audio pipeline honest; codec bugs are impossible to eyeball without tests.

---

#### [**AI Agent Orchestration Platform**](https://github.com/va2ai/ai-agent-orchestration-platform)

*"Roundtable refinement" system — parallel critic agents with convergence conditions on iterative outputs. FastAPI + WebSocket pub/sub, multi-provider LLM abstraction over OpenAI / Gemini / Anthropic.*

**Convergence is the hard part.** Parallel critics produce more diverse feedback than sequential ones, so outputs converge faster — but *when to stop* is a genuinely hard question. [`convergence.py`](https://github.com/va2ai/ai-agent-orchestration-platform/blob/main/src/ai_orchestrator/convergence.py) implements four stop conditions in priority order: **(1)** a user-provided custom function (escape hatch for domain-specific stops), **(2)** zero high-severity issues remaining, **(3)** max iterations, **(4)** document delta below threshold.

**Document delta uses `difflib.SequenceMatcher.ratio()`** (1 − similarity) — cheap, deterministic, and it doesn't require an LLM to judge whether the document has "really" changed. Every stop returns a `StopDecision` with `stopped_by` set to `"no_high_issues"`, `"max_iterations"`, `"delta_threshold"`, or `"custom"`, so the caller knows *why* the loop stopped, not just that it did. Explainability matters when you're going to re-run with different settings.

**Max iterations as a safety net.** I thought I could tune the other conditions tight enough to never need a ceiling. I was wrong — convergence detection is trickier than it looks in a design doc, and a hard cap is the only way to guarantee termination in prod. Shipping the safety net wasn't a compromise; it was the right call.

---

#### [**Edge Deep Research & Validation API**](https://github.com/va2ai/edge-ai-search-validation-service)

*Research agent on Cloudflare Workers ([live](https://deep-research-agent.vetapp.workers.dev/docs)). Multi-round web research with source-quality scoring and conflict detection, returning citation-backed synthesis via the OpenAI Responses API.*

**Explicit domain quality scoring** ([`src/worker.js`](https://github.com/va2ai/edge-ai-search-validation-service/blob/main/src/worker.js) — `domainQualityScore()`) — rather than letting the model "judge source quality" (which it does inconsistently), every URL gets a deterministic 1–5 score: `.gov` / `.edu` → 5, standards bodies (IETF, W3C, ISO, arXiv, IEEE, Nature) → 4, encyclopedic (Wikipedia, Britannica) → 3, blogs (Medium, Substack) → 2, social (Reddit, X) → 1. The ceiling on synthesis quality is set by the floor on source quality, so a deterministic filter at ingest is more valuable than a smarter synthesis prompt. A couple of low-quality sources poison the output worse than missing a good one.

**Conflict detection is a first-class output, not a post-hoc summary.** Each research round extracts both `facts` *and* `conflicts` — the synthesis prompt then receives `conflicts` as a separate block with the instruction "If sources conflict, say so explicitly and cite both." That turns disagreement into a feature instead of averaging it into confident nonsense.

**Ceiling.** Cloudflare Workers' 30-second execution limit bounds recursion depth — I traded some research thoroughness for sub-second global latency. OpenAI Responses API is the other dependency. Edge deployment was worth it for interactive UX; this API gets called inline.

---

#### [**GCP MCP Deployer**](https://github.com/va2ai/gcp-mcp-deployer)

*A Claude Skill that takes any MCP server repo and generates a production-ready Cloud Run deployment — Dockerfile, Artifact Registry, dedicated service account, Secret Manager, health checks, and Claude Desktop / Claude.ai connection config.*

**How it works.** The skill clones the repo, inspects `Dockerfile`, `package.json`, and `pyproject.toml` to identify the transport type, port, required secrets, and health check endpoint. Missing Dockerfiles fall back to templates (Node.js or Python). Output is a single `deploy.sh` that runs linearly: preflight → enable GCP APIs → create Artifact Registry repo (idempotent) → dedicated service account → Secret Manager secrets → IAM binding → Cloud Build → Cloud Run deploy → health check. Every step is idempotent — rerunning `deploy.sh` after a failed step doesn't duplicate anything.

**The checklist ended up more valuable than the generator.** Writing the skill forced me to make the deployment steps explicit, and I reach for the checklist manually more often than I reach for the generator itself. **Codifying the reps is the skill; the tool is just the surface area.**

---

#### [**AI Agent Platform**](https://github.com/va2ai/ai-agent-platform)

*Reusable chassis for agent patterns — parallel + compositional tool calls, agent delegation (Research ↔ Reddit), multi-depth research loops. Chat UI with model / agent / tool-mode / depth controls. Nine built-in tools (Tavily, Exa, Reddit, etc.).*

**Agent delegation**, not just tool calls. The Research agent can delegate into the Reddit agent for community perspectives; Reddit can delegate back to Research for fact-checking. Delegation is modeled as a tool call where the "tool" is another agent's `run()` — this keeps the LLM's mental model ("I'm calling a tool") stable, while giving me recursive composition.

**Tightly coupled to the Gemini SDK.** Moving to Claude or another model family would require rewriting the tool-call layer. I accepted that cost in v1 so I could ship. **"Pluggable" is aspirational** — the more pluggable I tried to make the tool-call contracts, the more complex they got. Concrete beats configurable in v1.

---

#### [**BVA Decision Intelligence**](https://github.com/va2ai/bva-decision-intelligence)

*Multi-agent research platform that turns natural-language research objectives into citation-verified reports with PDF/DOCX export. Full-text + Qdrant hybrid search, automatic Granted/Denied/Remanded/Mixed outcome classification.*

**The outcome classifier does more than the multi-agent reasoning.** Attorneys want structured, filterable results — not chat. A cheap four-class classifier (Granted / Denied / Remanded / Mixed) on every decision turns the library into a browsable workspace: "show me all PTSD grants where the Board preferred the private psych opinion." That's the actual workflow. The multi-agent reasoning is the cherry on top.

**Constraints.** Qdrant is deep in the retrieval layer — migrating to pgvector later would be painful. OpenRouter sits between my code and Gemini, which adds a failure mode I don't control. **Lesson: simple classifiers win when they unlock the workflow.** I was tempted to skip outcome classification because it felt too obvious; skipping it would have been the wrong call.

---

#### [**RAG Decision Analysis System**](https://github.com/va2ai/rag-decision-analysis-system)

*Extraction + hybrid retrieval pipeline for BVA decisions, validated on an outcome-balanced 100-decision test set. Graph-lite schema on PostgreSQL + pgvector.*

**Graph-lite over full graph DB.** Each decision is decomposed into entities (issue, veteran, service-connection claim, evidence items, holdings) and stored as relational rows with foreign keys — no Neo4j, no RDF. The retriever does hybrid search (BM25 + pgvector) over the **reasoning sections** specifically, not the whole decision, because BVA boilerplate otherwise dominates similarity scores.

**Eval harness before retriever.** I built the 100-decision test set first with hand-labeled correct outcomes and expected evidence citations. Retrieval quality was then measurable in recall@k and citation-match rate, which meant every change to chunking or weighting had a number attached. **The eval harness took longer than the retriever and it was the right call** — eval quality is the ceiling on pipeline quality, full stop.

**Fragile.** The test set ages as the BVA corpus evolves, and the hybrid BM25/vector weighting doesn't transfer cleanly to new domains — every corpus wants its own weight tuning.

---

#### [**DevPortAI RAG Fact-Check**](https://github.com/va2ai/devportai)

*Reference RAG fact-check monorepo — FastAPI backend, React frontend, PostgreSQL + pgvector, Docker Compose.*

**Template for grounding experiments.** I kept wanting to prototype grounding techniques (re-ranking, sentinel tags, adversarial critics) without rebuilding the plumbing each time, so I built a realistic-complexity template. Monorepo tooling is the main operational friction; pgvector index tuning gets tricky past a few million rows. **Multi-stage verification adds latency linearly** — there's a Pareto frontier between speed and grounding confidence and you have to pick a point.

---

### 🛠 How I Work

*   **Comprehension before shipping.** Every project above has a set of prompts, a state machine, or an algorithmic choice I can defend on its own terms — not "the AI wrote it this way." If I can't justify a choice, it gets rebuilt before merge.
*   **Evaluation before optimization.** Test harness first, then the pipeline. Recall@k and citation-match rate before chunking experiments. If I can't measure it, I can't improve it.
*   **Deterministic work in deterministic code.** Rankings, aggregations, and scoring happen server-side, injected as `[PRECOMPUTED_RESULT]` blocks. The LLM narrates; it does not count.
*   **Plain English over marketing language.** "Reduced hallucination" is cheap. "15% of sessions to under 1.5%, measured on a scored test set" is a claim.
*   **Override the model deliberately.** I keep a running note of where I throw out AI output and where I keep it. That's where the real decisions live.

---

### 🤝 Let's Connect

📍 **Location:** Miamisburg / Dayton / Cincinnati • Remote OK
📧 **Email:** [ai@vaclaims.net](mailto:ai@vaclaims.net)

If you're hiring for applied AI / LLM engineering — especially where grounding, evaluation, and operational cost matter — I'd like to talk. I shipped a multi-agent SaaS on GCP in the last year. I can walk you through every trade-off I made, down to the function.

<p align="center">
  <img height="170" src="https://github-readme-stats.vercel.app/api?username=va2ai&show_icons=true&theme=dark&hide_border=true" />
  <img height="170" src="https://github-readme-stats.vercel.app/api/top-langs/?username=va2ai&layout=compact&theme=dark&hide_border=true" />
</p>
