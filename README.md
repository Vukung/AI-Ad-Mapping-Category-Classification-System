# AI Customer Support Automation

https://github.com/user-attachments/assets/9c1d9fe9-484f-4feb-a64a-7a0dde463a90


AI workflow that automatically categorizes customer support queries using an LLM and generates operational insights through analytics dashboards.

This project demonstrates how LLMs can be integrated into automation pipelines to analyze customer support data and identify operational bottlenecks.

---

# Project Overview

Customer support teams receive queries from multiple platforms such as Instagram, WhatsApp, and Email. Manually categorizing these queries is time-consuming and prevents teams from quickly identifying systemic issues.

This system automates the process by:

1. Collecting customer queries
2. Classifying them using an LLM
3. Storing structured data
4. Visualizing insights through dashboards

The goal is to demonstrate **AI workflow automation and operational analytics**, not to build a production system.

---

# System Architecture

Components used:

| Component | Tool | Purpose |
|----------|------|------|
| Automation | n8n | Orchestrates workflow |
| LLM | Groq (LLaMA-3.1-8B) | Classifies customer queries |
| Storage | Google Sheets | Stores raw and classified data |
| Visualization | Looker Studio | Generates analytics dashboards |

Pipeline:

```
Google Sheets → n8n Workflow → Groq LLM Classification → Structured Dataset → Looker Dashboard
```
<img width="1920" height="1080" alt="Workflow" src="https://github.com/user-attachments/assets/8e05822c-d85d-4cb8-9b32-f65263d36061" />

---

# Workflow Pipeline

1. Manual workflow trigger in n8n
2. Fetch rows from Google Sheets dataset
3. Loop through customer queries
4. Send query text to Groq API
5. LLM classifies query into predefined categories
6. Store category and confidence score
7. Update dataset with structured output

---

# Dataset Structure

| Column | Description |
|------|------|
| Timestamp | Time of query |
| Platform | Instagram / WhatsApp / Email |
| customer_message | Raw support message |
| category | Classified issue type |
| confidence | LLM confidence score |

Example:

```
2026-03-13 | Instagram | "Where is my order?" | Order Status | 0.91
```
<img width="747" height="949" alt="Dataset - After1" src="https://github.com/user-attachments/assets/6169097b-5a79-48e6-812c-0ad155a2aa8b" />

---

# LLM Classification

The Groq LLaMA-3.1 model classifies queries into one of seven support categories:

- Order Status
- Delivery Delay
- Refund Request
- Product Complaint
- Subscription Issue
- Payment Failure
- General Question

Example Output:

```json
{
 "category": "Order Status",
 "confidence": 0.91
}
```

---

# Rate Limiting Strategy

Groq API limit:

```
30 requests / minute
```

Implemented solution:

- Loop node
<img width="1919" height="926" alt="Loop Node" src="https://github.com/user-attachments/assets/288c1bb5-f6bc-45dc-bfb9-b654677539f4" />
- Wait node (3 seconds delay)
<img width="1919" height="942" alt="Wait Node" src="https://github.com/user-attachments/assets/0313394d-58b5-4803-b625-9ef5dfb984b3" />


Processing rate:

```
~20 requests per minute
```

This prevents API throttling errors (HTTP 429).

---

# Dashboard Insights

Looker Studio dashboard visualizes:

- Issue distribution
- Query trends
- Platform distribution
- Top customer problems

Key findings from the dataset:

- Product complaints ≈ 22%
- Delivery delays ≈ 16%
- Instagram + WhatsApp generate the majority of queries

These insights highlight areas where automation can reduce support workload.

---

# Automation Opportunities

Based on the insights:

**Order Status**
Auto-send tracking link via logistics API.

**Delivery Delay**
Automated ETA notifications.

**General Questions**
AI FAQ chatbot using RAG.

**Refund Requests**
Automatic escalation to senior agents.

---

# Scalability Improvements

For production deployment:

Database upgrade

```
Google Sheets → PostgreSQL / BigQuery
```

LLM reliability

```
Primary model → Groq
Fallback model → OpenAI
```

Hybrid classification

```
Rules + LLM
```

---

# Demo

Project Documentation:

docs/project_overview.pdf

Workflow Screenshots:

screenshots/

Dashboard Preview:

dashboard/

---

# Tech Stack

- n8n
- Groq API
- LLaMA-3.1-8B
- Google Sheets
- Looker Studio
- JavaScript
