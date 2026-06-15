AI reconaissance is investigating the different compoonents within AI systems and exposing them. Common tools such as nmap can still be used but the targets are different.

Note: most AI infrastructure runs on unknown ports 

These are the different parts

model serving endpoints- the frameworks that load trained models into memory
orchestration tracking- platforms that manage the entire ML lifecycle 
vector databases- store the numerical embeddings 
model registries- store the actual model files

Here are some general commands for scanning purposes

scanning for AI services: nmap -p 5000,6333,6334,8000,8001,8002,8888,9000,9001 -sV 10.10.45.0/24
checking for AI components: nmap -p 22,80,443,5432 -sV 10.10.45.0/24

fingerprinting is actually determining what kinds of services are running behind these ports

doing this for AI systems is different because you have to consider other factors such as HTTP headers, JSON response structures, Error messages

response headers are the fastest way to identify services, these seem to be the general rule of thumbs

TorchServe returns TorchServe/0.x.x header.
Triton Inference Server includes a NV-Status/ GPU utilisation
FastAPI-based ML services show : uvicorn in the response.
OpenAI return x-request-id headers and structured JSON with an "object": "model" 

API Response Signatures

TensorFlow Serving returns: {"model_version_status": [{"version": "1", "state": "AVAILABLE"}]}
Triton returns: {"name": "fraud_detector", "versions": ["1"], "platform": "tensorflow_graphdef"}
MLflow error responses include stack traces referencing mlflow.server and mlflow.tracking namespaces.
OpenAI-compatible endpoints return: {"object": "model", "id": "llama-3.1-8b", "created": 1700000000}


Error Message Fingerprinting


Send integers to a TensorFlow Serving endpointand you get back an error mentioning tensorinfo_map
Send a bad request to an MLflow server, and the stack trace references mlflow.server, mlflow.tracking, or databricks namespaces.
MLflow path traversal errors exposing full server filesystem paths.
Databricks Mosaic AI returns Java 

Endpoint Naming Conventions
Traditional REST APIs use resource nouns: /users, /accounts, /products. AI endpoints use computational action terms that stand out immediately during directory brute-forcing.

Inference endpoints: /predict, /invocations (the SageMaker convention), /infer, /generate, /embeddings, /score
Model management: /v1/models, /v2/models
MLflow internal API: /api/2.0/mlflow/ 
Kubeflow pipelines: /pipeline/apis/v1beta1/

Enumeration- extracting information from the sources

here are the general commands
listing experiments: POST /api/2.0/mlflow/experiments/search
find registered models: GET /api/2.0/mlflow/registered-models/list
model version: GET /api/2.0/mlflow/model-versions/search
training runs: POST /api/2.0/mlflow/runs/search
downloadable artifacts: GET /api/2.0/mlflow/artifacts/list

you can use these commands to convert notebooks to cleartext: jupyter nbconvert --to script 

This is the general, recommended process for investigating AI agents
Phase 1: Passive Reconnaissance

Before you touch the target network, see what is already publicly visible.

Search Shodan, Censys, and FOFA for AI service banners on the organisation's IP ranges.
The dorks from Task 2 work here:
port:5000 "MLflow",
port:8888 title:"Home Page - Select or create a notebook",
http.title:"Ray Dashboard".
Search GitHub for leaked credentials.
filename:.env MLFLOW_TRACKING_URI,
filename:.env HF_TOKEN,
filename:config.json model_name site:github.com.
These turn up MLflow connection strings and Hugging Face tokens more often than you would expect.
Check arXiv and engineering blogs for published model architectures.

Teams regularly publish papers describing the exact frameworks and infrastructure they use.
This maps to ATLAS technique AML.T0000 (Search for Victim's Publicly Available Research Materials).
Check DockerHub and GitHub Container Registry for organisation-named ML images. Public container images frequently contain hardcoded configurations. Look at job postings. A listing for an "MLflow Administrator" or "Kubeflow Platform Engineer" tells you exactly what is deployed.

Phase 2: Active Scanning

Target the AI-specific ports from the Task 2 reference table:

nmap -p 5000,6333,8000,8001,8002,8080,8265,8500,8501,8888,9000,11434,19530 -sV --script=http-title,http-headers <target>

That single command covers most ML serving components.

Pay attention to gRPC services on ports 8001 and 8500 that Nmap may report as generic.
Follow up with grpcurl for anything that looks like gRPC.
Check for Prometheus metrics endpoints (`/metrics`) on every discovered service.
These are often on separate ports (8002 for Triton, 8082 for TorchServe) and leak deployment topology.
Phase 3: API Fingerprinting

Run ffuf or feroxbuster with an AI-specific wordlist against every discovered HTTP service. Your wordlist should include:

/v1/models
/v2/models
/v2/health/ready
/api/2.0/mlflow/experiments/list
/api/2.0/mlflow/registered-models/list
/pipeline/apis/v1beta1/pipelines
/api/serve/deployments/
/v1/schema
/v1/meta
/api/kernels
/api/contents
/openapi.json
/docs
/graphql
/metrics
/api/tags
/api/show
/collections
/healthz
/ping

For each endpoint that returns a 200, apply the fingerprinting techniques from Task 3: check response headers, parse JSON structure, and look at error messages from malformed requests.

Phase 4: Metadata Extraction

For every confirmed AI service, run the enumeration chain from Task 4.

On MLflow: Experiments, registered models, model versions (with artifact URIs and user IDs), training runs, and artifact listings. Five API calls that map the entire ML portfolio.
On Triton or TF Serving: Model config endpoints for tensor specs and framework identification.
On vector databases: Schema and collection endpoints for data type and embedding model identification.
On Jupyter: Kernel listings and notebook cell contents for cleartext credentials.
Phase 5: Supply Chain Review

Identify model download sources visible in configurations, notebook cells, and container build logs. Check whether internal model artifact buckets (S3, GCS, MinIO) are publicly readable. Audit requirements.txt and Pipfile contents for internal package names that could be squatted on PyPI. Check container registries for image pull access without credentials.
