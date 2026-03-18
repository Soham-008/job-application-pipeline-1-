# 🤖 AI-Powered Job Application Pipeline

An end-to-end automated job application system built with n8n, Groq LLaMA 3.1, and Google Sheets. Automatically scrapes job listings, filters them with AI, generates tailored cover letters, and logs everything — zero manual effort.

---

## 🚀 Features

- **Multi-source Job Scraper** — Pulls CS internship listings from 4 RSS feeds every 3 hours
- **AI Job Filter** — Groq LLaMA 3.1 analyzes and scores each listing for relevance to your profile
- **Auto Cover Letter Generator** — LLM generates tailored cover letters with your resume context, saved as Google Docs
- **Google Sheets Tracker** — All relevant jobs automatically logged with company, position, location, match score, and cover letter link
- **Fully Automated** — Runs on cloud n8n instance, no manual intervention required

---

## 🏗️ Architecture

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

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow automation engine |
| **Groq LLaMA 3.1** | AI job filtering + cover letter generation |
| **Google Sheets API** | Job application tracker database |
| **Google Docs API** | Cover letter document creation |
| **RSS Feeds** | Job listing sources |
| **OAuth2** | Google authentication |
| **Webhooks** | Event-driven triggers |

---

## 📋 Workflows

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

## ⚙️ Setup

### Prerequisites
- [n8n Cloud account](https://n8n.io) or self-hosted n8n instance
- [Groq API key](https://console.groq.com) (free tier available)
- Google account with Sheets and Docs access
- Google Sheets with columns: `Job ID | Company | Position | Location | Match Score | Status | Applied Date | Last Contact | Next Action | URL | Cover Letter`

### Installation

1. **Clone this repo**
```bash
git clone https://github.com/Soham-008/job-application-pipeline
cd job-application-pipeline
```

2. **Import workflows into n8n**
   - Open your n8n instance
   - Go to Workflows → Import
   - Import each `.json` file from the `/workflows` folder

3. **Set up credentials in n8n**
   - Add **Google Sheets OAuth2** credential
   - Add **Google Docs OAuth2** credential
   - Add **Groq API** credential with your API key

4. **Update your Google Sheet ID**
   - In each workflow, update the Google Sheets node to point to your spreadsheet

5. **Publish all workflows**
   - Click Publish on each workflow to activate automation

---

## 📊 Google Sheets Structure

| Column | Description |
|--------|-------------|
| Job ID | Auto-generated unique ID |
| Company | Company name |
| Position | Job title |
| Location | Job location |
| Match Score | AI relevance score (1-100) |
| Status | Applied / Interview / Rejected / Offer |
| Applied Date | Date applied |
| Last Contact | Last communication date |
| Next Action | Follow-up action needed |
| URL | Job listing URL |
| Cover Letter | Link to Google Doc cover letter |

---

## 🤖 AI Prompt Engineering

The Groq LLaMA 3.1 model uses custom prompts to:
1. **Filter jobs** — Only passes CS internships/entry-level roles relevant to a Computer Science undergraduate
2. **Score jobs** — Assigns a match score from 1-100 based on relevance
3. **Generate cover letters** — Creates tailored cover letters using resume context injection

---

## 📈 Results

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

## 📄 License

MIT License — feel free to use and modify for your own job search!
