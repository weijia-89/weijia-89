# Wei Jia

The romantic side of me never expected to work in tech. English and History double major, five years in government work and immigration law, months coordinating field operations for a voter registration drive, and I think it's pretty obvious to anybody reading this that I spent most of my early career trying to be somebody else. It took me a while to stop pushing down that critical quality: that I was an utter nerd who loved understanding how systems fit together.

QA engineer at Intuit Mailchimp now, and the testing problems I keep gravitating toward are the ones where a fixed assertion misses the point. LLM outputs are probabilistic. Accessibility violations give you an ID, not the actual HTML change. Privacy claims are easy to make and hard to prove from reading the code. Each one needed its own evaluation harness, because the off-the-shelf tools either don't exist or they stop one step short of useful. The repos below are where that went.

For anyone reading this from a hiring side, the projects below are organized by what they were built to do. For AI-tooling and code-review work I'd point at `lodestar` and `vibe-check`. For test infrastructure and LLM evaluation, `playwrighter` and `oncology-rag-lab`. `palamedes` and `wcag-auditor` cover the work where the methodology was as much of the project as the code.

---

## Projects

### [vibe-check](https://github.com/weijia-89/vibe-check)

A reviewer evidence surfacer for PRs that may contain LLM-generated code. It runs ten regex and AST heuristics in Python stdlib alone, with no trained model and no outbound network except `gh` for PR mode, and reports per-signal evidence (hallucinated APIs, bare `except:` blocks, AI tool markers, comment-phrasing boilerplate, declarative bias, and a handful of others documented in `references/AI_SIGNALS_RESEARCH.md`). The ten signals each have their own regression tests and a calibration ledger that records why every threshold is where it is.

Recent benchmarks (AICD Bench 2026, Wang ICSE 2025; CLAIMS C-005, C-008) report detectors of this class as below practical usability under distribution shift. The README repeats that in three places, the strict-quotes gate in CI prevents anyone from quoting drift-affected accuracy numbers as if they were stable, and the tool's stated job is to slow a reviewer down on suspicious patterns rather than to gate merge. The repo is also where I worked through what an honest evidence ledger looks like when the underlying claim is contested.

`AI detection` `code review` `Python` `regex` `AST` `calibration` `Brier score`

---

### [palamedes](https://github.com/weijia-89/palamedes)

Rigorous LLM research in two layers. A multi-agent dialectic synthesis prompt for one-shot human-driven deep research, plus an agent-loadable skill that gives an LLM coding agent the same epistemic discipline at the coding-task level. Both share one canonical evidence-tier table, one confidence-calibration doc, and one failure-log.

Each synthesis run coordinates three independent LLM research agents and then passes their combined output to a separate adversarial synthesis agent before anything reaches the report. The system can't surface a high-confidence claim without showing the source text that backs it, and it is instructed to adversarially self-review, to disconfirm rather than just take everything it outputs as truth. The part that took the most work was building in the understanding that three agents agreeing does not mean three independent data points. About 60% of the time, two same-provider models will get the same thing wrong in the same way (arXiv 2506.07962), and the prompt's job is to assume that is happening unless something forces it to update. This repo consolidates `research-synthesis-prompt` and `ai-research`, which were merged on 2026-05-16 because they were doing the same epistemics at two different scales.

`multi-agent` `adversarial synthesis` `evidence methodology` `confidence scoring` `prompt engineering` `agent skill`

---

### [lodestar](https://github.com/weijia-89/lodestar)

A voice-of-customer ingestion plus agentic bug-prioritization pipeline. It pulls GitHub issues, deduplicates with embeddings, ranks by recency-engagement-label composite weights, and produces a priority report a human triager can read in one screen. The composite scorer surfaces a `ScoreBreakdown` per item, so anyone reading the rank can see why something landed where it landed, which is the design principle the whole tool is built around: severity classification remains a human judgment, the tool's job is to compress a large issue backlog into a ranked candidate set with the rationale visible.

The test suite is where the discipline shows. `tests/moderate/test_patterns.py` includes false-positive avoidance tests like `test_scan_text_does_not_flag_github_handle_as_email`, which were written before the regex was tightened because the regex's failure mode was the test's job to catch. The PII detection module is covered by mutation testing via `mutmut` (`scripts/run_mutmut.sh`); the kill-rate report under `mutants/mutmut-stats.json` shows which mutants died and to which tests, so the coverage claim is backed by the actual mutation ledger rather than a line-coverage percentage.

`Python` `voice of customer` `agentic prioritization` `mutation testing` `embeddings` `dedup` `PII detection`

---

### [playwrighter](https://github.com/weijia-89/playwrighter)

A Playwright pattern library plus a working test-quality scorer, so an AI agent or a human writing E2E tests has both the patterns to follow and an automated way to check whether the suite actually follows them. The 23 pattern files under `patterns/` trace to Playwright's official docs and to conventions I verified across community projects, and `tools/score-tests.js` grades a directory of `.spec.ts` files against a rubric that mirrors the patterns directly. A `waitForTimeout` call costs 8 points, a CSS-selector locator costs 6, and the full rubric scores out of 100 with an 80-point CI threshold.

The scorer is intentionally regex-and-AST simple. It catches the syntactic decay that creeps into a suite over time, from the flake-fix that introduced a `waitForTimeout` to the quick-locator shortcut that landed a CSS selector instead of an accessible role, and it fails CI before the reviewer has to find them. The library and the scorer share a vocabulary, so a contributor or an AI agent reading the SKILL ends up writing tests that pass the scorer because the patterns and the rubric are the same artifact. The skill ships as a multi-tool bundle for Claude, Cursor, and Windsurf.

