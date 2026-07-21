# AZ-305: Design identity, governance, and monitor solutions

## Design governance

- First create a `hierarchical structure` for your organizational environment.
- `Tenant root group`
  - `management groups` - 
    - `subscriptions` - billing boundaries
      - `resource groups` 
        - `resources`
- The structure enables to apply governance strategies (`Azure policies` / `resource tags`) where they are needed

- `Management Groups`
  - can be nested up to 6 levels (not including the tenant group or the subscription level)
  - Keep the management group hierarchy reasonably flat
  - consider: 
    - geographical structure
    - organizational or departmental structure
    - production management group
    - sensitive information with separate management group

- `Subscriptions` - separate billing
  - development, test, and production
  - HR / Legal / R&D
  - example: dedicated shared services subscription
    - shared services subscriptions include `Azure ExpressRoute` and `Virtual WAN`.

- `Resource groups`
  - have their own location (region) assigned, this is where metadata is stored
  - resources in the resource group can be in different regions.
  - a resource in resource group can connect to resources in another resource group
  - resources can be moved between resource groups with some exceptions, each resource must be one, and only one resource group
  - resource groups can't be nested / can't be renamed

-`Resource Tags`
  - name/value pair, ex: env = production or env = dev, test.
  - You can assign one or more tags to each `Azure resource`, `resource group`, or `subscription` - not to management groups
  - Resource tags are not inherited
  - Consider using `Azure policy` to apply tags and enforce tagging rules and conventions.

- `Azure Policy`
  - Azure policies are `inherited` down the hierarchy - on all 4 levels
  - There are built-in policies https://learn.microsoft.com/en-us/azure/governance/policy/samples/built-in-policies
  - Groups of related policies called `initiatives`
  - List of built-in initiatives https://learn.microsoft.com/en-us/azure/governance/policy/samples/built-in-initiatives
  - examples: 
    - VMs limited to certain SKUs
    - Enforce product tag and value
    - Only deploy to certain locations
  - `Azure Policy compliance dashboard` 
  - Consider how to handle a noncompliant resource
    - Deny changes to the resource.
    - Log changes to the resource.
    - Alter the resource before or after the change.
  - `Azure Policy` vs `Azure RBAC` 
    - with `Azure Policy` doesn't depend on who made the change, it only evaluates the state of resource and acts to ensure the resource stays compliant.
    - with `Azure RBAC` it matters who makes the change, what they can do with the resource
    - If a user has access to complete an action based on `Azure RBAC` but the result is a noncompliant resource, `Azure Policy` still blocks the action.

- `Azure RBAC`
  - `Who`(Identity) / `What`(Role (Built-in/Custom)) / `Where` (Scope: Tenant/Managemnt Group/Subscription/Resource Group/Resource)
  - Role - group of permissions
    - Owner - for admins
    - Contributor - read write access
    - Reader - for observers, auditors, reviewers
  - Custom roles - grant precise level of access
  - Assign: an `Identity` to `Role` within a `Scope`
    - Azure RBAC is an `additive model` - effective permissions are the `sum of your role assignments`.
    
- `Azure landing zones` - https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/?tabs=hubspoke
  - infrastructure environment for hosting your workloads, management groups, subscriptions
  - platform landing zone
  - application landing zone
  - Landing zones are pre-provisioned through code. (Bicep or Terraform)
  - `Azure landing zone accelerator` - template

## Design authentication and authorization solutions

- `Identity and Access Management. (IAM)`
  - Unified identity management - Manage all your identities and access to apps in a central location
  - Seamless user experience - fast sign-in experience
  - Secure adaptive access - risk-based adaptive access policies without compromising the user experience 

- `Microsoft Entra ID`- https://learn.microsoft.com/en-us/entra/identity/
  - is a hybrid identity solution
  - with `Microsoft Entra Connect` you can bring on-premises identities into Microsoft Entra ID
  - consider using a `single Microsoft Entra instance`
  - consider phishing-resistant authentication methods like passkeys - origin-bound public-key cryptography and satisfy MFA in a single step. 
  - consider using SSO
  - consider overhead of managing separate identities

- `Microsoft Entra business-to-business (Microsoft Entra B2B)`
  - the external partner uses their own identity management solution
  - your partner users are invited as guest users.

- `Azure Active Directory B2C (business-to-customer)` - Microsoft Entra External ID
  - managing customer identities and their access to your apps
  - requires a new Microsoft Entra tenant

![image](../images/identities.png)

- `Conditional Access` 
  - tool that Microsoft Entra ID uses to allow (or deny) access to resource based on `signals`
  - you need a Microsoft Entra ID P1 or P2 license
  - if you have a Microsoft 365 Business Premium license, you also have access to Conditional Access features.

- `Privileged Identity Management`
  - Just-In-Time access 
  - ex: you request to be an admin for 2 hours
  - Activates a temporary, time-bound role assignment after you provide a business justification.

- `Identity protection`
  - provides `risk policy detection` - any identified suspicious actions
    - user risk - probability that a given identity or account is compromised
    - sign-in risk - probability that a given sign-in isn't authorized by the identity owner.
  - below workflow: 
    - administrator first configures the risk policies that then monitor for identity risks.
    - when risk is detected the policies enforce measures to remediate it.
    - A policy might, for example, prompt a user to reset their password in response to a risk detected.
    - The user then resets their password, and the risk is remediated.

