
Cloud computing is the delivery of services and infrastructure, such as VMs, storage, databases and networking, as well as IoT, machine learning (ML) and AI using a consumption based pricing model.
Cloud computing allows the user to up- or downscale the services or infrastructure they need, f.e. if the needed computing power increases you increase your IT footprint instead of building a new datacenter.

>[!note]
>Cloud services fall under *operational expenditure (OpEx)*, meaning you pay a regular sum for the resources you use.
>The opposite is *capital expenditure (CapEx)*, where you pay a one-time, up-front expenditure.
# Shared Responsibility Model
With a traditional datacenter the company is responsible for maintaining the physical space, ensuring security und maintaining and replacing server and the IT department is responsible for the digital infrastructure and the software.
When using cloud services the responsibilities are split between the cloud provider and the consumer depending on what service is used:
- *Software as a Service (SaaS)*
- *Platform as a Service (PaaS)*
- *Infrastructure as a Service (IaaS)*

![[Azure shared responsibility.png]]The cloud provider is always responsible for the physical space and the hardware, while the consumer is always responsible for administrative task including the data stored and who and what device can access the cloud.
# Cloud models
## Private Cloud
A private cloud is used by a single entity or cooperation, which can be hosted from an onsite datacenter, a dedicated offsite datacenter or even by a thrid party cloud provider
## Public Cloud
Public clouds are built, controlled and maintained by third party cloud providers and anyone can purchase cloud services.

>[!note] Multi-cloud
>It is also possible to use multiple public cloud provider, which means you manage resources and security in these environments.
## Hybrid Cloud
Hybrid clouds use an interconnected environment of public and private clouds that allow private clouds to temporarily use public cloud resources if computing demands surge. It is also possible to flexibly choose which services should be deployed in a public or private cloud.

|**Public cloud**|**Private cloud**|**Hybrid cloud**|
|---|---|---|
|No capital expenditures to scale up|Organizations have complete control over resources and security|Provides the most flexibility|
|Applications can be quickly provisioned and deprovisioned|Data is not collocated with other organizations’ data|Organizations determine where to run their applications|
|Organizations pay only for what they use|Hardware must be purchased for startup and maintenance|Organizations control security, compliance, or legal requirements|
|Organizations don’t have complete control over resources and security|Organizations are responsible for hardware maintenance and updates||
## [[Azure]]
### Azure Arc
A set of technologies that help to manage cloud environments.
### Azure VMware Solution
Allows consumers who already have a private cloud with VMware to run VMware workloads in Azure. This can be done if the consumer wants to migrate to a public or hybrid cloud.

# Service Types
![[Azure shared responsibility.png]]
## Infrastructure as a Service (IaaS)
When using IaaS you rent the hardware in the datacenter of the cloud provider.
### Scenarios
- **Lift-and-shift migration**:
  You replicate the existing on-prem datacenter with cloud resources and then move the things running on-prem to the IaaS.
- **Testing and development**:
  You configure a test environment that you can rapidly replicate. With IaaS you can start up and shut down multiple different environments, while maintaining complete control.

## Platform as a Service (PaaS)
Using PaaS the cloud provider is responsible for the hardware as well as maintaining operating systems, middleware, development tools, databases and business intelligence services.
### Scenarios
- **Development framework**:
  With PaaS you can build a framework on which you can develop or customize cloud-based applications with built-in software components. While featuring scalability, high-availability and multi-tenant capability
- **Analytics or business intelligence:**
  With tools that are provided with PaaS organizations can analyze and mine data to find insides and pattern, enabling them to improve buisiness decisions.

## Software as a Service (SaaS)
With SaaS you rent a and use a fully developed application, whether it is a email/ messaging application or financial software.
### Scenarios
- Email and messaging
- Business productivity applications
- Finance and expense tracking

