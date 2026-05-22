This is my writeup for the Boogeyman 1 Challenge.

The objective was to analyze a malicious email and follow how the threat group gained initial access all the way up to reaching their final objective.
 To begin analysis...
I first started by looking at the malicious email. I tried to get some background information such as 
-the email of origin
-the email it was sent to 
-the types of contents within the email.
  I noticed that there was an enclosed zip file with an "invoice" that was actually in the form of a .lnk file rather than the expected pdf file.

  Before this, I had to do some reformatting to perform email analysis...
  1. copying the Base64 block from the .eml file into a new file
  2. decode that into a zip file
  3. extract the zip file using the provided password
  4. parse the .lnk file
  5. then, I was able to find the encoded payload

Now, more indepth research, imitating endpoint security..
1. I had to find the domains the threat group used for access. I did this by looking at the .ScriptBlockText column in the Powershell logs and using commands to find unique entries.
2. Note: This command also helped me find the name of the enumeration tool the attacker downloaded.

Now that I had information about the tool, I backtracked to find the full path of the file accessed by the attacker. The logs also showed the encoding type used on the file.

My final task was network traffic analysis.  To answer the questions in this task, I used Wireshark for my queries. 
To start, I found the protcol and http method used by the attacker to execute C2 commands. I also discovered the exfilitrated file had passwords. To approach this task, I looked for POST requests around the time the file was accessed (from the json fike). I then decoded this in CyberChef to get the password.

This last part was more complicated than I was used to, which was finding an important credential. I had to filter for unique dns requests. This file was then opened in keepass to get said credential.

This challenge was definitely one of the more difficult ones I did, and I am not completely solid in my understanding. However, it iwas great practice and I know what to focus on!
