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

## Availability Solutions
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

**Multiple Subscriptions**
An organization can have multiple subscriptions within the same tenant or another tenant the organization owns. 
### Resource Group
*Resource Group* is a logical container that groups together resources that are to be managed as a group.
![[Resource Group.png]]

The basis for the group could be that the resources share the same life cycle, dependencies, access control or management requirements.
The resources could also be grouped by environment, business unit, region or other combinations.
![[Resource Group groupings.png]]

**Characteristics**:
- Resource Groups work at the management plane level
- Resource groups contain metadata about the resources they contain
- Deleting a resource group will remove all resources within it

**Organization**
When planing to create a Azure resource, you must first define access and billing, as this will shape the subscription and resource group strategy. 
The order these elements should be created is:
- Tenancy
- Subscription
- Management groups
- Resource groups
### Resource
A *Resource* is a billable item and the building block of all services and solutions in Azure. Each resource has an ID with a billing meter that calculates the amount consumed and billing rate.

Things to observe:
- Each resource can be associated with only one resource group
- Resources must belong to a resource group and con only exist in a single group, but can be moved between resource groups
- Resources can interact with other resources in the same groups. other groups and subscriptions
- Resources work at the data plane level
- Resources inherit all permissions set at the resource group level they belong to 
- When adding new resources to a resource group, they inherit those permissions ans any access assignments
- When moving resources, the resources lose the permissions of the resource group they belonged to and inherit those of the new resource group 

### Relationships
**Relationship between Tenants and Subscriptions**
![[Relationship between Tenants and Subscriptions.png]]
- **Entra ID tenant**
  Contains users, groups and M365 subscriptions
- **Azure subscription**
  Contains resource groups, including resources

**Subscription**
- Each Azure subscription can be associated with only one Entra ID tenant in a parent-child relationship
- Each subscription can have multiple resource groups, but each resource group can be part of only one subscription
- The billing owner can be changed
- A subscription can be moved to a different Entra ID tenant

**Resource**
- Each resource can be associated with only one resource group

**User**
- Each user can be associated with only one Entra ID tenant
- Each user can access more than one Entra ID tenant

**Group**
- Each group can be associated with only one Entra ID tenant

**M365 subscription**
- Each subscription can be associated with only one Entra IF tenant (a domain)

## Azure Resource Manager (ARM)
ARM is the deployment and management service for Azure, it provides a control and management layer for the creation, update and deletion of resources as well as control access, application of policy, governance and compliance. 
![[ARM architecture.png]]

ARM provides the following:
- Deploy, manage and monitor as it pertains to resource groupings rather than individual resources
- Access control and policies at the resource group level that are inherited by the resources in that group
- Application of tags to resources for billing logical grouping and management 
- Repeatable life cycle deployment that result in a consistent state
- Use of deployment template files to define the deployment of resources
- Creation of resource dependencies

- **Resource provider**
  Services that contains and provides Azure resources; f.e. a book is a reading resource and the library is the provider
- **Templates**
  [[(JSON) JavaScript Object Notation|JSON]] or Bicep files providing repeatable and automated deployment

# Core Resources
## Compute Resources
>[!note]
>The term "compute resource" means "a platform to execute code on" and is used to run software and process data.

The following compute resources are available on Azure:
- [[Virtual Machines]]
- [[Container]]
- App Services
- Functions
- Azure Virtual Desktop

### Virtual Machines
Azure has different VM types, each tailored to a different use case and workload.
The types are broken down into categories and family series, which are identified by a alphabetic character. A naming convention is also used to describe the intended use of the VM.
Some examples are:
- Subfamilies
- The number of virtual CPUs (vCPUs)
- Additive features
- Versions

**Familiy Series**
- **General purpose** (A, B, D family):
  VMs have a balanced CPU-to-memory ratio and are best suited for testing and development (A series only), burstable workloads (B series only) and general-purpose workloads (D series only).
- **Memory optimized** (E, M family):
  VMs have a high memory-to-CPU ratio and best suited for relational databases, in-memory analytics and workloads where bottlenecks originate in the memory
- **Storage optimized** (L family):
  VMs have high disk throughput and I/O and are best suited for data analytics, data warehousing and workloads there bottlenecks relate to the disk
- **GPU optimized** (N family):
  VMs have graphics processing units and are best suited for compute-intensive graphics- / gaming intensive, visualization and video conferencing / streaming workloads
- **High performance** (H family):
  VMs have powerful CPUs offering high-speed throughput network interfaces and are best suited compute and network workloads like SAP HANA

**Naming Convention**
```
[Family] + [Sub-family] + [# of vCPUs] + [Additive Features] + [Version]
```
 
- **Family**:
  The VM family series
- **Sub-family**:
  This represents the specialized VM differentiations
- **Number of vCPUs**:
  Number of vCPUs of the VM
- **Additive Features**:
	- *a*: AMD-based CPU
	- *d*: VM with a local temp disk
	- *i*: Isolated size
	- *l*: low memory size
	- *m*: memory-intensive size
	- *s*: Premium Storage capabilities
	- *t*: tiny memory size
- **Version**:
  Version of the VM family series

>[!note]
>Examples:
>- B2ms: B series, 2 CPUs, memory intensive, Premium Storage capable
>- D4ds v5: D series, 4 CPUs, local temp disk, Premium Storage capable, version 5

#### Deployment Considerations
![[VM deployment considerations.png]]
When deploying VMs on Azure you should take broader considerations.
- **Additional VM resources**
  A VM has a vCPU and memory, but you have to provide an OS, software, storage, networking, connectivity and security for the VM
- **Location and data residency**
  Not all VM types and sizes are available in all regions
- **VM quota limits**
  Each Azure subscription has quota limits that limit the number of VMs, the VMs total cores and VMs per series
