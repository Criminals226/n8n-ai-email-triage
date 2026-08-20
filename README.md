# n8n-ai-email-triage

🤖 AI Email Triage & Automation System

An AI-powered email classification and automation workflow built with n8n, Ollama (Llama 3.2 3B), Gmail, Google Sheets, and JavaScript.

🚀 Overview

The workflow monitors incoming Gmail messages, uses a locally hosted LLM to classify each email, assigns a priority and reason, then routes the email to a category-specific automation.

Workflow

Gmail Trigger
      ↓
AI Email Classifier
      ↓
Parse AI Result
      ↓
Route by Category
      ├── JOB        → Google Sheets
      ├── IMPORTANT  → Gmail Label
      ├── PROMOTION  → Gmail Label
      ├── SOCIAL     → Gmail Label
      ├── SPAM       → Gmail Label
      └── GENERAL    → Gmail Label

🧠 Categories

JOB — internships, jobs, interviews, recruitment and hiring communication

IMPORTANT — legitimate security alerts, account notifications and messages requiring attention

PROMOTION — discounts, advertisements, courses, products, services and marketing

SOCIAL — LinkedIn/social-network notifications and activity

SPAM — suspicious, phishing, fraudulent or deceptive messages

GENERAL — ordinary messages that do not clearly fit another category

🛠️ Tech Stack

n8n

Ollama

Llama 3.2 3B

Gmail API

Google Sheets API

JavaScript

🔄 How It Works

Gmail Trigger detects incoming email.

Llama 3.2 3B analyzes the email locally through Ollama.

The AI returns a category, priority and reason.

A JavaScript node parses the response into structured fields.

The n8n Switch routes the email based on category.

JOB emails are logged to Google Sheets; other categories receive their corresponding Gmail labels.

Example AI output:

Category: JOB
Priority: HIGH
Reason: The email contains an invitation to an interview for a cybersecurity internship.

Parsed output:

{
  "category": "JOB",
  "priority": "HIGH",
  "reason": "The email contains an invitation to an interview for a cybersecurity internship."
}

🔐 Why Ollama?

The classification model runs locally, so the workflow does not require an OpenAI API quota for its core classification.

Benefits include local processing, no cloud LLM quota for classification, and hands-on experience building AI automation.

⚙️ Setup

Install Ollama and the model:

ollama pull llama3.2:3b

Verify:

ollama list

The local Ollama endpoint used by n8n is:

http://localhost:11434

Connect Gmail and Google Sheets credentials in n8n, then build/import the workflow using the nodes described above.

🧪 Testing

Recommended test categories:

Test

Expected

Cybersecurity internship interview

JOB / HIGH

Legitimate security alert

IMPORTANT / HIGH

Course discount

PROMOTION / MEDIUM

LinkedIn notification

SOCIAL / LOW

Phishing-style message

SPAM / HIGH

General company information

GENERAL / LOW

Gmail's own spam filtering can move messages to Spam before n8n receives them. For end-to-end testing, use test messages that reach the Inbox.

📌 What I Learned

This project gave me hands-on experience with:

AI workflow automation

Local LLM integration

Prompt engineering

Structured AI output parsing

Conditional routing

Gmail API integration

Google Sheets automation

API/credential troubleshooting

Designing reliable AI classification rules

🔮 Future Improvements

Confidence scoring

Job application tracking

Interview calendar integration

Daily job summaries

Duplicate-email detection

Human approval before labeling

Analytics dashboard

MCP-based tool integration

👩‍💻 Author

Aleena Khalid

Cybersecurity graduate transitioning into AI and AI automation.

This project demonstrates practical skills in AI, LLMs, workflow automation, APIs and JavaScript.

📄 License

MIT License🤖 AI Email Triage & Automation System

An AI-powered email classification and automation workflow built with n8n, Ollama (Llama 3.2 3B), Gmail, Google Sheets, and JavaScript.

🚀 Overview

The workflow monitors incoming Gmail messages, uses a locally hosted LLM to classify each email, assigns a priority and reason, then routes the email to a category-specific automation.

Workflow

Gmail Trigger
      ↓
AI Email Classifier
      ↓
Parse AI Result
      ↓
Route by Category
      ├── JOB        → Google Sheets
      ├── IMPORTANT  → Gmail Label
      ├── PROMOTION  → Gmail Label
      ├── SOCIAL     → Gmail Label
      ├── SPAM       → Gmail Label
      └── GENERAL    → Gmail Label

🧠 Categories

JOB — internships, jobs, interviews, recruitment and hiring communication

IMPORTANT — legitimate security alerts, account notifications and messages requiring attention

PROMOTION — discounts, advertisements, courses, products, services and marketing

SOCIAL — LinkedIn/social-network notifications and activity

SPAM — suspicious, phishing, fraudulent or deceptive messages

GENERAL — ordinary messages that do not clearly fit another category

🛠️ Tech Stack

n8n

Ollama

Llama 3.2 3B

Gmail API

Google Sheets API

JavaScript

🔄 How It Works

Gmail Trigger detects incoming email.

Llama 3.2 3B analyzes the email locally through Ollama.

The AI returns a category, priority and reason.

A JavaScript node parses the response into structured fields.

The n8n Switch routes the email based on category.

JOB emails are logged to Google Sheets; other categories receive their corresponding Gmail labels.

Example AI output:

Category: JOB
Priority: HIGH
Reason: The email contains an invitation to an interview for a cybersecurity internship.

Parsed output:

{
  "category": "JOB",
  "priority": "HIGH",
  "reason": "The email contains an invitation to an interview for a cybersecurity internship."
}

🔐 Why Ollama?

The classification model runs locally, so the workflow does not require an OpenAI API quota for its core classification.

Benefits include local processing, no cloud LLM quota for classification, and hands-on experience building AI automation.

⚙️ Setup

Install Ollama and the model:

ollama pull llama3.2:3b

Verify:

ollama list

The local Ollama endpoint used by n8n is:

http://localhost:11434

Connect Gmail and Google Sheets credentials in n8n, then build/import the workflow using the nodes described above.

🧪 Testing

Recommended test categories:

Test

Expected

Cybersecurity internship interview

JOB / HIGH

Legitimate security alert

IMPORTANT / HIGH

Course discount

PROMOTION / MEDIUM

LinkedIn notification

SOCIAL / LOW

Phishing-style message

SPAM / HIGH

General company information

GENERAL / LOW

Gmail's own spam filtering can move messages to Spam before n8n receives them. For end-to-end testing, use test messages that reach the Inbox.

📌 What I Learned

This project gave me hands-on experience with:

AI workflow automation

Local LLM integration

Prompt engineering

Structured AI output parsing

Conditional routing

Gmail API integration

Google Sheets automation

API/credential troubleshooting

Designing reliable AI classification rules

🔮 Future Improvements

Confidence scoring

Job application tracking

Interview calendar integration

Daily job summaries

Duplicate-email detection

Human approval before labeling

Analytics dashboard

MCP-based tool integration

👩‍💻 Author

Aleena Khalid

Cybersecurity graduate transitioning into AI and AI automation.

This project demonstrates practical skills in AI, LLMs, workflow automation, APIs and JavaScript.

📄 License


MIT License
