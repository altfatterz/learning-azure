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
        - TODO
 
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
        - TODO


4. **Designing App Configuration and Deployment**
    
   - `Azure Resource Manage` - https://learn.microsoft.com/en-us/azure/azure-resource-manager/
   - `Azure Automation` - https://learn.microsoft.com/en-us/azure/automation/overview/
   - `Azure App Configuration` - https://learn.microsoft.com/en-us/azure/azure-app-configuration/
    
5. **Exploring Application Integration Services**

    - `API Management` - https://learn.microsoft.com/en-us/azure/api-management/
    - `Logic Apps` - https://learn.microsoft.com/en-us/azure/logic-apps/

   Feature-based comparison of the Azure API Management tiers: https://learn.microsoft.com/en-us/azure/api-management/api-management-features

6. **Exploring Azure AD App Proxy**

    - `Azure AD App Proxy` - https://learn.microsoft.com/en-us/azure/active-directory/app-proxy/