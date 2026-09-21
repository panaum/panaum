<div align="center">

# Anaum Pandit

**QA Engineer  ·  Test Infrastructure  ·  LLM Evaluation**

I build the systems that decide whether software is actually correct.<br>
Test infrastructure, browser automation, adversarial evaluation of language models,<br>
and the monitoring that catches failures before a client does.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/anaum-p-130a27401/)
[![Email](https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:anaump7@gmail.com)

</div>

---

## What I do

I own end-to-end test strategy for client web applications at Apexure — functional, regression, cross-browser, exploratory — with Playwright and Selenium suites running in CI and defects tracked to closure across design and engineering. Over the past year most of that work has turned into software: the manual QA process is now a monorepo of four apps and three services that the team runs on.

The other half of the job is testing systems that don't behave the same way twice. I run adversarial evaluations against language models — prompt injection, jailbreaking, guardrail validation — and measure whether defences actually hold rather than whether they look reasonable.

| Layer | What I own |
|---|---|
| **Strategy** | Coverage decisions, risk-based prioritisation, what gets automated vs. stays exploratory |
| **Automation** | Playwright and Selenium suites, PyTest, CI gating, regression on every deploy |
| **Tooling** | Building the instrument when none exists — crawlers, render matrices, diff engines |
| **Evaluation** | Adversarial test design, metric selection, benchmarking, defence validation |
| **Operations** | Deployment, log analysis, failure diagnosis, feeding findings back into the suite |

The principle behind all of it: if a failure mode can go undetected for a month, it needs an instrument, not a checklist. Manual QA finds what you remember to look for.

---

## Experience

**QA Analyst · Apexure** — Dec 2025 to present

Own end-to-end test strategy for client web applications. Automated suites integrated into CI/CD through GitHub Actions, defect lifecycle managed in JIRA across design and engineering teams, and performance and load testing with JMeter and k6 to validate API and UI reliability under production-scale traffic.

**QA Intern · Apexure** — Apr to Jul 2025

Executed manual and automated test cases for web applications, documented defects, and tracked resolution with development teams.

**Software Tester Intern · Applied Informatics** — Mar to Apr 2025

Designed structured test cases to validate system behaviour and produced traceability documentation for stakeholders.

---

## What I've built

### 🧪 Apexure QA Ecosystem
**Four apps, three services, one shared contract**

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=next.js&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=flat&logo=prisma&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)

A monorepo that answers one question for every page we ship: is this deliverable correct, and can we prove it to the client? A deliverables dashboard with QA boards and living client certificates, LinkSpy for crawl-based link and attribution monitoring, devicepreview for a fifteen-profile render matrix, and pagecheck for pre-launch verdicts — behind a single sign-in with signed HMAC handoffs between them.

785 commits, ~122,000 lines, 2,020 automated tests. Non-trivial logic lives in pure modules outside React and Prisma — scoring, whitelists, feed ordering, reminder windows — so it can be tested without a database or a clock.

The part I'd want reviewed is how the pieces stay honest, because most of it is enforced rather than agreed. Destructive Prisma commands are banned by a guard that runs as `prebuild`, so the build fails if anyone reintroduces one. The developer-facing view of an issue is a whitelist, so a field added later is hidden by default instead of leaked by default. Only QA can close or reopen a card, which puts the bounce-back metric in the gap between two people's acts — the one number a developer can't inflate. Duplicated contracts across TypeScript and Python carry a checksum of the canonical spec, and the tests fail when the copies drift.

The problem I'm proudest of solving: scanning a client's page was firing that client's analytics, so the measurement was contaminating what it measured. The fix was structural rather than a rule to remember — no module opens a browser of its own, every context comes from one guarded place with collector endpoints already refused, and a test fails the build if a new caller creates one directly.

