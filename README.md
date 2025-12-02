Azure Container Instances & Public DNS Name Label

This lab demonstrates the fast deployment of a public, serverless application using **Azure Container Instances (ACI)** and reinforces best practices for large-scale data transfer with **AzCopy** and **Shared Access Signatures (SAS)**.

ACI is a lightweight, pay-per-second service ideal for simple web demos, CI/CD tasks, and rapid Proof-of-Concepts (PoCs).

## 🎯 Lab Goals

  * Deploy a **public-facing Azure Container Instance** using a standard image.
  * Assign a custom **DNS name label** to automatically generate a public FQDN (Fully Qualified Domain Name).
  * Use the **AzCopy** utility to manage Azure Blob Storage containers programmatically.
  * Securely upload files to Azure Storage using **SAS tokens**.
  * Validate end-to-end public connectivity for both ACI and Storage.

-----

## 🛠️ Environment Setup

### 1\. Azure Storage Account Setup

We use a Storage Account to simulate a necessary application dependency, such as content or configuration files.

| Setting | Value | Purpose |
| :--- | :--- | :--- |
| **Name** | `staz104labxxxx` | Globally unique name. |
| **Type** | `StorageV2` | General-purpose account. |
| **Access** | **Enabled** | Required for public access tests. |

### 2\. AzCopy Operations (Data Transfer Best Practice)

**AzCopy** is the recommended tool for high-performance data movement in Azure.

  * **Create Blob Container:**
    ```bash
    azcopy make "https://<account>.blob.core.windows.net/tdimage"
    ```
  * **Upload Local File (Requires SAS):**
    ```bash
    # Generate a SAS token in the Portal for the container or account
    # Copy a local file to the newly created container
    azcopy copy "/path/to/localfile.txt" \
    "https://<account>.blob.core.windows.net/tdimage?<SAS_TOKEN>"
    ```
    > **Result:** You now have a file accessible via a structured, publicly routable blob URL: `https://<account>.blob.core.windows.net/tdimage/<file.txt>`.

-----

## 🐳 Azure Container Instance Deployment

We will deploy a simple web application container and expose it publicly using Azure's DNS naming convention.

| Setting | Value | Concept |
| :--- | :--- | :--- |
| **Name** | `aci-public-test` | |
| **Image** | `mcr.microsoft.com/azuredocs/aci-helloworld` | A standard testing image. |
| **Networking** | **Public** | Required for public FQDN. |
| **DNS Name Label** | `td-aci-demo-<unique>` | A unique custom prefix. |
| **Resulting FQDN** | `td-aci-demo-<unique>.<region>.azurecontainer.io` | Azure's automatic public endpoint format. |

### Validation

1.  Wait for the ACI to reach the **Running** state.
2.  Open the full **FQDN** (`<dns-name>.<region>.azurecontainer.io`) in any web browser.
3.  **Expected:** The "Hello World" sample page should appear, validating public connectivity and DNS resolution.

-----

## 🧠 Key Azure Concepts You Should Take Away

### ACI Networking & Naming

  * **DNS Name Label Requirement:** A custom DNS name label can **only** be assigned if the ACI container group is configured with **Public networking**. Private ACIs use private IP addresses and do not generate public FQDNs.
  * **FQDN Format:** Azure enforces a standard domain structure for its container service: `<dns-name>.<region>.azurecontainer.io`.

### Azure Storage & Security

  * **AzCopy is Paramount:** It is the **fastest and most efficient** way to move data into or out of Azure Blob Storage at scale.
  * **Blob URLs:** Storage endpoints always follow the fixed pattern: `https://<account>.blob.core.windows.net/<container>/<file>`.
  * **SAS is Key:** A **Shared Access Signature (SAS)** token provides secure, time-bound, and permission-specific access to Storage resources **without exposing the account key**. This is required for external tools like AzCopy to perform uploads/downloads securely.

## 🚀 Modern Azure Best Practices

While ACI is excellent for quick tasks, production deployments often use other services based on scale and complexity:

| Scenario | Recommended Service |
| :--- | :--- |
| **Long-running apps** with high orchestration needs | **Azure Kubernetes Service (AKS)** |
| **Serverless containers** with HTTP traffic and scaling rules | **Azure Container Apps** |
| **Web apps** with integrated dev/ops workflows | **Azure App Service for Containers** |
| **Batch processing** and micro-tasks | **ACI** (Often triggered by Logic Apps / Functions) |

-----

Would you like to extend this lab by configuring a **Mount Volume** to attach the file we uploaded in Storage (via AzCopy) to the running ACI container?
