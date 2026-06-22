For this lab, I set up a honeypot using an Azure Virtual Machine to safely observe attacker behavior. 
Honeypots are intentionally vulnerable systems designed to attract malicious traffic, and this setup gave me a controlled way to study real‑time logs without putting my actual environment at risk.
Using a VM made the process simple and cost‑effective since virtual machines are easy to deploy, pause, and reset. 
I also created a virtual network group as part of the architecture to keep everything organized and segmented.


My architecture for this lab is as follows...

1) I created a resource group to hold all the necessary machines and resources in the lab.
2) I created a virtual network group to easily isolate the traffic without needing firewalls and hardware. This would also "hold" the virtual machine.
3) I created a virtual machine in the network group. I didn't do any special configurations or requirements. I used the Standard E2s v3 size machine.
   
![Resource_Group](https://github.com/user-attachments/assets/912fb4d7-ce79-40dc-86da-0d377570da5d)

In order to make the VM appealing to attackers, I had to make sure they would be able to access it. The first way I did this by modifying the Network Security Group. I created a new Inbound security rule to allow any form of traffic from any organization.
In real life, this is extremely dangerous for systems, but I wanted the chance to examine as much data as possible.
Next, within the actual virtual machine, I turned off the Windows Firewall.

To test out the machine, I tried to "login" using various usernames and passwords. This successfully showed up in the VM's Event Viewer as failed logins
![Event_Viewer](https://github.com/user-attachments/assets/28e11690-6f68-4287-bdc2-4fe40e9f0ed8)


For easier analysis, I worked on a way to forward the logs. Within the Log Analytics on Azure, I created a new workspace within the resource group. This automatically forwarded all system logs to the workspace. 
Then I started on some simple analysis. I ran the VM for just over 24 hours, which accumulated about 1000 logs. To begin, I ran a KQL query to get an idea of the different types of logs and how many entries there were
![KQL_Query](https://github.com/user-attachments/assets/8fee1a67-dcb9-4646-8af8-83cfb9e18e53) 

One thing I really liked about using Azure is that I was able to easily export the query result as a .csv file. 
![Csv_table](https://github.com/user-attachments/assets/f26e8ac5-4982-43be-93ec-d1ab65729274). The top 2 results was a new process being created, as well as failed logins. From the sheer number, it is safe to assume that the majority is bots. 

I also formatted this table in the form of a pie table as well! ![pie-chart](https://github.com/user-attachments/assets/fc36891b-96e3-4879-91a0-d6e59da23fbe)

Another idea for further analysis (from Josh Madakor) on Youtube is to use Microsoft Sentinel to create a watchlist. Essentially, you can upload a .csv file with ip address ranges and the region of origion. Sentinal will spit out a geographical map with the location of the attackers based on the IP address.

I think this would have been a really cool feature, however, since I am a relatively new user, the feature didn't seem to be integrated yet for me. I want to come back in a few weeks and see if I can create the map!

Overall, this is what I learned:
-How honeypots work and why they’re useful for studying attacker behavior
-How to deploy and manage an Azure VM for security testing
-The importance of network segmentation and why virtual network groups matter
-How to safely analyze real‑time logs from a system that’s intentionally exposed
-How cloud environments make it easier to build security labs

This project gave me hands‑on experience with real attacker activity in a safe environment, helping me build hands on skills. 
It strengthened my understanding of cloud security, virtual networking, and threat analysis. 
It also helped me practice building an end‑to‑end security setup 

I was also able to relearn many of the skills I developed from my Azure Fundamentals exam: 

Azure networking basics- Creating a virtual network group helped reinforce VNet structure, subnets, and traffic control.

Security, identity, and governance- Setting up a honeypot required me to think about isolation, segmentation, and safe access 

Monitoring and logging- Observing attacker activity in real time helped me practice Azure’s monitoring concepts, which are a big part of the certification.

Cloud benefits -Using Azure made it easy to set up, pause, and delete resources

