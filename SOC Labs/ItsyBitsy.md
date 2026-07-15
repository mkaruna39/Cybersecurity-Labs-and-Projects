# ItsyBitsy Challenge 

This is my personal writeup and thought process for the Itsy Bitsy Challenge from TryHackMe

This challenge gave me more practice working with **Elastic**, especially building and refining queries.  
The scenario involved investigating an **IDS alert** that indicated possible **C2 communication** originating from a user in the **HR department**.

---

# **Initial Context**

- The suspicious activity occurred in **March 2022**  
- Logs were already ingested into **Elastic**  
- My goal was to identify:
  - The **source IP**
  - The **binary** used for downloading a file
  - The **filesharing website** accessed
  - The **downloaded file**
  - Any relevant **IOCs**

---

# **Log Volume Analysis**

To begin, I adjusted the time window to cover **March 2022**.  
After filtering, Elastic showed:

- **1482 logs** for the time period

This gave me a sense of how much data I’d be working with.

---

# **Finding the Associated IP Address**

On the left-hand side of Elastic, I used the **source_ip** field aggregation.

- I selected the IP with the **highest count**, assuming it was the attacker’s source  
- This assumption was correct — it matched the C2-related activity

---

# **Identifying the Legitimate Binary Used for Downloading**

The challenge stated that the attacker used a **legitimate binary** to download a file from the C2 server.

Steps:

1. Filter logs by the **source_ip**  
2. Inspect the **user_agent** field  
3. The binary name appeared clearly inside the user_agent string

This revealed the tool abused for downloading the payload.

---

# **Finding the Filesharing Website Used by the Infected Machine**

Following the same pattern:

1. Filter logs by **source_ip**  
2. Look at the **host** field  
3. Identify the filesharing domain  
4. Scroll down to the **uri** field to obtain the **full link**

This gave me the exact website accessed by the compromised machine.

---

# **Identifying the Downloaded File**

Using the link found in the previous step:

- I visited the website (safe because this was inside a VM)  
- The **challenge flag** was displayed clearly on the page  
- This confirmed the downloaded file and its contents

---

# **What Was Learned**

- **Elastic requires precise filtering.** Understanding fields like `source_ip`, `host`, `uri`, and `user_agent` is essential.  
- **C2 activity often hides behind legitimate binaries.** Attackers frequently abuse built-in tools to avoid detection.  
- **Log patterns matter.** Repeated filtering by IP helped reveal multiple stages of the attack.  
- **Elastic differs from Splunk.** The formatting and field structure require slightly different query habits.  
- **Hands-on practice builds confidence.** Even though this challenge was short, it strengthened my ability to navigate Elastic efficiently.

---

# **Why This Is Important**

- **Elastic is widely used in SOC environments.** Comfort with its interface and query structure is essential for real-world investigations.  
- **C2 detection relies on log correlation.** Identifying binaries, domains, and downloaded files helps reconstruct attacker behavior.  
- **Understanding user-agent abuse is critical.** Many attackers hide behind legitimate tools to blend in with normal traffic.  
- **Short challenges reinforce core skills.** Even simple exercises improve log analysis speed and accuracy.  
- **This builds a foundation for more advanced Elastic investigations.** Each challenge improves query intuition and analytical workflow.

---
