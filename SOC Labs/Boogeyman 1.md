# Boogeyman 1 Challenge — TryHackMe

This is my write-up for the **Boogeyman 1 Challenge** on *TryHackMe*.  
The objective was to analyze a **malicious phishing email** and trace how the threat group gained **initial access** all the way to their **final objective**.

---

# **Email Analysis**

I began by reviewing the malicious email to gather background information:

- *Email of origin*  
- *Recipient email*  
- *Contents of the message*  

Inside the email was a ZIP file containing an “invoice,” but instead of a PDF, it was actually a **`.lnk` file**, which is a common malware delivery method.

---

## **Reformatting & Extracting the Payload**

Before analyzing the `.lnk` file, I had to perform several steps:

1. **Copy the Base64 block** from the `.eml` file into a new file  
2. **Decode** the Base64 into a ZIP file  
3. **Extract** the ZIP using the provided password  
4. **Parse** the `.lnk` file  
5. Identify the **encoded payload** hidden inside the shortcut  

This revealed the malicious script the attacker intended to execute.

---

# **Endpoint‑Style Investigation**

## **1. Identifying Malicious Domains**

To imitate endpoint security analysis, I examined **PowerShell logs**, specifically the `ScriptBlockText` field.

This helped me:

- Identify **domains** used by the threat group  
- Discover the **enumeration tool** the attacker downloaded  

## **2. Backtracking the Payload**

Once I knew the tool name, I backtracked through the logs to find:

- The **full file path** accessed by the attacker  
- The **encoding type** used on the file  

This helped reconstruct the attacker’s workflow.

---

# **Network Traffic Analysis**

For the final task, I used **Wireshark** to analyze network activity.

## **1. Finding C2 Activity**

I identified:

- The **protocol** used  
- The **HTTP method** used to execute C2 commands  

## **2. Discovering Exfiltrated Data**

The exfiltrated file contained **passwords**.

To find this:

- I searched for **POST requests** around the time the file was accessed (based on the JSON logs)  
- I extracted the payload  
- I decoded it in **CyberChef** to reveal the password  

## **3. Extracting an Important Credential**

This was the most challenging part.

I had to:

- Filter for **unique DNS requests**  
- Identify the suspicious domain  
- Open the downloaded file in **KeePass** to retrieve the credential  

This step required careful filtering and attention to detail.

---

# **What Was Learned**

- **Email artefact analysis is foundational.** Understanding `.eml`, Base64, ZIP extraction, and `.lnk` parsing is essential for phishing investigations.  
- **PowerShell logs are gold.** `ScriptBlockText` reveals domains, tools, and attacker behavior.  
- **Network forensics ties everything together.** Wireshark and CyberChef help uncover C2 activity and exfiltrated data.  
- **Pivoting is critical.** Moving between email artefacts, endpoint logs, and network traffic mirrors real DFIR workflows.  
- **Attention to detail matters.** Missing a DNS request or mis-filtering logs can hide critical evidence.

---

# **Why This Is Important**

- **Boogeyman 1 builds core SOC skills.** Email analysis, PowerShell log review, and Wireshark investigation are foundational for incident response.  
- **Real attackers use simple techniques.** `.lnk` payloads, LOLBAS binaries, and encoded scripts are common in real-world breaches.  
- **This challenge simulates real DFIR work.** It forces you to pivot across multiple data sources and reconstruct attacker behavior.  
- **Growth comes from difficulty.** Even though this challenge was tough, it highlighted exactly what skills to strengthen moving forward.

---

