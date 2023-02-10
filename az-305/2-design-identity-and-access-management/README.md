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

  - `Hybrid Identity`
    - Simplify the user login experience by providing SSO, maintain the identity as the perimeter of security 
    - Cloud Identity - previously discussed
    - Synchronised Accounts <--- `Azure AD Connect` <--- On-Premise Indentities 
    - Managed Domain <--- `Azure AD DS` <--- Legacy Resources 

- `Azure AD Connect` - different [authentication methods](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/choose-ad-authn#decision-tree) 
  - `Azure AD Connect sync`
    - the on-premises `Azure AD Connect sync` component
    - service side in Azure AD called `Azure AD Connect sync service`
  - `Azure AD Connect Cloud Sync` - newer option

- `Azure AD Domain Services (DS)`
  - taking Active Directory on-premises and having a really similar thing in the cloud
  - you have legacy resource on-premises but you want to work with `Azure AD` which is built for modern authentication
  - You deploy a `Managed Domain` - which supports legacy features like
    - domain join
    - group policy
    - LDAP 
    - Kerberos / NTLM authentication

- `Azure AD external identities`
  - `Azure AD B2B` - intended for partner identities
    - Invite the partner identities to be `Guest Accounts` in our Tenant, this way we can grant permissions to those in similar fashion like for `Cloud Accounts` or `Synchronised Accounts` 
  - `Azure AD B2C` - intended for custom application
    - We create a whole new Tenant but that is dedicated to our application, which can have `Local Accounts` or `External identities` which integrate with social media identity providers (LinkedIn, GitHub, Facebook, Twitter)
    - As developer of the application we don't have to manage and identity platform
    - After creating the new Tenant you need to the followings
      - Register and application
      - Add identity providers (optional) you can just use the `Local Accounts`
      - Create user flows - customise the full experience user will have (sign in/sign up, profile edit, password reset, custom branding)
  - Microsoft is trying to merge these two up into something called `Azure AD External Identities`

Had issue to create a B2C Tenant, giving error like: `The subscription is not registered to use namespace 'Microsoft.AzureActiveDirectory'`

The found solution [from](https://adamstorr.azurewebsites.net/blog/subscription-is-not-registered-to-use-namespace-Microsoft.AzureActiveDirectory)
```bash
$ az account list
$ az provider register --namespace Microsoft.AzureActiveDirectory
$ az provider show -n Microsoft.AzureActiveDirectory
```


- `Azure Subscriptions`
  - each of subscriptions could be associated with a `single` Azure AD Tenant
