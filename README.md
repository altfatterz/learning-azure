
Useful commands 

```bash
$ brew update && brew install azure-cli 
$ az upgrade
$ az --version
$ az login
$ az account show
$ az account list --output table               // table, json, tsv
$ az group create --name MyResourceGroup --location eastus
$ az group delete --name MyResourceGroup --no-wait
$ az storage account list -o table
$ az account list-locations --query '[].name' -o tsv
$ az group list --query '[].name' -o tsv
```


Azure CLI docs: https://docs.microsoft.com/en-US/cli/azure/
https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-macos


Azure Cloud Shell
- Azure Cloud Shell requires an Azure file share to persist files


Tutorial: 

```bash
$ RG=VMTutorialResources
$ REGION=switzerlandnorth
$ az group create --name $RG --location $REGION
 
// create VM
$ az vm create \
--resource-group=$RG \
--name vm-demo-01 \
--image UbuntuLTS \
--admin-username zoltan
// will use the ~/.ssh/id_rsa and ~/.ssh/id_rsa.pub

$ az vm show -d -g $RG -n vm-demo-01 --query publicIps -o tsv
$ ssh zoltan@20.203.219.238

$ az group delete --name $RG --no-wait
```

Azure Pricing Calculator: https://azure.microsoft.com/en-us/pricing/calculator/

TCO Calculator: https://azure.microsoft.com/en-us/pricing/tco/calculator/



