# 🛡️ The GPS Protocol (Governance, Privacy, Security)

## 1. Data Privacy (The "Sanitization Layer")
To mitigate **Data Leakage**, we employ a strict **Local Sanitization Proxy**. No PII leaves the trusted Make.com environment.

### Redaction Rules (Regex)
* **Email Redaction:** `/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g` → `[EMAIL_REDACTED]`
* **Phone Redaction:** `/\d{3}[-.\s]??\d{3}[-.\s]??\d{4}/g` → `[PHONE_REDACTED]`
* **Address Scrub:** Filters out patterns matching street suffixes (St, Ave, Blvd) followed by zip codes.

## 2. Model Governance
### Hallucination Control
* **Strict JSON Mode:** The System Prompt enforces a rigid JSON schema. If the model outputs conversational text, the parser fails safely, preventing database corruption.
* **Temperature:** Set to `0.1` to maximize deterministic outputs.

## 3. Human-in-the-Loop (HITL) Protocol
Automation is an assistant, not the pilot.
* **Escalation:** Any "Rejection" status with a confidence score < 0.7 is flagged as "Needs Review."
* **Audit Trail:** The `AI Reasoning` field allows the user to verify the logic behind every automated update.
