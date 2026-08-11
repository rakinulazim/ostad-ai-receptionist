# Ostad Academy — AI Voice Receptionist Agent

An AI-powered voice receptionist built with **Retell AI** + **n8n** for Ostad Academy, an online education platform in Bangladesh. The agent ("Nabila") handles inbound calls in English and Bangla, answers course queries, books consultations, captures leads, and escalates to human staff when needed.

## Project Architecture

```
Retell AI (voice layer)
    └── calls n8n webhooks for each tool
            ├── Search Courses
            ├── Check Class Schedule
            ├── Book Consultation
            ├── Save Lead
            └── Transfer to Human

n8n (automation layer)
    ├── MCP Server (Ostad AI Receptionist - MCP Server)
    ├── Custom Function Webhooks (bridge for Retell → n8n tools)
    ├── Tool Workflows (one per action)
    └── Conversation Logging (Retell webhook → Google Sheets)

Google Sheets (database)
    ├── Courses
    ├── Leads
    ├── Appointments
    ├── Escalations
    └── CallLogs
```

## Files

| File | Description |
|------|-------------|
| `Ostad Academy — AI Receptionist Agent.json` | Retell AI agent config — voice settings, system prompt, tool definitions |
| `Ostad AI Receptionist - MCP Server.json` | n8n MCP Server workflow exposing tools to the AI |
| `Ostad AI Receptionist - Custom Function Webhooks.json` | HTTP webhook bridges for Retell custom tools |
| `Ostad Tool - Search Courses.json` | Searches course catalog in Google Sheets |
| `Ostad Tool - Check Class Schedule.json` | Returns batch dates and class schedules |
| `Ostad Tool - Book Consultation.json` | Checks availability and books appointments |
| `Ostad Tool - Save Lead.json` | Saves interested callers as leads |
| `Ostad Tool - Transfer to Human.json` | Creates support ticket and notifies team |
| `Ostad - Conversation Logging (Retell Webhook).json` | Logs full call transcripts and analysis post-call |
| `ostad_agent_system_prompt.txt` | Full system prompt used by the AI agent |
| `ostad_ai_receptionist_readme.pdf` | Detailed project documentation |

## Setup

### Prerequisites
- n8n Cloud instance
- Retell AI account
- Google account (Sheets + Calendar + Gmail)
- Telegram bot

### Steps
1. Import all `*.json` n8n workflow files into your n8n instance
2. Set up Google Sheets OAuth2, Google Calendar OAuth2, Gmail OAuth2, and Telegram credentials in n8n
3. Update the Google Sheet ID (`YOUR_GOOGLE_SHEET_ID`) and Calendar ID in the workflow files to point to your own spreadsheet/calendar
4. Import `Ostad Academy — AI Receptionist Agent.json` into Retell AI
5. Update webhook URLs in the Retell agent to match your n8n instance hostname
6. Activate all n8n workflows
7. Publish the Retell agent

## Features

- **Bilingual** — responds in English or Bangla, mirrors the caller's language
- **Course search** — fuzzy keyword matching against live Google Sheets data
- **Appointment booking** — conflict detection, confirmation via Google Calendar + Gmail + Telegram
- **Lead capture** — saves caller contact info + interested course to Sheets + Telegram notification
- **Human handoff** — creates a support ticket in Sheets + emails the support team
- **Call logging** — post-call webhook logs transcript, sentiment, intent, and duration

## Note on Credentials

Sensitive values (emails, Telegram chat ID, Google Sheet/Calendar IDs, n8n instance hostname) have been replaced with placeholders in these files. You will need to substitute your own values before importing.
