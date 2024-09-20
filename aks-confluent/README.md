Resource from: https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-cli

```bash
$ az login
```

### Define environment variables

```bash
export RANDOM_ID="$(openssl rand -hex 3)"
export MY_RESOURCE_GROUP_NAME="myAKSResourceGroup$RANDOM_ID"
export REGION="germanywestcentral"
export MY_AKS_CLUSTER_NAME="myAKSCluster$RANDOM_ID"
export MY_DNS_LABEL="mydnslabel$RANDOM_ID"
```

### Create a resource group

```bash
$ az group create --name $MY_RESOURCE_GROUP_NAME --location $REGION
```

When you create a new cluster, AKS automatically creates a second resource group to store the AKS resources.
More details here: https://learn.microsoft.com/en-us/azure/aks/faq#why-are-two-resource-groups-created-with-aks

### Create an AKS cluster

```bash
$ az aks create \
    --resource-group $MY_RESOURCE_GROUP_NAME \
    --name $MY_AKS_CLUSTER_NAME \
    --node-count 1 \
    --generate-ssh-keys
```

### Connect to cluster

```bash
$ az aks get-credentials --resource-group $MY_RESOURCE_GROUP_NAME --name $MY_AKS_CLUSTER_NAME
$ kubectl get nodes
```

Use `ktx` to switch to the given context.

### Create namespace and set to default

```bash
$ kubectl create ns confluent
$ kubectl config set-context --current --namespace confluent
## verify
$ kubectl config get-contexts
```

### Install Confluent For Kubernetes using Helm

```bash
$ helm repo add confluentinc https://packages.confluent.io/helm
$ helm repo update
$ helm upgrade --install confluent-operator confluentinc/confluent-for-kubernetes --set kRaftEnabled=true
## wait until the operator is up and running
$ kubectl get pods
```

### Deploy Confluent Platform

```bash
$ kubectl apply -f confluent-platform.yaml
```
