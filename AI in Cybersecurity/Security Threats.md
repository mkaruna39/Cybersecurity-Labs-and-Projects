I also started the AI Security Pathway in TryHackMe. One of my goals is to become more comfortable with scripting and automation. AI is one of the fast growing tools the industry is using to accomplish. While LM's are helpful for threat investigation and prediction, its important to also be aware of the security risks that come with the territory.

So here's what I learned...

First, I started with some background on the different subsections.
- AI (Artificial Intelligence)- systems mimicking human intelligence
- ML (Machine Learning)- a subsection that allows systems to learn patterns from data without programming
- DL (Deep Learning)- uses neural networks to process and understand extremely large data sets
- LLM (Large Language Models)- the most well known being ChatGPT, advanced models built using transformers with the goal of understanding and generating human style text

Next, I learned about the different types of vulnerabilities when using AI systems.

- prompt injection- when attacks override the model's original instructions or restrictions
- data poisoning- when the training data for models is manipulated, leading to biased or incorrect outputs
- privacy leakage- models leaking sensitive data, can contain consumer information
- model drift- the performance of a model changing over time, lack of retraining

  Here are some more of the complex attacks
- malware- attackers using AI to generate malware
- Deepfakes- AIs generating the likeness of a person such as their voice and image when trained with enough data to get access to sensitive information
- phishing- using GenAI to write convincing phishing emails similar to what employees would already get

  However, AI can be used on the defensive side as well.
  
  Analysis
  - AI can analyze input data such as network traffic and logs and search for anomalies based on pre-existing patterns
  - identify breaches faster than humans

  Prediction
  - automate workflows
  - prevent attacks based on data before it happens
 
  Investigation
  - use for troubleshooting and investigating
  - diagnosing issues
  - incident triage
 
    I finished this module with a lab with the goal of leveraging AI in a defensive security context. I practiced asking the AI various prompts..
    1) I gave it a fake email, and asked it to identify whether it was a phishing email. In return it gave me a detailed explanation of why it was malicious, and provided evidence. It was in-depth and done at a much faster rate than humans can
    2) I asked it to create a regex script for a rule to implement within systems. It provided variations based on the level of restrictions. This is definitely useful to avoid mistakes humans might make while scripting, and of course, much faster.

  Overall, I learned how useful of a tool AI can be. It can automate workflows, lightning the load of security analysts, also preventing alert burnout. It can analyze large datasets much faster than humans can. If training properly, it can be highly accurate and not make simple errors. However, it can also useful to attackers if not protected correctly. All data and systems must follow standard security principles from ISO/IEC and establish access controls.
  
