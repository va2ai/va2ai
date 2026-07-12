# Christopher Combs

**Applied AI Engineer · Full-Stack Product Builder · Founder of [V2V Intelligence](https://vaclaims.net)**

📍 Dayton, OH · 🔍 Open to **applied AI / agent systems / backend** roles, remote or hybrid

I build AI products for document-heavy, high-stakes workflows — retrieval, multi-agent orchestration, evaluation, citation validation, and the SaaS layer underneath. 15+ years building software; shipping LLM systems in production since mid-2023, from LangChain retrieval-QA prototypes to a live multi-agent legal-intelligence platform I run solo.

> AI can generate code. The hard part is shipping systems that are traceable, useful, and safe enough for real users — especially when "wrong" has a legal cost.

[Resume](https://vaclaims.net/resume) · [LinkedIn](https://www.linkedin.com/in/va2ai) · [Email](mailto:ai@vaclaims.net) · [vaclaims.net](https://vaclaims.net)

---

## Flagship: V2V Intelligence — live, in production

[V2V Intelligence](https://vaclaims.net) is a legal-intelligence SaaS for U.S. veterans' disability claims, serving veterans, accredited advocates, and attorneys. It unifies the sources a VA claim actually turns on — Board of Veterans' Appeals (BVA) decisions, Court of Appeals for Veterans Claims (CAVC) case law, 38 CFR regulations, and the VA's M21-1 adjudication manual — behind agentic workflows where every output traces back to a government source.

I built and operate all of it: product, AI pipelines, infrastructure, and billing.

- **Attorney Research Swarm** — 9-stage coordinator (decompose → plan → retrieve → score → detect conflicts → synthesize) that drafts a research memo with every citation checked against the retrieved authority table before it reaches the user.
- **Appeal Strategy Swarm** — paste a BVA decision, get a court-ready battlecard: detected board errors, predicted government defenses, recommended appeal theories.
- **Search Swarm** — deterministic, no-LLM search across 5 legal corpora, ordered by authority weight before relevance: a binding regulation outranks 200 non-precedential decisions that merely cite it.
- **Real-time phone agent** — voice intake bridging Twilio Programmable Voice and Gemini Live, with PIN auth and 7 tool functions callable mid-call.
- **The SaaS layer** — React 19 + TypeScript + Firebase + Stripe billing with 4 pricing tiers, usage gates, and tier-gated premium workflows, deployed on Cloud Run and Cloudflare Workers.

The product code is private (`va-claims-intel` — app and agent pipelines, `bvaapi2` — the legal-research data plane and MCP tool surface, `crazy-caller` — the Twilio + Gemini Live voice bridge). **I'm glad to do a live code walkthrough of any of it.**

---

## Now building: Varep

**Varep** *(private — walkthrough on request)* is a citation-locked VA claims-file workspace for VSOs, accredited representatives, and law firms. Where V2V answers "what does the law say," Varep answers "what does *this veteran's file* say": upload a C-file, and the system ingests, OCRs, classifies, and quality-checks the records, then builds a living case workspace — timeline, evidence gaps, deadlines, staged tasks, and drafted work product, all tied to source documents with page-level citations.

The engineering choices are the point:

- **Provenance is enforced, not decorative** — source evidence, generated work product, legal authority, and notes are kept structurally separate; factual outputs must cite source pages.
- **The vector DB is not the system of record** — durable document storage plus relational metadata is; the retrieval index is rebuildable.
- **Analysis goes stale on purpose** — new evidence or a corrected document marks downstream outputs stale until refreshed, so nobody acts on an outdated AI conclusion.

---

## Public code

- **[citation-validator](https://github.com/va2ai/citation-validator)** — post-generation hallucination validator for legal AI output. Five-layer architecture: sentinel-tagged retrieval, grounded generation, structured extraction, deterministic cross-reference, adversarial critic. Catches fabricated regulation citations, docket numbers, and case references before they reach a user.
- **[decision-lens](https://github.com/va2ai/decision-lens)** — multi-agent document analysis pipeline that turns dense administrative decisions into grounded, citation-checked reports. LangGraph orchestration, LiteLLM model routing, Instructor structured outputs, ChromaDB retrieval, FastAPI + React, with an adversarial critic in the synthesis loop.
- **[llm-compaction-benchmark](https://github.com/va2ai/llm-compaction-benchmark)** — benchmarks conversation-compaction quality across 6 Gemini models (36 pairwise combinations). Built to pick a compaction model for long-running agent sessions with data instead of guessing.
- **[va-bench](https://github.com/va2ai/va-bench)** — 20-prompt evaluation catalog for VA legal-research workflows, with a single-agent sandbox runtime for testing orchestration chains.

---

## How I think about AI reliability

I ship LLM systems in a domain where a wrong citation has legal consequences. Every rule below was earned from a production failure, not a blog post.

**Reach for determinism before generation.** One of the highest-leverage components in V2V — the Search Swarm — contains no LLM. Ranking by legal authority weight instead of hit count is what an attorney actually needs, and deterministic code is faster, cheaper, and testable. The bugs that mattered were unglamorous: an eCFR hierarchy quirk that yields `3.3.310` if you concatenate section prefixes naively, and an upstream API that silently falls back to party-name search when full-text isn't wired.

**Validate against the retrieved record, not against plausibility.** LLMs will state a correct legal rule and attach the wrong citation — polished, confident, and wrong in a way users can't spot. My validation loop pairs a deterministic cross-reference (does this citation exist in the retrieved authority?) with an adversarial model critic (does the source actually support this claim?). Each catches what the other misses. The public [citation-validator](https://github.com/va2ai/citation-validator) is this pattern, extracted.

**Give failures an address.** Claims analysis runs as staged agents — intake, authority mapping, evidence evaluation, conflict detection, synthesis — because those stages fail differently, and a failure you can localize is a failure you can fix. Tools are least-privilege per role: intake agents can't call analysis tools, which shrinks the blast radius and keeps traces readable.

**Measure before scaling.** Before indexing the full Board corpus, I validated the retrieval schema against a 100-decision evaluation harness. Before trusting a compaction model in long-running agent sessions, I benchmarked [6 models across 36 combinations](https://github.com/va2ai/llm-compaction-benchmark). Evals up front are cheaper than debugging in production — and they turn "I think this works" into a number.

**Treat domain rules as architecture, not prompt text.** Board decisions are persuasive, not precedential; a regulation outranks the policy manual. That hierarchy is enforced in code — authority-first ordering in search, citation checks at synthesis — not left to a sentence in a system prompt the model may ignore. Getting this right is the difference between a legal research tool and a liability.

---

## Stack

- **Languages & product** — TypeScript, Python, React 19, Node/Express, FastAPI, Tailwind, Vite
- **AI, agents & retrieval** — OpenAI, Claude, Gemini (incl. Gemini Live realtime voice), LangChain/LangGraph, LiteLLM, Instructor, MCP, ChromaDB, pgvector, eval harnesses
- **Infra & billing** — Google Cloud (Cloud Run, Firebase), Cloudflare Workers + R2, Fly.io, Docker, PostgreSQL, Stripe, Twilio
- **Quality & delivery** — Playwright, Vitest, GitHub Actions, SSE/WebSocket streaming UIs

---

💼 **Currently:** building V2V Intelligence and Varep, running the Generative AI Ohio meetup, and looking for applied AI / agent systems / backend roles where production judgment matters.

📫 [ai@vaclaims.net](mailto:ai@vaclaims.net) · [LinkedIn](https://www.linkedin.com/in/va2ai) · [Resume](https://vaclaims.net/resume)
