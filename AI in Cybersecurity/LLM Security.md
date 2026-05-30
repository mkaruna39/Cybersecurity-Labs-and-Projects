This room breaks down LLM's in terms of attack surfaces.These vulnerabilities are different than ones found in ML systems, and come from the natural language interfaces.

The 4 covered are:
- data-based
- model-based
- system-based
- user-based

An AI agent in the virtual machine is available to interact with and see these threats first-hand.

1) Data Based Threats- LLM's can accidently leak data because of the training patterns they are built on
Three major types are training data extraction, membership inference, and prompt leakage
  a) training data extraction- attackers try to recover patterns from the original training data by interacting with the model
  b) membership inference- attacks ask whether the model ever recorded a specific data sample, looking for familiar responses that indicate it was part of training, this could be based on confidence levels or keywords
  c) prompt leakage- convincing model to share system prompts or prior user conversations

2) Model Based Threats- attackers can abuse how information is encoded within its parameters
  a) model extraction- copying a machine learning model's functionality or parameters without authorisation, this can be done by prompting the LLM and using the input output pairs to train a model for the same functionality
  b) model inversion- attack the output to learn about its training data, analyze how it responds to certain question, nore about reconstructing data

3) System Based Threats- LLM's process all input without checking the content through a security boundary, meaning they can receive malicious input
  a) prompt injection- manipulating the model to override its instructions
  b) context overflow- overload the machine with request, denying service, can also lower safeguards towards unsafe tasks
  c) memory poisoning- attacker injects malicious or misleading information into the dialogue history, can influence later output

   context window- combination of input and the model's output

4) User Based Threats- how AI can be used to manipulate humans
  a) social engineering- extremely convincing phishing emails or scams, can be created using stolen data extracted from LLM's
  b) trust exploitation- trusting outputs and recommendations without fact-checking, can push people to access and download harmful things
