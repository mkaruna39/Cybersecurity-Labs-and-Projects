
# Azure Honeypot Lab

## Observing Attacker Behavior in a Controlled Cloud Environment

For this lab, I set up a **honeypot** using an **Azure Virtual Machine (VM)** to safely observe attacker behavior. Honeypots are intentionally vulnerable systems designed to attract malicious traffic, and this setup gave me a controlled way to study **real‑time logs** without putting my actual environment at risk. Using a VM made the process simple and cost‑effective since virtual machines are easy to deploy, pause, and reset. I also created a **virtual network** as part of the architecture to keep everything 


## Lab Architecture

### Resource Group

- **Resource Group:**  
  I created a **resource group** to hold all the necessary machines and resources in the lab.

### Virtual Network

- **Virtual Network:**  
  I created a **virtual network** to easily isolate the traffic without needing physical firewalls and hardware.  
  This network also **hosts the virtual machine**.

### Virtual Machine

- **Virtual Machine:**  
  I created a **virtual machine** inside the virtual network.  
  - No special configurations or requirements were added.  
  - **Size:** Standard E2s v3

---
   
![Resource_Group](https://github.com/user-attachments/assets/912fb4d7-ce79-40dc-86da-0d377570da5d)

## Making the VM Appealing to Attackers

To make the VM appealing to attackers, I had to ensure it was easily accessible from the internet.

### Network Security Group (NSG)

- **Inbound Rule:**  
  I modified the **Network Security Group (NSG)** by creating a new **inbound security rule** that allows **any traffic from any source**.  
  - In a real environment, this is extremely dangerous.  
  - For this lab, it maximized the amount of data and attack activity I could observe.

### Windows Firewall

- **Firewall Disabled:**  
  Within the virtual machine, I turned off the **Windows Firewall** to further reduce protections and increase visibility into attacker behavior.

---


## Initial Testing

To test the machine, I attempted to “log in” using various usernames and passwords.

- These attempts showed up in the VM’s **Event Viewer** as **failed logins**, confirming that logging was working correctly.

---
![Event_Viewer](https://github.com/user-attachments/assets/28e11690-6f68-4287-bdc2-4fe40e9f0ed8)


## Log Forwarding and Analysis

### Log Analytics Workspace

For easier analysis, I set up log forwarding using **Azure Log Analytics**.

- I created a **Log Analytics workspace** within the same resource group.
- System logs from the VM were automatically forwarded to this workspace.

  ### Data Collection

- I ran the VM for **just over 24 hours**.
- This generated **around 1,000 logs**.

### KQL Query

To begin analysis, I ran a **KQL (Kusto Query Language) query** to:

- Get an overview of the different types of logs.
- Count how many entries existed for each log type.

![KQL_Query](https://github.com/user-attachments/assets/8fee1a67-dcb9-4646-8af8-83cfb9e18e53) 


### CSV Export

One feature I really liked about using Azure is the ability to **export query results as a `.csv` file**.

- I exported the KQL results to a CSV table.
- The **top two results** were:
  - **New process creation events**
  - **Failed login attempts**
- From the sheer number of these events, it is safe to assume that the majority of activity came from **bots**.
 
![Csv_table](https://github.com/user-attachments/assets/f26e8ac5-4982-43be-93ec-d1ab65729274). 


### Visualization

- I also formatted the data into a **pie chart** to visualize the distribution of log types.
   ![pie-chart](https://github.com/user-attachments/assets/fc36891b-96e3-4879-91a0-d6e59da23fbe)

## Future Work: Microsoft Sentinel Watchlists

Another idea for further analysis (inspired by **Josh Madakor** on YouTube) is to use **Microsoft Sentinel** to create a **watchlist**.

- You can upload a `.csv` file with:
  - IP address ranges  
  - Region of origin  
- Sentinel can then generate a **geographical map** showing the locations of attackers based on IP addresses.

I think this would be a really cool feature to add. However, since I am a relatively new user, the feature did not seem to be fully integrated for my account yet. I plan to come back in a few weeks and see if I can create the map.

---

## What I Learned

### Honeypots and Attacker Behavior

- **How honeypots work** and why they’re useful for studying attacker behavior.
- **How to safely expose a system** in a controlled environment to attract malicious traffic.

### Azure Virtual Machines and Networking

- **How to deploy and manage an Azure VM** for security testing.
- **The importance of network segmentation** and why virtual networks matter.
- How to use **virtual networks** to isolate traffic without physical firewalls.

### Log Analysis and Threat Monitoring

- **How to safely analyze real‑time logs** from a system that’s intentionally exposed.
- How **cloud environments** make it easier to build security labs and reset them as needed.
- How to use **Log Analytics** and **KQL** for basic threat analysis.

---

## Skills Reinforced from Azure Fundamentals

This project also helped me **relearn and reinforce** many skills from my **Azure Fundamentals** exam:

- **Azure networking basics:**  
  Creating a virtual network helped reinforce **VNet structure**, **subnets**, and **traffic control**.

- **Security, identity, and governance:**  
  Setting up a honeypot required me to think about **isolation**, **segmentation**, and **safe access**.

- **Monitoring and logging:**  
  Observing attacker activity in real time helped me practice **Azure monitoring concepts**, which are a big part of the certification.

- **Cloud benefits:**  
  Using Azure made it easy to **set up**, **pause**, and **delete** resources as needed.

---

## Overall Impact

This project gave me **hands‑on experience** with real attacker activity in a **safe environment**, helping me build practical, **hands‑on security skills**. It strengthened my understanding of:

- **Cloud security**
- **Virtual networking**
- **Threat analysis**
- **End‑to‑end security lab design**

It also helped me practice building a complete security setup—from architecture and exposure to logging, analysis, and future improvements.