`Playwright` `E2E testing` `pattern library` `test quality scorer` `AI agent skill` `TypeScript` `JavaScript`

---

### [oncology-rag-lab](https://github.com/weijia-89/oncology-rag-lab)

A working LLM pipeline (LlamaIndex + ChromaDB + Ollama) that pulls oncology entities out of synthetic clinical notes (AJCC stage, regimen, ECOG, cancer type). The pipeline isn't the interesting part. The testing infrastructure around it is what I spent the time on, the DeepEval suite scored against gold-standard labels, the regression gate that fails CI if pass rate drops more than 5% versus baseline, A/B drift detection between model versions, and Arize Phoenix tracing every retrieval and extraction. Same patterns a production oncology platform runs at 150M documents, scaled down to 20 synthetic notes on a laptop.

The 20 notes are 8 base notes plus 12 adversarial edge cases I wrote specifically to fail naive extraction, including copy-forward staleness, half-filled SmartPhrase templates, Dragon transcription errors, and two different staging systems showing up in the same chart. The synthetic corpus has known limits and `FIDELITY_REVIEW.md` writes them down. I compared the base notes against 3 de-identified MTSamples transcriptions and listed 12 specific ways the synthetic notes do not look like the real thing. An extractor that passes here is not production-ready, and saying so in the README is part of the deliverable.

`LLM eval` `DeepEval` `RAG` `ChromaDB` `LlamaIndex` `Arize Phoenix` `drift detection` `Python`

---

### [wcag-auditor](https://github.com/weijia-89/wcag-auditor)

The standard WCAG workflow is to run axe-core, read the violation ID, look up the criterion, and figure out what to actually change. wcag-auditor puts a deterministic rule engine in the middle of that. Playwright injects axe-core into the page, violations come back as structured objects, each one runs through a per-rule fix template that has enough HTML context to produce a suggestion specific enough to act on. Pydantic validates the output before it hits your terminal, and the HTML you audit never leaves your machine because nothing is sent anywhere.

axe-core catches roughly 30-40% of WCAG 2.2 issues. wcag-auditor doesn't change that number, it makes the 30-40% easier to act on. An earlier version used an LLM for the fix-generation step; v0.3 swapped it for deterministic rule templates because the deterministic version was auditable, faster, and didn't need a 14GB model running in the background to spit out a suggestion that was already mostly templated anyway. Deciding to walk away from a shipped LLM feature because the boring version was just better took longer than it should have.

`accessibility` `WCAG 2.2` `axe-core` `Playwright` `Pydantic` `Python`

---

## Earlier work

### [no-log-rsvp](https://github.com/weijia-89/no-log-rsvp)

Stores event title, timestamp, and headcount. No names, no emails, no IPs, no accounts. Everything deletes 24h after the event. A regex PII guard rejects event descriptions containing personal information at the API boundary, and there's an eval suite that measures the guard's precision and recall on canned PII strings, because the regex is the security boundary and silent regression in the regex is the actual risk. EXIF and XMP metadata get stripped from uploaded images on the way in. `SECURITY.md` and `PRIVACY_MODEL.md` walk through the threat model, the data inventory, and what regex PII detection cannot catch (which is most things).

`privacy engineering` `PII detection` `FastAPI` `SQLite` `Python`

### [toebeans](https://github.com/weijia-89/toebeans)

A Kotlin Multiplatform + Compose pet medication tracker. The interesting part is the engineering discipline the project carries. Nineteen ADRs under `docs/adr/` walk through the design choices and why each one was made. CI runs fitness functions on every push and macrobenchmarks gate performance regressions, so the perf budget is enforced by the pipeline rather than by anyone remembering to check. The test suite covers edge cases like midnight-straddle detection for medications that span the day boundary; `MidnightStraddleDetectionTest.kt` is a useful read on test-as-spec discipline because the docstring lays out the algorithmic specification before the tests assert against it.

`Kotlin Multiplatform` `Compose` `ADRs` `fitness functions` `macrobenchmarks` `test as spec`

---

## What connects them

The thing these have in common is probably that I started each one because some other tool was doing 80% of what I needed, and what ended up taking the time was building enough scaffolding around it to do something useful with the other 20%. The shape of the scaffolding varies by project. What's consistent is the section of the README where I had to write down, in plain English, which gaps the scaffolding doesn't close, which is usually the harder part of the project anyway.

[`northwind-qa`](https://github.com/weijia-89/northwind-qa) is the worked Playwright example. It's a 51-test suite against a React 19 + Vite e-commerce SUT ([`example-e-commerce-website`](https://github.com/weijia-89/example-e-commerce-website)) that scores 91.4/100 on playwrighter's own rubric and ships seven real bug reports with regression-test guards. Not pinned because it leans on `playwrighter` for the patterns and `vibe-check` for the review side; it's the place where the two come together rather than its own argument.

---

## Stack

Python · TypeScript · JavaScript · FastAPI · Playwright · axe-core · LlamaIndex · ChromaDB · DeepEval · Arize Phoenix · Ollama · Pydantic · SQLite · uv · pytest · GitHub Actions

---

## Licensing

The repos use three license families. The reusable patterns and pipelines (`lodestar`, `oncology-rag-lab`, `wcag-auditor`, `no-log-rsvp`, `mashit`) ship MIT because the point is for them to be adopted and built on. The methodology-as-deliverable work (`vibe-check`, `palamedes`, `playwrighter`) ships PolyForm Noncommercial 1.0.0 with an Iron Law addendum that keeps the calibration and strict-quotes guards intact in any derivative. `toebeans` ships AGPL, the right default for an end-user app where source-availability for derivatives matters. Each repo's `LICENSE` file is the source of truth.

---

## Contact

[LinkedIn](https://linkedin.com/in/wei-jia)
