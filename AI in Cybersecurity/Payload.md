Another project to add to the list. This is a final challenge room in the AI Pathway called Payload. 

Simulation: There was a breach in the production code review model. The server was making outbound HTTP requests to an unknown address. It was blocked by an automated detection rule but I have to investigate the incident using tools from previous rooms.


Within the provided directory in the virtual machine, I have 4 main components to work with
1- network and deployment logs
2- the production model
3- a potential replacement model that is UNTESTED
4- a baseline model for comparison

First, I started with reading the deployment logs to get some background information.
  - I discovered that the replacement model actually came from a different source than the original, so off the bat, a little suspicious
  - The automated alert was only triggered 21 days AFTER the replacement model was deployed

Second, I decompiled the production model for further analysis. I did this using the built in fickling tool in the model directory.
Findings: 
-the payload was using system functions to execute shell commands
-a curl command for an outbound http request to a attacker.com domain
-a shell command to retrieve the name of the host

When I did modelscan, i got the same result. There was 1 critical issue, being that there are system commands from the OS

Next, I decided to check for architecture layered threats using inspect_h5_py on the production model. 
After the test ran, I found a suspicious lambda layer with a "manipulate_output" function, whih often contains code that runs at model inference time

I then went back into the beacon_capture.log and the candidate model to get the full flag. For the log I had to do a simple cat command. For the rest of the flag, I was able to find it in the layer scan of the candidate model. 
