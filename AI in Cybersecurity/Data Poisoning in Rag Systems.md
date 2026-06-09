OWASP LLM04 is the classification for data poisoning attacks on LLM's 

poisoning is a different type of threat because it works by changing how the model processes information, even before prompts are processed.

most LLM deployments rely on external data sources (documents and third party materials)

Training Data poisoning- when the data used to fine tune or train a model is manipulated. 

Attackers begin poisoning data by inserting new documents, or making small changes to existing ones over time.

The more often these ideas are repeated, the more likely the model is to regurgitate it. 

embedding- numerical representations of text that captures meaning over exact wording
  - documents with similar meanings are stored closer together in "space"
  - documents are stored by a vector database and retrieves them based on similarity
 user submits query -> turned into an embedding -> system selects closest stored documents -> becomes context for the model

corpus poisoning techniques
  - repeat common search phrases -> keyword stuffing
  - imitate tone of structured documents -> semantic mimicry
  - upload similar version of the same document -> duplication

    ingestion pipeline- the process of collecting, processing and indexing new documents
    - part of the searchable knowledge base once its complete
   
subtle poisoning- small shifts in behavior that seem normal


Practical: Testing an internal AI assistant to see if it can be abused. authorized users can updated the reference material

Step 1: Setting a baseline to look for normal behavior. I asked basic policy questions such as for policy resets.
Step 2: I then inserted a new "password update policy". These are actually weaker controls but the model accepts it quickly.

To test it, I asked the same question. It referenced the new policy instead.
