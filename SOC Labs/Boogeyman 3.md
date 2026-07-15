# The Boogeyman 3

This is my write-up for the **final capstone challenge** in the *SOC Level 1* pathway.  
In this scenario, the threat actor **Boogeyman** infiltrated the organization through another **phishing email**, and I was given access to logs through the **Elastic Stack (ELK)** to investigate the incident.

---

# **Situation Overview**

The challenge provided an estimated **date range**, which helped narrow down the volume of logs to review.  
My objective was to reconstruct the attacker’s actions, identify persistence mechanisms, privilege escalation, credential theft, lateral movement, and malicious downloads.

---

# **Investigation Steps**

## **1. Identifying the Initial Payload**

I began by searching ELK for the **payload filename** mentioned in the challenge.  
This led me to the earliest event associated with:

- **PID:** 6392  
- **Action:** Initial execution of the malicious file

Further log entries showed the attacker attempting to **copy the payload** to another location.  
Another entry confirmed it successfully executed using:

- **Executable:** `rundll32.exe`

---

## **2. Persistence Mechanism**

The challenge hinted that the payload created a **scheduled task** for persistence.  
I searched ELK for:

- `rundll32`  
- scheduled task creation events  

This aligned with the expected behavior of the malware.

---

## **3. C2 Connection Discovery**

It was also stated that the payload established a **C2 (Command and Control) connection**.

To find the related IP address, I queried ELK for:

- **PowerShell events**  
- network-related command execution  

This helped me backtrack to the exact event where the C2 connection was initiated.

---

## **4. Privilege Escalation**

The next task indicated that the attacker gained **administrator access**.

By searching for the **scheduled task name** again, I located the process used for **privilege escalation**, confirming elevated access was obtained.

---

## **5. Credential Dumping via GitHub**

The attacker attempted to download a **credential dumping tool** from GitHub.

By searching ELK for:

- GitHub URLs  
- download events  

I found the **exact link** used to retrieve the credential dumping utility.

---

## **6. Lateral Movement Using Stolen Credentials**

The attacker successfully used stolen credentials to access another machine.

I queried ELK for:

- the downloaded file name  
- `user*` patterns  

This revealed:

- **The credentials used**  
- **The target machine accessed**

While searching for a file the attacker attempted to enumerate, I also discovered **another set of stolen credentials**, including:

- **Username**  
- **Hostname** of the compromised machine

---

## **7. Parent Process Identification**

To identify the **parent process** of the malicious command, I filtered for:

- `event.code = 1`  
- the previously discovered hostname  

This search also revealed the **username** associated with the newly dumped credentials.

I attempted to locate the corresponding hash but could not find it — a reminder of how easy it is to miss details in large log sets.

---

## **8. Third Account Used**

Another account was used by the attacker.  
I found it by filtering for:

- parent command lines  
- `event.code = 1`  
- the same hostname  

This confirmed the attacker’s continued lateral movement.

---

## **9. Final Malicious Download**

The last task required finding a link to another file the attacker attempted to download.

Due to the large number of logs, I used a more targeted filter:

- search for `http`  
- exclude the known hostname  
- filter by event code  

This quickly surfaced the correct malicious download URL.

---

# **What Was Learned**

- **Effective log filtering is essential.** Choosing the right keywords dramatically reduces noise and speeds up investigations.  
- **Attack chains leave consistent patterns.** Privilege escalation, persistence, credential dumping, and lateral movement all produce recognizable log artefacts.  
- **ELK requires precision.** Even small mistakes in filters can hide critical evidence.  
- **Iterative searching is normal.** Revisiting earlier clues often reveals new connections.  
- **Threat actors reuse techniques.** Rundll32 execution, scheduled tasks, PowerShell commands, and GitHub downloads are common attacker behaviors.

---

# **Why This Is Important**

- **Log analysis is a core SOC skill.** Real-world incidents rely heavily on SIEM tools like ELK to reconstruct attacker activity.  
- **Understanding attacker workflows improves detection.** Recognizing how payloads behave helps analysts build better alerts and correlation rules.  
- **Precision reduces investigation time.** Knowing how to filter effectively prevents analysts from getting overwhelmed by large log volumes.  
- **This capstone simulates real DFIR challenges.** It reinforces practical skills needed for incident response, threat hunting, and SOC operations.  
- **Mastering log-based forensics builds confidence.** The ability to trace an attacker across multiple systems is foundational for advancing in cybersecurity.

---
