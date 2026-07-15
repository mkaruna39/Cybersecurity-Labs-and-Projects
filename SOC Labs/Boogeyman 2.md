# Boogeyman Challenge 2 

This write-up covers my investigation for the **second Boogeyman challenge** on *TryHackMe*.  
In this challenge, I had access to additional forensic tools, including:

- **Volatility** — extract artefacts from RAM memory dumps  
- **Olevba** — extract macros from Office 365 documents  

The goal was to analyze a **phishing email** and the **memory dump** from the victim’s workstation to uncover evidence of malicious activity.

---

## Situation Overview

An **HR employee** received an email containing a **malicious attachment**.  
Shortly afterward, IT staff observed **suspicious commands** being executed on her desktop.

My task was to analyze the artefacts, identify the malicious payload, and trace the attacker’s activity through the memory dump.

---

# Email & Attachment Analysis

## 1. Background Information

I began by opening the provided **`.eml` file** to gather initial details:

- Origin email address  
- Recipient address  
- Type of attached file  

## 2. Integrity Check

To verify the attachment, I calculated its **MD5 hash**:
```bash
md5sum malicious_attachment.docm
```

3. Macro Extraction with Olevba
Using Olevba, I extracted and examined the macros embedded in the attachment:
```bash
olevba malicious_attachment.docm
```

Result:  
I quickly identified the URL used to download the malicious payload, along with additional suspicious paths and processes referenced in the macro output.

# Memory Analysis with Volatility

## 1. Process Enumeration
I analyzed the raw memory dump using Volatility to identify executed processes, creation timestamps, PIDs, and PPIDs:
```bash
vol -f "memory_dump.raw" windows.pslist
```

By reviewing the process list, I located the PID and PPID of the process responsible for executing the malicious payload.

## 2. C2 Shell Investigation

The Olevba results suggested that a C2 (Command and Control) shell had been executed.

I returned to Volatility to identify the PID and file path of the executable responsible:
```bash
vol -f "memory_dump.raw" windows.pstree
```

Important detail:  
I initially missed that the payload process spawned another process, which then established the C2 connection.
This small oversight highlights how easy it is to miss critical clues during an investigation.

## 3. Command Line Extraction
```bash
vol -f "memory_dump.raw" windows.cmdline
```
This helped confirm the exact paths and executables involved.

# Network Analysis
To identify the IP address and port used for the C2 connection:
```bash
vol -f "memory_dump.raw" windows.netscan
```
This provided a list of all network connections and their associated processes, allowing me to pinpoint the remote IP used by the attacker.

# Scheduled Task Discovery
The final part of the challenge involved identifying a scheduled task implanted by the attacker after the C2 shell was established.

The relevant Windows command was:
```bash
schtasks
```
Since I wasn't familiar with the syntax, I used
```bash
strings "memory_dump.raw" | grep schtasks
```
This allowed me to locate the malicious scheduled task and complete the challenge.

# **What Was Learned**

- **Tool synergy matters.** Using Volatility and Olevba together provides a fuller picture of both *memory artefacts* and *malicious document behavior*.  
- **Small details can change the entire investigation.** Missing a single spawned process initially hid the C2 connection, showing how easy it is to overlook critical evidence.  
- **Keyword familiarity is essential.** Knowing common attacker patterns and terms (like *schtasks*, *cmdline*, *netscan*) speeds up memory searches and reduces time spent guessing.  
- **Iterative analysis is normal.** Revisiting outputs multiple times often reveals clues missed during the first pass.  
- **Understanding attacker workflow improves accuracy.** Recognizing how payloads spawn processes, establish C2 shells, and create persistence helps build a clearer timeline.

---

# **Why This Is Important**

- **Phishing remains a primary entry point** for attackers, and analyzing malicious attachments is a core skill in incident response.  
- **Memory forensics is critical** for uncovering post-exploitation activity that logs may not capture.  
- **Attackers rely on predictable behaviors**, such as scheduled tasks, payload downloads, and C2 shells — learning these patterns strengthens defensive capabilities.  
- **Tool proficiency directly impacts investigation quality.** Knowing how to use Volatility modules and document-analysis tools makes investigations faster and more meaningful.  
- **This challenge reinforces real-world DFIR workflows**, helping build intuition for how compromises unfold and how to trace them effectively.
