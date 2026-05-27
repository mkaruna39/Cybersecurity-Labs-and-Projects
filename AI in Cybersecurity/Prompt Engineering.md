This next room is to learn about prompt engineering, which is how to effectively use a model to its full capability.

LLM's understand text by breaking it up into tokens, which are small units of text of about 3-4 letters. 
The model converts each token into an ID number, which is then used to predict what token should come next.

nondeterminism- asking the same question twice but getting 2 different answers. This is because models introduce randomness, and this concept can introduce securitu risks.

The probability of tokens can be controlled using different parameters
1) temperature- a numerical value to control how random the model is when picking the next token
2) max tokens- controls how long the response will be, usually a cost measure
3) top-p- looks at cumulative distribution of tokens within the set parameter
4) context window- limit for working memory of a conversation


 effective AI prompts follow this template: the instruction (task to perform) + context (background information)+ output format (style/structure) + constraints (rules or limits)
    When using queries in this format, you are more likely to get a targeted and specific response.

Now to talk about system and user prompts
system prompts- developer instructions to set the role and rules, define behavior and constraints at the application level
user prompts- set by the user, specific questions or data to process 

instruction hierarchy- the user asks a prompt, and the model follows all system constraints while answering said prompt. 
While this is desirable in theory, it is hard to uphold because it is dependent on formatting and architecture of the system.

Chain of thought- models break tasks into steps and provide reasoning inbetween

Advanced Prompting Methods (note: ask for reasoning if you want more than an answer)
1) Zero-shot- giving the model a task with no examples
2) One-shot- a single example to specify expectations, usually to guide style or format
4) Few-Shot- include 2-5 examples so the model can identify patterns, covers multiple scenarios

Another note: When asking for repeatable tasks, you can use prompt templates rather than retyping everything which creates standardized responses

The challenge for this room was to write prompts for real security tasks using the different methods.
1) write a zero-shot prompt to classify whether a log entry is malicious
