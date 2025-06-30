# Langflow Kubernetes Helm Charts

<div class="column" align="middle">
  <img alt="GitHub License" src="https://img.shields.io/github/license/langflow-ai/langflow-helm-charts">
  <img alt="GitHub Release" src="https://img.shields.io/github/v/release/langflow-ai/langflow-helm-charts?filter=langflow-ide-*">
  <img alt="GitHub Release" src="https://img.shields.io/github/v/release/langflow-ai/langflow-helm-charts?filter=langflow-runtime-*">
</div>


## Charts
- [LangFlow IDE](./charts/langflow-ide/): Full experience of Langflow, optimized for prototyping and testing new flows. 
- [LangFlow Runtime](./charts/langflow-runtime/): Productionize Langflow flows as standalone services.

## Steps to deploy langflow-runtime on Google Kubernetes Engine

1. **Open Google Cloud Shell**
   - Go to the [Google Cloud Console](https://console.cloud.google.com/) and click the Cloud Shell icon in the top right.

2. **Authenticate and Set Project**
   ```sh
   gcloud auth login
   gcloud config set project YOUR_PROJECT_ID
   ```

3. **Create a GKE Cluster**
  * you can also do this with GUI on the google workspace
   ```sh
   gcloud container clusters create langflow-cluster \
     --zone=us-central1-a \
     --num-nodes=3
   gcloud container clusters get-credentials langflow-cluster --zone=us-central1-a
   ```

4. **Install Helm if not already installed**
  * Skip this step if ur on Google Cloud Shell
   ```sh
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```

5. **Add the Langflow Helm Repository**
   ```sh
   helm repo add langflow-ai https://langflow-ai.github.io/langflow-helm-charts/
   helm repo update
   ```

7. **Deploy Langflow Runtime with custom values.yaml in this repo**
```sh
helm install my-langflow-app-with-flow langflow/langflow-runtime \
  -n langflow \
  -f https://raw.githubusercontent.com/waydxd/langflow-helm-charts/refs/heads/main/charts/langflow-runtime/values.yaml
```

8. **Access the Application**
   - Get the external IP:
     ```sh
     kubectl get svc
     ```
   - Open the EXTERNAL-IP in your browser.
   - If succed, you should see detail	"Not Found"

> For more advanced configuration, refer to
## Langflow IDE vs Runtime

Langflow offers two distinct Kubernetes charts for deployment: one for the Integrated Development Environment (IDE) and another for the Runtime environment. 
This separation is designed to enhance security, optimize resource allocation, and streamline management. 
Understanding the rationale behind these deployment options will help you make informed decisions about how to best deploy and manage your applications.

The `ide` chart is designed to provide a complete environment for developers to create, test, and debug their flows. It includes both the API and the UI.

The `runtime` chart is tailored for deploying applications in a production environment. It is focused on stability, performance, isolation and security to ensure that applications run reliably and efficiently.

### Why is important to have separate deployments?
- Security
  - Isolation: by separating the development and production environments, we can better isolate different phases of the application lifecycle. This isolation minimizes the risk of development-related issues impacting the production environment.
  - Access Control: different security policies and access controls can be applied to each environment. Developers may require broader access in the IDE for testing and debugging, whereas the runtime environment can be locked down with stricter security measures.
  - Reduced Attack Surface: the runtime environment is configured only include essential components, reducing the attack surface and potential vulnerabilities.

- Resource Allocation
  - Optimized Resource Usage and cost efficiency: by separating the two, we can allocate resources more effectively. Additionally, each flow can be deployed independenly, providing fine-grained resource control.
  - Scalability: the runtime environment can be scaled independently based on application load and performance requirements, without affecting the development environment.
