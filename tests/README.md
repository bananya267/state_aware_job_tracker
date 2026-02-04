# 🧪 Testing Strategy

## 1. Privacy Unit Tests (Regex)
We utilize a "Red Team" approach to validate the sanitization layer before deployment.

| Input String | Expected Output | Status |
| :--- | :--- | :--- |
| "Call me at 555-0199 regarding the role." | "Call me at [PHONE_REDACTED] regarding the role." | ✅ PASS |
| "Email: john.doe@gmail.com for info." | "Email: [EMAIL_REDACTED] for info." | ✅ PASS |

## 2. Hallucination Stress Testing
* **Scenario:** Feed Gemini ambiguous emails (e.g., "We will get back to you soon" - which is neither a rejection nor an invite).
* **Expected Result:** Confidence score < 0.8.
* **Action:** Route to HITL folder.

## 3. Integration Testing
* **Cycle:** Weekly dry-run using a synthetic "Test Application" email to verify Google Auth tokens and Notion API connectivity.
