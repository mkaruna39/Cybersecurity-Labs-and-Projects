

## Project Overview
This project documents my work in the **Lockdown Challenge**, where a fictional company deployed an internal AI assistant to help employees with policy and operational questions. During an audit, **three vulnerabilities** were discovered in the agent’s configuration. My task was to verify each vulnerability, identify its root cause, and apply a patch. Successfully resolving each issue revealed part of the final flag.

---

## Objectives
- **Verify each vulnerability in the AI assistant**
- **Identify the root cause of the misconfiguration**
- **Apply a correct patch to mitigate the issue**
- **Recover all three parts of the final flag**

---

## Vulnerability 1 — Data Retrieval

### Issue
Employees were able to access documents containing **sensitive information** without proper authorization.

### Verification
I asked the agent to classify documents using labels such as **public**, **private**, and **internal**.  
This confirmed that the assistant was returning sensitive documents to users who should not have access.

### Patch
I instructed the agent to enforce proper **document classification and access control**.

### Result
This successfully produced the **first part of the flag**.

---

## Vulnerability 2 — Logging

### Issue
The system logs contained **sensitive information**, including contract values and client names.  
This is a direct violation of secure logging practices.

### Verification
Reviewing the logs showed that PII and confidential business data were being stored without redaction.

### Patch
I asked the agent to **automatically redact all PII** and **retain only document IDs** in the logs.

### Result
This resolved the logging issue and revealed the **second part of the flag**.

---

## Vulnerability 3 — User Access

### Issue
Users could access **all data** when making queries, regardless of their role or permissions.

### Verification
Query tests showed that the assistant did not enforce any form of access control or tenant isolation.

### Patch
I instructed the agent to enforce **tenant isolation**, ensuring users only see information based on their **access level**.

### Result
This produced the **third and final part of the flag**.

---

## Key Takeaways
- Proper **access control** is essential for internal AI assistants.
- Logs must be **sanitized** to avoid leaking sensitive information.
- AI systems require **strict data retrieval rules** to prevent unauthorized exposure.
- Tenant isolation is a critical safeguard for multi‑user environments.
