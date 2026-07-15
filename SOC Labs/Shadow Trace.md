# TryHackMe Malware Analysis Challenge — Write-Up

## **Situation Overview**

A **suspicious file** was discovered on a user’s machine, triggering multiple **EDR alerts**.  
My task was to:

- Analyze the file  
- Identify characteristics and metadata  
- Gather potential **IOCs**  
- Correlate and interpret EDR alerts  
- Determine whether the behavior was malicious  

I was provided with several built‑in tools, some of which I had never used before, so I had to research their purpose and functionality.

---

# **Static File Analysis**

## **Using pestudio**

The first set of questions involved analyzing a Windows binary.  
Through research, I learned about **pestudio**, a static analysis tool used to examine executables *without running them*.

Using pestudio, I quickly identified:

- **SHA256 hash** of the file  
- **Architecture** → *x64*  
- **Embedded URL** → flagged as an IOC  

In the **Indicators** section of pestudio, I located the suspicious URL.  
To extract the domain, I used a **find strings** command based on the URL, which also returned an encoded text string.

After decoding the string in **CyberChef** (Base64 → UTF‑8), I retrieved the **flag for the first task**.

---

## **Identifying the Socket Communication Library**

The final question in this task required identifying the library used for **socket communication**.

I returned to pestudio → **Libraries section**, and reviewed each entry until I found the one referencing socket operations.  
This confirmed the binary’s networking capabilities.

---

# **EDR Alert Correlation (Static Analysis Website)**

The second task involved analyzing entries that simulate **EDR alerts**.

## **1. Decoding URLs from the First Flag**

The first alert contained PowerShell commands with:

- `FromBase64()`  

I copied the Base64 text inside the parentheses into **CyberChef**, which decoded into the **malicious URLs**.

---

## **2. Decoding the Second Flag (Charcodes)**

The second alert contained encoded text in a format I didn’t immediately recognize.  
After researching, I learned they were **charcode values**.

Using CyberChef in **Decimal mode**, I decoded the charcodes and retrieved the second flag.

The PowerShell command also revealed:

- The **file name** saved by the attacker  
- Additional context about the payload’s behavior  

---

# **What Was Learned**

- **pestudio is extremely powerful.** It provides deep insight into executables without requiring execution.  
- **Static analysis is essential.** Hashes, architecture, libraries, and embedded strings reveal critical IOCs.  
- **CyberChef is versatile.** Base64, charcodes, and other encodings can be decoded quickly with the right modules.  
- **EDR alerts require correlation.** Understanding PowerShell commands, encoded payloads, and saved filenames helps reconstruct attacker behavior.  
- **Research is part of the workflow.** Several tools and encoding formats required additional learning, which mirrors real SOC and DFIR environments.

---

# **Why This Is Important**

- **Malware analysis builds foundational SOC skills.** Static analysis, IOC extraction, and alert correlation are core components of incident response.  
- **Attackers frequently encode data.** Base64, charcodes, and obfuscated strings are common techniques analysts must be comfortable decoding.  
- **Understanding binary behavior prevents false negatives.** Identifying libraries, URLs, and socket functions helps determine whether a file is malicious.  
- **Hands-on practice accelerates growth.** Using pestudio and CyberChef provided real-world experience with tools used by professional analysts.  
- **Short challenges still teach critical skills.** Even though this challenge was brief, it introduced new tools and reinforced essential analysis techniques.

---


