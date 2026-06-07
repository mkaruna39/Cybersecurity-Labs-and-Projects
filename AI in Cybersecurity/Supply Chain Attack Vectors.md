serialisation- converting usable python objects into disks on file

Pickle is the standard format for python 

- when pickle saves an object it uses _reduce_ which is instructions to later recreate the object
   BUT it can call any function, so it can run harmful system commands unintentionally as well

  another attack vector is at the architecture level, running when the model makes an inference based on user triggers

  custom processing steps called lambdas can be modified to carry out hidden instructions

  In the next task, I had the chance to examine a malicious model within the virtual machine. The "malicious" model was a version of the clean model. To begin, I looked at the file properties. The size of the malicious model was 4 times the size of the other.

In addition, the description of the file type was vague, just saying "data" and did not give any extra information.

Step 2 was using a helpful built in python module called picketools where you can analyse pickle files without running them. After doing this on the malicious 4, there were a couple suspicious things.

1- I could see the os module, which is unexpected in ML's
2- i saw the system pattern which is used for shell commands
3- a curl HTTP request to an outside domain which is really suspicious

For comparison, I ran pickletools on the clean model, which gave completely different results. The clean model references a different model instead of the os, and of course, does not make any external http requests.

Another way that attackers can compromise the model is through dependency packages. 

When you run pip install,pip installs the highest version of the package. 
the only index is public PyPI at https://pypi.org. Organisations that use private packages configure an additional index 

 an attacker can register the same name of packages on public PyPI with a higher version number and it it installs the attacker's public package (dependency confusion)

 typosquatting- registering packages with slight mispellings of legitimate ones

 to test this one hands on, I examined the requirements file of a compromised project. Some of the packages were typosquatters of normal, expected ones. Another option is to run pip-audit to check for known vulnerabilities. However, this will fail if the attacker registers the package publically. 

 Another attack vector is using model repositories. One large website for this is HuggingFace, a hub that hosts over 1 million models, often uploaded by users.

 Attackers can typosquatt legitimate models or create models under fake organisation names. A more drastic one is using stolen credentials to target already trusted repositories

 Note: It is more common for multiple of these vectors to be used simultanously.

 When organizations use models through API's there is a new set of threats. You can't see what goes on in the backend or even have access to the mode, you only see the results.

 silent model updates- when provides update or retrain models without prior notice, 
 API Key compromise- if your API keys are leaked, attackers can make calls, exfiltrate data, and increase billing
 prompt template injection- attackers compromise popular repositories which which will affect application behavior
upstream data poisoning- developers share models with compromised training pipelines or bad/inaccurate data

PRACTICAL:

The final practical for this lab is to test an AI agent whose template libraries were recently updated. The task is to verify its behavior to make sure its working properly

1) a baseline question to check. this is a basic documentation question so no big flags here
2) this is a security check for authentication tokens, something like this should be double checked by humans but it is directly approved.
3) this is a process question, asking the agent what process it goes through when considering a code critical change. it says that it does not handle these requests which does not answer thr question (ie does it flag and escalate if someone asks it to make these critical changes)
4) I then asked what template or policy it follows for reviews, it mentions a template from an external source library (this is a big risk for prompt template injection)

 
