# Marketing AI Agency – n8n Automation Workflow

This repository contains a full **n8n automation workflow** (`marketing-ai-workflow.json`) where every marketing role is mapped to an **AI Agent** powered by GPT-4o.

---

## 🗺️ Workflow Overview

```
graph LR

A([Client Brief + Market Data + Goals])
  --> B[🧭 STRATEGIST Agent]
  --> C[🏷️ BRAND MANAGER Agent]
  --> D[🎨 CREATIVE DIRECTOR Agent]
  --> E[📣 DIGITAL MARKETING MANAGER Agent]
  --> F[🔁 Feedback Hub]
  --> G[📋 CONTENT MARKETING MANAGER Agent]
  --> H[✍️ CONTENT WRITER & COPYWRITER Agent]

H --> I[🖼️ VISUAL DESIGNER Agent]
H --> J[🎬 VIDEO MARKETING EXECUTIVE Agent]

I & J --> K[🔀 Merge]
  --> L[🖥️ MARKETING DESIGNER Agent]
  --> M[🧩 UI & UX DESIGNER Agent]
  --> N[🚀 Distribution via Digital Marketing Manager]
  --> O[📊 Performance Data Feedback Loop]
  --> P[🎯 DESIGN DIRECTOR Agent]

P --> Q[🖥️ MARKETING DESIGNER Agent – Design Track]
P --> R[🧩 UI & UX DESIGNER Agent – Design Track]

Q & R --> S[🔀 Merge Final Assets]
  --> T[✅ DESIGN DIRECTOR Final Review]
  --> U[🚀 Approved for Distribution]
```

---

## 🤖 AI Agent Roles

| Node | Role | Output |
|------|------|--------|
| 🧭 STRATEGIST | Analyses brief, market data & goals | Strategy Document |
| 🏷️ BRAND MANAGER | Defines brand positioning & voice | Brand Brief |
| 🎨 CREATIVE DIRECTOR | Develops concepts, runs brand review | Approved Assets + Content Briefs |
| 📣 DIGITAL MARKETING MANAGER | Plans & runs campaigns | Campaign Plan + Performance Reports |
| 🔁 Feedback Hub | Routes performance & creative data | Feedback signals |
| 📋 CONTENT MARKETING MANAGER | Plans content strategy | Editorial Calendar + Content Briefs |
| ✍️ CONTENT WRITER & COPYWRITER | Produces written content | Articles, Copy, Scripts, CTAs |
| 🖼️ VISUAL DESIGNER | Designs infographics & illustrations | Visual asset specs |
| 🎬 VIDEO MARKETING EXECUTIVE | Produces video briefs | Social videos, explainers, brand films |
| 🖥️ MARKETING DESIGNER | Creates campaign assets | Ads, emails, landing page layouts |
| 🧩 UI & UX DESIGNER | Designs flows & pages | Wireframes, conversion-optimised pages |
| 🎯 DESIGN DIRECTOR | Directs design quality | Design direction brief |
| 🖥️ MARKETING DESIGNER (Design Track) | Final campaign asset production | Campaign asset pack |
| 🧩 UI & UX DESIGNER (Design Track) | Final page & flow production | Developer-ready pages |
| ✅ DESIGN DIRECTOR Final Review | Final quality gate | Approval / Revision |

---

## 🚀 How to Import into n8n

1. Open your **n8n** instance
2. Go to **Workflows → Import from File**
3. Select `marketing-ai-workflow.json`
4. Add your **OpenAI API credentials** to the `🤖 OpenAI GPT-4o` node
5. Activate the workflow and trigger it with a manual run
6. Fill in your `clientBrief`, `marketData`, and `goals` in the trigger node

---

## ⚙️ Requirements

- n8n v1.x or later
- OpenAI API key (GPT-4o access)
- `@n8n/n8n-nodes-langchain` package (included by default in n8n v1+)

---

