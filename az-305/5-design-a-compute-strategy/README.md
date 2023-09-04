## Chapter 5 - Design a Compute Strategy

1. **Hosting and Shared Responsibility**

   - IaaS
   - PaaS
   - FaaS 
   
2. **The Path to the cloud is individual**

   - Architect and Plan
   - Lift and Shift
   - Handover
   - Re-architect and optimize
   - Handover and maintain

3. **Virtual machines**
    - `very versatile` - supports many scenarios, from new solutions to lift-and-shift migrations
    - `full controll` - control everything from the operating system up. Remotely manageable
    - `higher maintenance` - you are responsible for maintenance, patching, security and more

    - VMs have a `name`, not easy to change
    - You deploy to a specific `region/zone` - impacts available hardware (VM families)
    - You deploy it to Network - `region/zone` must match with the VM config
    - NIC resource - (can be more than 1) - provides connectivity
    - Operating System image (marketplace or custom) and disk (at least an OS disk

    - `VM families`
    - control vCPU, RAM, max data disks, max IOPS, temp storage, cost/month
    - VM family types
        - general purpose (balanced CPU to memory, suits dev/test and smaller workloads)
        - compute optimized (high CPU to memory ratio, suits medium workloads)
        - memory optimized (high memory to CPU ratio, suits relation database servers)
        - storage optimized (high disk throughput and IO requirements)
        - GPU (graphics rendering and deap learning workloads)
        - HPC (powerful CPU and network for high performance batch parallel processing)

    - `OS Disk`
        - Premium SSD, Standard SSD, Standard HDD - influence the IOPS of our VM

    - `VM Network` - does span an entire region

    - `VM scale sets` (VMSS)
        - features
            - VM based - with similar benefits and maintenance overheads
            - High Availability - lots of VMs running with the same config
            - Autoscaling - provides capabilities for scaling out to meet demand, autoscaling rules
        - Defined by `image` and `configuration`
        - VMSS instances - deployed to a region or to multiple availability zones within a region
        - Common to use a `load balancer` with VMSS (2 options)
            - `Application Gateway` - is an HTTP/HTTPS web traffic load balancer with URL based routing, SSL
              termination, session persistence and web application firewall
            - `Azure Load Balancer` - supports all TCP/UDP network traffic, port forwarding and outbound flows
        - Possible to use custom image but also
            - Custom data - pass a script, configuration file `while the VM is being previsioned`. The data will be
              saved on the VM.
            - User data - pass a script, configuration file that will be accessible by your
              app `throughout the lifetime of your VM`
        - Upgrade policy - Automatic (random order), Manual, Rolling
        - Enable instance termination notification
        - Enable over-provisioning - improves provisioning success rates and reduces deployment time, you are not billed
          for the extra VMs,

4. **Container Solutions** 

5. **Application hosting**

6. **Large-scale compute**