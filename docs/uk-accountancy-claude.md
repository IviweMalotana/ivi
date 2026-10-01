# Ivi — AI/Claude Implementation Specialist · UK Outsourced Finance Firm

## Cover letter (paste on Upwork)

You asked for examples of what applicants have built. Here they are, mapped to your four projects.

**1. Internal knowledge hub.** For three years at bOnline I led AI initiatives end to end. I led the team of five that took the internal AI support chatbot to a 60% resolution rate on live customer traffic. I also led the build of the AI receptionist for inbound VOIP calls. Both ran on a structured knowledge base with the AI as the answerer. Your SOPs are the same shape.

**2. AI financial reporting + Xero.** I built Emerald Path, a UK accountancy platform, end to end in 3 months. Xero, Sage, HMRC MTD and Companies House integrations. AI gateway that drafts management commentary from a P&L export with every line tied to a source row. FRS-102 iXBRL engine hitting 100% tag coverage on the Xero demo company. This is your closest reference on my CV.

**3. Automated reporting and board decks.** Emerald Path outputs into structured documents that follow the firm's own template, not a generic one. Finance team reviews before send. Same pattern extends to your PowerPoint or Google Slides packs via python-pptx.

**4. Explore AI opportunities.** At Dixon AI I ran a 10-stage pipeline that turns founder ideas into shipped features. The first three days are always discovery. Applied to your firm: I shadow one partner, one manager, one associate. I come back with 6-10 use cases ranked by hours saved and blast radius.

I mapped all four step-by-step here, with a real `SKILL.md` and a working plan for weeks 1-3:
https://iviwemalotana.github.io/ivi/uk-accountancy-claude.html

**Which to prototype first: #2 (AI + Xero).** You said you're particularly interested. My Emerald Path reference is the closest match. And by end of week 2, a first-draft management commentary lands in a partner's inbox, a real output your finance team can react to.

Rather see the Emerald Path gateway running than read about it? I'll screen share it on a call this week.

Ivi
Cape Town

---

**Prototype #2 first (AI + Xero reporting).** You said you're particularly interested. My closest reference (Emerald Path) already pulls Xero, categorises transactions with grounded AI, and produces reports. By end of week 2 a first-draft management commentary lands in a partner's inbox. Projects 1 and 3 build off #2. Project 4 (discovery) runs in parallel from day 1.

---

**Emerald Path** is the UK accountancy platform I built end to end in three months. It runs a firm's whole practice. Four steps.

### 1. The firm signs up. A tenant auto-bootstraps.

A new firm creates an account. Behind the scenes: a per-firm Postgres schema is created, row-level security switches on, an authorizer starts gating every API call by membership. All traffic is fronted by AWS WAF. Point-in-time restore and CloudTrail are on from minute one. **The firm is isolated from every other firm on the platform, three layers deep.**

`AWS Cognito` · `Aurora Postgres` · `Row-level security` · `AWS WAF` · `SAM / CloudFormation`

### 2. The firm onboards a client.

They enter a company name. **Companies House (via Inform Direct) pulls the company:** directors, share structure, filing history. **SmartSearch runs the AML check** in the background. **Adobe Sign or DocuSign sends the engagement letter** for signature. Billing kicks in on the firm's subscription with a partner revenue-share ledger keeping settlement clean.

`Companies House API` · `Inform Direct` · `SmartSearch AML` · `Adobe Sign` · `DocuSign`

### 3. The client's accounts data flows in.

**Xero, Sage or QuickBooks connect via OAuth** (frontend-callback pattern so keys never touch a shared server). Bank transactions land continuously. **The AI gateway categorises each transaction** against the firm's chart of accounts and proposes journals. Every AI proposal cites the source transaction row. **A partner reviews before anything books.**

`Xero API` · `Sage API` · `QuickBooks API` · `Claude (grounded citations)` · `Python + AWS Lambda`

### 4. Year-end. Statutory accounts, VAT, corporation tax, filed.

The **FRS-102 statutory accounts engine** builds full accounts from the bookkeeping. The **iXBRL tagger reaches 100% coverage on the Xero demo company** (deterministic, chart-of-accounts driven, not AI-guessed). **CT600 corporation tax computes.** **VAT flows through HMRC MTD** (tested against agent, individual and org sandbox users first). **Croner-i** is wired in for tax law reference. Filed to HMRC and Companies House.

`FRS-102 engine` · `iXBRL @ 100%` · `CT600` · `HMRC MTD` · `Companies House filing` · `Croner-i` · `IRIS Elements handoff`

---

**Built with.** Serverless-first. One operator can run the whole thing without an infra team.

