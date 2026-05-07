# Wei Jia

The romantic side of me never expected to work in tech. English and History double major, five years in government work and immigration law, months coordinating field operations for a voter registration drive, and I think it's pretty obvious to anybody reading this that I spent most of my early career trying to be somebody else. It took me a while to stop pushing down that critical quality: that I was an utter nerd who loved understanding how systems fit together.

QA engineer at Intuit Mailchimp now, and what I keep gravitating toward is the part of the testing problem that doesn't have a clean answer: LLM pipelines where the output is probabilistic and a fixed assertion misses the point, accessibility audits where the violation ID tells you what's wrong but not what to actually change, network scans where the risk isn't in any individual open port but in what the device combination implies together. It turns out it was really just a matter of cobbling together enough of the problem space to understand what kind of evaluation infrastructure each system actually needed. These six repos are what came out of that.

---

## Projects

### [research-synthesis-prompt](https://github.com/weijia-89/research-synthesis-prompt)
Multi-agent adversarial research synthesizer

Three LLM agents (Gemini Deep Research, ChatGPT Deep Research, Claude) run the same prompt independently, then a fourth agent ingests all three outputs and applies Hegelian dialectics: strongest case for each claim, strongest case against it, synthesis that survives both. Cross-agent disagreements surface as `[CONFLICT]` flags rather than being averaged away. Three agents all citing the same three papers is one data point, not three, and the system knows that.

The evidence methodology is the part that required the most iteration: a study design hierarchy with tier-based confidence ceilings, pre-registration checks, effect size gates for high-confidence claims, HARKing detection, meta-analysis quality checklist (I², funnel plot, double-counted underlying RCTs). Claims without source text backing get held. The output is an HTML research report with an inline evidence ledger and a structured `<summary>` block formatted for downstream adversarial agent ingestion.

`multi-agent` `adversarial synthesis` `evidence methodology` `confidence scoring` `prompt engineering`

---

### [oncology-rag-lab](https://github.com/weijia-89/oncology-rag-lab)
RAG pipeline for structured clinical entity extraction, with eval infrastructure

A working LLM pipeline (LlamaIndex + ChromaDB + Ollama) that extracts oncology entities from synthetic clinical notes: AJCC stage, regimen, ECOG, cancer type. The evaluation layer around it is the main artifact: DeepEval metrics, Arize Phoenix observability, A/B drift comparison, regression gate that fails CI if pass rate drops more than 5 points. Same patterns production oncology platforms run at 150M documents, applied to 8 docs on a laptop.

`LLM eval` `DeepEval` `RAG` `ChromaDB` `LlamaIndex` `Arize Phoenix` `drift detection` `Python`

---

### [wcag-auditor](https://github.com/weijia-89/wcag-auditor)
WCAG 2.2 accessibility auditor with rule-based fix generation

The standard WCAG workflow: run axe-core, read the violation ID, look up the criterion, figure out what to actually change. This puts a structured fix engine in the middle of that. Playwright injects axe-core into the page; violations come back as structured objects; each one gets passed through per-rule fix templates with enough HTML context to produce a suggestion specific enough to act on. Pydantic validates the output before it hits your terminal. Audit history in SQLite, HTML stays on the machine.

axe-core catches ~30-40% of WCAG 2.2 issues; this doesn't change that number. It makes the 30-40% easier to act on.

`accessibility` `WCAG 2.2` `axe-core` `Playwright` `Pydantic` `Python`

---

### [network-scanner](https://github.com/weijia-89/network-scanner)
Rule-ranked network scanner

ARP sweep + nmap on a /24, followed by a rule engine walking the device list and flagging the cameras, the unencrypted MQTT brokers, the streaming sticks, the routers with too many admin surfaces. Risk scoring looks at device combinations, not individual ports in isolation. Everything stays on the box. Real scans need `--confirm` as a legal acknowledgment that you own the target subnet.

`network security` `nmap` `infosec` `Python`

---

### [android-hardener](https://github.com/weijia-89/android-hardener)
Read-only CIS Android Benchmark auditor

Connects to a device over ADB, evaluates 15 CIS Android Benchmark controls, and produces a hardening report ordered by severity, all without ever writing to the device. Mock fixtures let the eval suite run without a connected phone.

`Android` `CIS Benchmark` `ADB` `security auditing` `Python`

---

### [no-log-rsvp](https://github.com/weijia-89/no-log-rsvp)
Privacy-by-design RSVP API: headcounts, not names

Stores event title, timestamp, and headcount. No names, no emails, no IPs, no accounts. Everything deletes 24h after the event. A regex PII guard rejects event descriptions containing personal information at the API boundary. EXIF/XMP metadata stripped from uploaded images. Threat model and full data inventory in SECURITY.md + PRIVACY_MODEL.md.

`privacy engineering` `PII detection` `FastAPI` `SQLite` `Python`

---

## What connects them

Six projects, one recurring gap. Each standard tool produces data (port states, violation IDs, LLM outputs, a device list, a source document) but the step from that data to a useful answer had to be built from scratch each time. For oncology-rag-lab, that meant DeepEval metrics wired to a regression gate, because probabilistic outputs don't fail cleanly against fixed assertions. For wcag-auditor, getting from a rule ID to the actual HTML change required per-rule fix logic with enough HTML context to make the suggestion worth following. For network-scanner, risk scoring walks the full device list for dangerous combinations: the camera-plus-admin-port-plus-unencrypted-broker pattern no individual port flag would catch. For android-hardener, a strictly read-only pipeline keeps the audit results trustworthy; write access would undermine that. For no-log-rsvp, the privacy claim is enforced at the schema level: no description column, anonymous IDs only, 24h auto-delete. For research-synthesis-prompt, the problem was that three agents agreeing doesn't mean three independent data points; if all three cited the same papers, the synthesis had to catch the shared source base and flag it, not inflate the confidence score.

| Problem | Project |
|---|---|
| LLM outputs are probabilistic, unit tests aren't enough | oncology-rag-lab |
| Accessibility violations need context to fix, not just IDs | wcag-auditor |
| Network risk depends on device combination, not individual ports | network-scanner |
| Compliance gaps aren't visible without a device in hand | android-hardener |
| Privacy properties can't be proven by reading the code alone | no-log-rsvp |
| Synthesis agents compound noise unless the methodology gates them | research-synthesis-prompt |

---

## Stack

Python · FastAPI · Playwright · axe-core · LlamaIndex · ChromaDB · DeepEval · Arize Phoenix · Ollama (oncology-rag-lab) · Pydantic · SQLite · nmap · ADB · uv · pytest · GitHub Actions

---

## Contact

[LinkedIn](https://linkedin.com/in/wei-jia)
