Cloud computing is the delivery of services and infrastructure, such as VMs, storage, databases and networking, as well as IoT, machine learning (ML) and AI using a consumption based pricing model.
Cloud computing allows the user to up- or downscale the services or infrastructure they need, f.e. if the needed computing power increases you increase your IT footprint instead of building a new data center.

>[!note]
>Cloud services fall under *operational expenditure (OpEx)*, meaning you pay a regular sum for the resources you use.
>The opposite is *capital expenditure (CapEx)*, where you pay a one-time, up-front expenditure.

# NIST Definition
The National Institute of Standards and Technology (NIST) has a multifaceted definition that describes characteristics of cloud computing and services as well as deployment models.
Essential **characteristics** of cloud computing:
- *On-demand self-service*:
  Users should be able to provision their own resources from the cloud provider as needed, without needing to involve human interaction (service ticket).
- *Broad network access*:
  Cloud environments are inherently networked environments, accessible over the network. Without network access, cloud computing is not possible.
- *Resource pooling*:
  Cloud provider resources are pooled and served to multiple tenants automatically. This includes physical and virtual resources and applies not only to computing capacity but also to storage and networking.
- *Rapid elasticity*
  Cloud resources should be able to scale "infinitely", scaling up or down, based on demand - even automatic, in some cases
- *Measured service*:
  Cloud resources are metered as users are charged for what they use. Usage can be monitored, controlled and reported

Three service models:
- *Software as a Service (SaaS)*
- *Platform as a Service (PaaS)*
- *Infrastructure as a Service (IaaS)*

Deployment models:
- *Private cloud*
- *Community cloud*
- *Public cloud*
- *Hybrid cloud*
# Shared Responsibility Model
With a traditional data center the company is responsible for maintaining the physical space, ensuring security und maintaining and replacing server and the IT department is responsible for the digital infrastructure and the software.
When using cloud services the responsibilities are split between the cloud provider and the consumer depending on what service is used:
- *Software as a Service (SaaS)*
- *Platform as a Service (PaaS)*
- *Infrastructure as a Service (IaaS)*

![[Azure shared responsibility.png]]The cloud provider is always responsible for the physical space and the hardware, while the consumer is always responsible for administrative task including the data stored and who and what device can access the cloud.
# Evolution of the Cloud Model and Architecture
## Evolution
![[Evolution of Cloud Models.png]]

>[!note] Public and Edge Computing
>- *Public cloud* has a centralized data collection, processing and analysis approach.   
>- *Edge computing* has a distribute computing approach where data is collected, processed and analyzed locally. This provides better latency and can be approached when a company has strict compliance on where data is stored and processed.
> 

## Cloud Architecture
![[Evolution Cloud Architecure.png]]
# Cloud models
![[Cloud models.png]]

|**Public cloud**|**Private cloud**|**Hybrid cloud**|
|---|---|---|
|No capital expenditures to scale up|Organizations have complete control over resources and security|Provides the most flexibility|
|Applications can be quickly provisioned and deprovisioned|Data is not collocated with other organizations’ data|Organizations determine where to run their applications|
|Organizations pay only for what they use|Hardware must be purchased for startup and maintenance|Organizations control security, compliance, or legal requirements|
|Organizations don’t have complete control over resources and security|Organizations are responsible for hardware maintenance and updates||

## Private Cloud
A private cloud is used by a single entity (tenant), which can be hosted from an onsite datacenter, a dedicated offsite datacenter or even by a third party cloud provider. The hardware and resources are dedicated to the tenant and they are in complete control of it. 

Characteristics:
- Computing resources can be created on-premises or provided by a cloud provider, resources are only available within the capacity provisioned
-  Hardware is dedicated to a single tenant
- On-premises resources may be decommissioned if a cloud provider is used
- Computing resource access is possible via a local/private network; the private cloud may be disconnected from the internet
- The tenant has complete control of the security, governance, hardware and compliance

Examples of private cloud platforms:
- Azure Stack
- Red Hat OpenShift
- VMware vCloud Suite
## Public Cloud
Public clouds are built, controlled and maintained by third party cloud providers and can be used by multiple tenants, which all have a share of the resources.

>[!note] Multi-cloud
>It is also possible to use multiple public cloud provider, which means you manage resources and security in these environments.

Characteristics:
- Consumption-based billing, you pay only for the resources you used
- Almost unlimited resources are available
- Performance, scalability and elasticity is provided, rapid, on-demand and automated provisioning and de-provisioning of compute resources are available
- Availability, reliability, fault tolerance and redundancy are provided
- Resource access is possible via internet and privately managed networks
- Self-service management though a web browser or a command-line interface
- Least control over security, protection and compliance
- Access can be configured with [[Entra ID]] or Windows Server Active Directory when you sync the identity provider directories.

