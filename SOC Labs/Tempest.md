# Tempest Challenge 

This is my write-up for the **TryHackMe Tempest challenge**.  
The objective was to analyze and handle various **artifacts from a compromised Windows machine**, primarily using **Sysmon logs**, packet captures, and several forensic tools.

---

# **Objective**

- Analyze artifacts from a compromised machine  
- Use **Sysmon**, **TimelineExplorer**, **Sysmon View**, and **Wireshark**  
- Identify malicious files, commands, domains, and attacker techniques  

---

# **Investigation Steps**

## **1. Finding SHA256 Hashes**

SHA256 hashes help determine whether a file has been tampered with.  
I used the PowerShell command:

```powershell
Get-FileHash -Algorithm SHA256 .\filename
```
## **2. Converting Sysmon Logs to CSV**

The Sysmon logs were originally provided in **ETVX** format.  
To make analysis easier, I researched how to convert **ETVX → CSV**, which reinforced the importance of **standardizing log formats** so analysts can properly interpret and filter data effectively.

---

## **3. Opening Logs in TimelineExplorer**

After converting the logs, I opened the CSV file in **TimelineExplorer**, which allowed me to:

- **Sort events**  
- **Filter by Event ID**  
- **Quickly identify suspicious activity**

This tool made navigating large Sysmon datasets significantly easier.

---

## **4. Visualizing Logs in Sysmon View**

I imported the Sysmon XML file into **Sysmon View**, which provided:

- **Visual process trees**  
- **File creation timelines**  
- **Network activity summaries**

These visualizations helped me better understand the attacker’s workflow and how different events were connected.

---

# **Further Analysis**

## **Finding the Malicious Document**

I needed to identify the malicious **`.doc` file** that created the suspicious process.

Key Sysmon identifiers:

- **Event ID 11** → File creation  
- **File extension** → `.doc`

Filtering for these in TimelineExplorer revealed:

- **The file name**  
- **The host machine** it was downloaded on  

---

## **Finding the IPv4 Address**

Next, I needed the IPv4 address associated with the malicious domain.

Key Sysmon identifier:

- **Event ID 22** → DNS query  

I filtered for entries where the **ProcessID** matched the malicious activity, which made the answer easy to locate.

---

## **Finding the Payload Command**

This part required careful keyword selection and log pivoting.

Before finding the command executed by the payload, I first needed the **full path** of the payload:

- **Event ID 11** → File creation  
- **Keyword:** `startup` (because earlier analysis showed execution occurred at startup)

Once I had the full path, I was able to locate the **exact command** used by the attacker in the logs.

---
# **Wireshark Analysis**

## **1. Finding the Payload URL**

Since I already knew the host, I filtered in Wireshark for:

- **The URL associated with the payload**  
- **Basic packet details** such as method, protocol, and request type  

This part was straightforward once the correct filters were applied.

---

## **2. Analyzing a Sensitive File**

I located the relevant packet using the **domain name**, then:

- **Decoded the data in CyberChef** to reveal its contents  
- **Enumerated the ports** on the desktop to determine whether any could have been used for remote command execution  

This helped confirm how the attacker interacted with the compromised machine.

---

## **3. Finding the Reverse SOCKS Proxy**

This was one of the trickiest tasks.

After looking up a hint, I searched the logs for the keyword **“socks”**, which revealed:

- The **reverse SOCKS proxy** the attacker used  
- The method they relied on to establish the remote connection  

---

# **What Was Learned**

- **Sysmon is extremely powerful.** Event IDs like **11** (file creation) and **22** (DNS query) make it possible to reconstruct attacker behavior.  
- **Keyword selection matters.** Many tasks required knowing *exactly* what to filter for.  
- **Log format standardization is essential.** Converting **ETVX → CSV** made analysis significantly easier.  
- **Visualization tools help.** Sysmon View provided clarity that raw logs alone couldn’t.  
- **Network forensics ties everything together.** Wireshark + CyberChef revealed payload URLs, sensitive data, and attacker techniques.  
- **Persistence and research are part of the job.** Looking up hints and experimenting with filters mirrors real-world SOC work.

---

# **Why This Is Important**

- **Tempest reinforces real DFIR skills.** Log conversion, Sysmon filtering, and packet analysis are core components of incident response.  
- **Attackers rely on predictable behaviors.** Startup execution, DNS queries, reverse proxies, and payload downloads are common patterns analysts must recognize.  
- **This challenge builds confidence.** Even though the second half was difficult, it highlighted exactly what skills I need to strengthen.  
- **Hands-on practice is invaluable.** Working through real artifacts prepares me for more advanced labs and real-world investigations.

---

