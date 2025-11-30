Azure Container Instances & Public DNS Name Label

This lab demonstrates how to deploy a public Azure Container Instance (ACI) with a custom DNS name label, and how to use AzCopy to create storage containers and upload files.

ACI is a lightweight, serverless container environment great for demos, microtasks, CI pipelines, and POCs.

🎯 Goal of This Lab

Deploy a public-facing Azure Container Instance

Assign a DNS name label → auto-generates a public FQDN

Use AzCopy to create blob containers

Upload files to Azure Storage using SAS tokens

Validate public connectivity

🛠️ Environment Setup
Storage Account
Setting	Value
Name	staz104labxxxx
Type	StorageV2
Public Access	Enabled
Blob Container

Created via Portal or AzCopy:

azcopy make "https://<account>.blob.core.windows.net/tdimage"


Upload a local file:

azcopy copy "/path/to/localfile" \
"https://<account>.blob.core.windows.net/tdimage?<SAS>"


Result:
You now have a publicly accessible blob endpoint.

🐳 Azure Container Instance Deployment
Setting	Value
Name	aci-public-test
Image	mcr.microsoft.com/azuredocs/aci-helloworld
Networking	Public
DNS Label	td-aci-demo-<unique>
FQDN Format	td-aci-demo-<unique>.<region>.azurecontainer.io

When deployed, open the FQDN in any browser → Hello World page appears.

🧠 Key Azure Concepts You Should Take Away
✔ A DNS name label requires Public networking

Private ACIs cannot generate a DNS label.

✔ AzCopy is the fastest way to move data into Azure Storage

azcopy make → create containers

azcopy copy → upload/download

azcopy list → enum files

✔ Blob URLs always follow this pattern

https://<account>.blob.core.windows.net/<container>/<file>

✔ FQDN uses Azure’s container instance domain

<dns-name>.<region>.azurecontainer.io

✔ SAS is required for AzCopy uploads

Without a valid SAS token → upload fails.

📝 Modern Azure Best Practices

While ACI is excellent for demos and microservices, in production Azure recommends:

Scenario	Recommended Service
Long-running apps	AKS
Serverless containers	Azure Container Apps
Web apps w/ containers	App Service for Containers
Batch processing	ACI + Logic Apps / Functions

AzCopy remains the best-practice tool for blob transfer at scale.
📝 Lab Summary

This lab validates your ability to:

Deploy serverless workloads with ACI

Assign and test DNS name labels

Manage Azure Storage programmatically

Use SAS tokens securely

Validate connectivity end-to-end

Great for demonstrating real-world Azure operations skills.
