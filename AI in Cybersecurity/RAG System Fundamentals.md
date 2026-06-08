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
