
- Root management group 
---> management-group-A
---> management-group-B
-------> subscription-1
-----------> resource-group-1
---------------- resource-1 (Vnet)
---------------- resource-2 (VM)
---------------- resource-3 (Storage Account)
-----------> resource group-2


Azure Policies - apply it to `management-group`/`subscription/resource-group/resource` level
- https://docs.microsoft.com/en-us/azure/governance/policy/overview
- uses JSON format

Azure RBAC - apply it to `management-group`/`subscription/resource-group` level

- Azure AD tenant (users are created here, like a Billing Admin)
  - Subscription1
  - Subscription2
  - Subscription3

Subscription types:
- Free Trial
- Pay-As-You-Go
- Enterprise Aggreement Support

Subscription
- Billing Unit that aggregates all costs of underlying resources
- Contain the resource groups and their associated resources
- Scoping level for governance and security
- Can only be associated with a single organisation (Azure AD tenant) at a time


Manamgent Groups
- https://docs.microsoft.com/en-us/azure/governance/management-groups/overview

