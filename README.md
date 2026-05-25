# Wei Jia

Senior SDET / AI Quality Engineering Lead focusing on LLM evaluation, RAG testing, Playwright automation, and source-of-truth validation.

I'm a big nerd at heart and I'm currently deep diving into utilizing AI harnesses like [`the one Mozilla used to capture 200+ zero-days for Firefox`](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/), [`agonist-antagonist supersetting`](https://pmc.ncbi.nlm.nih.gov/articles/PMC12011898/) for strength training, and setting up a home RAG pipeline that I can finally dump the hundreds of e-books I've picked up from [`Humble Bundle`](https://www.humblebundle.com/books) and various online book clubs that I've been a part of.

## What I work on

- LLM and RAG evaluation harnesses with regression gates, drift checks, traces, and schema-validated reports
- Playwright test systems that incorporate best practices and guard against flakiness
- Data-product QA that traces and cross-compares APIs, transforms, and source records
- Reviewer aids for AI-generated code and research output backed by a lot of research
- Accessibility and privacy-adjacent tooling
- Research-based skills to help fight disinformation and LLM hallucination with deterministic gates against biases, evidence tiering according to the hierarchy of evidence, and pattern detection for study manipulation like HARKing and p-hacking

## Active Projects

| Repo | What it is | Description |
| --- | --- | --- |
| [`oncology-rag-lab`](https://github.com/weijia-89/oncology-rag-lab) | Synthetic clinical RAG evaluation testbed | LlamaIndex, ChromaDB, Ollama, Pydantic schemas, DeepEval, Phoenix traces, GitHub Actions, 8 base oncology notes, 12 adversarial edge-case notes, baseline-pinned CI regression gate with 5% threshold, A/B model drift comparison, deterministic mocked LLM responses.
| [`playwrighter`](https://github.com/weijia-89/playwrighter) | Playwright pattern library plus test-quality scorer | 23 pattern files, 8 templates, `validate-suite.sh`, `score-tests.js`, 100-point rubric, default 80 threshold, penalties for `waitForTimeout`, `networkidle`, brittle selectors, missing assertions, weak assertions, XPath, `nth-child`, and manual visibility checks. |
| [`northwind-qa`](https://github.com/weijia-89/northwind-qa) | Worked Playwright suite that dogfoods playwrighter | 50 application tests plus 1 auth setup, 45 passing outright, 5 expected-fail regression guards, 7 bug reports with repro steps, axe accessibility checks, CI, and a 91.4/100 playwrighter score. Toy SUT, useful as a worked quality-system example if you're interested in my professional work ;) |
| [`vibe-check`](https://github.com/weijia-89/vibe-check) | Reviewer aid for PRs that may contain LLM-generated code | 10 heuristic signals, JSON/Markdown output, drift-aware calibration, local telemetry, strict-quote claims gate, and regression tests for false positives. Less an vibe-code detector, more a vibe-check that encourages you to not just dump the diff into Claude and call it a day. |
| [`lodestar`](https://github.com/weijia-89/lodestar) | Public-data VoC and agentic bug-prioritization pipeline | GitHub issue ingest, deduplication, PII moderation, descriptive ranking, human rationale slots, TF-IDF themes, worked escalation, 172 tests, and 76% mutation-test kill rate. Portfolio demo for a job I didn't get :( |

Supporting repos:

- [`wcag-auditor`](https://github.com/weijia-89/wcag-auditor): Playwright + axe-core accessibility auditor. Earlier versions used local Ollama for fix suggestions; v0.3 replaced that path with deterministic templates because I wanted to stop fighting hallucinations. Still limited by what axe-core can catch which is roughly 30%~ of all issues.
- [`palamedes`](https://github.com/weijia-89/palamedes): evidence synthesis, source tiers, quote gates, adversarial review, and confidence scoring. I'm secretly very proud of this work because it combines my inherent skepticism with a tool that can do a lot of the tedious footwork of reading systematic reviews and meta-analyses, uncovering nuances in findings, and weeding out low-quality studies that you don't realize are low-quality until you're 20 minutes deep before realizing all the p-values are suspiciously at threshholds for significance. 

## Internal work

My most complex and effectful work all lives in my employer's repo! Can't show that off but can describe it generally:

At Intuit Mailchimp, I worked on an internal AI/RAG governance agent for a 12+ team GTM program across a 100+ feature release surface and 50+ team channels. It retrieved sanitized data from project trackers, team channels, internal wikis, cloud artifacts like PRDs, TDDs, roadmaps, and so on. To guard against hallucination, this agent was trained to surface hypotheses that were validated through source-document review, owner-index checks, locked roadmap date checks, and deterministic search results like SQL queries.

I also built a backend-to-frontend test suite for the user-facing reporting and analytics surface areas covering all 12+ pages, 30+ components, and hundreds of metrics. The suite first runs an independent Playwright crawler, Chrome CDP network-log capture, and Python validation scripts against source-of-truth data from our back-end. Artifacts included sanitized network logs, per-page CSV outputs, JSON, screenshots, traces, DOM snapshots, and backend metric-key mappings. 

## Scope notes

- oncology-rag-lab is synthetic and local but it is definitely not clinical-grade, FDA-grade, production medical software, or a PHI system. It's a hobbyist lab experiment that was vetted by my partner who is a psychiatrist but definitely not an oncologist. 'This looks right to me,' is what she said after a twenty-minute scan :)
- vibe-check is reviewer support, not merge-blocking truth and not an AI detector and not intended to be used to penalize people for AI-usage. It's just a little warning label, like a sell-by date, that you can probably safely ignore 99% of the time unless you've got a critical event in the near future and in which case you should probably do a thorough sniff test.
- northwind-qa uses a toy SUT! It proves test-system discipline, not production traffic experience.
- My healthcare exposure includes practical healthcare-operations adjacency through helping my partner, a practicing psychiatrist, launch a telehealth private practice, including tech-stack evaluation, PII-flow mapping, access-boundary thinking, and HIPAA/cloud-service requirement review. I do not claim production healthcare SDET work or formal compliance ownership but I am not unfamiliar with all the fun intricacies of HIPAA-specific requirements for E2E encryption, PII data flows, and cloud-computing requirements.

## Stack

Python · TypeScript · JavaScript · Playwright · pytest · GitHub Actions · LlamaIndex · ChromaDB · DeepEval · Phoenix · Ollama · Pydantic · axe-core · Chrome CDP · SQLite · uv

## Contact

[LinkedIn](https://linkedin.com/in/wei-jia)
