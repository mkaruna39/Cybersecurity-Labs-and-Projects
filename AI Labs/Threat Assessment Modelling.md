***Project Overview:***

This repository documents my work in the AI Threat Modeling Assessment Room, where I completed three interactive scenarios focused on securing an AI system architecture. 
Each scenario required placing defensive shields on an AI pipeline to mitigate specific attack vectors. 
This exercise strengthened my understanding of AI system design, LLM threat surfaces, and defensive control placement.

***Objectives***

- Identify attack surfaces in AI system architecture
- Apply appropriate defensive controls
- Model threats across prompt, retrieval, and data layers
- Understand how LLMs can be manipulated or compromised through user input, retrieval, and data poisoning

***Skills Demonstrated***

- AI threat modeling using architectural diagrams
- Understanding of LLM attack vectors (prompt injection, data leakage, data poisoning)
- Defensive control placement across multiple system layers
- Reasoning about AI system behavior and failure modes
- Secure design thinking for AI pipelines

### Scenario 1 — Prompt Injection Attack
Attack Summary: Prompt injection occurs when user input manipulates system instructions, causing the LLM to execute unintended actions or reveal sensitive data.

Defensive Shields Placed:

1. Prompt Layer
I placed a shield on the prompt component because:

- This is where user input merges with system instructions
- Without validation or sanitization, malicious input can override intended behavior
- Protecting this layer prevents unauthorized instruction manipulation

2. LLM Agent

I also shielded the LLM agent, since the model executes the final composed prompt

If the LLM receives manipulated instructions, it may:

- Leak sensitive data

- Misuse tools

- Execute harmful actions

What I Learned:

- Prompt injection is fundamentally a trust boundary violation
- LLMs cannot inherently distinguish malicious vs. benign instructions
- Defense requires input validation, instruction isolation, and strict tool‑use constraints

### Scenario 2 — Data Leakage Attack
Attack Summary: Data leakage occurs when the system unintentionally exposes sensitive information during retrieval or response generation.

Defensive Shields Placed

1. LLM Layer
   
- The LLM decides what information appears in the final output
- Without safeguards, it may surface sensitive retrieved data
- Shielding this layer enforces response filtering and output constraints

2. Retrieval Layer

- This component fetches contextual data
- Weak filtering can return confidential or internal information
- Shielding ensures safe retrieval, access control, and content sanitization

3. Database Layer
   
- Stores embeddings, records, or contextual data
- If exposed, it becomes a direct leakage source
- Shielding protects stored sensitive data and prevents unauthorized access

What I Learned
- Retrieval‑augmented generation (RAG) systems introduce new leakage pathways
- LLMs can unintentionally reveal internal documents if retrieval is not filtered
- Defense requires retrieval access control, database hardening, and output‑layer filtering

### Scenario 3 — Data Poisoning Attack
Attack Summary: Data poisoning occurs when attackers inject malicious data into training or retrieval sources, influencing model behavior.

Defensive Shields Placed

1. Retrieval Layer
   
- Poisoned data can be stored and later retrieved
- Once retrieved, it influences model outputs even after deployment
- Shielding prevents malicious data ingestion and retrieval contamination

2. Database Layer

- Stores training data, embeddings, or behavioral logs
- If attackers inject malicious data, it directly affects model behavior
- Shielding ensures data integrity, validation, and tamper‑resistance

What I Learned:  AI systems are vulnerable not only at inference time but also at data ingestion time

Poisoned data can cause:

- Biased outputs
- Misleading responses
- Security bypasses

Defense requires data validation, source trust scoring, and continuous integrity monitoring
