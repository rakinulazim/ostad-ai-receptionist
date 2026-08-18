# 🎙️ Ostad AI Receptionist — Voice Agent System

An end-to-end **AI-powered voice receptionist** built for Ostad Academy using **Retell AI**, **n8n**, and **MCP (Model Context Protocol)**. The agent handles inbound phone calls autonomously — answering questions, booking consultations, capturing leads, and escalating to a human when needed — all via natural voice conversation.

---

## 🎯 Overview

This system replaces a traditional receptionist with an intelligent AI voice agent that can:
- Answer questions about courses, fees, and schedules
- Book consultation appointments in real time
- Save prospective student leads to a CRM (Google Sheets)
- Escalate complex queries to a human agent
- Log every call with full transcript and analysis

---

## 🏗️ System Architecture

```
Inbound Phone Call (Retell AI)
        ↓
   AI Voice Agent (LLM + System Prompt)
        ↓
   MCP Tool Calls (via n8n MCP Server)
   ┌─────────────────────────────────┐
   │  search_courses                 │
   │  check_class_schedule           │
   │  book_consultation              │
   │  save_lead                      │
   │  transfer_to_human              │
   └─────────────────────────────────┘
        ↓
   Google Sheets (CRM Database)
   Google Calendar (Appointments)
   Gmail + Telegram (Notifications)
        ↓
   Call Webhook → Conversation Logging
```

---

## 🧠 Agent Capabilities

| Tool | What It Does |
|------|-------------|
| **Search Courses** | Answers questions about available courses, fees, duration |
| **Check Class Schedule** | Returns upcoming batch start dates and class timings |
| **Book Consultation** | Checks availability and books a slot in Google Calendar |
| **Save Lead** | Captures prospective student info into the CRM |
| **Transfer to Human** | Escalates the call and alerts the support team |

---

## 🛠️ Tech Stack

![Retell AI](https://img.shields.io/badge/Retell_AI-Voice_Agent-blueviolet?style=flat)
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=flat&logo=n8n&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google_Sheets-34A853?style=flat&logo=google-sheets&logoColor=white)
![Google Calendar](https://img.shields.io/badge/Google_Calendar-4285F4?style=flat&logo=google-calendar&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-D14836?style=flat&logo=gmail&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=flat&logo=telegram&logoColor=white)

---

## 📁 File Structure

```
ostad-ai-receptionist/
├── ai-receptionist-agent.json          # Main Retell AI agent config
├── mcp-server.json                     # n8n MCP Server (exposes tools to agent)
├── custom-function-webhooks.json       # Webhook handler for custom functions
├── conversation-logging-webhook.json   # Logs full call transcript + analysis
├── tool-book-consultation.json         # Tool: check availability & book slot
├── tool-check-class-schedule.json      # Tool: fetch batch/schedule info
├── tool-save-lead.json                 # Tool: save lead to Google Sheets
├── tool-search-courses.json            # Tool: search course catalog
├── tool-transfer-to-human.json         # Tool: escalate call to human agent
└── agent-system-prompt.txt            # Full system prompt for the AI agent
```

---

## 🚀 Setup Guide

### Prerequisites
- [Retell AI](https://retellai.com) account with a phone number
- n8n instance (self-hosted or cloud)
- Google account with Sheets, Calendar, and Gmail API enabled

### 1. Set Up Google Sheets (CRM Database)

Create a Google Sheet with these tabs:

| Tab | Columns |
|-----|---------|
| **Courses** | CourseName, Duration, Fee, BatchStart, Schedule, Time |
| **Leads** | LeadID, Timestamp, Name, Phone, Email, InterestedCourse, Source |
| **Appointments** | AppointmentID, Timestamp, Name, Phone, Date, Time, Status |
| **Escalations** | Name, Phone, Reason, Status |
| **CallLogs** | CallID, Timestamp, CallerNumber, Duration, Intent, Sentiment, Summary, Transcript |

### 2. Import n8n Workflows

Import all `.json` files into n8n (**Workflows → Import from File**):
1. `mcp-server.json` — Start here, this is the main MCP server
2. All `tool-*.json` files — Individual tool workflows called by the agent
3. `conversation-logging-webhook.json` — Set its URL as the Agent Webhook in Retell
4. `custom-function-webhooks.json` — For custom function integration

### 3. Add Credentials

In n8n, add the following credentials and connect them to the relevant nodes:
- **Google Sheets OAuth2** — for all Sheets read/write nodes
- **Google Calendar OAuth2** — for the Book Consultation tool
- **Gmail OAuth2** — for email notifications
- **Telegram Bot API** — for Telegram alerts

### 4. Update Configuration Placeholders

Replace these placeholders in the workflow nodes with your actual values:
- `YOUR_GOOGLE_SHEET_ID` → Your Google Sheet ID (from the URL)
- `YOUR_GOOGLE_CALENDAR_ID` → Your Google Calendar ID
- `YOUR_EMAIL@gmail.com` → Your notification email
- `YOUR_TELEGRAM_CHAT_ID` → Your Telegram chat ID

### 5. Configure Retell AI Agent

- Import `ai-receptionist-agent.json` into your Retell dashboard
- Paste the contents of `agent-system-prompt.txt` as the system prompt
- Set the MCP Server URL to your n8n MCP Server webhook URL
- Attach your Retell phone number to the agent

### 6. Activate

- Toggle all n8n workflows to **Active**
- Set the `conversation-logging-webhook.json` webhook URL as the **Agent Webhook** in Retell
- Test by calling your Retell phone number

---

## 💬 Sample Call Flow

```
Caller: "Hi, I want to know about your web development course."
Agent:  "Of course! Our Full-Stack Web Development course is 6 months long,
         costs 15,000 taka, and the next batch starts March 1st with classes
         on Saturday and Sunday. Would you like to book a free consultation?"
Caller: "Yes, please book me for Saturday at 10 AM."
Agent:  "Could I get your name and phone number?"
Caller: "I'm Ahmed, my number is 017XXXXXXXX."
Agent:  "Perfect! I've booked your consultation for Saturday at 10 AM.
         A counselor will call you at 017XXXXXXXX to confirm. Is there
         anything else I can help you with?"
```

---

## 👤 Author

**Rakinul Azim** — AI Automation Engineer & Full-Stack Developer

[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat&logo=github&logoColor=white)](https://github.com/rakinulazim)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/rakinulazim)
