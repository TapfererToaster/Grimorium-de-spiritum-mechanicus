# Azure Arc
A set of technologies that help to manage cloud environments.
# Azure VMware Solution
Allows consumers who already have a private cloud with VMware to run VMware workloads in Azure. This can be done if the consumer wants to migrate to a public or hybrid cloud.
# Global Infrastructure
The key components of the global infrastructure are:
- **Azure physical data center**:
  secure facility that hosts the hardware, also called *premises*
- **Edge locations**:
  facilities where traffic enters and leaves the MS global network/ wide area network. These facilities allow that computing power is closer to users and improve network latency, as fewer hops across fewer ISPs are used.
- **Microsoft global network**:
  a large private network with global/cross-ocean fiber cables owned or leased by MS. They connect data centers to regional gateways and edge locations  

>[!note]
>Microsoft also offers a service called *ExpressRoute*, where traffic from a customer's *Multiprotocol Label Switching (MPLS)* WAN to enter Microsofts network. This creates a low-latency private network connection from the customer to the Azure regions, which bypasses the internet entirely.
>
## Azure Regions and Geographies
![[Regions and Geographies.png]]
The location of a resource will be chosen based on a organizations needs. The needs can be a *technical driver*, such as service capability / availability or latency in that region, or a *business driver*, such as compliance, data residency or cost.

Microsofts data centers are organized in groups, called *regions*, which belong to a *geography* and are deployed in pairs for disaster recovery. 
The data centers inside a region are connected by a low-latency network, which provides the basis for *availability zones*.
*Regional services*, such as VMs are created in the data center of a region are dependent on being co-located in that region.
There are also *non-regional services*, such as Traffic Manager and Front Door that are only available in specific regions. Services like DNS and Entra Domain Services are *geo-based* to ensure the data is kept within a geography.

- **Europe** geography:
	- *North Europe* region, located in Ireland
	- *West Europe* region, located in Netherlands
- **UK** geography:
	- *UK South* region, located in London
	- UK *West* region, located in Cardiff
- **Asia** geography:
	- *East Asia* region, located in Hong Kong
	- *Southeast Asia* region, located in Singapore

