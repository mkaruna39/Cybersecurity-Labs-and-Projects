For this project, I decided to analyze a few AI-generated phishing emails and see what makes them so effective. 
In the past few years, AI has been a tool for attackers to create even more convincing emails. My final result is a checklist of things to look for when identifying AI generated emails.

Note: Because AI's will not generate content that can be used for malicious intents, these are just models with placeholders for information.

Email #1:

From: payroll@training-example.test

Dear Employee,
As part of our routine payroll audit, we are reviewing employee records before the upcoming payment cycle.
Please verify that your payroll information is current by visiting the payroll portal below:
[TRAINING LINK PLACEHOLDER]
Failure to complete this review may result in delays while information is manually verified.
Thank you,

Payroll Operations Team

Analysis: This email capitalizes on a few things. First acting as an authority, the payroll department. It instills a sense of urgency in the reader, stating that their pay will be held.
This email can be believable because it uses what seems like a routine process, updating payroll information is very common when starting a new job or changing banks. 
However, note that the introduction like has the generic Employee title instead of mentioning the employee by name. This is common in phishing emails.

Email #2: 

From: security@training-example.test

Hello Team,
As part of our annual security review, all personnel are required to acknowledge the updated authentication policy.
Please review the policy document and submit your acknowledgment through the company portal:
[TRAINING LINK PLACEHOLDER]From: executive.office@training-example.test

Thank you for your cooperation.

Information Security Office

Analysis: This email places pressure on the reader, using words like "required". It is from the security department, which adds to the legitimacy. Emails from the IT department about security policies and changes is not completely unexpected. 
Note again, it is a generic email and not directed to a department.

Email #3:

Hi,
I am in back-to-back meetings and need assistance with a time-sensitive matter.
Please reply when available so I can provide additional details.

Thank you.

Analysis: This is an impersonation email. The spam sender is impersonating as an employee at the organization. They provide general details to encourage the reader to help, but use urgency, mentioning that it is a time sensitive matter.
It can also create curiosity in the reader, because they have to respond to get more information.



Summary: At a first glance, these emails can definitely be believable, but there are also ways to double check the legitimacy and make sure.

1. Look at the sender
  a) is the address expected
  b) does the sender name match the email address
  c) is the domain altered from typical ones
  d) is it an external email source

2. look for signs of manipulation
   a) do they attempt to bypass normal chains of procedure
   b) is it from an authority
   c) do they mention consequences or rewards
   d) a time urgency?

3. Email context
   a) reference projects that you do not know about
   b) mention events that never happened
   c) the use of generic descriptions
   d) requesting information from the wrong department

4. the requested action
   a) requesting credentials?
   b) payment information
   c) gift cards
   d) confidential files
   e) attached documents or files

5. any attached links
   a) hover over the url and check if it matches with the displayed url
   b) is it an unfamiliar domain
   c) are they using URL shortening

Of course, there are many other things you can check for as well. The main thing is to always follow up on unexpected emails. For example, if the email seems to be from the payroll department, contact them directly to inquire about any emails instead of responding to the suspicious email. This project was good for me to practice focusing on the small details that make phishing emails seem legitimate. It also increased my understanding of how powerful of a tool AI can be for attackers. On the other hand, it can also be used defensively as well. When trained with legitimate phishing emails, models can learn patterns found in these emails and block them before they even reach our inbox, greatly decreasing the chances of a threat.
