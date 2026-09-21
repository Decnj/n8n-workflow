# N8N AUTOMATION WORKFLOWS PORTFOLIO

## Project Overview

This repository contains a collection of workflow automations built with n8n in a dedicated sandbox environment using test credentials and sample data.

The purpose of this project is to demonstrate practical workflow automation skills, including data transformation, system integration, scheduling, error handling, and AI-powered processes. All workflows were developed, tested, and documented independently without impacting production environments.

Each workflow includes:

- Exported workflow JSON files
- Setup and configuration documentation
- Testing scenarios and assumptions
- Screenshots and implementation details
- Best-practice workflow design principles

---

## Project Objectives
The project was created to:

- Learn and apply n8n fundamentals
- Build end-to-end workflow automations
- Integrate with external services and APIs
- Implement workflow validation and error handling
- Explore AI-powered automations
- Develop reusable workflow templates
- Document and test workflow implementations

---

## Technologies Used

- n8n
- Docker
- Gmail
- Google Drive
- OpenAI
- OpenWeatherMap API
- Webhooks
- REST APIs
- CSV Processing
- JSON Data Transformation

---

## Core Concepts Covered
The workflows demonstrate practical use of:

- Webhooks
- HTTP Requests
- Conditional Logic (IF)
- Data Transformation
- File Processing
- Scheduling (Cron)
- Loops and Iteration
- API Integrations
- Error Handling
- AI Automation
- Workflow Documentation

---

## Implemented Workflows

| Workflow | Description |
|-----------|------------|
| Gmail Attachment Saver | Automatically stores incoming email attachments in Google Drive |
| Webhook Lead Formatter | Receives and standardises incoming lead information |
| CSV Data Cleaner | Cleans, validates, and transforms CSV data |
| HTTP API Integration | Retrieves and processes data from external APIs |
| Scheduled Weather Reporter | Generates weather updates on a defined schedule |
| AI Email Summariser | Uses AI to summarise email content |

---p

## Repository Structure

```bash
n8n-project/
│
├── workflows/
│   ├── gmail-attachment-organizer.json
│   ├── webhook-lead-formatter.json
│   ├── csv-cleaner.json
│
├── documentation/
│   ├── gmail-saver.md
│   ├── webhook-formatter.md
│   └── csv-cleaner.md
│
├── media/
│   ├── gmail-attachment-organizer-demo.mov
│   ├── gmail-attachment-organizer.png
│   ├── webhook-demo.mov
│   └── csv-cleaner.mov
│
└── README.md
```

---

## Phase 1: Environment Setup

### Step 1: Create Sandbox Accounts
Create test accounts for:

- Gmail
- OpenAI (or AI provider)
- Test webhook services
- Weather API (OpenWeatherMap)
- Dummy CRM/Test Data Source

Examples:

- testautomation@gmail.com
- weather-test@gmail.com

Never connect production systems.


### Step 2: Install and Configure n8n
Options:

1. Local Installation
```bash
  npm install n8n -g
  n8n start
```
OR

2. Docker
```bash
  docker run -it --rm \
-p 5678:5678 \
-v ~/.n8n:/home/node/.n8n \
n8nio/n8n
```
Access the n8n editor on: http://localhost:5678


## Phase 2: Testing Approach

All workflows were tested using:

- Sandbox accounts
- Dummy datasets
- Sample webhook payloads
- Non-production APIs
- Test email accounts

Testing covered:

- Successful execution paths
- Invalid inputs
- Data validation
- Missing fields
- API failures
- Error handling scenarios

Importing a Workflow:

- Open n8n.
- Select Import Workflow.
- Choose a JSON file from the workflows directory.
- Configure required credentials.
- Execute and test the workflow.


## Documentation

Detailed setup guides, screenshots, workflow logic, node explanations, assumptions, and testing results are available in the documentation directory.
