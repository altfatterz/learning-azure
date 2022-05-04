
Useful commands 

```bash
$ brew update && brew install azure-cli 
$ az upgrade
$ az --version
$ az login
```


```bash
$ az account show
$ az account list -o table
$ az account list-locations -o table
```


How many clouds is Microsoft Azure?
    
```bash
$ az cloud list --output table
IsActive    Name               Profile
----------  -----------------  ---------
True        AzureCloud         latest
False       AzureChinaCloud    latest
False       AzureUSGovernment  latest
False       AzureGermanCloud   latest
```


```bash
$ az ad user list -o table
```

More details: https://build5nines.com/microsoft-azure-is-multiple-clouds-public-us-gov-china-and-germany/

Azure geographies:
https://infrastructuremap.microsoft.com/
https://azure.microsoft.com/en-us/global-infrastructure/geographies/#geographies

Azure regions with pairs:
https://docs.microsoft.com/en-us/azure/availability-zones/cross-region-replication-azure

Azure regions with availability zones:
https://docs.microsoft.com/en-us/azure/availability-zones/az-overview

```bash
$ az group create --name MyResourceGroup --location eastus
$ az group delete --name MyResourceGroup --no-wait
$ az group list --query '[].name' -o tsv
```

```bash
$ az storage account list -o table
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


PowerShell:

1. Running in Docker

```bash
$ docker run -it mcr.microsoft.com/azure-powershell pwsh
$ Connect-AzAccount -UseDeviceAuthentication
WARNING: To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code D92RLQWNT to authenticate.

Account              SubscriptionName      TenantId                             Environment
-------              ----------------      --------                             -----------
altfatterz@gmail.com My Azure Subscription f3730dbf-6176-4a2e-93c0-f762af479d33 AzureCloud
```

```bash
$ Get-AzLocation
$ Get-AzureADUser
```


2. Running with brew installation

https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-macos?view=powershell-7.2

```bash

$ brew install --cask powershell
$ pwsh

```

Update:
```bash
brew update
brew upgrade powershell --cask
````


```


Azure Pricing Calculator: https://azure.microsoft.com/en-us/pricing/calculator/

TCO Calculator: https://azure.microsoft.com/en-us/pricing/tco/calculator/



