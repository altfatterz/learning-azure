## Chapter 8 - Design Apps for the Cloud 

1. **Design Message-Driven Solutions**

   - [Azure Storage Queue](https://learn.microsoft.com/en-us/azure/storage/queues/)
     - HTTP/S access to a queue anywhere in the world. Format: https://<storage account>.queue.core.windows.net/<queue>
     - The queue name must be all lowercase
     - Allows up to 500 TB of messages in a queue. Each message up to 64KB
     - Delivery guarantee: At-Least-Once
     - Ordering guarantee: No
     - Message expire after maximum of 7 days
     - Built-in roles:
       - `Storage Queue Data Contributor` - Allows for read, write, and delete access to Azure Storage queues and queue messages
       - `Storage Queue Data Message Processor` - Allows for peek, receive, and delete access to Azure Storage queue messages
       - `Storage Queue Data Message Sender` - Allows for sending of Azure Storage queue messages
       - `Storage Queue Data Reader` - Allows for read access to Azure Storage queues and queue messages
       
   - [Azure Service Bus (Queue / Topic)](https://learn.microsoft.com/en-us/azure/service-bus-messaging/)
     - advanced messaging over HTTP/S or AMQP: filtering, dead-lettering
     - Allows up to 80 GB of messages in a queue. Each message up to 100 MB
     - Delivery quarantee: 
       - At-Least-Once
       - At-Most-Once
     - Ordering guarantee: Yes, First-In-First-Out
     - Namespace - a container for the queues and topics
     - Pricing Tier
       - Basic (shared capacity, only queues, max message size 256 KB)
       - Standard (shared capacity, queues and topics, max message size 256 KB)
       - Premium (dedicated capacity, queues and topics, max message size 100 MB, zone redundancy, private access, JMS support)
     - Topics:
       - provides publish / subscribe messaging
       - messages are sent to the topics
       - recipients are connecting to subscriptions, which are basically function as duplicate queues
     - Built-in roles:
       - `Azure Service Bus Data Owner` - Allows for full access to Azure Service Bus resources.
       - `Azure Service Bus Data Receiver` - Allows for receive access to Azure Service Bus resources.
       - `Azure Service Bus Data Sender` - Allows for send access to Azure Service Bus resources.

2. **Design Event-Driven Solutions**
 
    - [Event Grid](https://learn.microsoft.com/en-us/azure/event-grid/)
      - Designed for distributing events for reactive event-driven solutions, (MQTT and HTTP protocols)
      - Easy integration with several Azure services as event publishers or event handlers
        - Event publishers: Storage Accounts, Azure Container Registry, Resource Groups, App Services, partner apps, etc
        - Event handlers: Azure Function, Serice Bus Topic / Queue, Webhook (Azure Automation runbooks and Logic Apps)
      - Characteristics:
        - dynamically scalable
        - low cost
        - serverless
        - at least once delivery of an event
      - `Azure Event Grid` - a fully managed PaaS service on Azure
      - `Event Grid on Kubernetes with Azure Arc` - lets you use Event Grid on your Kubernetes cluster
      - Built-in roles:
        - `EventGrid Contributor` - Lets you manage EventGrid operations.
        - `EventGrid Data Sender` - Allows send access to event grid events.
        - `EventGrid EventSubscription Contributor` - Lets you manage EventGrid event subscription operations. 
        - `EventGrid EventSubscription Reader` - Lets you read EventGrid event subscriptions.
      
  - [Event Hub](https://learn.microsoft.com/en-us/azure/event-hubs/)
    - Designed for real-time big data ingestion and streaming
    - Scales to support millions of events per second at sub-second latency
    - Support of AMQP, [Apache Kafka](https://learn.microsoft.com/en-us/azure/event-hubs/azure-event-hubs-kafka-overview)
    - Namespace - a container pricing tear and throughput
    - Characteristics:
      - Reliable asynchronous message delivery (enterprise messaging as a service) that requires polling
      - Advanced messaging features like first-in and first-out (FIFO), batching/sessions, transactions, dead-lettering, temporal control, routing and filtering, and duplicate detection
      - At least once delivery of a message
      - Optional ordered delivery of messages
    - Pricing Tier:
      - Basic (1 consumer group, 100 broker connections, 1 day message retention)
      - Standard (20 consumer group, 1000 broker connections, 7 day message retention)
      - Premium (100 consumer group, 10K broker connections, 90 day message retention, Schema Registry)
    - Built-in roles:
      - `Azure Event Hubs Data Owner` - Allows for full access to Azure Event Hubs resources.
      - `Azure Event Hubs Data Receiver` - Allows receive access to Azure Event Hubs resources.
      - `Azure Event Hubs Data Sender` - Allows send access to Azure Event Hubs resources.

      - Comparing messaging services: https://learn.microsoft.com/en-us/azure/service-bus-messaging/compare-messaging-services#comparison-of-services

    - Event Grid  --> Reactive programming --> Event distribution (discrete) --> React to status changes
    - Event Hubs  --> Big data pipeline    --> Event streaming (series)      --> Telemetry and distributed data streaming
    - Service Bus --> Enterprise messaging --> Message                       --> Order processing and financial transactions
   

3. **Exploring Caching Services**
    
    - [Azure Cache for Redis](https://learn.microsoft.com/en-us/azure/azure-cache-for-redis)
      - Microsoft managed OSS/Enterprise Redis
      - Read write to an in-memory data store
      - Access session state a nd other data fast
      - Service Tiers:
        - Basic (6 versions, C0 - shared, C1-6 - dedicated)
        - Standard (6 versions)
        - Premium (6 versions, VPN support, Redis cluster, high network bandwith)
      - Built-in roles:
        - Redis Cache Contributor - Lets you manage Redis caches, but not access to them.
 
    - [Content Delivery Network](https://learn.microsoft.com/en-us/azure/frontdoor/)
      - Speed up access to static content (images, CSS or HTML)
      - Azure Front Door and CDN kind of merging into a single product
      - Leverage Microsoft or partner network
      - `CDN profile` 
        - network provider (Microsoft, Akamai, Edgio (formerly Verzion)
        - pricing and other features
      - `Endpoint`
        - http:<CDN-endpoint-name>.azureedge.net 
        - the public facing endpoint that users will be directed to
      - `Origin / Origin Group`
        - the underlying solution (webapps, storage accounts) that users are actually accessing (mapped to an endpoint)
      - Features:
        - accelerate static and dynamic (with Azure Front Door) content
        - caching rules
        - HTTPS custom domain support
        - File compression
        - Geo-filtering - block access based on location
      - Built-in roles:
        - CDN Endpoint Contributor - Can manage CDN endpoints, but can’t grant access to other users.
        - CDN Endpoint Reader - Can view CDN endpoints, but can’t make changes.
        - CDN Profile Contributor - Can manage CDN profiles and their endpoints, but can’t grant access to other users.
        - CDN Profile Reader - Can view CDN profiles and their endpoints, but can’t make changes.

4. **Designing App Configuration and Deployment**
    
   - [Azure Resource Manage](https://learn.microsoft.com/en-us/azure/azure-resource-manager)
     - `ARM template` - infrastructure as code, JSON vs Bicep (less verbose) based
     - Azure Portal, Azure Powershell, Azure CLI --> Azure Resource Manager --> Provision Infrastructure
     - `Resource provider` - A service that supplies Azure resources. Example: `Microsoft.Compute`, `Microsoft.Storage` etc
     
   - [Azure Automation](https://learn.microsoft.com/en-us/azure/automation/overview/)
     - `Process Automation`
       - schedule or trigger runbooks (Powershell, python, graphical)
       - executed on `Azure Sandbox Worker` or on `Hybrid Runbook Worker`
     - `Configuration Management`
       - `Change Tracking and Inventory`
         - Supports change tracking services in your environment to help you diagnose unwanted changes and raise alerts
       - `Azure Automation State Configuration`
         - Manage your DSC (desired state configration) resources and apply configurations to virtual or physical machines from a DSC pull server in the Azure cloud.
     - `Update Management`
        - Allows report and to rollout update compliance across Azure and other clouds, and on-premises within a definied maintenance window
     - Key components:
       - Automation Account - container for shared resources and identities
       - Automation Worker (Azure Sandbox Worker or Hybrid Automation Worker)
     - Built-in roles:
       - `Automation Contributor` - Manage azure automation resources and other resources using azure automation.
       - `Automation Job Operator` - Create and Manage Jobs using Automation Runbooks.
       - `Automation Operator` - Automation Operators are able to start, stop, suspend, and resume jobs
       - `Automation Runbook Operator` - Read Runbook properties - to be able to create Jobs of the runbook.
       
   - [Azure App Configuration](https://learn.microsoft.com/en-us/azure/azure-app-configuration/)
     - Complements Azure KeyVault, which is used to store application secrets. App Configuration makes it easier to implement the following scenarios:
       - Centralize management and distribution of hierarchical configuration data for different environments and geographies
       - Dynamically change application settings without the need to redeploy or restart an application
       - Control feature availability in real-time (feature flags)
     - The easiest way to add an App Configuration store to your application is through a client library provided
     - Every request to an Azure App Configuration resource must be authenticated. (access key or Azure AD credentials (preferred))
     - Recommendation: managed identity from Azure AD allows Azure App Configuration to easily access other Azure AD protected resources.
     - Built-on roles
       - `App Configuration Data Owner` - Allows full access to App Configuration data.
       - `App Configuration Data Reader` - Allows read access to App Configuration data.

5. **Exploring Application Integration Services**

    - [API Management](https://learn.microsoft.com/en-us/azure/api-management/)
      - `API Gateway`
        - Acts as a facade to backend services by accepting API calls and routing them to appropriate backends
        - Verifies API keys and other credentials such as JWT tokens and certificates presented with requests
        - Enforces usage quotas and rate limits
        - Optionally transforms requests and responses as specified in policy statements
        - If configured, caches responses to improve response latency and minimize the load on backend services
        - Emits logs, metrics, and traces for monitoring, reporting, and troubleshooting
        - Virtual Network connectivity:
          - `None`
          - `Internal` 
            - the API gateway can connect to backend via private IP,
            - We get an Internal Load Balancer. The client cannot connect unless they are in this internal virtual network 
          - `External`
            - the API gateway can connect to backend via private IP still, but with Public Load Balancer
      - `Management plane`
        - manage users, get insights from analytics, import API schemas (OpenAPI, WebSocket, GraphQL backends)
        - setup policies like quotas or transformations on the APIs
      - `Developer portal`
        - Web interface that we can provide to business partners
      - Built-in roles:
        - TODO
        
    - [Logic Apps](https://learn.microsoft.com/en-us/azure/logic-apps/)

   Feature-based comparison of the Azure API Management tiers: https://learn.microsoft.com/en-us/azure/api-management/api-management-features

6. **Exploring Azure AD App Proxy**

    - [Azure AD App Proxy](https://learn.microsoft.com/en-us/azure/active-directory/app-proxy/)