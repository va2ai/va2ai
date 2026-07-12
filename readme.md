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

## Public code

- **[citation-validator](https://github.com/va2ai/citation-validator)** — post-generation hallucination validator for legal AI output. Five-layer architecture: sentinel-tagged retrieval, grounded generation, structured extraction, deterministic cross-reference, adversarial critic. Catches fabricated regulation citations, docket numbers, and case references before they reach a user.
- **[decision-lens](https://github.com/va2ai/decision-lens)** — multi-agent document analysis pipeline that turns dense administrative decisions into grounded, citation-checked reports. LangGraph orchestration, LiteLLM model routing, Instructor structured outputs, ChromaDB retrieval, FastAPI + React, with an adversarial critic in the synthesis loop.
- **[llm-compaction-benchmark](https://github.com/va2ai/llm-compaction-benchmark)** — benchmarks conversation-compaction quality across 6 Gemini models (36 pairwise combinations). Built to pick a compaction model for long-running agent sessions with data instead of guessing.
- **[va-bench](https://github.com/va2ai/va-bench)** — 20-prompt evaluation catalog for VA legal-research workflows, with a single-agent sandbox runtime for testing orchestration chains.

---

## How I think about AI reliability

The signal isn't "AI built this." It's whether I can explain the architecture, the trade-offs, the failure modes, and the validation strategy.

**Deterministic beats generative — when it does.** The Search Swarm uses no LLM at all. Naive rank-by-hits drowns the controlling regulation under hundreds of Board decisions that cite it; ordering by legal authority weight matches how an attorney actually reads the law — and it's faster, cheaper, and reproducible. The bugs that mattered were unglamorous: a section-prefix collision in eCFR's hierarchy that yields `3.3.310` if you concatenate naively, and an upstream API that silently degrades to party-name search when full-text isn't wired.

**Validation converts uncertainty into metadata.** LLMs will state a correct legal rule and attach the wrong citation — a failure users can't spot and legal workflows can't tolerate. Deterministic checks catch citations that don't exist in the retrieved record; a model-based critic catches overstatement and unsupported inference. You need both. The biggest reliability win I've found is not a bigger model — it's a loop that surfaces model uncertainty explicitly instead of hiding it inside polished prose.

**Staged agents isolate failure.** Claims analysis is not one retrieval call. Intake, authority mapping, evidence evaluation, conflict detection, and synthesis all fail differently, so they run as separate stages with clear handoff contracts — failures become isolatable and repairable. Tooling is least-privilege per role: intake agents don't get analysis tools. Role-gating reduces accidental misuse and makes agent behavior debuggable.

**Domain constraints are architecture.** BVA decisions are persuasive, not precedential — the system must never render one as binding. Regulation, court precedent, and agency policy sit in a hierarchy the retrieval and synthesis layers have to respect. Getting this right is what separates a legal AI tool from a liability.

---

## Stack

- **Languages & product** — TypeScript, Python, React 19, Node/Express, FastAPI, Tailwind, Vite
- **AI, agents & retrieval** — OpenAI, Claude, Gemini (incl. Gemini Live realtime voice), LangChain/LangGraph, LiteLLM, Instructor, MCP, ChromaDB, pgvector, eval harnesses
- **Infra & billing** — Google Cloud (Cloud Run, Firebase), Cloudflare Workers + R2, Fly.io, Docker, PostgreSQL, Stripe, Twilio
- **Quality & delivery** — Playwright, Vitest, GitHub Actions, SSE/WebSocket streaming UIs

---

💼 **Currently:** building V2V Intelligence, running the Generative AI Ohio meetup, and looking for applied AI / agent systems / backend roles where production judgment matters.

📫 [ai@vaclaims.net](mailto:ai@vaclaims.net) · [LinkedIn](https://www.linkedin.com/in/va2ai) · [Resume](https://vaclaims.net/resume)
