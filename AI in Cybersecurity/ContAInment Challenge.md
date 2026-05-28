This is my writeup for the ContAInment Challenge in the AI Security Pathway. 
The situation is that a ransom note was discovered on the workstation of an employee. Data was then stolen and exfilitrated. 

My goal is to...
-investigate the incident
-identify how the attacker got access
- recover the stolen data

I will do this with the help of an AI security agent. The hint given to start off is that the attacker left some important information in a pcap file. So, I navigated to the Documents directory and ran the command ls to see what all was available.
There were 4 dates, with many pcap sessions inside each data. To narrow down the search, I ran ls -la for two reasons, to see what was accessed more frequently and the size of the files. Files that hold hexes and data take up a lot more storage.


Once I found the correct pcap, the AI agent reassembled it using the appropriate tool. Within the reassembled pcap file, I found the password to unencrypt the project zip file.
Within the zip file, there was a "flags" text file. I had the ai agent use libery prime to find the flag. 

This one stumped me up, the AI agent misinterpreted some of my queries and got confused with the paths if they were not exact. It also got confused struggled finding some of the files even with the full path.

I think this demonstrates how powerful of a tool it can be if these bugs and issues can be ironed out.
