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
- **Dynamic Action Routing:**
  - **High Risk:** Creates Jira tickets and sends urgent Slack alerts.
  - **Medium Risk:** Creates Jira tickets and logs data.
  - **Low Risk:** Sends approval emails and updates the Audit Log.
- **Full Traceability:** Automatic linking between the Audit Log (Sheets) and the technical tickets (Jira).

## 📂 Repository Contents
- `/n8n-blueprints`: Contains the JSON export of the complete workflow.
- `/prompts`: The finalized System Prompt used for the Risk Assessment.
- `/docs`: Technical write-up and design decisions.

## ⚙️ Setup & Installation
1. **Import Workflow:** Import the `.json` file from `/n8n-blueprints` into your n8n instance.
2. **Credentials:** Configure your credentials for Google Gemini, Jira, Slack, and Gmail nodes.
3. **Google Sheets Localization:** - The workflow uses the `=HIPERVINCULO()` formula (Spanish/European localization). 
   - **Important:** If your Google Sheets is set to English/US, please update the formula in the Google Sheets nodes to `=HYPERLINK()` and replace semicolons (`;`) with commas (`,`).
4. **Trigger:** Use the Postman payloads provided in the section below to test the different logic branches.
5. **Domain Configuration:** Update the placeholders `your-domain.atlassian.net` in the **Slack** and **Google Sheets** nodes with your actual Atlassian instance URL to ensure the ticket links work correctly.

## 🧪 Testing the Workflow
To trigger the automation, send a **POST** request to your n8n Webhook URL with the following JSON payloads:

### Case 1: High Risk (Triggers Jira & Slack Alert)
```json
{
  "vendor_name": "CloudCore Data Systems",
  "vendor_email": "insert-gmail@gmail.com",
  "category": "IT Infrastructure",
  "data_access_scope": "Admin access to production database containing PII",
  "estimated_contract_value": 75000,
  "submitter_name": "Luciano Test"
}
```

### Case 2: Low Risk (Triggers Gmail Approval & Audit Log)
```json
{
  "vendor_name": "EcoLogistics Supplies",
  "vendor_email": "insert-gmail@gmail.com",
  "category": "Office Furniture",
  "data_access_scope": "No access to sensitive data / Physical delivery only",
  "estimated_contract_value": 5000,
  "submitter_name": "Luciano Test"
}
```

### Case 3: Error Handling (Triggers Slack Error Notification)
```json
{
  "vendor_name": ".",
  "vendor_email": "insert-gmail@gmail.com",
  "category": ".",
  "data_access_scope": ".",
  "estimated_contract_value": ".",
  "submitter_name": "."
}
```
