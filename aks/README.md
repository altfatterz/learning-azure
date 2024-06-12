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

### Deploy a sample app

```bash
$ kubectl apply -f aks-store-quickstart.yaml
```

### Check created resources

```bash
$ kubectl get pods
$ kubectl get svc
$ kubectl get deploy
$ kubectl get sts
```

### Export IP

```bash
$ export IP_ADDRESS=$(kubectl get service store-front --output 'jsonpath={..status.loadBalancer.ingress[0].ip}')
```
Access the IP in the browser


### Cleanup

```bash
$ az group delete --resource-group $MY_RESOURCE_GROUP_NAME
$ az group list -o table 
```

