# Invite Only Challenge — TryHackMe

This is my write-up for the **Invite Only** challenge on *TryHackMe*.  
In this room, the goal is to analyze **flagged artefacts** and convert them into actionable **threat intelligence** using a built‑in TI application, a flagged **IP address**, and a **SHA256 hash**.

---

# **Situation Overview**

I was provided with:

- A **flagged IP address**
- A **SHA256 hash**
- Access to a built‑in **threat intelligence application**

My task was to investigate these artefacts, determine their relationships, and extract meaningful intelligence about the threat.

---

# **Investigation Steps**

## **1. Identifying the File Behind the SHA256 Hash**

Since **SHA256 hashes correspond to files**, my first step was to determine which file the hash represented.

I immediately submitted the hash to **VirusTotal**.

> *Result:*  
> The report identified the file as a **WIN32 EXE**, confirming it was a Windows executable.

---

## **2. Finding Execution Parents**

Next, I examined the **Relations** tab in VirusTotal.

From this section, I gathered:

- The **execution parents** of the file  
- Additional **related hashes**  
- The **name of a dropped file**

The challenge instructed me to focus on the **second execution parent**.  
After copying that hash into VirusTotal, I found **four dropped files** associated with it.

---

## **3. Investigating the Flagged IP Address**

I then moved on to the **IP address**.

In the **Community** section of VirusTotal, I found a tag:

- **SyncRat**

This revealed the **malware family** associated with the artefacts.

In the same comment, I also found a link to the **original report** that the challenge referenced — something easy to miss without careful reading.

---

## **4. Reading the Original Threat Report**

The remaining questions in the challenge came from the linked article, so I opened it in my browser.

The report, published by **Checkpoint**, described a malware campaign involving:

- **Expired and released Discord links** used to deliver payloads  
- Payloads capable of **bypassing Windows detection systems**  
- Tools designed to **steal browser cookies**  
- A phishing technique called **ClickFix**, where attackers create fake issues and fake “Fix” buttons to pressure users into acting quickly  
- Abuse of the legitimate **Discord platform** to redirect victims to malicious destinations

This background helped contextualize the artefacts I analyzed.

---

# **What Was Learned**

- **Threat intelligence requires correlation.** Hashes, IPs, and dropped files only become meaningful when connected through reports and relationships.  
- **VirusTotal’s Relations tab is invaluable.** It quickly reveals execution parents, dropped files, and behavioural links.  
- **Community comments often contain hidden clues.** Tags, references, and external links can provide critical context.  
- **Reading original reports strengthens understanding.** TI articles explain attacker methodology, helping analysts interpret artefacts more accurately.  
- **Malware campaigns often abuse legitimate platforms.** Discord, GitHub, and other trusted services are frequently used to bypass detection.

---

# **Why This Is Important**

- **Threat intelligence transforms raw artefacts into actionable insights.** Understanding how hashes, IPs, and malware families connect helps SOC teams respond effectively.  
- **Modern malware uses creative delivery methods.** Techniques like ClickFix and Discord‑based payloads highlight the need for continuous learning.  
- **Analysts must be comfortable with TI tools.** VirusTotal, community tags, and external reports are essential components of real-world investigations.  
- **This challenge reinforces core SOC skills.** Hash analysis, IP investigation, and report correlation are foundational for threat hunting and incident response.  
- **Growing confidence is key.** The challenge felt straightforward, which is a strong indicator that my skills are improving.

---
