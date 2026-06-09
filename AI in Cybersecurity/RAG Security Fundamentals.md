Retrieval-Augmented Generation (RAG) is when AI models can reference external documents and materials instead of training data to answer prompts

Because this process happens right at inference time, misleading documents will affect the response. It can also affect the model's overall behavior. This can happen when documents have instructions that override the system.

These are the main components of RAG systems
a) embedding model- converts text into vectors 
b) vector store- stores document embeddings
c) retriever- finds relevant documents for the querys
d) LLM- generates the response using the documents for more context

the risks
- malicious documents coming in
- poisoned documents with misleading information
- the content influencing responses and behavior

the attack surfaces
- untrusted/ outdated and malicious documents can enter the system
- text is converted into vectors, making it hard to inspect manually
- documents are selected by relevance, instead of considering safety
- retrieved documents are injected into the model's context window

retrieval abuse- malicious documents influence model output during retrieval
a) passive poisoning- malicious content is ingested and left in the knowledge base
b) active manipulation- content is created based on common queries

in this situation, detection is more about observing how the systems will behave over time instead of identifying singular outputs

some potential guardrails:
- limiting how retrieved text is inserted
- separating retrieved data from system instructions
- apply heuristics to flag certain patterns

  red flags
  - unexpected retrieval patterns
  - the repeated retrieval of the same documents
  - gradual chenges in response or behavior (key sign of data poisoning)
