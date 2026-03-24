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
