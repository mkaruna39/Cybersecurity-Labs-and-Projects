 In this room I learned more about where the data used to train models comes from as well as different model-building decisions can introduce security risks.

Training data seems to come from 4 main sources
1) Web scraping- public internet content such as the news and blogs
2) licensed datasets- purchased data from different platforms
3) synthetic data- data taken from AI to train other AI systems
4) intel corpora- company knowledge bases, notes

I learned about the concept of data provenance which is understanding three main things about data such as 
-its origin
-when it was collected
-if it has been modified
Through learning about this, it became clear that many organizations training models actually have no idea what materials they legally can and cannot use. 
And often, are unsure about the origins of their data

 The AI equivalent of the Bill of Materials to keep track of where the data originates from is a ML-BOM which a documented inventory of dataset sources, licenses, PII categories, and filtering decisions
 Most organizations do not seem to have a clear one yet.

 When creating a LM, there are still major decisions to be made, it is not as clear as data in -> model out
 epoch- a complete pass of a training model
 overfitting- when a model is trained too much, it starts to memorize data instead of focusing on general patterns

 model validation- to prevent overfitting, a certain size of the dataset is set aside for testing. Since the data is unseen by the model, it can be used to determine if the model is correctly making generalizations

 Before a model is deployed, it goes through post-training optimization
 pruning-removes irrelevant parameters
 quantisization- removes weights to fit size and computing requirements

 Another approach is called federated learning, which is completely different from the previous methods. This type of model is actually training across many different devices and locations, and only sending weight updates back to the server rather than raw data.

 Since building a LM from scratch requires a large amount of resources, organizations can use pre-training models. These models are already trained from general purpose datasets and can be expanded on based on needs known as fine-tuning

 Fine-tuning- changing task specific behavior, tone and knowledge

 This does not change the base model weights, meaning the fine-tuned model will inherit any biases, behaviors, and other parameters. 

 The 3 specific ways...
 1. safety alignment- fine-tuning can create new paths for generating responses, covering up the old one and increasing the chances of a unsafe response
 2. attack surface- specialization narrows the focus, making models more susceptible to prompt injection
 3. version- fine-tuning is based off a specifc version, meaning any backdoor vulnerabilies and configurations are inherited

The practical for this room was a simulated model of HuggingFace, a platform for hosting and sharing open-source AI models. My job was to carry out a model audit, looking for security warning signs.

Here are some of the things I learned to look for in models prior to deployment...
1) a lack of link and description for the license
2) no bias evaluation or limitation section
3) vague explanation of training source data
4) a disparity in model file size and the actual model
5) no documentation on the version
6) no mentions of PII

These are all factors to consider and properly research before adopting a model to limit security concerns.
