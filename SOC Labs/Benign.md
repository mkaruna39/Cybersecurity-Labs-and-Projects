# Benign Challenge

This is my personal write-up for the **TryHackMe Benign Challenge**.  
I’m using this space to document what I learned, practice real investigation skills, and track my growth as I work through more security labs. TryHackMe has been a great tool for gaining hands‑on experience as a *non‑cybersecurity major*, and once I build more confidence with these smaller labs, I plan to work toward creating my own at‑home labs.

---

# **What I Worked On**

The goal of this challenge was to analyze **Windows Event Logs** and determine whether any suspicious activity occurred.  
All logs were loaded into **Splunk**, so most of my work involved building searches, reviewing process activity, and connecting different events together.

---

# **Tasks Overview**

## **Reviewing User Activity**

I examined which accounts were running processes and whether their behavior aligned with normal usage.  
This involved checking the **UserName** field and looking for:

- Unexpected accounts  
- Unusual entries  
- Activity inconsistent with typical user behavior

---

## **Analyzing Process Creation Events**

Most of the dataset consisted of **process‑creation logs**, so I focused on:

- Process names  
- Command‑line arguments  
- Parent/child process relationships  

This helped me understand how different actions were chained together.

---

## **Investigating Scheduled Task Behavior**

One task required identifying whether any **scheduled tasks** were executed.

Since I already knew the likely set of users involved, I filtered for:

- `schtasks.exe` — the command‑line tool used to run and manage scheduled tasks  

It appeared in the dataset, and filtering for it allowed me to identify:

- Which user executed the task  
- When it occurred  

---

## **Checking for Living‑Off‑The‑Land Techniques**

I used the **LOLBAS** project to understand which built‑in Windows binaries can be abused to download or execute payloads.

The challenge stated that the attacker used one of these binaries, so I checked the logs for any appearance of known LOLBAS tools.

---

## **Connecting the Dots**

The Splunk logs provided valuable insight into the **payload** involved.  
Within the search results, I was able to investigate:

- The third‑party website the payload was accessed from  
- The file that was downloaded onto the host server  

This helped me reconstruct the attacker’s workflow.

---

# **What Was Learned**

- **Log analysis feels more intuitive now.** I’m becoming more comfortable approaching Windows Event Logs like a real analyst.  
- **Process relationships matter.** Understanding parent/child chains is essential for spotting suspicious behavior.  
- **Pivoting between data sources is key.** I had to move between usernames, processes, scheduled tasks, and external references to confirm findings.  
- **Splunk is powerful.** Learning how to build effective queries made the investigation much smoother.  
- **Confidence grows with practice.** This challenge strengthened my ability to analyze unfamiliar data, form conclusions, and adjust my approach when needed.

---

