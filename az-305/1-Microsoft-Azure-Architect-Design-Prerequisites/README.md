# AZ-305 Microsoft Azure Architect Design Prerequisites

## The core architectural components of Azure

![image](../images/microsoft-service-categories.png)

- `Azure regions, region pairs, and sovereign regions`
  - Region: one or more datacenters networked with low latency, ex: East US, West Europe
  - Region pairs:
    - most Azure regions are paired with another region within the same Geography (300 miles away)
    - example: `West US` <-> `East US`, `Switzerland North` <-> `Switzerland West`
  - Sovereign Regions - isolated from main instance of Azure
    - They have their own `independent identity systems`, `networks`, and `data centers`  
    - US DoD Central, US Gov Virginia, US Gov Arizona, etc - operated by screened U.S. personnel
    - China East, China North, etc - partnership between Microsoft and 21Vianet, Microsoft doesn't directly maintain the datacenters
- `Availability Zones`
  - Within a region, made up of one or more datacenters equipped with `independent power`, `cooling`, and `networking`
  - Each zone is an isolation boundary - if one fails, the others continue operating
  - Azure services that support AZs can be:
    - `Zonal service` - example VMs, managed disks, IP addresses
    - `Zone-redundant service` - automatically `replicates across zones` (ex: zone-redundant storage, SQL Database)
    - `Non-regional service` - always available (ex: Microsoft Entra ID, Azure DNS, Azure Traffic Manager)
- `Azure datacenters`
  - https://datacenters.microsoft.com/
  - https://datacenters.microsoft.com/globe/explore
- `Azure resources and Resource Groups`
  - anything you deploy (create/provision) in Azure is a `resource` (VMs, Virtual Network, database, etc)
  - every resource must belong to only one `resource group` - you move resources between resource groups
  - `resource groups` cannot be nested
  - deleting a `resource group` deletes all resources, actions on `resource group` apply to all resource within it 
- `Azure subscriptions`
  - when creating an Azure account you a single Azure subscription is created for you
  - you can create multiple Azure subscriptions for dev, test and prod workloads, or for different departments
  - subscriptions separate billing and access control

```bash
$  az account list-locations --output table
```

![image](../images/azure-subscriptions-boundaries.png)
  - Azure subscription links to an Account which is an identity in Microsoft Entra ID
  - Two boundaries:
    - `Billing boundary` - how an Azure account is billed, you can create multiple subscriptions for different billing requirements
    - `Access control boundary` - dev and prd with different spending limits and access rules

- `Azure management groups`
  - Azure management groups you can organise subscriptions
  - You can use it for governance — like `access policies` or `compliance rules` - all subscriptions will inherit it
    - ex: limit VM locations to the US West Region in a Management Group called Production
      - The resource or subscription owner can't override it, which strengthens governance.
  - Can be nested up to 6 levels
  - Every `Microsoft Entra tenant` has a single top-level `Tenant Root Group`
  - A single directory supports up to 10,000 management groups.
  - Each management group and subscription can have only one parent.

- `Hierarchy of resource groups, subscriptions, and management groups`

![image](../images/management-group-hierarchy.png)

## Azure compute services

- `Azure Virtual Machines`
  - IaaS service, remove the need to buy and maintain physical server hardware 
  - Total control over the operating system (OS)
  - The ability to run custom software.
  - When you provision a VM:
    - Size - number of cores, amount of RAM
    - Storage Disks
    - Networking
  - VM family sizes:
    - B-Series: Burstable, cost-efficient - Dev/test workloads with occasional CPU spikes
    - D-series: General purpose - Web servers, small-to-medium app servers
    - E-series: Memory optimized - In-memory databases, analytics workloads
    - F-series: Compute optimized - CPU-intensive application tiers
    - M-series: Large memory footprint - Large enterprise databases
    - L-series: Storage optimized - High-throughput storage and data processing
    - N-series: GPU enabled - AI training/inference and graphics workloads 
  - Standard_D2s_v5
    - D: the VM family (general purpose in this case)
    - 2: the number of vCPUs in this size
    - s: supports Premium SSD storage - supports premium managed disks.
    - v5: hardware generation for that family
    - More in-depth explanation: https://learn.microsoft.com/en-us/azure/virtual-machines/vm-naming-conventions
  - VM Scale Sets
    - create and manage groups of identical, load-balanced VMs.
  - VM Availability Sets
    - Don't add cost, only pay for VM size 
    - They reduce the chance that all VMs are affected by one maintenance event or hardware failure.
    - Availability Sets group VMs into
      - Update domain: VMs that can be rebooted together during planned maintenance.
      - Fault domain: VMs that share a potential power or network failure point.