| Layer | What |
|---|---|
| **Frontend** | React + TypeScript + Tailwind + Vite. Mobile-responsive design system with tokens, decision tables and pre-commit checks. No-code page builder for firm-branded pages (draft / publish / version history). |
| **Backend** | Python 3.12 on AWS Lambda. REST APIs, webhooks, event-driven pipelines on DynamoDB streams. C# .NET for interop where inherited. |
| **Data** | Aurora PostgreSQL + DynamoDB. Per-tenant Postgres schemas with row-level security. Point-in-time restore on Aurora. DynamoDB pagination corrected across every query/scan site. |
| **Infra** | AWS Serverless. Lambda, Aurora, DynamoDB, Cognito, S3, CloudFront, WAF, CloudTrail, SES. SAM / CloudFormation IaC. eu-west-2. Dev, staging, production, plus satellite dev environments. |
| **AI** | Claude Code, Claude Cowork, OpenAI Codex. LLM gateway with grounded citation-backed generation. Per-phase model routing (Opus plans, Sonnet builds, Haiku runs). Six custom Claude Skills. Nine subagent definitions. |
| **Integrations shipped** | Xero, Sage, QuickBooks, HMRC MTD, Companies House, Inform Direct, IRIS Elements, Croner-i, SmartSearch AML, Adobe Sign, DocuSign, Freshdesk, Sumsub, GitHub OAuth, Cognito. |

---

**Already safe.** You're an outsourced finance firm. Everything below is in Emerald Path today.

- [x] **Three-layer tenant isolation.** Per-firm Postgres schema + row-level security + an authorizer that gates every API call by firm membership. Verified by E2E tests on every PR.
- [x] **AWS WAF fronting the API.** Blocks the obvious classes (SQLi, XSS attempts, bad bots) before they reach the app.
- [x] **Point-in-time restore on Aurora + CloudTrail on the account.** Rollback to any second in the last 35 days. Every action logged with actor + timestamp.
- [x] **Password reset hardened against brute force and code enumeration.** Full BRD written before the code. Public writeup.
- [x] **XSS sanitisation on all user-authored content** (pages, notes, ticket comments). Byte-identity HTML round-trip tests catch regressions in the sanitiser.
- [x] **API Gateway throttled with 429 retry and batching.** Prevents a runaway job from taking down the account.
- [x] **Every AI-generated line cites its source row.** Nothing client-facing sends without a partner tapping OK.
- [x] **Production database backups + tenant-isolation audit** packaged as a reusable Claude Skill so a partner (or another dev) can run them without me.

---

## 1. Internal knowledge hub

> "We have a growing library of SOPs, processes, company values, service standards, templates, onboarding information and internal documentation. We would like to create an effective AI-enabled internal knowledge system that our team can use."

**I've built this. For three years at bOnline I led AI initiatives end to end.** I led the team of five that took the **internal AI support chatbot from concept to a 60% resolution rate** on live customer traffic. I also led the build of the **AI receptionist that handles inbound customer calls** across bOnline's VOIP product. Both run on a structured knowledge base (product docs, pricing, policies, troubleshooting procedures) with the AI as the answerer. **Your SOPs, service standards, templates and onboarding docs are the same shape.** Same pattern, same guardrails, same deflection metric.

1. **Ingest your library.** SOPs, processes, values, service standards, templates, onboarding docs. One shared folder, one index.
2. **Build the chatbot on Claude** with tools scoped to your knowledge base. Every answer cites the SOP paragraph it came from.
3. **Wire it into where your team already works** (Slack, Teams, or a Claude.ai surface). No new app to learn.
4. **Track deflection weekly.** Same metric that got bOnline to 60%. Iterate on the top-10 unanswered questions.

**Time.** 5-7 working days once your library lands in a shared folder.

---

## 2. AI financial reporting + Xero · start here

> "Prototype a reporting solution that combines financial data from Xero with Claude/AI... retrieving structured financial information from Xero; monthly and YTD variance analysis; identifying unusual movements; generating first-draft management commentary; KPI and trend analysis; feeding into management accounts or board reporting packs."

**I've built this.** Emerald Path (the platform walked through above) already pulls Xero via OAuth, categorises transactions with Claude, and produces structured reports for month-end and year-end. **This is your closest reference on my CV.**

1. **Xero OAuth + pull.** P&L, YTD, budget for one pilot client.
2. **Variance analyzer.** Flag any line moving > 15% or > £5,000.
3. **Commentary generator.** One sentence per flagged line, tagged with the source row.
4. **Structured output.** HTML or Markdown block that drops into your management accounts pack.

**Time.** Working prototype for one pilot client by end of week 2.

### `.claude/skills/month-end-commentary/SKILL.md`

