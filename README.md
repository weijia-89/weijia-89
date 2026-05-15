# Wei Jia

The romantic side of me never expected to work in tech. English and History double major, five years in government work and immigration law, months coordinating field operations for a voter registration drive, and I think it's pretty obvious to anybody reading this that I spent most of my early career trying to be somebody else. It took me a while to stop pushing down that critical quality: that I was an utter nerd who loved understanding how systems fit together.

QA engineer at Intuit Mailchimp now, and the testing problems I keep gravitating toward are the ones where a fixed assertion misses the point. LLM outputs are probabilistic. Accessibility violations give you an ID, not the actual HTML change. Privacy claims are easy to make and hard to prove from reading the code. Each one needed its own evaluation harness, because the off-the-shelf tools either don't exist or they stop one step short of useful. The four repos below are where that went.

---

## Projects

### [research-synthesis-prompt](https://github.com/weijia-89/research-synthesis-prompt)

A multi-agent research prompt iterated over several months to produce epistemic research reports. No more getting 'well actually' when you talk about your favorite new habit you picked up, now you'll be the one going on about the replication crises and the importance of the hierarchy of evidence.

Each run coordinates three independent LLM research agents and then passes their combined output to a separate adversarial synthesis agent before anything reaches the report. The system can't surface a high-confidence claim without showing the source text that backs it, and it's instructed to adversarially self-review, to disconfirm instead of just taking everything it outputs as truth. Cross-agent disagreements surface as `[CONFLICT]` flags so the synthesis agent has to deal with them on the way through. The part that took the most work wasn't the synthesis logic but building in the understanding that three agents agreeing doesn't mean three independent data points. Just because all the people at your gym drink pre-workout doesn't mean that the 2000% of your DV of taurine they're taking will be anything more than placebo effect. The goal is to incorporate skepticism, not normalize global confusion.

The latest version names the agents by capability class instead of by provider (live-web search, deep-research, strong reasoning, highest-reasoning long-context synthesis), because the bit that matters is whether the three agents share an architecture, not whose logo they carry. There's a published number for this. About 60% of the time, two same-provider models will get the same thing wrong in the same way (arXiv 2506.07962), and the prompt's job is to assume that's happening unless something forces it to update.

`multi-agent` `adversarial synthesis` `evidence methodology` `confidence scoring` `prompt engineering`

---

### [oncology-rag-lab](https://github.com/weijia-89/oncology-rag-lab)

A working LLM pipeline (LlamaIndex + ChromaDB + Ollama) that pulls oncology entities out of synthetic clinical notes: AJCC stage, regimen, ECOG, cancer type. The pipeline isn't the interesting part. The testing infrastructure around it is what I spent the time on: DeepEval metrics, Arize Phoenix observability, A/B drift comparison between model versions, a regression gate that fails CI if the pass rate drops more than 5 points. Same patterns a production oncology platform runs at 150M documents, scaled down to 20 synthetic notes on a laptop.

The 20 notes break out as 8 base notes plus 12 adversarial edge cases I wrote specifically to fail naive extraction. Copy-forward staleness. Half-filled SmartPhrase templates. Dragon transcription errors. Two different staging systems showing up in the same chart. A real clinical-NLP extractor has to survive all of that and more.

The synthetic corpus has known limits and `FIDELITY_REVIEW.md` writes them down. It compares the base notes against 3 de-identified MTSamples transcriptions and lists 12 specific ways the synthetic notes don't look like the real thing. An extractor that passes here is not production-ready, and saying so in the README is part of the deliverable.

`LLM eval` `DeepEval` `RAG` `ChromaDB` `LlamaIndex` `Arize Phoenix` `drift detection` `Python`

---

### [wcag-auditor](https://github.com/weijia-89/wcag-auditor)

The standard WCAG workflow is: run axe-core, read the violation ID, look up the criterion, figure out what to actually change. wcag-auditor puts a deterministic rule engine in the middle of that. Playwright injects axe-core into the page, violations come back as structured objects, each one runs through a per-rule fix template that has enough HTML context to produce a suggestion specific enough to act on. Pydantic validates the output before it hits your terminal. Audit history goes into SQLite, HTML stays on the machine.

axe-core catches roughly 30-40% of WCAG 2.2 issues. wcag-auditor doesn't change that number. It makes the 30-40% easier to act on. An earlier version used an LLM for the fix-generation step; v0.3 swapped it for deterministic rule templates because the deterministic version was auditable, faster, and didn't need a 14GB model running in the background to spit out a suggestion that was already mostly templated anyway. Deciding to walk away from a shipped LLM feature because the boring version was just better took longer than it should have.

`accessibility` `WCAG 2.2` `axe-core` `Playwright` `Pydantic` `Python`

---

### [no-log-rsvp](https://github.com/weijia-89/no-log-rsvp)

Stores event title, timestamp, and headcount. No names, no emails, no IPs, no accounts. Everything deletes 24h after the event. A regex PII guard rejects event descriptions containing personal information at the API boundary, and there's an eval suite that measures the guard's precision and recall on canned PII strings, because the regex is the security boundary and silent regression in the regex is the actual risk. EXIF and XMP metadata get stripped from uploaded images on the way in. `SECURITY.md` and `PRIVACY_MODEL.md` walk through the threat model, the data inventory, and what regex PII detection cannot catch (which is most things). If the threat model is operator log scraping after a breach, fine. If it's anything more serious, the architecture isn't enough and the README admits it.

`privacy engineering` `PII detection` `FastAPI` `SQLite` `Python`

---

## What connects them

The thing all four of these have in common is probably that I started each one because some other tool was doing 80% of what I needed, and what ended up taking the time was building enough scaffolding around it to do something useful with the other 20%. The shape of the scaffolding varies by project. What's consistent is the section of the README where I had to write down, in plain English, which gaps the scaffolding doesn't close, which is usually the harder part of the project anyway.

---

## Stack

Python · FastAPI · Playwright · axe-core · LlamaIndex · ChromaDB · DeepEval · Arize Phoenix · Ollama (oncology-rag-lab) · Pydantic · SQLite · uv · pytest · GitHub Actions

---

## Contact

[LinkedIn](https://linkedin.com/in/wei-jia)
