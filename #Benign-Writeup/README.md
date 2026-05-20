This folder contains my personal writeup for the TryHackME Benign Challenge. I’m using this space to document what I learned, practice real investigation skills, and track my growth as I work through more security labs. TryHackMe has been a great tool to get more exposure as a non-cybersecurity major. Once I build up confidence in these smaller labs, I will work towards my own at-home labs.



What I Worked On:
The goal of this project was to analyze Windows event logs and figure out whether anything suspicious was happening. The logs were loaded into Splunk, so most of my work involved building searches, reviewing process activity, and connecting different events together.

Tasks Overview:

Reviewing user activity:  
I looked at which accounts were running processes and whether their behavior matched what I’d expect from normal usage. I did this by checking the UserName list and looking for unexpected names and entries.

Analyzing process creation events:  
Most of the logs were process‑creation entries, so I spent time examining process names, command‑line arguments, and parent/child relationships.

Investigating scheduled task behavior:  
One part of the challenge pushed me to identify if any scheduled tasks were executed. Since I had already been given the likely set of users, I used schtasks.exe in my query, since its the command-line tool used to run and managed scheduled tasks. In this dataset, it did appear and filtering for it helped my identify which specific user executed the task and when.

Checking for “living off the land” techniques:  
I used the LOLBAS project to understand which built‑in Windows binaries can download or execute payloads. In the information provided, I was told the attacker used one of these binaries. Then I checked the logs to see if any of those binaries appeared.

Connecting the dots:  
The Splunk logs were incredibly useful in learning more about the payload. Within the search result, I was able to look more into the third-party website the payload was accessed from as well as the file that was downloaded onto the host server.

What I Learned:
This challenge helped me get more comfortable with approaching log analysis like a real analyst. I learned how to break down Windows Event Logs and follow process relationships. I also had to pivot between multiple points of data, much like analysts do to confirm actual threats. Overall, it strengthened my ability to investigate unfamiliar data, form conclusions, and adjust my approach.