```bash
$ az vm list-skus --location switzerlandnorth --resource-type virtualMachines --output table
ResourceType     Locations         Name                       Zones    Restrictions
virtualMachines  SwitzerlandNorth  Standard_A1_v2             1,2,3    None
virtualMachines  SwitzerlandNorth  Standard_A2m_v2            1,2,3    None
...
virtualMachines  SwitzerlandNorth  Standard_B1s               1,2,3    None
...
```

- `Azure virtual desktop`
  - Is a managed option for remote desktop access where desktops and apps stay in the cloud instead of on local devices.
  - Azure Virtual Desktop is often easier to operate than building separate VM-based desktop environments for each user group
  - It integrates with Microsoft Entra ID for identity and access controls.
  - It supports single-session and multi-session Windows experiences, depending on user and workload needs.
  - You create `Host Pools` these pools are filled with VMs - referred as `Session Hosts`
    - can be configured:
      - `Pooled` (Multi-Session) - Multiple users share a single, powerful VM at the same time to save money
      - `Personal` (Single-Session) - Every user gets their own dedicated, private VM

- `Azure containers`
  - `Azure Container Instances` - fastest and simplest way to run a container in Azure
  - `Azure Container Apps` - next to `Azure Container Instances` include built-in load balancing and scaling
  - `Azure Kubernetes Service` - container orchestration service

- `Azure functions`
  - event-driven, serverless compute option that doesn’t require maintaining virtual machines or containers.
  - triggers like: HTTP, Timer, queue message
  - output: API response, Storage write, Event publish

- `AI, machine learning and IoT/Edge services in Azure`
  - `Azure AI services` 
    - provides prebuilt capabilities for common AI scenarios: language, speech, vision
    - useful when you want to add intelligent features through APIs instead of training your own model first
  - `Agentic AI patterns`
    - Agentic applications combine an `AI model` with `instructions`, `context`, and `tool use` to complete multistep goals
  - `Azure Machine Learning`
    - When you need to build, train, and manage custom machine learning models
  - `IoT and Edge services`
    - when your solution centers on connected devices and telemetry
    - The flow commonly starts with devices sending telemetry data through `IoT Hub` 
    then uses `cloud analytics to generate insights` and model updates that IoT Edge runtime applies near the devices.
  
      
- `Application hosting options`
  - `VMs`
  - `Containers`
  - `App Service`
    - App Service handles most of the infrastructure decisions (Endpoints can be secured, Sites can be scaled quickly, Deployment)
    - HTTP-based service for hosting 
      - `Web apps`
      - `API apps` - can build REST-based web APIs
      - `WebJobs` - run a program or a script, often used to run background tasks as part of your application logic.
      - `Mobile Apps` - build a back end for iOS and Android apps
    - support multiple languages
    - support for Windows or Linux operating system

## Azure storage services

- `Azure storage accounts`
- `Azure storage redundancy`
- `Azure storage services`
- `Azure data migration options`
- `Azure file movement options`

## Azure identity, access, and security

- `Azure directory services`
- `Azure authentication methods`
- `Azure external identities`
- `Azure conditional access`
- `Azure role-based access control`
- `Azure Zero Trust model`
- `Azure defense-in-depth`
- `Encryption and key management in Azure`
- `Microsoft Defender for Cloud`

## Introduction to the Microsoft Cloud Adoption Framework

Methodologies:

- `Strategy`
- `Plan`
- `Ready`
- `Migrate`
- `Innovate`
- `Govern`
- `Manage`
- `Secure`

## Introduction to the Microsoft Azure Well-Architected Framework

The Azure Well-Architected Framework pillars:

- `Reliability`
- `Security`
- `Cost Optimization`
- `Operational Excellence`
- `Performance Efficiency`
- `Trade-offs between pillars`
