- `Key Concepts`
    - Traditionally security was based on the network perimeter
    - Cloud changed the resource hosting and access model
    - Modern solutions are now designed with identity at the center
    - Identities like: cloud resources, SaaS, cloud managed devices, on-premise resources
    - Azure Active Directory - to store all those identities
      - handles authentication (AuthN) and authorization (Authz)
      - application identity (Applications) 
      - user account (Staff)

- `Azure AD`
  - Azure AD Tenant has a domain like: 
    - altfatterzgmail.onmicrosoft.com
    - cloudnativecoach.onmicrosoft.com
    - flowable.com
    Azure AD Tenant has a licence like:
    - Azure AD Premium P2
    - Azure Free
  - Tenant ID
  - Azure AD resources (Identity Types)
    - `User Accounts` (Staff Members) - 
      - uses credentials like username/password)
      - can be cloud users, synchronized, guest users
    - `Applications` (Service Principals)  
      - represent application in use within the tenant
      - uses a secret token or certificate for authentication
      - can include apps running on Azure or somewhere else
    - `Managed Identities` 
      - to provide identity for Azure resources
      - leverages the Azure platform for authentication
      - only supports services running in Azure
      
  - Groups
    - reduce the effort required to manage access
    - type: `Security` or `Microsoft 365`
    - membership type: `Assigned` or `Dynamic` (required Azure AD P1 license) (`Dynamic User` / `Dynamic Device`)
      - Azure AD roles can be assigned only to the `Assigned` type
      - Azure AD roles cannot be assigned to the `Dynamice` types
    - You cannot add or remove members from a `Dynamic` group

  - Users
    - `Create user` option - Cloud user
    - `Invite user` option (Azure AD B2B)



- `Azure Subscriptions`
  - each of subscriptions could be associated with a `single` Azure AD Tenant
