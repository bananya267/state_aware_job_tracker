
# 🏛️ System Architecture & Design

## Architectural Decisions Records (ADR)

### ADR 01: Pre-Inference PII Masking
* **Context:** HR emails contain sensitive contact info. Sending this to a public LLM poses a data leakage risk.
* **Decision:** Implement a local Regex scrubbing layer within the Make.com execution environment.
* **Consequence:** The AI receives "context" without "identity."

### ADR 02: Composite Key Identity Management
* **Context:** Recruiters often send multiple emails (Application Received, Interview Invite) for the same role.
* **Decision:** Use a `Company Name` + `Job Title` composite lookup in Notion.
* **Consequence:** Ensures the database acts as a "Single Source of Truth" and handles updates rather than creating duplicates.

### ADR 03: Confidence-Based Routing
* **Context:** AI is probabilistic. It may misinterpret a "we'll keep you on file" email as an active "Interview."
* **Decision:** Implement a numeric threshold ($0.8$). Scores below this route to a manual review folder.

## 📉 Cost & Scalability Analysis
### Token Economics (Gemini 1.5 Flash)
* **Input Window:** ~500 tokens (Email Body + System Prompt).
* **Output Window:** ~50 tokens (JSON Object).
* **Cost Per Run:** ~$0.00015.
* **Scalability:** The current architecture handles up to 50 concurrent executions per minute (Make.com limits) before requiring a queue-based decoupling (e.g., SQS), which is sufficient for personal use.

## Data Schema (Notion)
The Notion database requires the following properties:
| Property | Type | Description |
| :--- | :--- | :--- |
| **Company** | Title | Name of the organization |
| **Role** | Select | Job Title |
| **Status** | Status | Applied, Interview, Offer, Rejected |
| **Confidence** | Number | 0.0 - 1.0 score from Gemini |
| **AI Reasoning** | Text | XAI Audit log (e.g., "Detected 'regret to inform'") |
| **Last Updated** | Date | Auto-managed timestamp |