Examples of public cloud platforms:
- Microsoft [[Azure]]
- Amazon Web Services (AWS)
- Alibaba Cloud
- Google Cloud Platform
## Hybrid Cloud
Hybrid clouds use an interconnected environment of public and private clouds that allow private clouds to temporarily use public cloud resources if computing demands surge. 
It is also possible to flexibly choose which services should be run in a public or private cloud.

Characteristics:
- Allows the tenant to use resources on the cloud providers public cloud and other resources on on-premises private cloud, which can be connected with Microsoft ExpressRoute
- Allows to extend computing resource capacity to a public cloud
- Private cloud hardware must be maintained by the tenant, public cloud hardware is maintained by the cloud provider
# Service Types
![[Service Types 1.png|700x274]]
## Infrastructure as a Service (IaaS)
When using IaaS you rent the hardware in the datacenter of the cloud provider and can host virtual machines and infrastructure services.

Characteristics:
- The tenant creates Virtual Machines (installing OS and software), storage, and computing resources 
- Resources such as processors, memory and VMs can be increased with self-service without redeploying or creating new VMs
- Direct access and complete control over networking, security, VMs, the OS and roles/services, such as servers installed or running on the VMs

Microsoft IaaS resources:
- Azure Virtual Machines
- Azure Storage
- Azure networking

>[!note]
>IaaS VMs do not provide scale, elasticity, high availability and disaster recovery no resources that provide these functions are not designed and implemented.
### Scenarios
- **Lift-and-shift migration**:
  You replicate the existing on-prem datacenter with cloud resources and then move the things running on-prem to the IaaS.
- **Testing and development**:
  You configure a test environment that you can rapidly replicate. With IaaS you can start up and shut down multiple different environments, while maintaining complete control.
## Platform as a Service (PaaS)
Using PaaS the cloud provider is responsible for the hardware as well as maintaining operating systems, middleware, development tools, databases and business intelligence services.
The tenant hosts their application, code, data and business logic.

Characteristics:
- Provides a ready-to-use environment and platform with pre-deployed resources, development frameworks, languages and runtimes. This allows for fast deployment of applications, code business logic, data, ...
- Provides on-demand autoscaling
- No direct access or control over VMs and other applications, services or roles and which versions are running on the VMs

Microsoft PaaS services:
- Azure App Service
- Azure SQL Database
- Cosmo DB
- Azure Files
- Azure Active Directory Domain Services
### Scenarios
- **Development framework**:
  With PaaS you can build a framework on which you can develop or customize cloud-based applications with built-in software components. While featuring scalability, high-availability and multi-tenant capability
- **Analytics or business intelligence:**
  With tools that are provided with PaaS organizations can analyze and mine data to find insides and pattern, enabling them to improve buisiness decisions.
## Serverless/ Function as a Service (FaaS)
With FaaS the runtime layer is abstracted, which allows the tenant to run code and business logic and the cloud provider hosts it in their language, runtime and compute environment.

Characteristics:
- Event-based workloads are the best use case
- Long-running tasks are not well suited 
- Execution environment cannot be customized
- Cloud provider supports specific languages and runtimes
- The tenant only controls the application, code and business logic layers

Microsoft FaaS resources:
- Azure Functions
- Azure Logic Apps
## Software as a Service (SaaS)
With SaaS you rent a and use a fully developed application, whether it is a email/ messaging application or financial software.

Characteristics:
- Cloud provider is responsible for the application/solution and its updates, scalability, availability and security
- Applications can be directly configured and instantly used 

Microsoft SaaS solutions:
- Microsoft Teams
- Microsoft Exchange Online
- Microsoft SharePoint Online
- Microsoft OneDrive
- Microsoft Dynamics 365

Other SaaS solutions:
- Zoom
- Salesforce
- Dropbox
- Google Mail/ Docs
### Scenarios
- Email and messaging
- Business productivity applications
- Finance and expense tracking
## Analogy
![[Cloud Services Analogy.png|660x318]]

# Cloud Networking Building Blocks
## Logical network isolation
*Overlay networks* are created to isolate logical networks and their traffic from one another and the physical network. Encapsulation protocols like VXLAN, Generic Routing Encapsulation (GRE) and others are used for this. 

## Public and private addressing
[[IPv4#Public and Private Addresses|Public and private IP addresses]] 