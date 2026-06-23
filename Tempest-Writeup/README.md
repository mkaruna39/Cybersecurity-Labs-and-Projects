This is my write-up for the TryHackMe Tempest challenge! 

Objective:
- To analyze and handle various artifacts from a "compromised" machine. This was mostly done using Sysmon, which are logs that track and system activities and changes.

1. I had to find the SHA256 hashes. Hashes are a good way to check whether a file has been tampered with. I was familar with the windows powershell command for this already. Using the simple command Get-FileHash -Algorithm SHA256 .\filename I was able to answer those questions easily.
  
2. For easier analysis,the sysmon file had to be converted to CSV from ETVX. I had to do some research for this command, but it emphasizes the importance of standardizing the format of logs so analysts can properly make sense of the data.
   
3.I was then able to open this CSV file into TimelineExplorer, a Windows application used to sort and filter CSV files.

4. I also used a new tool called Sysmon View. Upon importing the xml file of the Sysmon logs, I was able to see insighful visualizations of the different processes and activities.

   Now into further analysis...
   I had to find the name of the malicious doc that created the process. I knew the Sysmon ID for file creation is 11 and that it is a .doc so after using these filters in TimelineExplorer, I was able to find the exact file name and host machine it was downloaded on.

   Next, I had to find the IPv4 address of the domain. This was relatively easy. I used the Sysmon ID for DNS query which is 22, and then I filtered for where the ProcessID was relevant.

   After this, things began to become a little more difficult. Most of the skill was knowing what keywords to search for. For example, a big task was finding the command executed by the payload. Before doing this, I had to find the full path of the payload in the logs using the EventID 11 for file creation and the keyword startup, since in the previous task, it had been determined that execution happened upon startup.


   The next set of tasks was done using Wireshark. This was useful for me to practice my commands. I already knew the host, so I was able to query for the URL to the payload. The rest of the analysis for that task was relatively simple, just extracting basic information from the wireshark packet.

The main part of the next task is analyzing a sensitive file. I found the packet in wireshark using the domain name, and decoded the data in cyberchef. I then enumarated the list of ports on the desktop to see if any of them could have been used for remote command execution.

The next step is to find the the reverse socks proxy the attacker used to create the connection. I actually had to look up a hint for this one. Once, I looked up "socks" in the logs, I found it.

I definitely want to redo the 2nd half of this one for better understanding. This is one of the more difficult challenges I have done. I had to spend some time researching the correct keywords.
   