```markdown
---
name: month-end-commentary
description: Draft a month-end commentary from a Xero P&L.
             Every claim points back to a source row.
---

# Month-end commentary

You draft a first pass. A partner reads it before it goes out.
Never send it yourself.
Never make up a number.

## What you get

- The month's P&L, exported from Xero.
- The client's engagement letter (tells you their industry).
- The firm's voice guide at /voice.md.

## What you do

1. Read the P&L. For any line that moved > 15% or > £5,000,
   write ONE sentence about what changed. Tag it with the source row.
2. Group the sentences under: Revenue, Gross margin, Overheads.
3. Add an "outlook" line only if a trend runs three months.
   Otherwise write "nothing to flag this month".
4. Keep every source row visible. Partner clicks to check.

## What you never do

- Invent a reason ("customer wins in Q3") unless it's in the notes file.
- Use "significantly" without a number next to it.
- Send it. Hand back to the partner. Leave the ticket in "For review".
```

---

## 3. Automated reporting and board decks

> "The objective is not generic AI-generated PowerPoint slides. We want consistent, professional outputs that follow our reporting methodology and brand, with our finance team reviewing and refining the final output."

**I've built this.** Emerald Path's reporting outputs follow the firm's own template, not a generic one. FRS-102 accounts, iXBRL tags, CT600 tax comps all produced in the firm's format. Same pattern extends to PowerPoint (python-pptx) or Google Slides for board decks.

1. **Fingerprint your template.** Fonts, colours, layouts, section order, house language.
2. **Build the fill Skill.** Takes the variance + commentary from Project 2 and drops them into the right template slots.
3. **Brand check.** Blocks generic AI phrases. Enforces your section headings. Respects your voice guide.
4. **Finance team reviews** in Slides or PPTX before it sends. Nothing autosend.

**Time.** 5 working days after Project 2's commentary works.

---

## 4. Explore other AI opportunities · in parallel from day 1

> "We would also like you to understand how our outsourced finance business operates and help us identify other high-value opportunities for AI and automation... client onboarding, month-end processes, financial review and QA, meeting preparation, action tracking, client queries, internal knowledge, proposals..."

**I've built this.** At Dixon AI I ran a 10-stage pipeline that turns founder ideas into shipped features. The first three days are always discovery: mapping every workflow and ranking by ROI. Same approach applied to your firm.

1. **Three days shadowing.** One partner, one manager, one associate. Daily / weekly / monthly work mapped.
2. **Rank every candidate** by hours saved × frequency × blast radius if wrong.
3. **One page per use case.** What it does, hours saved, risk, order to build.

**Time.** Delivered by end of week 1, before I start the Project 2 build.

---

**How I stop the AI being wrong on client work.** Every line cites a source. Nothing sends without a partner tapping OK.

| Step | What | Why |
|---|---|---|
| **1 · cite** | Every line points to a source. | "Margin down 4pt" must link back to a P&L row. No source, no send. |
| **2 · score** | Confidence check. | Anything the AI is unsure about gets a reason attached. |
| **3 · quarantine** | Held for review. | Not hidden. Partner sees what the AI wanted to say and why it was held. |
| **4 · human** | Partner taps OK. | Accept, reject or rewrite. Every tap is logged. |
| **5 · send** | Send is locked if any line has no source. | Locked on the server, not just the UI. No way around it. |

---

**Weeks 1-3.** What lands, and when.

| Week | Focus | Delivered |
|---|---|---|
| **Week 1** | Discovery + first Skill | 3 days shadowing (partner, manager, associate). ROI map of 6-10 use cases. First Skill wired for a pilot client. |
| **Week 2** | Xero + commentary prototype | Xero OAuth. Variance analyzer. Commentary generator with source-row citations. First draft in a partner's inbox by Friday. |
| **Week 3** | Board deck + handover | Fill Skill for your existing template. Brand check enforcer. Handover doc so the team can add Skills without me. |

---

**Emerald Path (Dixon AI).** UK accountancy platform built end to end in three months. Xero, Sage, QuickBooks, HMRC MTD, Companies House, Inform Direct, Croner-i, SmartSearch AML, Adobe Sign, DocuSign. AI gateway with sourced proposals through 20 rounds of QA. FRS-102 iXBRL engine at 100% tag coverage on the Xero demo company.

**bOnline (3 years, Product Owner).** Led team of five taking an AI chatbot to 60% resolution rate on live customer traffic. Also led the build of the AI receptionist for inbound VOIP calls. Ran payments, billing, fraud, credit control end to end. Scaled billing capacity 7x. Introduced bOnline's first fraud, compliance and KYC frameworks.

**Dixon AI (in parallel with Emerald Path).** Six production Claude Skills. Nine subagent definitions. 10-stage delivery pipeline. 40+ concurrent worktrees at peak. The `.md` files live in the repo. A partner can edit them without me.

**LifeCheq.** Insurance and financial products, full SDLC in a regulated South African financial-services environment.

---

© Ivi Malotana · Cape Town · iivii.malotana@gmail.com
