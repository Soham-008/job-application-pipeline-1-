# AI-Powered Job Application Pipeline

An end-to-end automated job application system built with n8n, Groq LLaMA 3.1, and Google Sheets. Automatically scrapes job listings, filters them with AI, generates tailored cover letters, and logs everything — zero manual effort.

---

## Features

- **Multi-source Job Scraper** — Pulls CS internship listings from 4 RSS feeds every 3 hours
- **AI Job Filter** — Groq LLaMA 3.1 analyzes and scores each listing for relevance to your profile
- **Auto Cover Letter Generator** — LLM generates tailored cover letters with your resume context, saved as Google Docs
- **Google Sheets Tracker** — All relevant jobs automatically logged with company, position, location, match score, and cover letter link
- **Fully Automated** — Runs on cloud n8n instance, no manual intervention required

---

## Architecture

```
Schedule Trigger (every 3 hrs)
        ↓
┌─────────────────────────────┐
│  RSS Feed Sources (4 feeds) │
│  - RemoteOK Intern Jobs     │
│  - RemoteOK Junior Jobs     │
│  - RemoteOK Dev Intern      │
│  - We Work Remotely         │
└─────────────────────────────┘
        ↓
    Merge Node
        ↓
   Limit (5 jobs)
        ↓
  Groq LLaMA 3.1 AI Filter
  (scores & classifies jobs)
        ↓
   IF relevant == true
        ↓
  Google Sheets Logger ──────────────────────────────────┐
        ↓                                                 │
  Cover Letter Generator (triggered on new row)          │
        ↓                                                 │
  Groq LLaMA 3.1 (writes cover letter)                  │
        ↓                                                 │
  Google Docs (saves formatted cover letter)             │
        ↓                                                 │
  Google Sheets Update (saves Doc link) ←────────────────┘
```

---

##  Workflows

### Workflow 1 — Log Job Application (Manual)
Webhook-triggered workflow for manually logging a job application.

**Endpoint:** `POST /webhook/log-job`
```json
{
  "job_id": "JOB001",
  "company": "Google",
  "position": "Software Engineer Intern",
  "location": "San Diego, CA",
  "match_score": "85",
  "status": "Applied",
  "applied_date": "2026-03-16",
  "last_contact": "",
  "next_action": "Follow up in 1 week",
  "url": "https://careers.google.com"
}
```

### Workflow 2 — Job Scraper (Automated)
Runs every 3 hours. Scrapes 4 RSS feeds → AI filter → logs to Google Sheets → triggers cover letter generation.

### Workflow 3 — Cover Letter Generator (Triggered)
Triggers when a new row is added to Google Sheets. Generates a tailored cover letter using LLM and saves it as a Google Doc.

---
## AI Prompt Engineering

The Groq LLaMA 3.1 model uses custom prompts to:
1. **Filter jobs** — Only passes CS internships/entry-level roles relevant to a Computer Science undergraduate
2. **Score jobs** — Assigns a match score from 1-100 based on relevance
3. **Generate cover letters** — Creates tailored cover letters using resume context injection

---

## Results

- Processes **20+ job listings** per day automatically
- Runs **every 3 hours** without manual intervention
- Generates a **unique cover letter** for every relevant job found
- **Zero manual effort** required after initial setup

---

## 👤 Author

**Soham Kulkarni**
- GitHub: [@Soham-008](https://github.com/Soham-008)
- LinkedIn: [soham-kulkarni-250b20282](https://linkedin.com/in/soham-kulkarni-250b20282)
- Email: skulkarni435656@sdsu.edu

---
