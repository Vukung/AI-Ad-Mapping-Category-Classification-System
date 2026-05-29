# Ad Creative Intelligence - Automated Classification System

> *From hundreds of unstructured ad links to a fully-mapped, analytics-ready dataset - automatically.*

---
https://github.com/user-attachments/assets/9c1d9fe9-484f-4feb-a64a-7a0dde463a90

## Background

At **Aristok Technologies**, ad performance analysis was bottlenecked by a fundamental data problem: thousands of ad creatives across Google and Meta campaigns had no structured metadata. Analysts had to manually visit each ad link, read the copy, and hand-enter attributes - influencer, format, product category, theme, season, offer type, and more - into a spreadsheet. Then pivot tables. Then analysis.

It was slow, inconsistent, and didn't scale.

This system eliminates that entirely.

---

## What It Does

The pipeline ingests raw ad metadata (primary text, secondary text, description, URL) provided by the client and **automatically classifies each creative into structured attributes** using LLM inference - no human tagging required.

```
Client Ad Data (Links + Copy) → n8n Pipeline → Groq LLM Classification → Structured Dataset → Looker Dashboard
```

**Classified Attributes per Ad:**

| Attribute | Example |
|-----------|---------|
| Product | "Running Shoes - Air Max Series" |
| Category | Footwear |
| Format | Video Ad |
| Theme | Performance / Sport |
| Season | Summer |
| Offer | 20% Off |
| Influencer | Yes / No |
| ... | ... |

---

## System Architecture

| Component | Tool | Role |
|-----------|------|------|
| Workflow Automation | n8n | Orchestrates the full pipeline |
| LLM Inference | Groq (LLaMA-3.1-8B) | Classifies ad attributes from copy |
| Data Source | Google Sheets | Raw ad metadata input |
| Storage | Google Sheets | Structured output dataset |
| Visualization | Looker Studio | Campaign analytics dashboards |

---

## Pipeline

<img width="1920" height="1080" alt="Workflow" src="https://github.com/user-attachments/assets/8e05822c-d85d-4cb8-9b32-f65263d36061" />

1. **Trigger** - Manual or scheduled workflow run in n8n
2. **Ingest** - Fetch raw ad rows from Google Sheets (primary text, secondary text, description, link text)
3. **Loop** - Iterate through each ad creative
4. **Classify** - Send ad copy to Groq API with structured prompt
5. **Parse** - Extract and validate LLM output against schema
6. **Write** - Update Google Sheets row with classified attributes + confidence scores
7. **Visualize** - Looker Studio dashboard reflects updated dataset

---

## Prompt Engineering & Schema Enforcement

The LLM is prompted to return a strict JSON schema for every ad:

```json
{
  "product": "string",
  "category": "string",
  "format": "video | image | carousel",
  "theme": "string",
  "season": "summer | winter | festive | evergreen",
  "offer_present": true,
  "offer_type": "discount | free_shipping | bundle | none",
  "influencer_ad": false,
  "confidence": 0.88
}
```

Post-processing logic validates outputs, handles edge cases, and retries on malformed responses - ensuring the dataset stays clean.

---

## Rate Limiting & Reliability

Groq API limit: `30 requests / minute`

**Implemented solution:**
- Loop node with controlled batch sizing
- Wait node (3s delay between batches)
- Retry logic on HTTP 429 / timeout errors
- Fallback prompt on low-confidence outputs

**Effective throughput:** ~20 classifications/minute with zero manual intervention.

---

## Impact

| Before | After |
|--------|-------|
| Manual attribute tagging per ad | Fully automated classification |
| Hours of analyst time per campaign cycle | Pipeline runs in minutes |
| Inconsistent, human-error-prone data | Standardized schema across all campaigns |
| Analysis blocked until tagging complete | Real-time dashboard updates |

> **~80% reduction in manual tagging effort.** Enabled the team to run cross-campaign creative analysis that was previously not feasible at scale.

---

## Dashboard Insights

Looker Studio dashboards built on the structured output enable:

- **Creative performance by category** - which product types drive the most engagement
- **Format analysis** - video vs. image vs. carousel performance trends
- **Offer impact** - conversion lift from discount/bundle creatives
- **Seasonal trends** - campaign timing vs. creative theme alignment
- **Platform breakdown** - Google Ads vs. Meta Ads creative strategy comparison

---

## Scalability Path

| Current | Production-Ready Upgrade |
|---------|--------------------------|
| Google Sheets | PostgreSQL / BigQuery |
| Manual trigger | Scheduled / webhook-based ingestion |
| Groq (primary) | Groq + OpenAI fallback |
| LLM-only classification | Hybrid: rules engine + LLM |
| Single pipeline | Parallel batch processing |

---

## Tech Stack

`n8n` · `Groq API` · `LLaMA-3.1-8B` · `Google Sheets` · `Looker Studio` · `JavaScript`

---

## Context

Built during a Business Analyst internship at to solve a real operational bottleneck in ad campaign analysis. The system moved the team from reactive, manual data work to proactive, automated insight generation.