[→ Repository](https://github.com/panaum/dashboard)

---

### 🛡️ LLM Red-Team Evaluation Suite
**Adversarial testing for language models**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat&logo=sqlite&logoColor=white)
![Postman](https://img.shields.io/badge/Postman-FF6C37?style=flat&logo=postman&logoColor=white)

A harness that fires adversarial prompts at four Groq-hosted models — a static prompt library, PAIR (Chao et al. 2023), and many-shot jailbreaking — and stores every prompt/response pair in SQLite so runs stay comparable. 432 attacks across six categories, mapped to the OWASP LLM Top 10.

The models diverge by roughly an order of magnitude. Role confusion succeeded 85.4% of the time (41/48) and prompt injection 60.7% (68/112), while hallucination held at 0% (0/80) across all four. Of the three defence layers tested, only system-prompt hardening showed a consistent effect — and the undefended baseline swung wider between runs than most of the effects being measured.

The more useful half of the project was auditing my own instrument. Success is judged by embedding similarity against refusal and compliance anchors, which measures the register of a reply rather than its content — and inflates exactly the category that tops the table. Under that lens the escalation result I'd previously reported dissolved: five runs with accumulated attack memory spanned three attacks end to end, indistinguishable from sampling noise. The README now says so, and validating the judge against hand labels is the next thing to do.

[→ Repository](https://github.com/panaum/llm-redteam-suite)

---

### 🎯 Design Sentinel
**Visual QA and design-drift detection**

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![Figma](https://img.shields.io/badge/Figma-F24E1E?style=flat&logo=figma&logoColor=white)

Walks a Figma file node by node against the live URL built from it, reporting drift in typography, colour, spacing and layout. Designs get approved, builds get shipped, and nobody checks whether they match — this catches it at internal review instead of in client feedback.

Eight checks in one scan: screenshot diff, visual drift, spell check, SEO, PageSpeed, accessibility, tech-stack detection. In active use on client projects.

[→ Repository](https://github.com/panaum/diffcheck)

---

### 🤖 Agentic OS
**Modular multi-agent system**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat&logo=mongodb&logoColor=white)

A planner agent that decomposes spoken instructions into subtasks and routes them to specialised agents for mail, browser control and booking — intent recognition on spaCy and RapidFuzz, persistent memory in JSONL and MongoDB, voice I/O through Whisper and pyttsx3. Built to find where multi-agent coordination actually breaks: ambiguous intent, subtask ordering, and recovery when one agent fails mid-plan.

---

## Running it in production

```
write test → CI run → deploy → scan → triage → file defect → verify fix → repeat
```

I deploy and operate these tools myself, which is what taught me how test infrastructure really behaves: which checks go stale, which alerts get ignored after a week, and why a suite that passes is not the same thing as a system that works. Two of the standards above exist because DB and DOM assertions were green while the rendered page was broken.

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

Cross-browser, visual regression, accessibility and exploratory testing · defect lifecycle management in JIRA · CI/CD gating through GitHub Actions

**LLM & AI quality**

![DeepEval](https://img.shields.io/badge/DeepEval-6E56CF?style=flat)
![Ragas](https://img.shields.io/badge/Ragas-FF6B6B?style=flat)
![PromptFoo](https://img.shields.io/badge/PromptFoo-4B32C3?style=flat)
![TruLens](https://img.shields.io/badge/TruLens-1B9AAA?style=flat)
![Arize](https://img.shields.io/badge/Arize-2D3748?style=flat)

Red teaming and adversarial input · prompt injection and jailbreak testing · guardrail validation · hallucination, bias and toxicity detection · retrieval precision and recall for RAG · agent task-completion and multi-step reasoning validation · BLEU / ROUGE / BERTScore

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
![JIRA](https://img.shields.io/badge/JIRA-0052CC?style=flat&logo=jira&logoColor=white)

---

## Education

**B.Tech, Computer Science** — Government College of Engineering & Technology (GCET), Kashmir

CGPA 7.74 / 10

---

## Certifications

- **Evaluating & Debugging Generative AI** — DeepLearning.AI
- **LLM Evaluation with Ragas & DeepEval** — DataTalks.Club
- **Python for Test Automation** — Test Automation University, Applitools
- **Prompt Engineering for Developers** — DeepLearning.AI

---

## Currently learning

Going deeper on evaluation rather than collecting more frameworks: RAG retrieval precision and how you score a pipeline whose failures are spread across components; Model Context Protocol and what testing tool-using models properly would involve; and enough system design vocabulary to describe failure modes I've been debugging by instinct.

---

## Where I'm headed

The question I keep running into is whether our evaluation instruments measure what they claim to. My red-team suite reports attack success rates from a judge that scores the tone of a reply rather than its content — so the headline number is an upper bound, not an estimate. My device-render matrix stops at a warning when the evidence is ambiguous rather than guessing a verdict. Both are the same problem: a test that is confidently wrong is worse than no test.

The next thing I'm building is the validation study for that judge — hand-labelled samples, agreement measured per category, and a correction factor for every rate I've published. I'd like to keep working on this class of problem, and I'm heading toward graduate study in Europe to do it properly.

**Open to** conversations about AI safety and LLM evaluation, test infrastructure and SDET work, and research collaborations — whether that's a role, a thesis position, or someone who wants a second pair of eyes on an evaluation pipeline.

If you work on LLM evaluation, adversarial robustness, or test infrastructure, I'd like to hear from you. Reach me at [anaump7@gmail.com](mailto:anaump7@gmail.com).

---

<div align="center">

QA Engineer at [Apexure](https://www.apexure.com) · Srinagar, India<br>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/anaum-p-130a27401/)
[![Email](https://img.shields.io/badge/Email-EA4335?style=flat&logo=gmail&logoColor=white)](mailto:anaump7@gmail.com)

</div>
