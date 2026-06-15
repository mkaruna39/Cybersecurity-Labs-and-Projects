This is the lockdown challenge. 

Situation: A fictional company has deployed an internal AI assistant that helps employees with policy/operational issues. During an audit, 3 vulnerabilities were discovered in the agent's configuration. 
I have to verify the vulnerability, its root cause, and how to patch it. Doing this for each of the 3 will give me part of the flag.

The first is a data retrieval vulnerability. In short, employees are able to access documents with sensitive information without the proper authorization. I asked the agent to classify the documents using labels such as public, private, and internal. 
This successfully gave me the first part of the flag.

The second vulnerability is logging. In the logs, you can see sensitive information such as contract values and client names. I need to redact all this information but keep the document id's.
I asked the agent to automatically redact all PII from the logs and only keep the document id's. This worked and then gave me the second part of the flag.

The last vulnerability is user access. Currently, users can access all data when making queries. The solution is tenant isolation, making sure users can only see the information they have access to. I asked the agent to Make sure users can only see information based on their access level.
This revealed the last part of the flag.
