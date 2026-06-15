For this lab, there are 3 interactive scenario I have to place shields on an AI architecture diagram to prevent attacks.

Scenario #1
The first scenario is prompt injection. I placed a shield on the prompt and LLM agent. This worked because the prompt is where user input is incorporated into the system instructions

I also did the LLM because the model executes the final prompt. If it receives manipulated instructions, it may follow them and expose sensitive data or misuse tools

Scenario #2- a data leakage attack

I placed shields on the
LLM- The model decides what to include in the response. Without safeguards, it may surface sensitive retrieved data.

Retrieval stage-This component fetches contextual data. If filtering is weak, it may return sensitive information.

Database-Stores embeddings or records that may contain confidential data. If exposed , it becomes a source of leakage.

Scenario #3- Data Poisoning
Retrieval-If poisoned data is stored and later retrieved, it influences model outputs even after deployment.

Database-Stores training or behavioral data. If attackers inject malicious data, it directly affects model behavior.
