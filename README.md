#  AI Email Triage & Automation System

An AI-powered email classification and automation system built with **n8n**, **Ollama**, **Llama 3.2 3B**, **Gmail**, **Google Sheets**, and **JavaScript**.

The system automatically reads incoming emails, classifies their intent, assigns a priority, and routes each one to the right automation branch — no manual sorting required.

<p align="left">
  <img alt="n8n" src="https://img.shields.io/badge/n8n-workflow-orange">
  <img alt="Ollama" src="https://img.shields.io/badge/Ollama-Llama%203.2%203B-blue">
  <img alt="Gmail" src="https://img.shields.io/badge/Gmail-API-red">
  <img alt="Status" src="https://img.shields.io/badge/status-active-brightgreen">
</p>

---

##  Table of Contents

- [Overview](#-project-overview)
- [Why This Project](#-why-this-project)
- [How It Works](#-how-it-works)
- [Example](#-example-in-action)
- [Tech Stack](#-tech-stack)
- [Setup](#-setup)
- [Configuration](#-configuration)
- [Project Structure](#-project-structure)
- [Roadmap](#-roadmap)
- [License](#-license)

---

##  Project Overview

Managing a large volume of email manually is slow and repetitive. This project automates the first — and most tedious — step of email management: **triage**.

It uses a **locally hosted Large Language Model (LLM)**, so no email content is ever sent to a third-party AI API. Every message is read, understood, and classified entirely on your own machine.

The workflow watches your Gmail inbox, sends each new message to **Llama 3.2 3B** (via Ollama) for analysis, and sorts it into one of six categories:

| Category | Icon | Description |
|---|---|---|
| **JOB** | 💼 | Job opportunities, internships, interviews, recruitment & hiring |
| **IMPORTANT** | 🔐 | Legitimate security alerts, account notifications, urgent messages |
| **PROMOTION** | 📢 | Discounts, ads, courses, products, services, marketing |
| **SOCIAL** | 👥 | LinkedIn and other social-network notifications |
| **SPAM** | 🚨 | Suspicious, phishing, fraudulent or deceptive emails |
| **GENERAL** | 📩 | Ordinary informational or conversational emails |

---

## 💡 Why This Project

- **Privacy-first** — the LLM runs locally via Ollama, so no email content leaves your machine.
- **Zero API cost** — no per-request charges from a hosted AI provider.
- **Fully automated** — once set up, it runs continuously with no manual intervention.
- **Extensible** — built on n8n, so new categories, actions, or integrations (Slack, Notion, Discord, etc.) can be added as new branches without touching the core logic.

---

## 🔄 How It Works

```text
Gmail Trigger
      ↓
Basic LLM Chain           →  builds the prompt sent to the model
      ↓
Ollama / Llama 3.2 3B     →  classifies intent + priority
      ↓
JavaScript Parser         →  extracts structured JSON from the LLM response
      ↓
Switch                    →  routes based on category
      ├── JOB        → Google Sheets   (logged for later review)
      ├── IMPORTANT  → Gmail Label
      ├── PROMOTION  → Gmail Label
      ├── SOCIAL     → Gmail Label
      ├── SPAM       → Gmail Label
      └── GENERAL    → Gmail Label
```

**Step-by-step:**

1. **Gmail Trigger** — polls your inbox and fires the workflow whenever a new email arrives.
2. **Basic LLM Chain** — packages the subject, sender, and body into a structured prompt.
3. **Ollama / Llama 3.2 3B** — the local model reads the prompt and returns a classification along with a short justification.
4. **JavaScript Parser** — validates and parses the model's raw text output into clean JSON (e.g. `{ "category": "JOB", "priority": "high" }`), so a malformed LLM response doesn't break the workflow.
5. **Switch node** — routes the email down one of six branches based on the parsed category.
6. **Action nodes** — either append a row to Google Sheets (for JOB emails you want to track) or apply a Gmail label (for everything else).

---

## 🧩 Example in Action

**Incoming email:**

> **From:** noreply@techcorp.com
> **Subject:** Interview Invitation — Backend Engineer Role
> **Body:** "Hi Alex, thanks for applying! We'd like to schedule a technical interview for the Backend Engineer position next week..."

**What happens:**

1. Gmail Trigger picks up the new message.
2. The LLM Chain sends the subject + body to Llama 3.2 3B with a classification prompt.
3. The model responds:
   ```json
   { "category": "JOB", "priority": "high", "reason": "Interview invitation from a company" }
   ```
4. The JavaScript Parser cleans this into structured JSON.
5. The Switch node routes it to the **JOB** branch.
6. A new row is appended to **Google Sheets**: `Date | Sender | Subject | Priority | Status`.

**Contrast this with a promotional email** ("50% off your next order!") — it would be classified as `PROMOTION` and simply get a Gmail label applied, with no row added to the sheet. This is the core value of the system: important, actionable emails (like job leads) get tracked and surfaced, while noise gets filed away automatically.

---

## 🛠️ Tech Stack

| Component | Role |
|---|---|
| [n8n](https://n8n.io) | Workflow automation / orchestration engine |
| [Ollama](https://ollama.com) | Runs the LLM locally |
| Llama 3.2 3B | Classifies email intent and priority |
| Gmail API | Reads incoming mail and applies labels |
| Google Sheets API | Logs JOB-category emails |
| JavaScript (n8n Code node) | Parses and validates LLM output |

---

## ⚙️ Setup

### Prerequisites
- [n8n](https://docs.n8n.io/hosting/installation/) installed (self-hosted or Docker)
- [Ollama](https://ollama.com/download) installed with the `llama3.2:3b` model pulled:
  ```bash
  ollama pull llama3.2:3b
  ```
- A Google Cloud project with the Gmail API and Sheets API enabled
- OAuth2 credentials for Gmail and Google Sheets configured in n8n

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/ai-email-triage.git
   cd ai-email-triage
   ```
2. Import the workflow into n8n:
   - Open n8n → **Workflows** → **Import from File**
   - Select `workflow/AI Email Triage System.json` from this repo
3. Set your Gmail and Google Sheets credentials in the respective nodes.
4. Point the Ollama node at your local instance (default: `http://localhost:11434`).
5. Activate the workflow.

---

## 🔧 Configuration

| Setting | Where | Notes |
|---|---|---|
| Polling interval | Gmail Trigger node | How often n8n checks for new mail |
| Model name | Ollama node | Defaults to `llama3.2:3b`; swap for a larger model if you have the hardware |
| Sheet ID | Google Sheets node | The spreadsheet used to log JOB emails |
| Label names | Gmail nodes | Customize label names/colors per category |

---

## 📁 Project Structure

```text
.
├── workflow/
│   └── AI Email Triage System.json   # n8n workflow export (import this)
├── screenshots/
│   ├── AI email classifier.png       # Classification logic / LLM node
│   ├── AI Email triage system.png    # Full workflow overview
│   └── AI Job tracker.png            # Google Sheets job log
└── README.md
```

---

## 🖼️ Screenshots

**Full workflow overview**
![AI Email triage system](./screenshots/AI%20Email%20triage%20system.png)

**Email classifier logic**
![ai_email_classifier](./screenshots/AI%20email%20classifier.png)

**Job tracker (Google Sheets log)**
![AI Job tracker](./screenshots/AI%20Job%20tracker.png)

---

## 🗺️ Roadmap

- [ ] Add a confidence-score threshold with human-review fallback for low-confidence classifications
- [ ] Slack/Discord notification for high-priority JOB and IMPORTANT emails
- [ ] Support for multiple LLM backends (OpenAI-compatible endpoints)
- [ ] Dashboard for reviewing classification accuracy over time

---

## 📄 License

This project is open source under the [MIT License](LICENSE).
