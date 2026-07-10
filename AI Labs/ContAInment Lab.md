***Project Overview***

This repository contains my investigation and analysis of the ContAInment Challenge from the TryHackMe AI Security Pathway. 
The scenario simulates a real‑world security incident: a ransom note appears on an employee workstation, followed by confirmed data theft and exfiltration. 
My task was to use both traditional SOC workflows and an AI security agent to identify the attacker’s access vector and recover stolen data.

***Objectives***
- Investigate the incident
- Determine how the attacker gained access
- Recover stolen and encrypted data
- Document findings in a structured, reproducible workflow

***Tools & Techniques Used***
- Linux command‑line enumeration (ls, ls -la)
- Packet capture analysis
- AI‑assisted pcap reconstruction
- Password extraction from network traffic
- Archive decryption and artifact recovery
- AI‑assisted flag identification (Liberty Prime)

**Investigation Workflow**

1. Directory Reconnaissance
   
I began by navigating to the workstation’s Documents directory and enumerating contents:

Code:
```
ls
ls -la
```
This allowed me to assess:

- File sizes (large pcaps often contain payloads or hex dumps)
- Access timestamps (recently modified files often indicate attacker activity)

This narrowed the search to the pcap most likely containing exfiltration‑related traffic.

2. Packet Capture Analysis
   
The challenge hint indicated the attacker left important information inside a pcap. After identifying the correct file, I used the AI agent to reassemble the packet capture.

The reconstructed pcap revealed:
 A password embedded within the network traffic, this password decrypted a project .zip file stored on the workstation

 3. Data Recovery

Inside the decrypted archive, I located: flags.txt and additional project files related to the exfiltrated data

Using the AI agent’s Liberty Prime capability, I extracted and validated the challenge flag.

***What I Learned:***

This challenge strengthened several core SOC and AI‑security skills:

 1. Practical Packet Analysis
    
I learned how to:

- Identify meaningful pcaps among many files
- Use file size and timestamps to prioritize analysis
- Interpret reconstructed network traffic to extract sensitive data

 2. AI‑Augmented Incident Response

I gained hands‑on experience with:

- AI‑assisted pcap reconstruction
- AI‑driven artifact extraction
- Understanding how AI agents interpret (and sometimes misinterpret) file paths and commands

3. Handling AI Tool Limitations

I encountered several challenges:

- The AI agent struggled with non‑exact file paths
- Some commands required repeated clarification
- Context retention issues caused misinterpretation of queries

This taught me how to:

- Provide precise instructions
- Validate AI output
- Maintain control of the investigation even when tooling behaves unpredictably

4. Realistic SOC Workflow Simulation
   
I practiced:

- Threat hunting
- Incident triage
- Data recovery
- Evidence‑based documentation for reproducibility
