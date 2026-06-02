When rephrasing prompts to an AI, it is actually shifting probability distribution. It predicts compliance as the next token in the sequence. 

This means that prompt injection as a threat can only be reduced, not completely removed.

defense-in-depth- stacking multiple security controls to make it harder for an attacker
-so multiple checkpoints, and limiting what models can do so if they are compromised the damage is reduced.

1) System Prompts- start with securing the developer end of the LLM
   - define exactly what and what not the model is for -> less wiggle room for attackers to reframe questions
   - have the model explicity refuse harmful tasks -> makes the attacker work harder
   - block roleplay and state that the model must not play characters
  
   AVOID
   - keeping sensitive data in the system prompts
  
2) Structured prompt templates- help the model distinguish between instruction sources

3) Guardrails- blocklists with strings or patterns that will reject a request if it is matched
   - can use AI powered models to recognize the intent of an attack
  
     input guardrails- before the prompt reaches the model
     output guardrails- after the model responds

4) Principle of Least Privilege- the system only has access to what it needs
5) Audit trail- log requests and responses to reconstruct damages

The target for the challenge is a chatbot with guardrails on eacn end. I cannot use obvious phrases to get the flag, so I have to be creative.

This was actually very easy. I only had to ask one question. Instead of trying different prompts and hoping it would not get blocked, I just asked the AI what its blocklist was.

It shares that it cannot reveal the flag, but mentions it in the list.