## 🤖 Agent 04 – Digital Marketing Manager (Standalone)

`agent-04-digital-marketing-manager.json` is the **detailed standalone workflow** for the Digital Marketing Manager agent — the central routing orchestrator of the full agency system.

### Dual-mode operation

| Mode | Trigger | What it does |
|------|---------|-------------|
| **LAUNCH** | `POST /webhook/approved-assets` | Receives approved creative assets from upstream agents, builds channel execution plan, routes to distribution agents |
| **OPTIMIZE** | Schedule Trigger (daily 07:00) | Pulls data from all channels, generates optimization signals, sends upstream to Creative Director / Content Manager |

### Workflow topology

```
[LAUNCH path]
Webhook (approved assets)
  → Set LAUNCH Context
  → Merge Triggers
  → GA4 + Google Ads + Meta Ads + HubSpot + Airtable (parallel)
  → Merge All Performance Data
  → 🤖 AI Agent (GPT-4o) – Digital Marketing Manager
  → Parse Output
  → Router LAUNCH vs OPTIMIZE
      → [LAUNCH] Router – Channel Distribution
          → Launch PPC / Email / Social / Google+SEO / Video (parallel)
          → Merge Launch Results
          → Google Sheets Log + Slack Launch Notification
          → Webhook Response

[OPTIMIZE path]
Schedule Trigger (daily 07:00)
  → Set OPTIMIZE Context
  → [same data collection path]
  → 🤖 AI Agent (GPT-4o) – Digital Marketing Manager
  → Parse Output
  → Router LAUNCH vs OPTIMIZE
      → [OPTIMIZE] Router – Optimization Signals
          → Signal → Creative Director
          → Signal → Content Manager
          → Signal → PPC Agent
          → Signal → Email Agent
          → Signal → Social Agent
          → Slack – Creative Refresh Alert (if ROAS below threshold)
          → Google Sheets Performance Report
          → Slack – Daily Optimization Report
```

### n8n nodes used (34 total)

`AI Agent` · `Router (Switch)` · `Merge` · `Schedule Trigger` · `Webhook` · `HTTP Request` · `Google Sheets` · `Slack` · `Code` · `Set` · `Respond to Webhook` · `OpenAI GPT-4o (LLM)`

### Integrations

`Google Analytics 4` · `Google Ads API` · `Meta Ads API` · `HubSpot` · `Airtable` · `Google Sheets` · `Slack` · `OpenAI GPT-4o`

### Environment variables required

| Variable | Description |
|----------|-------------|
| `GA4_PROPERTY_ID` | Google Analytics 4 property ID |
| `GOOGLE_ADS_CUSTOMER_ID` | Google Ads customer account ID |
| `GOOGLE_ADS_DEVELOPER_TOKEN` | Google Ads API developer token |
| `META_ADS_ACCOUNT_ID` | Meta (Facebook) ad account ID |
| `AIRTABLE_BASE_ID` | Airtable base ID for campaign performance |
| `GOOGLE_SHEETS_SPREADSHEET_ID` | Google Sheets file ID for logging |
| `SLACK_CHANNEL_CAMPAIGNS` | Slack channel ID for campaign notifications |
| `SLACK_CHANNEL_DESIGN_DIRECTOR` | Slack channel ID for Design Director alerts |

### Credentials required in n8n

`googleAnalyticsOAuth2` · `googleAdsOAuth2Api` · `facebookGraphApi` · `hubspotPrivateAppApi` · `airtableTokenApi` · `googleSheetsOAuth2Api` · `slackApi` · `openAiApi`

### Behavioral rules (enforced in system prompt)

- Every optimization action **must cite a specific data point** — no opinions
- **Never end a test** before `minimum_sample` is reached
- If **ROAS drops below threshold**: set `creative_refresh_required: true` and fire Slack alert to Design Director
- All outputs are **strictly typed JSON** matching the LAUNCH or OPTIMIZE schema
