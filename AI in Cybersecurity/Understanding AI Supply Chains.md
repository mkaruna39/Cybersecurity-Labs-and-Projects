
supply chain attacks rely on exploiting trust instead of bypassing defenses.

When you are installing packages, you are trusting all of the packages pulled in by dependencies -> long chain of trust

transfer learning- using a model someone else has already training and adapting it 

there are 4 components in the AI supply chain
1) models - pretrained weights and structures
2) datasets- training and evaluation data
3) frameworks- libraries that train and run models
4) dependencies- supporting packages that the frameworks use

   serialised objects- files that execute the code when model.load() is run

   2 ways organizations implement AI
   1) downloading model files
   2) using hosted API services
  
  4 different attack layers for the supply chain

  1) model layer- attackers can embed code within the model files
  2) dependency layer- attackers can upload packages in the dependencies that look legitimate
  3) data layer
  4) infrastructure layer- attackers access repositories, pipelines

  The practical for this room is to evaliate a model before deploying to production. For my initial review, I follow these steps
Model Card tab: Read the documentation.
Files tab: Examine the model weight format. 
Security tab:  Review what the scanner found.
Sidebar: Check the organisation, download count, and creation date.
Community tab: Check for discussions or warnings from other users.

There are 3 issues in the security scan
1) a dangerous import that can execute system commands
2) a function that can invoke subprocesses
3) the organization is not verified

in addition, the model in review uses pickle serialisation -> in conclusion not safe to deploy

  This model is not 
