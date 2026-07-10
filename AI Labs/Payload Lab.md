**Project Overview:**

This repository contains my analysis of the Payload Challenge, the final room in the TryHackMe AI Security Pathway. 
The scenario simulates a breach in a production code‑review model, where the server began making unauthorized outbound HTTP requests to an unknown address. 
Although an automated detection rule blocked the activity, I was tasked with performing a full investigation using tools and techniques from previous rooms.

**Objectives**

- Investigate anomalous outbound activity
- Analyze production and candidate AI models
- Identify malicious payloads and inference‑time behavior
- Recover the full challenge flag through log and model analysis

**Investigation Components**

The virtual machine provided four key artifacts:

- Network and deployment logs
- The production model (suspected compromised)
- An untested replacement model
- A baseline model for comparison

These allowed me to perform a layered investigation across deployment history, model internals, and runtime behavior.

***Step‑by‑Step Analysis***

1. Deployment Log Review- I began by examining the deployment logs to establish a timeline and identify anomalies.

Key findings:

- The replacement model originated from a different source than the original model, immediately raising suspicion.
- The automated detection rule triggered 21 days after the replacement model was deployed, suggesting delayed malicious behavior or dormant payload activation.
- This established the replacement model as a likely entry point for the breach.

2. Decompiling the Production Model- To analyze the model internals, I used the built‑in fickling tool to decompile the production model.

Findings:

- The payload invoked system functions to execute shell commands.
- A curl command attempted an outbound HTTP request to attacker.com.
- A shell command retrieved the hostname, indicating reconnaissance behavior.

These behaviors confirmed that the model contained embedded malicious code.

3. ModelScan Verification- Running modelscan validated the decompilation results.

Output summary:

- 1 critical issue detected
- The critical issue was the presence of OS‑level system commands embedded in the model
- This reinforced that the model was compromised and executing unauthorized operations.

4. Architecture Layer Threat Analysis- Next, I used inspect_h5_py to analyze the model’s internal architecture for inference‑time threats.

Findings:

- A suspicious lambda layer named manipulate_output was discovered
- Lambda layers often contain custom code executed during inference
- This layer likely served as the execution point for the malicious payload

This confirmed that the threat was embedded at the architectural level, not just in surface‑level metadata.

5. Retrieving the Full Flag
   
To complete the challenge, I examined:

- beacon_capture.log using a simple cat command
- The candidate model, which contained the remaining portion of the flag within its layer scan output

By combining the log data and the candidate model’s suspicious layer contents, I recovered the full challenge flag.

***What I Learned:***

This challenge strengthened several advanced AI‑security skills:

- Model decompilation and static analysis
- Detection of malicious inference‑time behavior
- Threat modeling across AI architecture layers
- Identifying unauthorized system‑level calls embedded in model weights
- Comparing production, baseline, and candidate models to isolate malicious modifications
- Using logs and model artifacts together to reconstruct attacker behavior
