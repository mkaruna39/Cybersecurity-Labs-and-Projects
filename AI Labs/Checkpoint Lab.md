

## Project Overview
This project covers my analysis of a quick but important room in the TryHackMe AI Security Pathway. The simulation involves four different **code-review models** (Candidates A, B, C, and D). All models have been evaluated, and **three out of four are flagged as unsafe**. My task was to analyze the **model load session for Candidate A** and compare it against the other candidates to identify supply chain failures and uncover the challenge flag.

---

## Objectives
- **Goal:** Analyze model load sessions for supply chain issues  
- **Goal:** Compare safe vs. unsafe AI model behavior  
- **Goal:** Identify inference-time security failures  
- **Goal:** Recover the flag by correlating model behavior with suspicious commands  

---

## Findings

### 1. Model Load Session Analysis
Candidate A showed suspicious behavior immediately:

- **Restricted file access:** The model load for Candidate A attempted to access a restricted password file within the first few lines.  
- **Disabled safety checks:** A security review flag was disabled during inference, indicating an intentional bypass of safety mechanisms.  

These findings clearly distinguished Candidate A from B, C, and D.

---

### 2. Policy Template Query
I queried **Agent A** to determine which policy review template it uses:

- **Template used:** The agent responded with `CommunityReview`, which appears to be a standard policy review template.  
- **Inference:** The unsafe behavior was not caused by the policy template itself, but by deeper supply chain issues within the model.  

---

### 3. Identifying Linked Supply Chain Failures
A hint in the room stated that **the two supply chain failures for Agent A are related**, and that the connection between them reveals the flag.

Key clues:

- **Curl command:** One of the critical flags in the model load session was a `curl` command.  
- **Session ID:** The command contained a `sessionid` parameter.  
- **Flag discovery:** When I queried the agent about the `sessionid`, it revealed the flag.  

This confirmed that the unsafe model behavior and the suspicious outbound command were part of the same supply chain compromise.

---

## What I Learned
- **Lesson:** Supply chain attacks in AI models can manifest through unsafe model loading behavior and inference-time modifications.  
- **Lesson:** Disabling internal safety checks is a strong indicator of model tampering.  
- **Lesson:** Outbound commands (such as `curl`) embedded in model load sessions often signal exfiltration attempts.  
- **Lesson:** Querying agents directly can reveal hidden metadata or compromised parameters.  
- **Lesson:** Comparing multiple model candidates helps isolate malicious behavior and validate safe baselines.  

---

## Key Takeaways
- **Takeaway:** Candidate A demonstrated clear signs of supply chain compromise.  
- **Takeaway:** Restricted file access plus disabled safety flags indicates high-risk model behavior.  
- **Takeaway:** Suspicious outbound commands often contain embedded identifiers or flags.  
- **Takeaway:** Linked supply chain failures must be analyzed together to uncover root causes.  
- **Takeaway:** Even quick rooms reinforce critical AI-security concepts like provenance validation and inference-time threat detection.  
