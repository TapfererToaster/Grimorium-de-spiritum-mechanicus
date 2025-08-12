Clients have only one [[(OSI) Open Systems Interconnection-Modell#Default Gateway|Default Gateway]] configured, if it fails the client will not be able to reach other networks. The *First Hop Redundancy Protocol (FHRP)* provides redundancy and prevent this single point of failure by enabling multiple routers to be connected to the same VLAN.

# Options
- *Hot Standby Router Protocol*: Cisco-proprietary, IPv4
- *HSRP for IPv6*: Cisco-proprietary, IPv6
- *Virtual Router Redundancy Protocol version 2 (VRRPv2)*: non-proprietary, IPv4
- *VRRPv3*: non-proprietary, IPv4 + IPv6, more scaleable than VRRPv2
- *Gateway Load Balancing Protocol (GLBP)*: Cisco-proprietary, IPv4, allows for router redundancy and load balancing between multiple routers
- *GLBP for IPv6*: Cisco-proprietary, IPv6
- *ICMP Router Discovery Protocol (IRDP)*: legacy protocol, specified in RFC 1256, IPv4

# Virtual Router
Multiple routers are configured as one virtual router by sharing an IP address and a MAC address, which allows for router redundancy.
![[Virtual Router.png]]
When the active router fails the redundancy protocol changes the standby router to the new forwarding router without the disrupting the host devices connection.
![[Forwarding router fails.png]]
The following steps happen:
1. The standby router stops receiving Hello messages from the forwarding router
2. The standby router takes the role of forwarding router and assumes the IP and MAC address of the virtual router

# Hot Standby Router Protocol
HSRP is a Cisco-proprietary FHRP protocol.
## Priority and Preemption
>[!note]
>By default the router with the numerically highest IPv4 address is elected as the active router, however it is adviced to manually configure the routers.

### Priority
The router with the highest priority is the active (forwarding) router and if priorities are equal the router with the numerical highest IPv4 address is elected as active router.
The default priority is 100 and the priority range is from 0 to 255. To assign a priority ti a router interface use the `standby priority priority-number` command.
### Preemption
The active router will remain in this role even if another router with a higher priority comes online. Preemption is the ability to trigger a re-election process, when a router with a higher priority comes online, so it can assume the role of active router.
To enable preemption on a router interface use the `standby preempt` command.
## States and Timers
- *Initial*: The state is entered through a configuration change or when an interface first becomes available
- *Learn*: Router has not determined the virtual IP address and has not yet seen a hello message from the active router; the router waits for a message from the active router
- *Listen*: the router knows the virtual IP address, but is not the active or standby router; it listens for hello messages from these routers
- *Speak*: The router sends hello messages and actively participates in the election of the active and/or standby router
- *Standby*: The router is a candidate to become the next active router and sends periodic hello messages
- *Active*; the router is the active router

>[!note]
>The active and standby HSRP routers send hello packets to the HSRP group multicast address every 3 seconds by default. The standby router will become active if it does not receive a hello message from the active router after 10 seconds. You can lower these timer settings to speed up the failover or preemption. However, to avoid increased CPU usage and unnecessary standby state changes, do not set the hello timer below 1 second or the hold timer below 4 seconds.

