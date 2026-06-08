In this room, I will gain even more hands-on experience by building an internal supply chain security testing lab for AI systems. 

The first line of defense -> eliminating pickle, since this is the cause of serialisation attacks, removing this will stop or atleast reduce the chances of code execution when loading
the solution is to use Safetensors, a specific serialisation format that guarantees that code cannot be executed while loading (the conversion step from pickle to safetensor is not protected)

the 2nd way to implement this is to use Pytorch weights_only=TRUE, during deserialisation pickle is restricted to only reconstruct tensor objects (the actual weights in the model)

Since serialisation only protects you from code being executed when loading, you need to verify the model to check whether it has been tampered with.

One method to do this is by using checksum for file integrity. I compared the expected checksum against the individually calculated ones. After doing this, I found that one pair of sums did not match, meaning that part of the model was tampered with. 

Some other methods for verification
1) check the model card- one without specific or vague information is usually suspicious
2) check adapters (small files that can be attached to base models for specific tasks), downloaded adapters can still be malicious


   when planning to integrate or aquire new models, follow a similar verification process (model aquisition framework)
   1. Quarantine- Download the model into an isolated staging area, never directly into production	
2. Source verification-Verify the author, organisation, and repository
3. Integrity check-Compute SHA-256 checksums and compare against the author's published values	
4. Security scan-Run Fickling, ModelScan, and dependency auditing tools
5. Approve or reject-Based on findings, either approve it or reject it for deployment

Next, you can view the sessions stream for any unexpected objects and patterns. When looking at the session stream for the malicious model I can see a [Dangerous] import and [Critical] call. And on load completion, an integer object was loaded instead of a model.


There are also steps you can take to close the gap between deployment and and download, and this is scanning the model. Fickling, a form of static analysis, decompiles bitcode into a readable format without execution. 
- ModelScan is a more advanced version of this, assigned threat levels to each finding. It is important to note that scanners can only detect familiar patterns.

Attackers can also get around this by attacking infrastruture instead. One method of doing this is using lamdbas to run harmful functions. These layers only run when the model makes predictions, so it is undetected when loading the model.
This is why it is important to run architecture inspections and enumerate every layer to check for flags. When running the layer inspection on the malicious model, it reported 5, with one being a lambda layer marked as suspicious.  
Note: for suspicious .h5 files, you can do deeper inspection with h5py which will not execute any files either

Version Pinning: it is also important to make the model is prepared for updates. As mentioned earlier, they will be updated with the most recent version, which is an advantange point for attackers to upload higher versioned "packages". 
You can do this by setting a restriction on the version in the requirements file, which is also called version pinning. You can take this further with lockfiles which restricts the cryptographic hash as well.



pip-audit is a built in vulnerability scanner which will run the dependencies against known threat databases

Note: for organizations using internal packages, use a private index so it never uses public packages. 

A good practice is to create a Standard bill of materials for every model. This lists all of the "Ingredients" such as the packages and frameworks. If there is information released about a threat, this is a easy and quick way to check if your model is impacted. 

These standard formats still need to be updated for ML because these also include models and datasets not a strict # of files.

As noted earlier, these methods are not useful for mitigation when the model is being called through API's, so different approaches have to be taken here.
1) evaluate the security posture meaning the certifications, data handling, versioning, etc.
2) monitor the outputs using baseline responses to prompts and flag unexpected/ changes in behavior
3) version control and review changes/ deletion of system prompts
4) isolate any downloaded models and test it with known answers to prompts
