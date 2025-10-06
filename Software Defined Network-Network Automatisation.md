# Openflow
The Openflow protocol served as the first major protocol of the *Software Defined Network (SDN)* movement, but 
It was developed by Martin Casado and allows the decoupling of a network device's control plane and data plane.

>[!note]
>- The *control plane* can be thought of as the brain of a network device
>- The *data plane* can be thought of as the hardware or application-specific integrated circuits (ASICs) that perform packet forwarding.

Meaning Openflow is used to directly interface with the hardware tables (f.e. forwarding information) that instruct a network devices how to forward traffic. This programming of the data plane allows to more granularly define the way traffic traverses the network. Be aware that this adds more complexity.

![[Programmable ASICs.png]]
>[!note]
>[P4](https://p4.org/) stands for *Programming Protocol-independent Packet Processor*, which specifies how the data plane devices (switches, NICs, routers, etc.) process packets.

Decoupling the control and data plane exposes two new interfaces:
- *Northbound API*: allows external applications to interact with the network controllers
- *Southbound API*: used by the network controller to define the packet forwarding in the data plane
# Network Functions Virtualization (NFV)
*Network functions virtualization (NFV)* refers to VMs that that operate as network devices (router, firewall, load balancer, intrusion detection system, etc.), which are traditionally deployed as hardware. 
## Virtual Switching
Virtual switches are software-based switches that reside in the hypervisor kernel and provide network connectivity between VMs/containers and the node, which connects to external networks, but also functions such as MAC learning, link aggregation and more. 
Virtual switches allow to rapidly create new network functions in software.
>[!note]
>Common virtual switches are: 
>- VMware vSwitch
>- Microsoft Hyper-V Virtual Switch
>- a Linux bridge
>- Open vSwitch (OVS)

## Network Virtualization
Network virtualization can be achieved by using an overlay-based protocol such as *Virtual Extensible LAN (VXLAN)* to build connectivity between hypervisor-based virtual switches.
This creates Layer 2 connectivity and tunneling between VMs that run on different physical hosts independent of the physical network. This means that the physical network could be Layer 2, Layer 3 or a combination.

>[!note] Underlay and Overlay Network
>- *Underlay Network*: the physical network that you physically cable up
>- *Overlay Network*: virtualized network that dynamically creates tunnels between virtual switches 

Network virtualization solutions use a centralized control plane to distribute the mapping information between the overlay and underlay networks. This centralized approach limits the scalability, interoperability and flexibility, which gives way to alternatives like *Ethernet VPN (EVPN)*. EVPN is a distributed protocol, in which every network device distributes the mapping information (via *Border Gateway Protocol (BGP)*) to the rest of the network to establish VXLAN tunnels.

# Bare Metal Switching
![[Bare Metal Switching.png]]

# Data Center Network Fabrics
*Data Center Network Fabrics*, is a change in the mindset of network operators from managing individual boxes one at a time to managing a system in its entirety. Meaning that when a network is deployed and managed as a system, it needs to be thought of as a system, including the upgrade process.
When upgrading fabrics can be swapped out, migrating from system to system, but not individual components. 
In addition to treating the network as a system, network fabrics also do:
- offer a single management interface
- offer distributed default gateways across the fabric
- offer multipathing capabilities
- Use SDN controller to manage the system

Example for network fabrics are:
- Cisco Application Centric Infrastructure (ACI)
- Arista Converged Cloud Fabric (CCF)
- Big Switch Big Cloud Fabric (BCF)
- Aruba Fabric Composer

# Software Defined Wide Area Network (SD-WAN)
With broadband and internet costs being a fraction of costs for private-line circuits, the use of site-to-site VPN tunnels has increased, laying the groundwork for SD-WAN.
Common designs for remote offices include private *Multiprotocol Label Switching (MLPS)* circuits for low-latency applications and a public internet connection as backup with a VPN connection to corporate. When traffic is divided between circuits the complexity of the routing protocol configuration is increased and the granularity of the route to the destination is decreased. 
The architecture of SD-WANs consists of an overlay protocol over the internet or a private WAN, often over two or more internet circuits at branch sites and fully encrypted using *Internet Protocol Security (IPSec)*.