![image](../images/identity-protection.png)

- `Access Reviews`
  - is a planned review of the access needs, rights, and history of user access.

- `Service principals for applications`
  - it acts as a dedicated application account that can authenticate without human intervention.
  - authenticates using one of two methods:
    - Client Secret
    - Certificates

- `Managed identities`
  - The "Secret-Less" Service Principal
  - Microsoft recommends avoiding traditional Service Principals in favor of `Managed Identities`
  - A managed identity is a service principal wrapped in a protective layer managed entirely by Azure 
  - If your code runs inside Azure (Azure VM / Azure Function) you can turn on Managed Identity on resource level
  - Azure will automatically handle creating the Service Principal, authenticating it, and rotating the credentials behind the scenes.
  - two types
    - `System-assigned`
      - an identity is created in Microsoft Entra tied to the lifecycle of an Azure resource
      - when the resource is deleted, Azure automatically deletes the identity.
      - only that Azure resource can use that identity to request tokens from Microsoft Entra ID
    - `User-assigned`
      - create the managed identity as a standalone resource
      - you can assign it to one or more instances of an Azure resources

- `Azure Key Vault`
  - Manages 
    - secrets - passwords / tokens / API keys 
    - keys - these are encryption keys to encrypt your corporate data
    - certificates - these are public or private TLS/SSL certificates for with Azure and internal connected services
  - Standard tier
  - Premium tier - offers hardware security module (HSM)-protected keys
  - Logging and monitoring in Key Vault - helps you understand how and when secrets are accessed.
  - Permission model
    - Azure role-based access control (recommended)
    - Vault access policy - determines whether a given `security principal`, namely a user, application or user group, can perform different operations on keys, secrets and certificates.
  - Tips:
    - Consider using separate key vaults
    - Consider access to the key vault
    - Consider data protection for your key vault
      - `Soft delete` - like recycle bin, can be recovered
      - `Purge protection` - if enabled then it can be recovered during the configurable retention period

## Design a solution to log and monitor Azure resources

- `Azure Monitor`
  - `Azure Monitor Logs`
    - Collect any data, then transform to optimize costs, remove personal data and route data your Log Analytics workspace
  - `Azure Monitor Metrics`
    - stores numeric data in a time-series database.
  - `Azure Monitor` collects data using `Data Collection Rules` (DCRs)
  - The following data types are collected through DCRs by the `Azure Monitor Agent` (AMA):
    - Windows events
    - Performance counters
    - Syslog
    - Custom logs (text and JSON) - JSON log files on a local disk, collected via AMA
    - Consider queries on Logs data - `Kusto Query Language (KQL)`
    - Consider `alerts` based on Logs and Metrics data

![image](../images/azure-monitor.png)

- `Azure Monitor Logs (Log Analytics) workspaces`
  - Log data is stored in Azure Monitor Logs (Log Analytics) workspace
  - A `workspace` is an Azure resource that serves as an administrative boundary or geographic location for data storage
  - You can deploy one or more `workspaces` in your Azure subscription
  - With Azure RBAC you can grant users and groups only the amount of access they need to work with monitoring data in a workspace
  - Data in an `Azure Monitor Logs workspace` is organized into `tables` - Each table stores different kinds of data
  - You can set billing and retention for each `workspace`.
  - `Workspaces` are hosted on physical clusters, dedicated clusters can be requested

- `Azure Workbooks and Azure Insights`
  - Workbooks 
    - rich visual reports
    - combine data from disparate sources within a single report
  - Insights
    - `Azure Insights` provide a customized monitoring experience for particular applications and services.
    - `Azure Insights` collect and analyze both logs and metrics.
      - Application Insights
      - Container Insights
      - Network Insights
      - Resource group insights
      - Virtual machine insights
      - Azure Cosmos DB insights
      - Azure Key Vault insights
      - Azure Storage insights
  - Best for standard operational logging (<500 GB/day).

- `Azure Data Explorer` - https://learn.microsoft.com/en-us/azure/data-explorer/
  - analyze high volumes of data in near real time
  - supports long data retention in a cost effective manner
  - uses also KQL
  - Best for high-volume logs (>500 GB to Petabytes/day).
  - You create an `Azure Data Explorer Cluster`

- `Azure Monitor vs Azure Data Explorer`
  - Choose Azure Monitor if:
    - You need immediate, turnkey visibility into Azure VMs, AKS clusters, networks, or applications.
    - You want out-of-the-box alerts, action groups, and portal dashboards without managing database infrastructure. 
    - Your daily log ingestion is moderate (generally under 500 GB/day).
  - Choose Azure Data Explorer if:
    - You are processing high-volume telemetry (IoT sensor data, clickstream logs, games telemetry, high-frequency operational metrics).
    - You want to build a custom internal analytics app or SaaS product that queries data using KQL.
    - You want to export high-volume Azure Monitor logs to ADX via Event Hubs to reduce Log Analytics storage and query costs.

- `Microsoft Sentinel`
  - is Microsoft's cloud-native platform that unifies `SIEM` (Security Information and Event Management) 
  and `SOAR` (Security Orchestration, Automation, and Response) into a single service.
