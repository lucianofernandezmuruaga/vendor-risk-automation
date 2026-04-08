# AI-Powered Vendor Risk Assessment & Onboarding Automation

## 🚀 Project Overview
This project automates the vendor intake process by utilizing Generative AI (Google Gemini) to analyze risk, categorize vendors, and trigger cross-functional workflows. Built with **n8n**, the system handles everything from classification to audit logging and incident notification.

## 🛠 Tech Stack
- **Automation:** n8n
- **AI Engine:** Google Gemini (LLM)
- **Integrations:** Jira (Incident Management), Slack (Real-time Alerts), Google Sheets (Audit Log), Gmail (Communications)
- **Testing:** Postman

## 🧩 Key Features
- **Intelligent Risk Tiering:** Automated classification (High, Medium, Low) based on contract value and data access scope.
- **Resilient Error Handling:** Built-in "Fail-Safe" logic to handle nonsensical data or AI service outages without breaking the workflow.
- **Dynamic Action Routing:** - **High Risk:** Creates Jira tickets and sends urgent Slack alerts.
  - **Medium Risk:** Creates Jira tickets and logs data.
  - **Low Risk:** Sends approval emails and updates the Audit Log.
- **Full Traceability:** Automatic linking between the Audit Log (Sheets) and the technical tickets (Jira).

## 📂 Repository Contents
- `/n8n-blueprints`: Contains the JSON export of the complete workflow.
- `/prompts`: The finalized System Prompt used for the Risk Assessment.
- `/docs`: Technical write-up and design decisions.

## ⚙️ Setup & Installation
1. Import the `.json` file from `/n8n-blueprints` into your n8n instance.
2. Configure your credentials for Google Gemini, Jira, Slack, and Gmail.
3. Use the provided Postman payloads in the documentation to trigger the Webhook.
