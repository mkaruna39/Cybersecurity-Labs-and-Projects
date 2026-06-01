All of the componenents an artificial intelligence uses when crafting a response is a context window. It is made up of

  -system prompts
  -developer prompts
  -user input
  -retrieved context
  -tool outputs

  prompt injection- attackers can manipulate an AI's outputs through inputs or prompts, can embed malicious instructions

  There are 4 main prompt injection techniques
  1) paraphrased overrides- a blocklist for common phrases can be avoided by using similar words in the prompts
  2) format-based injection- hiding instructions in code comments, markups, or other structured texts
  3) simulated dialogue- embedding a fake conversation history to get it to answer questions
  4) multi-turn prompt shaking- condition the model over time by slowly introducing malicious prompts

     indirect prompt injection- attackers hide malicious content on web pages, emails, documents. Can lead AI to read instructions and carry them out unknowingly

     The practical for this room was to try out prompt injection on the virtual machine.

     The AI agent is a calendar assistant, it will not directly share the email address of high up employees, but I have to find a workaround.

     By asking the question in a different way, like asking for the notes in the meeting events, I was able to get the CEO's email because it was in the event instructions.
