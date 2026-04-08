# 🛡️ Vendor Risk Assessment: Technical Design Write-up

## 1. Executive Summary
This project delivers a resilient, automated pipeline for vendor risk onboarding. By integrating **n8n** with **Google Gemini (LLM)**, the system transitions from manual data entry to an intelligent orchestration that classifies risks, triggers specific operational workflows (Jira, Slack, Gmail), and maintains a high-fidelity audit trail.

---

## 2. Core Architecture & Design Decisions

### **Intelligence & Groundedness**
The classification engine uses a **Groundedness Protocol** within the system prompt. This ensures the AI ignores external biases and strictly evaluates the vendor based on:
* **Data Access Scope:** PII, Database, or Public data.
* **Contract Value:** Tiered thresholds for financial exposure.
* **Concise Auditing:** The AI is instructed to generate justifications in short, structured paragraphs to ensure maximum efficiency for human auditors.

### **Resilience: The "Fail-Safe" Circuit**
A key pillar of this design is the handling of **malformed or "garbage" data** (e.g., placeholder symbols like ".").
* **Logical Fallback:** The AI Agent is configured with an "On Error -> Continue" policy.
* **Safe Navigation:** A Switch node utilizes deterministic logic `{{ ($json.risk_tier || "error").toLowerCase() }}` to catch any undefined outputs.
* **Result:** Invalid inputs never break the workflow; they are automatically routed to a dedicated "Manual Review" path.

### **Traceability & Monitoring**
To ensure 100% visibility for managers and security teams, the system implements:
* **Slack Orchestration:** Real-time alerts including **hyperlinks** to Jira tickets and internal logs, allowing for immediate action from a mobile or desktop interface.
* **Dual-Tab Logging (Google Sheets):** 1. **Audit Log:** A clean record of all successful classifications with direct links to Jira.
    2. **Error Log:** A dedicated sheet for tracking failed inputs or system exceptions, ensuring no vendor request is ever lost.

---

## 3. Toolchain & AI Integration

### **Orchestration Stack**
* **n8n:** Used for its ability to handle complex branching and multi-service integration (Jira, Google, Slack) without the overhead of custom scripts.
* **Google Gemini:** Selected for its advanced reasoning capabilities and structured output handling.

### **AI-Assisted Development**
During the build process, AI was utilized as a **Technical Consultant** for:
* **Structural Optimization:** Ensuring the JSON schemas between the AI Agent and the downstream nodes remained consistent.
* **Efficiency Tuning:** Refining the prompt to prioritize concise, audit-ready summaries over long-form justifications for the end user.
* **Best Practices:** Implementing secure placeholders for internal domain URLs to maintain repository security.

---

## 4. Roadmap & Future Enhancements
Given more development time, the following features would be prioritized:
* **Pre-Validation Layer:** A custom Node.js/Python node to perform regex and schema validation before hitting the LLM, optimizing API costs and latency.
* **Dynamic Questionnaires:** Auto-generating specific security questions based on the identified risk (e.g., asking for encryption details only if the vendor handles sensitive data).
* **Batch Processing:** Extending the Webhook logic to handle multi-vendor CSV uploads for bulk onboarding.
