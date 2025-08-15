
## 🔧 Activity
1. Gone through the fundamentals of Helm (known as the Kubernetes package manager), including the advantages of Helm 3, its core components, and prerequisites for using it effectively. 
2. Also revised Git basics with the use of git add -A.
## 🔍 Key Learnings

- Helm simplifies Kubernetes application deployment by packaging manifests into reusable charts.
- Provides a consistent way to install, upgrade, and manage applications across environments.

#### Advantages of Helm 3

- Removed Tiller (server-side component), making Helm more secure and easier to use as kubernetes functionalities grows
- Works directly with Kubernetes API and RBAC.
- Better release management, improved security, and simplified upgrade/rollback, 3-way strategic merge patch
- Strong ecosystem of community-maintained charts, enabling rapid deployments.

#### Helm Components

- Charts: Packaged application templates (YAML manifests).
- Repositories: Storage locations for charts (e.g., ArtifactHub).
- Releases: An instance of a chart deployed to a Kubernetes cluster.

#### Prerequisites for Using Helm

- A working Kubernetes cluster.
- kubectl configured and access rights set up.
- Helm client installed on the local machine.

### Git add -A refresher

- git add -A stages all changes in the working directory (new, modified, deleted files).
- Useful for ensuring a complete snapshot of work before committing.

#### Came across this scenerio: 
- Accidently created additional directory locally which was pushed to git repo with usual changes
- As deleting the directory locally dint delete the folder in actual repo 
- ``` git add -A ``` helped sync the local and remote repo sync and effectively push the changes

#### 💰 Cost optimization with Helm
- Helm can reduce deployment time and operational overhead, Lowering infrastructure management costs.
- Customers can benefit from faster time to-market, consistent deployments, and simplified application lifecycle management.

## 🖥️  Commands / Configs
```
# Install Helm 
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
or
brew install helm

# Add a repository
helm repo add bitnami https://charts.bitnami.com/bitnami

# Search for a chart
helm search repo nginx

# Install a chart
helm install my-nginx bitnami/nginx

# Verify the release
helm list

# Check the service created
kubectl get svc

```
