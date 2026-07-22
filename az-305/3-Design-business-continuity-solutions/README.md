# AZ-305: Design business continuity solutions

## Design high availability and disaster recovery strategies

- `High Availability and Disaster Recovery` (HADR)

- Describe recovery time objective and recovery point objective
  - `Recovery Time Objective` (RTO) - `maximum amount of time` available to bring resources online after an outage or problem
    - if it takes longer could be financial penalties
    - can be specified for the whole solution or for individual components like SQL Server
    - the whole solution RTO is determined by the slowest component to recover
  - `Rocovery Point Objective` - the `point in time` to which a database should be recovered 
    and equates to the maximum amount of data loss that the business is willing to accept

- Explore high availability and disaster recovery options
  - `IaaS` - You choose, configure, and manage your custom HADR strategy.
  - `PaaS` - Offers built-in, highly automated `HADR` features with minimal user configuration required.
  
    
- Describe Azure high availability and disaster recovery features for Azure Virtual Machines
  - these enhance `availability`:
    - `Availability Sets` (Datacenter-level) - 99.95% - use it as a fallback with regions where `Availability Zones` not supported
      - Spread your VMs across different racks, power units, and switches within a single datacenter. This protects against localized server hardware failures.
    - `Availability Zones` (Region-level) - 99.99% - recommended
      - Spread your VMs across physically separate datacenters within an Azure region
    - `Azure Site Recovery`
      - Disaster Recovery as a Service (DRaaS)
      - it continuously syncs your data to a secondary location so you can flip a switch and spin up replicas 
      if your primary location goes down.
      - it works across hybrid and multi-cloud environment
  - you cannot combine Availability Sets and Availability Zones 

- Describe high availability and disaster recovery for PaaS deployments
  - HA: 
    - Built-in Node Redundancy
    - Zone Redundancy with a single toggle
  - DR: 
    - PaaS offers turnkey tools to handle cross-region replication and failover


## Design a solution for backup and disaster recovery

- Design for backup and recovery
- Design for Azure Backup
- Design for Azure blob backup and recovery
- Design for Azure files backup and recovery
- Design for Azure virtual machine backup and recovery
- Design for Azure SQL backup and recovery
- Design for Azure Site Recovery