>[!note]
> The complete list can be found [here](https://learn.microsoft.com/en-us/azure/reliability/regions-list).

>[!note]
>Data transfer leaving a region (*egress*) is billed, while traffic within a region (*ingress*) is free.

## Azure Edge Zones
*Edge Zones* allow Azure to incorporate *edge computing*, which data processing and code to be executed closer to the end user. The workload processing is completed outside the MS data center regions, but within MS *Point-of-Presence (POP)* locations or third-party data center locations.

>[!note] Point of Presence (POP) locations
>Are locations outside of MS data center regions but have Azure hosted services for workload processing.

![[Edge Zones.png]]
- **Edge Zones**:
  - Provide services such as ExpressRoute, CDN and Azure Front Door service
  - part of Azure public cloud (shared) platform resources (Microsoft hardware) within POP edge locations
  - Connected to MS global network
- **Azure Edge Zones with the carrier**:
	- part of Azure public cloud platform resources but are shipped to exist within the carrier's data center locations
	- part of the carrier's global network
- **Azure private Edge Zones**:
	- part of Azure Stack (private) cloud platform (customer or third-party hardware) within customer's location or a third-party hosting provider
	- Zones are part of the customer or hosting provider's private network and not of a carrier or Microsoft network
	- Zones can be connected to Microsoft data center regions with a hybrid connectivity using a VPN or ExpressRoute   
## Sovereign Regions
Azure provides *sovereign regions*, these are isolated cloud-platform regions that run dedicated hardware and isolated networks where they are located. These support greater compliance for specific markets and have their own portals with different URLs and service endpoints in DNS.

## Availability Components
Microsoft is responsible to provide a range of availability options, that the customer has to implement according to their availability strategy that meets the Service-Level Agreement (SLA). 
>[!note]
>MS SLAs are based on financial compensation when the defined SLA is not met.

*Availability solutions* are based on the components and resources that need to be protected:
![[Availability Components 1.png]]
### Adopting an Availability Model
You should look at possible failures at the level you are concerned with, determine its impact and probability and then implement a solution. The drive for a model should to determine your required SLA, which can be increased by combining different availability components.
### Availability Sets
*Availability sets* deploy VMs in different *fault domains* and *update domains*, meaning the hardware is in physically different racks. This is done to mitigate "within-rack failures" and to ensure only one instance is updated at a time to ensure the update does not impact operations.

![[Availability Sets.png|470x378]]

Some points to consider are:
- A VM is part of a single availability set
- VMs can only be added to an availability set when created, later addition is not possible unless the VM is deleted and redeployed
- VMs need to be created in the same resource group as the availability set to add the VM to the set
- Three fault domains and five update domains (max. 20 can be configured) are part of an availability set

### Fault Domains
A *Fault domain* is a group of resources in a data center rack that share the same power and network connection. This prevents that in case of a outage all VMs are affected.
![[Fault Domains.png|482x342]]

### Update Domains
VMs in the same update domain will be updated and rebooted at the same time. Because of this you should put each VM/server in different update domains, so there is always a server available.
![[Update Domains.png|485x420]]
### Availability Zones
Some regions are further divided into availability zones.
Availability zones provide redundancy within a region to protect the operation of resources against a "data center failure".
Azure resources are placed into:
- **Zonal services**: 
  resources can be placed into a specific zone based on needs, performance or latency
- **Zone-redundant services**:
	  - replication of resources across zones is automatic
	  - you cannot define the replication settings governing how resources are distributed across zones
- **Non-regional services**:
	- Services are available in all geographies and are not affected by zone or region wide outages

![[Availability Zones.png]]

>[!note]
>The availability of resources is provided by each zone using independent power, cooling and networking. A outage in one zone does not affect the availability of resources in other zones.
>To protect against an entire region outage, resources can be replicated in a secondary region.
### Proximity Placement Groups
Proximity Placement Groups are a logical entity and an architectural component that ensures low latency by placing compute resources physically adjacent and collocated within the same physical data center.
In combination with *Availability Sets* you can put the compute resources into different racks, to resolve potential issues with outage and failures. 
![[Proximity Placement Groups.png]]

>[!note]
>A few things concerning proximity placement groups:
>- They are Azure resources and need to be created before they can be used
>- They can be used wit VMs, VM Scale Sets and availability sets
>- When creating the compute resource, specify the proximity placement group previously created
>- To move existing compute resources into a proximity placement group, the resource will need to be stopped
>- They are set at the resource level, not the individual virtual machine level, for availability and VM Scale Sets
>- Workloads such as SAP HANA with SAP NetWeaver is an example of the importance of having the machines separated for high availability/SLA but close enough for minimum latency

# Resource Management
The managing of data and workloads should include the following:
- **Availability**
  Managed though redundancy, replication and traffic management
- **Protection**
  Managed through backup and disaster recovery
- **Security**
  Managed through threat protection and security posture management
- **Configuration**
  Managed through automation, scripting and update management
- **Governance**
  Managed through access control, compliance and cost management
- **Monitoring**
  Managed through the collection of security incidents, events resource health, performance metrics, logs and diagnostics

>[!note]
>The foundation for management within Azure is the *Azure Resource Manager (ARM)* API, which is also the management deployment service for Azure. It provides a management layer and orchestration engine that processes all requests for Azure.
>
## Management Scopes 
*Role-Based Access Control (RBAC)* and Azure Policies can be targeted at the following levels:
- Management group level 
- Subscription level
- Resource group level
- Resource level

![[Management Scopes.png]]

### Management Groups
*Management Groups* are logical container that group multiple Azure subscriptions. Allowing to govern and manage access control and policies.
![[Management groups.png]]
Management groups have the following limitations:
- They can only contain other management groups and subscriptions, not resource groups or resources
- The scope is per tenant not across tenants
- The Parent management group cannot be deleted or moved, only renamed
- Only one parent management group is supported and only one parent hierarchy is a single tenant
- Supports up to 1000 child management groups
- Up to six hierarchy levels are supported

### Subscriptions
*Subscriptions* group together resources that share the same billing and access controls.
![[Subscription.png]]
The subscription should be created in a new or existing Entra ID tenancy as it is the foundation and starting point. 

A few things to consider:
- Subscriptions cannot be merged, but can be moved into other subscriptions.
- If a subscription is is deleted, all resources within the subscription are deleted; if a subscription expires the resources remain
- The billing owner can be changed
- You should create the tenancy, subscription and resource groups in that order

**Relationship between Tenants and Subscriptions**
