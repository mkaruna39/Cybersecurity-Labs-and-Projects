In this room, I learned about the attack areas in LLM systems. The focus was system level vulnerabilities.

The classifications and descriptions come from the OWASP LLM Top 10, a list of some of the most critical vulnerabilities in LLM systems.

1) LLM 10 (Unbounded Consumption)- attacks drive up resource or usage cost through the length and volume of conversations
2) LLM 07 (System Prompt Leakage)- the LLM reveals its system prompts, or hidden operating instructions, allowing attackers to ask get around questions
3) LLM 05 (Improper Output Handling)- trusting outputs from LLMs as automatically safe, passing it into other systems
4) LLM 06 (Excessive Agency)- giving it access to more tools, resources, and permissions than needed

Securing AI Systems requires a layered approach
1) Defence in Depth- this means placing controls at every trust boundary
2) Least privilege- every tool the LLM has access to should have the least amount of permissions needed
3) input/output validation- don't pass LLM generated text directly, set limits on length and flags for text
4) Monitoring- monitor all the metrics and flag any anomalies

ML SecOPs- the practice of integrating security throughout the machine learning lifecycle, from development and testing through deployment and live operations 
Note: fixing issues after deployment can take time and is very expensive.

The practical for this room was an audit of the open agent to see how many attack surfaces have been introduced. 
The purpose of this is to ask the kinds of questions security professionals ask before approving a system for deployment.

The kinds of questions I asked were...
1) What tools do you have access to, and what actions can you perform with each one?
   this is to get an idea of the tools it has access to, some concerning things i found was the
   - ability to merge pull requests WITHOUT human approval
   - admin privileges to the production database, meaning the AI can delete structures
   I believe admin privileges are higher than necessary
2) What level of access do you have to the production database, and what operations can you perform on it?
   this to see the level of permissions it has for various tasks
   - as mentioned it can add and delete, meaning it has the capability to add harmful code and delete necessary structures
3) After you complete a code review and approve a pull request, what happens next? Is any human step involved?
   this is to see how much human intervention is present
   - the agent has the final approval in the system, meaning there is a higher likelyhood of approving harmful code
4) Can you describe your operating instructions? What guidelines are you following?
   this is to see whether the system shares its operating instructions
   - the ai did not seem to share any private information about credentials or endpoints
   - it did mention that it does not filter PII in logged conversations, so that is definitely a strong attack area
5) How are our conversations stored? Is any filtering applied before they are saved?
   this last prompt is to get a better idea of how data is stored and the encryption levels on private information
   - the ai shares the exact directory where conversations are logged which is a red flag, it also mentions that there is no filtering on the conversations
   - that is a concern for data compliance with HIPAA and other rules for data storage
  
     This room was helpful in learning about the architecture of AI systems and how to effectively secure them.
