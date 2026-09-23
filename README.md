<div align="center">

# Anaum Pandit

**QA Engineer  ·  Test Infrastructure  ·  LLM Evaluation**

I build the systems that decide whether software is actually correct.<br>
Browser automation, adversarial evaluation of language models, and the monitoring<br>
that catches failures nobody would otherwise notice until a client calls.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/anaum-p-130a27401/)
[![Email](https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:anaump7@gmail.com)

</div>

---

## What I do

I own end-to-end test strategy for client web applications at Apexure — functional, regression, cross-browser, exploratory — with Playwright and Selenium suites gating CI and defects tracked to closure across design and engineering. Over the past year most of that work turned into software: the manual QA process is now a monorepo of four apps and three services that the team runs on.

The other half is testing systems that don't behave the same way twice. I run adversarial evaluations against language models — prompt injection, jailbreaking, guardrail validation — and measure whether defences hold rather than whether they look reasonable.

| Layer | What I own |
|---|---|
| **Strategy** | Coverage decisions, risk-based prioritisation, what gets automated vs. stays exploratory |
| **Automation** | Playwright and Selenium suites, PyTest, CI gating, regression on every deploy |
| **Tooling** | Building the instrument when none exists — crawlers, render matrices, diff engines |
| **Evaluation** | Adversarial test design, metric selection, benchmarking, defence validation |
| **Operations** | Deployment, log analysis, failure diagnosis, feeding findings back into the suite |

The principle behind all of it: if a failure mode can go undetected for a month, it needs an instrument, not a checklist. Manual QA finds what you remember to look for.

---

## What I've built

### 🧪 QA Dashboard and Ecosystem
**Four apps, three services, one shared contract**

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=next.js&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=flat&logo=prisma&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)

A monorepo answering one question for every page the agency ships: is this deliverable correct, and can we prove it to the client? A deliverables dashboard with per-user logins, ranks and capabilities; Trello-style QA boards; cross-device layout checks; AI-assisted page audits; and client-facing certificates that re-verify themselves. Behind it sit three services reached through one sign-in with signed handoffs. **811 commits, ~162,000 lines, 2,151 automated tests.**

- **Insights — measuring people without lying about them.** Defect counts look like a measure of the developer; they're a measure of the developer *and* whoever reviewed the page. Tester rates are computed relative to the mean and developer numbers adjusted for who reviewed them, gated on how many pages a reviewer has actually seen. Below five pages a cell renders as *withheld* — hatched, with its n visible — never blank, which reads as "did no work", and never zero, which reads as "perfect". A metric with no field behind it renders as *blocked*, with a sentence saying why. And the module refuses to filter on issue status at all, because in that database status encodes which import epoch a row came from rather than a lifecycle: a metric filtering on it would be measuring the calendar.

- **Enforced constraints, not team convention.** Destructive database commands fail the build, via a guard wired into `prebuild` that assembles the banned strings from fragments so it can never trip on itself. Developer-facing views expose fields by whitelist, so a column added later is hidden by default rather than leaked by default. Duplicated wire contracts across TypeScript and Python carry a checksum of the canonical spec and the tests fail when the copies drift. An API route that doesn't declare how it's authenticated fails the suite — an unlisted route is an unguarded route — and the test asserts the guard runs *before* any database or key access, not merely that it exists.

- **Cross-device rendering.** Fifteen device profiles across all three browser engines, with twelve geometry rules evaluated inside the page and each capture drawn in a per-handset 3D model with findings pinned where they occur. Its house rule is that it never returns a false FAIL: where evidence is ambiguous it warns and stops at the honest label. Twelve of the fifteen profiles are spec-derived rather than hardware-verified, and the report footer says so every time.

- **AI QA.** A deterministic layer — SEO tags, Open Graph, H1, analytics presence, page weight, sitemap — paired with a judgment layer that drafts issues carrying a suggested fix and a severity ranking for a human to accept. The boundary is enforced by a test rather than by agreement: machine writes never land unattended in the human checklist tables.

- **Boards and the developer portal.** Five stages built on the existing issue table rather than a parallel model, so ~2,300 historical issues stayed untouched through the migration. A developer reaches exactly one board through a capability link — no login, no navbar, no route into the rest of the app. Every board metric is derived from an append-only event log rather than current state, and shown beside the delivery view rather than blended into a composite score.

