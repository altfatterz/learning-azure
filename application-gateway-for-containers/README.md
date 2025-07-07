### Application Gateway for Containers 

Supported regions: https://learn.microsoft.com/en-us/azure/application-gateway/for-containers/overview#supported-regions

```bash
# Sign in to your Azure subscription.
SUBSCRIPTION_ID='<your subscription id>'
az login
az account set --subscription $SUBSCRIPTION_ID

# Register required resource providers on Azure.
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.Network
az provider register --namespace Microsoft.NetworkFunction
az provider register --namespace Microsoft.ServiceNetworking

# Install Azure CLI extensions.
az extension add --name alb
```

- AKS cluster 
  - should be in the supported regions
  - should use `Azure CNI` or `Azure CNI Overlay`
  - should have the workload identity feature enabled

#### Create AKS cluster

```bash
AKS_NAME='app-gw-for-containers-demo-aks'
ACR_NAME='appgwforcontainersdemoacr'
RESOURCE_GROUP='app-gw-for-containers-demo'
# az account list-locations -o table
LOCATION='westeurope'
# https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/general-purpose/dpsv6-series?tabs=sizebasic
VM_SIZE='Standard_D2ps_v6' # The size needs to be available in your location

az group create --name $RESOURCE_GROUP --location $LOCATION

az acr create --name $ACR_NAME --resource-group $RESOURCE_GROUP --sku basic

az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --location $LOCATION \
    --node-vm-size $VM_SIZE \
    --network-plugin azure \
    --enable-oidc-issuer \
    --enable-workload-identity \
    --attach-acr $ACR_NAME
```

#### Install the ALB Controller

1. Create a user managed identity for ALB controller and federate the identity as Workload Identity to use in the AKS cluster.

```bash
AKS_NAME='app-gw-for-containers-demo-aks'
RESOURCE_GROUP='app-gw-for-containers-demo'
# ALB Controller requires a federated credential with the name of azure-alb-identity. Any other federated credential name is unsupported.
IDENTITY_RESOURCE_NAME='azure-alb-identity'

mcResourceGroup=$(az aks show --resource-group $RESOURCE_GROUP --name $AKS_NAME --query "nodeResourceGroup" -o tsv)
mcResourceGroupId=$(az group show --name $mcResourceGroup --query id -otsv)

echo "Creating identity $IDENTITY_RESOURCE_NAME in resource group $RESOURCE_GROUP"
az identity create --resource-group $RESOURCE_GROUP --name $IDENTITY_RESOURCE_NAME
principalId="$(az identity show -g $RESOURCE_GROUP -n $IDENTITY_RESOURCE_NAME --query principalId -otsv)"

echo "Waiting 60 seconds to allow for replication of the identity..."
sleep 60

echo "Apply Reader role to the AKS managed cluster resource group for the newly provisioned identity"
az role assignment create --assignee-object-id $principalId --assignee-principal-type ServicePrincipal --scope $mcResourceGroupId --role "acdd72a7-3385-48ef-bd42-f606fba81ae7" # Reader role

echo "Set up federation with AKS OIDC issuer"
AKS_OIDC_ISSUER="$(az aks show -n "$AKS_NAME" -g "$RESOURCE_GROUP" --query "oidcIssuerProfile.issuerUrl" -o tsv)"
az identity federated-credential create --name "azure-alb-identity" \
    --identity-name "$IDENTITY_RESOURCE_NAME" \
    --resource-group $RESOURCE_GROUP \
    --issuer "$AKS_OIDC_ISSUER" \
    --subject "system:serviceaccount:azure-alb-system:alb-controller-sa"
```

2. Install the ALB Contoller with Helm

```bash
# installs the helm chart into `default` namespace and the alb-controller is installed into `azure-alb-system` namespace 
az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_NAME
helm upgrade alb-controller oci://mcr.microsoft.com/application-lb/charts/alb-controller \
    --version 1.6.7 \
    --set albController.podIdentity.clientID=$(az identity show -g $RESOURCE_GROUP -n azure-alb-identity --query clientId -o tsv)

3. Vierfy the ALB Controller installation

helm list
kubectl get pods -n azure-alb-system
kubectl get gatewayclass azure-alb-external -o yaml
```

#### Install the Application Gateway For Containers resource

The next step is to link your `ALB controller` to `Application Gateway for Containers`

There are two strategies:

1. `Bring your own (BYO) deployment`:
  - the following resources are created by Azure Portal / CLI / Terraform etc
    - `Application Gateway for Containers` resource
    - `Application Gateway for Containers Frontend` resource
    - `Applciation Gateway For Containers Assocation` resource
    
    
2. `Managed by ALB Controller`:
   - ALB Controller creates an `Application Gateway for Containers` resource 
   when an `ApplicationLoadBalancer` custom resource is defined


#### Resources:

https://learn.microsoft.com/en-us/azure/application-gateway/for-containers/