- **Monitoring**
  It is very important to be able to monitor your resources
- **Backup**
  Microsoft does not automatically back up the OS or software running on the VMs 
- **Update Management**
  Microsoft does not automatically update the OS or software running on the VMs
- **Availability**
  The two components addressing the availability SLA requirements are availability sets and availability zones. The VMs will have downtime and by default Microsoft will not provide availability zones or sets
- **Scalability**
  The ability to handle increased loads while still being available. VMs typically do not support scaling, however IaaS like Scale Set can provide this functionality 
  >[!note] 
  >Scale set is an IaaS with "auto-scaling" for VM-based workloads such as web and application services. You specify the number of VMs and OS type to be deployed in the Scale Set. These provide multiple fault domains and update domains as they are automatically deployed and protect against data center failures.
  >This provides automatic scaling, load balancing, availability, and fault tolerance.
  
### [[Container]]
The container services are described as:
- **Azure Container Instances (ACI)**: [[Cloud#Platform as a Service (PaaS)|Platform as a Service]] for containers running in Azure
- **Azure Container Apps (APA)**: Like ACI, but provides scaling and load balancing
- **Azure Kubernetes Service (AKS)**: Container hosting platform and orchestration service for managing containers

#### Azure Container Instances (ACI)
*Azure Container Instances* are a multitenant, serverless *Container as a Service (CaaS)* platform, which is basically a PaaS for containers running on Azure. You can create a container without concerning yourself with management or additional services and orchestration.
ACI are best suited for jobs that:
- are rarely used or scheduled,
- temporary or demand-driven/event-driven 
- do not interact with other containers or services 
- do not require advanced orchestration, load balancing or autoscaling

>[!note]
>*serverless* means that Microsoft provides and manages the underlying hosts, VMs, container engine, orchestration components and so on.
#### Azure Container Apps (ACA)
*ACA* is a fully managed serverless container service platform using Kubernetes, DAPR, Kubernetes Event-Driven Autoscaling (KEDA) and envoy open-source technologies.
It allows applications and microservices to be built without having to manage container orchestration.

>[!note]
>No direct access to the underlying APIs, control plane and cluster management of Kubernetes is given.

#### Azure Kubernetes Service (AKS)
*AKS* is a container hosting platform and orchestration service for managing containers at a large scale. 

> [!NOTE]
> Like ACI it can be considered as a CaaS, but is designed for highly customizable, scalable and portable workloads

The AKS service architecture has two elements, the master node (the control plane managed by Microsoft) and the worker node (Pods managed by the customer). The master node is responsible for scheduling the worker nodes, Pods. The worker nodes are the VMs that run the node's components and the container runtime. 

### Azure App Service
![[Azure App Service.png]]

*Azure App Service* is a [[Cloud#Platform as a Service (PaaS)|Platform as a Service]] providing a website and code-hosting platform fully managed by Microsoft. It supports Windows and Linux workloads, multiple frameworks and programming languages.
Each app service runs inside an "App Service plan", each with a tariff that has a set of features and functionalities. 
The App Service plan defines the number, the type, size, properties and the region of the VMs that will be created.
The pricing tiers of App Service are:
- *Free and shared*
  intended for development and testing; no SLA is provided; your app will run on the same resources as other customers and there is no support for autoscaling, hybrid or VNet connectivity
- *Basic service plan*
  intended for low traffic usage that dies not require advanced autoscaling and traffic management features; it has basic load balancing across instances, custom domains and hybrid connectivity is supported 
- *Standard service plan*
  intended for running production workloads; it supports custom domains, autoscaling, hybrid and VNet connectivity
- *Premium V2/V3*
  intended for larger scale and performance production workloads; it supports all features of the standard plan, plus private endpoints
- *Isolated V2/V3*
  intended for mission-critical workloads required to run on a virtual network in a private, dedicated environment

### Azure Functions
*Azure Functions* is a [[Cloud#Serverless/ Function as a Service (FaaS)|Function as a Service (FaaS)]] or "serverless code engine". It is used for events that trigger code, the Functions code is then executed as a response to that event trigger.

### Azure Virtual Desktop Service
![[Azure Virtual Desktop Service.png]]
*Azure Virtual Desktop Service* is a [[Cloud#Platform as a Service (PaaS)|PaaS]] that allows users to connect to their desktops and applications hosted in Azure from any location with internet connection or a private managed network (such as Microsoft ExpressRoute service).
Virtual Desktop Service has the following components:
- *Microsoft-provided and -managed platform services*:
  PaaS functions where Microsofts provides managed web access, gateway and broker roles; these allow secure connectivity service layer that connects users with their desktop and apps
- *Host pools*:
Collection of VMs that will run the users' desktop and remote apps.
>[!note]
>There are two load-balancing methods:
>- *Depth mode*:
>  saves costs by utilizing a single VM to host users' sessions before placing a session on the next VM, this is called *vertical load balancing*
>- *Breadth mode*:
>  best performance as each user session is created on the next available VM and never on the same VM as the last session, this is called *horizontal load balancing*
- *Customer-created desktops*:
Infrastructure resources providing the users' desktop and apps
- *Customer-created remote applications*:
Applications no longer need to be installed on the VM OS itself, through the MSIX app attach functionality they are decoupled from the OS and are dynamically attached to the VM the user is connected to
- *Users profiles*:
FSLogix profile container is used to store the user profile on a *Virtual Hard Disk (VHD)*, which is then dynamically attached to the VM the user is connected to
- *Third-party provided and managed virtual desktop platform services*:
Vendors such as Citrix and VMware are part of the ecosystem and provide additional features; you can also use their presentation or management layers to connect to your virtual desktop

## Azure Network Services