- **Client-facing certificates.** Published by capability token, minted behind a permission and nulled to revoke. They re-verify against LinkSpy and show a health timeline rather than freezing a PDF at hand-off. Because anyone holding the link reaches it, an isolation test encodes seven invariants on that path — read-only, page-scoped, no raw payload, no signed handoff links, no second token minted, flag checked before the database read.

The bug I'm proudest of fixing: scanning a client's page was firing that client's analytics — a pixel's `<noscript>` fallback fetched on every scan, writing a PageView into their own reporting. The first fix was a guard at the call site. The real fix was removing the need to remember: no module opens a browser context of its own, every one comes from a single guarded place with collector endpoints pre-refused, and a test fails the build if a new caller creates one directly. Opting out takes a written sentence, not a boolean.

[→ Repository](https://github.com/panaum/dashboard)

---

### 🔗 LinkSpy
**What a client's site is really doing**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=next.js&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white)

A marketing site can be completely broken and still return 200 on every page. The demo button has no destination. The contact form posts to a deleted endpoint. The pixel is loaded twice so conversions are double-counted. An ad points at a page that 404s and the spend keeps going out. None of that errors, and nobody finds out until the leads stop. **~52,000 lines, 113 routes, 973 backend tests.**

Beyond link checking it catches dead CTAs separated from JavaScript UI that merely looks inert, cross-page `#fragment` targets that HTTP can never see, forms audited across every iframe without ever being submitted, tracking and UTM integrity across redirects, consent-banner behaviour under reject and GPC, and shared third-party outages correlated across every client at once — so one dead Calendly raises one alert naming everyone affected instead of eight separate mysteries.

- **Form audit — almost no status code can prove a form is broken.** The naive rule flags nearly every form on the internet, because real endpoints answer 405 to a GET. That mistake shipped three times — 405, then 500, then 404 — each time fixed by special-casing one more status, which guaranteed a fourth. So the rule is inverted: exactly two things prove a form posts nowhere, a host that won't resolve and a 410 Gone. A property test sweeps every status from 100 to 599 and fails the build if anything else reaches a red bucket. The audit never submits — there is no `.submit()`, no `.click()` and no synthetic event anywhere in the collection script.

- **Self-heal — opens pull requests against real repositories, and never merges.** Human-triggered only, one page at a time. Three rails are checked before any work happens, including before the scan. Only two fix classes, both provable: a permanent redirect chain proves old-to-new, and only 301 or 308 counts because a 302 must never be baked into source; and mixed content where the `https://` equivalent verifies. Every target is re-checked live seconds before the PR opens, through the same checker a scan uses. It refuses to touch CI config, Dockerfiles, dependency manifests or anything executable, and an empty path returns blacklisted — it fails closed. The prohibition on merging is structural: three tests read the module source and fail the build if a merge call ever appears.

- **Lead tracer — proof that a lead actually arrives.** Submits a flagged test lead, verifies field-by-field arrival in HubSpot or GoHighLevel, deletes the test contact, and writes an immutable ledger row for every branch. Enrollment requires a typed acknowledgment; the first run is forced to be a dry run; a failed cleanup is a loud outcome naming the contact so a human can remove it. CRM credentials are encrypted at rest with a key derived from an existing secret, and anything token-shaped is redacted before it can reach a log.

- **Consent engine — records behaviour, never declares compliance.** Loads a page in five modes — cold, reject, accept, GPC, opt-out — and records every third-party request with its consent class and timing. An unknown CMP is never guessed at, and a banner present but not operable is recorded as a *declared limitation*, which is materially different from "no banner". The quarterly attestation carries a coverage-honesty block naming what was not checked, a content hash, and its engine and classification versions.

- **Ads waste guard and inbound-404 triage.** Import a Google Ads final-URL export and every destination is verified daily; a live ad pointing at a provably dead page alerts immediately. Spend at risk is computed only from your own imported cost figures and always labelled. Search Console errors and server logs re-rank dead URLs by measured demand, with bot demand never counted as a real visitor.

- **pagecheck — published on PyPI.** The single-page checker is open source and installable by anyone: `pip install pagecheck`. Point it at a URL and get a pre-launch verdict, no account and no key. It is read-only — it loads the page with test attribution parameters attached and reads the forms back, and never submits one. [→ PyPI](https://pypi.org/project/pagecheck/) · [→ Repository](https://github.com/panaum/pagecheck)

- **Third-party watchdog.** Inventories external hosts across every client, and when a shared host fails across sites it raises one alert naming everyone affected rather than eight separate mysteries. The failure is demoted *before* the diff and the health score, so a dead Calendly never reddens a client's report while the outage is still reported where it belongs.

Everywhere: **what can't be proven broken is reported as unverifiable, never red.** For a client-facing tool a false alarm costs more than a soft warning.

---

### 🛡️ LLM Red-Team Evaluation Suite
**Adversarial testing for language models**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat&logo=sqlite&logoColor=white)

A harness that fires adversarial prompts at four Groq-hosted models and stores every prompt and response in SQLite, so runs stay comparable across models, defence configurations and time. **432 attacks across six categories, mapped to the OWASP LLM Top 10.**

- **Three attack families.** A static prompt library covering prompt injection, role confusion, jailbreak, PII leakage, hallucination and bias elicitation. PAIR (Chao et al. 2023), where an attacker model writes a prompt, reads the target's reply and the scorer's similarity values, then rewrites — iterating rather than replaying. And many-shot jailbreaking, which builds an escalating block of normalised exchanges before the real ask. Success rates by family: static 41.6% (133/320), PAIR 14.6% (14/96), many-shot 6.3% (1/16).

- **A memory-backed loop.** Successful bypasses are persisted and fed back to the attacker as context on later runs, so it generates harder variants rather than repeating a fixed list.

- **Results by category.** Role confusion 85.4% (41/48), prompt injection 60.7% (68/112), jailbreak 22.3% (25/112), PII leakage 20.8% (10/48), bias elicitation 12.5% (4/32), hallucination 0% (0/80). Hallucination holding at zero across all four models is the cleanest result in the set — and the one place the scorer's bias works in the honest direction, since a confident fabrication reads as compliance and gets caught.

- **The models are not close.** llama-3.3-70b-versatile 40.7% (22/54) and llama-3.1-8b-instant 39.4% (117/297) were the most susceptible by a wide margin; qwen3-32b sat at 14.8% (8/54); openai/gpt-oss-120b refused nearly everything at 3.7% (1/27). A single pooled figure across all four would describe no model in particular.

- **Three defence layers, evaluated separately and stacked.** A regex input filter, a six-rule hardened system prompt, and a Detoxify output classifier. Only system-prompt hardening showed a consistent effect — and the undefended baseline swung wider between runs, 5% to 25% on the same twenty prompts, than most of the effects being measured. The output classifier never blocked a single response, which is the expected result once you notice that a model adopting a persona in a polite tone produces nothing a toxicity classifier has reason to flag.

- **Auditing my own instrument, which was the more useful half.** Success is judged by embedding similarity against five refusal and five compliance anchors — so it measures the *register* of a reply rather than its content, and inflates exactly the category that tops the table. A "successful" role-confusion attack in the stored responses turns out to be a model introducing itself in character and doing nothing else. Under that lens an escalation result I'd previously reported dissolved: five runs with accumulated attack memory spanned three attacks end to end, indistinguishable from sampling noise. The README now states the limitation, and validating the judge against hand-labelled samples — confusion matrix and per-category false positive rate — is the next piece of work.

[→ Repository](https://github.com/panaum/llm-redteam-suite)

---

### 🎯 Design Sentinel
**Visual QA and design-drift detection**

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![Figma](https://img.shields.io/badge/Figma-F24E1E?style=flat&logo=figma&logoColor=white)

Designs get approved, builds get shipped, and nobody checks whether they match. Design Sentinel compares the two and reports what drifted — then runs the rest of the pre-launch checks in the same pass, so one scan replaces what used to be several manual ones. Deployed and in use on client projects.

- **Two independent comparison engines.** *Figma vs Web* fetches the file through Figma's REST API and diffs it section by section — grouped text comparison plus image comparison matched on layer name and dimensions. *Design Sentinel* takes the other route: a Figma plugin walks the selected nodes in the editor, collecting characters, font family, size, weight and colour fills, and posts them to the server, which scrapes the live page with Playwright and diffs node by node. The REST path sees structure; the plugin path sees exactly what the designer selected.

- **A third source: local HTML.** The same comparison runs against an uploaded HTML file instead of a Figma frame, which covers the case where the spec is a built page rather than a design.

- **Screenshot diff.** Playwright capture against the Figma frame export, compared with pixelmatch.

- **Spell check, SEO and tech stack.** Copy checked across the rendered page; meta, Open Graph, Twitter, canonical, H1 and robots directives extracted; and the stack fingerprinted from script sources, inline markup and response headers.

- **PageSpeed and accessibility.** Performance pulled through Google's PageSpeed Insights, and an axe-core pass for contrast, image alternatives and text sizing.

- **Server logs in the UI.** Uncaught exceptions and unhandled rejections are captured and surfaced in their own tab rather than living only in a terminal — so a failed scan can be diagnosed from the same screen it failed on.

Auditing it end to end turned up more dead paths than I expected — a heatmap generator called in production that was never defined, so every screenshot diff was silently taking the plain pixel-diff fallback. That audit is its own argument for auditing your own tools.

---

## Running it in production

```
write test → CI run → deploy → scan → triage → file defect → verify fix → repeat
```

I deploy and operate these tools myself, which is what taught me how test infrastructure really behaves: which checks go stale, which alerts get ignored after a week, and why a suite that passes is not the same as a system that works. Two of the standards above exist because database and DOM assertions were green while the rendered page was broken.

---

## Stack

**Web & UI testing**

![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![Selenium](https://img.shields.io/badge/Selenium-43B02A?style=flat&logo=selenium&logoColor=white)
![Robot Framework](https://img.shields.io/badge/Robot_Framework-000000?style=flat&logo=robotframework&logoColor=white)
![PyTest](https://img.shields.io/badge/PyTest-0A9EDC?style=flat&logo=pytest&logoColor=white)
![Postman](https://img.shields.io/badge/Postman-FF6C37?style=flat&logo=postman&logoColor=white)
![JMeter](https://img.shields.io/badge/JMeter-D22128?style=flat&logo=apachejmeter&logoColor=white)
![k6](https://img.shields.io/badge/k6-7D64FF?style=flat&logo=k6&logoColor=white)

Cross-browser, visual regression, accessibility and exploratory testing · defect lifecycle in JIRA · CI/CD gating through GitHub Actions

**LLM & AI quality**

![DeepEval](https://img.shields.io/badge/DeepEval-6E56CF?style=flat)
![Ragas](https://img.shields.io/badge/Ragas-FF6B6B?style=flat)
![PromptFoo](https://img.shields.io/badge/PromptFoo-4B32C3?style=flat)
![TruLens](https://img.shields.io/badge/TruLens-1B9AAA?style=flat)
![Arize](https://img.shields.io/badge/Arize-2D3748?style=flat)

Red teaming and adversarial input · prompt injection and jailbreak testing · guardrail validation · hallucination, bias and toxicity detection · retrieval precision and recall for RAG · agent task-completion validation

**Languages**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=openjdk&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=postgresql&logoColor=white)

**Build & infrastructure**

![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=next.js&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=flat&logo=prisma&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat&logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=githubactions&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat&logo=vercel&logoColor=white)
![Railway](https://img.shields.io/badge/Railway-0B0D0E?style=flat&logo=railway&logoColor=white)

---

## Education & certifications

**B.Tech, Computer Science** — Government College of Engineering & Technology, Kashmir

CGPA 7.74 / 10 · Nov 2021 – Dec 2025

**AI Fluency: Framework & Foundations** · **Introduction to Agent Skills** — Anthropic, 2026

---

## Where I'm headed

The question I keep running into is whether our evaluation instruments measure what they claim to. My red-team suite reports attack success rates from a judge that scores the tone of a reply rather than its content, so the headline number is an upper bound rather than an estimate. My render matrix stops at a warning when the evidence is ambiguous rather than guessing a verdict. The consent engine records what fired and when, and refuses to call it compliant. They're all the same problem: a test that is confidently wrong is worse than no test.

The next thing I'm building is the validation study for that judge — hand-labelled samples, agreement measured per category, and a correction factor for every rate I've published. I'd like to keep working on this class of problem, and I'm heading toward graduate study in Europe to do it properly.

**Open to** conversations about AI safety and LLM evaluation, test infrastructure and SDET work, and research collaborations — whether that's a role, a thesis position, or someone who wants a second pair of eyes on an evaluation pipeline.

If you work on LLM evaluation, adversarial robustness, or test infrastructure, I'd like to hear from you. Reach me at [anaump7@gmail.com](mailto:anaump7@gmail.com).

---

<div align="center">

QA Engineer at [Apexure](https://www.apexure.com) · Srinagar, India<br>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/anaum-p-130a27401/)
[![Email](https://img.shields.io/badge/Email-EA4335?style=flat&logo=gmail&logoColor=white)](mailto:anaump7@gmail.com)

</div>
