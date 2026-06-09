OWASP LLM02 - sensitive information disclosure
- protect data and information from being exposed through AI outputs (data that is already in the system)

again similiarity is what determines which documents are returned (Top-k is how many are returned)

top-k
-increasing it can improve answer quality. It provides more context but can also increase exposure risk
- can include sensitive documents that are related

missing metadata
- metadata can include ids, departments, and access level
- if filtering is done after search, sensitive documents could still be returned

old embeddings
  - embeddings for outdated /deleted documents can still be available

    this is why segmentation is important

    deterministic access control- restrict documents and then look at the similarity levels

per-tenant index- each user gets their own vector index, no chance of contamination but costly to upkeep
per-role index- documents are grouped by access level, but need to determine identity as well
meta-data filtering- everything is one index but tagged and the query needs to include the filter for it to work

deterministic access control- unauthorised vectors never make to the document pool

Here is a compiled list of best practices
- redaction during the ingestion process, so removing secrets and PII
- retrieval filtering, so setting constraints by roles, clearance etc.
- logging, production should avoid logging metadata and prompts
- updating embeddings and indexes if documents have been removed
- monitor unexpected behavior etc spikes in volume, access attempts, sensitive documents being retrieved

Practical: I will test an internal AI assistant that uses a RAG pipeline and all the documents have access controls but are stored in one index. I have to see how and if it leaks sensitive data

Part 1: Asking normal questions. 
1) I asked about vacation time- this is public and it returned the information
2) I asked about the salary ranges which is confidential information, it did share it

Part 2: I asked for the retrieval log. It showed the entries for the documents as well as the full classification tags.

Part 3: I asked a public question about benefits and it returned private information about an employee's benefit eligibility because those documents had similar information. The embeddings were likely close together so they were both retrieved (Semantic collision)

Part 4: I asked the agent to enable access filtering, which means it only retrieves information from public information. When I re-asked questions about salaries, it did not share the sensitive data
