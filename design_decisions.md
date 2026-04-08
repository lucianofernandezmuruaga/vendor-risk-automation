Project Write-up: AI-Powered Vendor Risk Automation

1. Overview
This project implements an automated pipeline for vendor risk assessment. It transforms a manual, error-prone intake process into a structured, multi-channel orchestration. The system doesn't just "pass data"; it uses a Large Language Model (LLM) to perform grounded reasoning, ensuring that every vendor is classified based on objective risk criteria before triggering downstream actions in Jira, Slack, and Gmail.

2. Key Design Decisions
Orchestration Layer: Why n8n?
I chose n8n for its node-based flexibility and its ability to handle complex branching logic more transparently than traditional script-heavy solutions. It allowed for a clear separation between the "AI Brain" (Gemini) and the "Operational Muscle" (Jira/Slack/Sheets).

Resilience & Error Handling (The "Fail-Safe" Circuit)
One of the most critical design decisions was the implementation of a deterministic fallback path.

The Problem: LLMs can occasionally return malformed JSON or fail due to nonsensical input (e.g., users entering "." in all fields).

The Solution: I configured the AI Agent with an "On Error -> Continue" policy and a Switch node with "Safe Navigation" logic: {{ ($json.risk_tier || "error").toLowerCase() }}.

Result: If the AI fails to classify a vendor, the system automatically routes the case to a "Yellow Alert" Slack notification and logs the incident for manual review, preventing the entire automation from crashing.

Groundedness Protocol (Prompt Engineering)
The system prompt was engineered using a Groundedness Protocol. I explicitly instructed the model to ignore its own internal assumptions and strictly use the provided 'Scope' and 'Value' fields. I also added a "Nonsensical Data" clause to force an "Error" classification if the input was clearly invalid.

3. Using AI in the Build Process
Throughout the development of this workflow, I utilized AI as a Senior Technical Partner.

Debugging: AI helped troubleshoot the Structured Output Parser when it initially returned raw JSON strings instead of mapped fields.

Refinement: I used iterative prompting to compress the AI's justification length, moving from long paragraphs to concise, audit-ready summaries.

Security: AI assisted in identifying where to use placeholders (like the Atlassian domain) to maintain security best practices in public repositories.

4. Future Improvements (What I'd do with more time)
Advanced Validation: I would implement a custom Python or JavaScript node to perform regex validation on inputs before they even reach the LLM, reducing API costs and latency.

Dynamic Questionnaire Generation: Moving from a simple classification to generating a pre-filled PDF security questionnaire based on the specific risks identified (e.g., focusing on encryption if the vendor handles PII).

Batch Processing: Adding a loop to handle CSV uploads, allowing the system to process dozens of vendors in a single execution.
