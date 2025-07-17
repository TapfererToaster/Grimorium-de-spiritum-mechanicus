# The Basics of Routing
![[Routing Basic.png]]
When a router receives a frame that is addressed to its MAC address it de-encapsulates it and inspects it IP address. If the  destination IP address in the packet is its own the router will continue to de-encapsulate the message as it is a message for the router itself.

If the destination IP address is not its own the router will try to route the packet to forward it to its destination. To do that it looks up the destination IP address in a routing table to find a suitable route. If one is found the packet will be forwarded, if not the router will discard the packet 
## Routing Table
The routing table is a database of known destinations. 
It can be thought of as a set of instructions: 
- To send a packet to destination X, forward it to next hop Y
- If the destination is in a directly connected network, forward the packet directly to the destination
- If the destination is the router's own IP address, do not forward the packet (continue to de-encapsulate)

>[!note]
>You can view the routing table on a Cisco router with the command `show ip route`

>[!note]
>Without any configuration the routing table will be empty.

### Connected Routes
A *connected route* is a route to the network an interface is connected to. For each interface that has an IP address and is in an up/up state one connected route is automatically added to the routing table. 
A connected route will state that the network is directly connected and which interface it is connected to.

>[!note]
>A route to more than one destination address is called a *network route*. 
>Connected Routes are network routes.

### Local Routes 
A *local route* is a route to the IP address configured on the router's interface, which tells the router that a packet destined for the configured IP address is for the router itself.
To specify the IP address of the interface, a local route uses a `/32` prefix length ( all bits are set to `s`), regardless of the netmask configured on the interface.

>[!note]
>One local route is also automatically added to the routing table for each interface that has an IP address and is in an `up/up` state.

>[!note] 
>A route to a single destination IP address (a single host) is called a *host route*.

### Cisco CLI example
We configure the the interface g0/0 on a router
```
R1# configure terminal
R1(config)# interface g0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
```

Now we check the routing table with `show ip route`
```
R1# show ip route
...
192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks 
C     192.168.1.0/24 is directly connected, GigabitEthernet0/0 
L     192.168.1.1/32 is directly connected, GigabitEthernet0/0
```

We can see that a connected and a local route have been automatically added.

## Route selection
Router select the *most specific matching route* , when determining which route in the routing table it will use to forward a packet.
>[!info]
>- *matching route*: The packets destination IP address is part of the network specified in the route
>- *most specific*: the route with the longest prefix length

![[Route Selection.png]]
- R1 receives a frame on its G0/0 INTERFACE
- The destination MAC is its own, it de-encapsulates it 
- The packets destination IP address is 192.168.1.1
- R1 performs a routing table lookup and finds two matching routes
  192.168.1.0/24
  192.168.1.1/32
- R1 chooses the most specific route 192.168.1.1/32

>[!note] 
>Routers will not flood a packet out of all ports when there is no matching route in the routing table, instead it drops the packet.
>Here are the actions a router can make. 
>![[Router Actions.png]]

# Static Routing
*Routing* is also used to refer to the processes routers use to learn routes.
There are two main methods:
- *Dynamic routing*: 
  Routers use *dynamic routing protocols* to share information with each other and build their routing tables
- *Static routing*:
  An engineer/admin manually configures routes on the router

>[!remember]
>When forwarding a packet towards its destination the router must encapsulate and address it to the MAC of the *next hop*, which is the next router in the path to the destination.

![[Figure 9.7 Routing.png]]

To establish communication between two hosts, each router only needs a route to the next hop, meaning R1 does not need to know about the network between R2 and R3.  
In other words R1 does not need to know what path the packet will take after R2.

## Configuring static routes
#Cisco_CLI 
### Static routes specifying the next-hop
To configure a static route to the next hop you use
```
ip route destination-network netmask next-hop-IP 
```

![[Static Route next hop configuration.png]]
>[!note]
>A static route that specifies only the next-hop IP address is called a *recursive static route*
>It is recursive, because the route necessitates multiple lookups in the routing table to forward a packet:
>- lookup to ding the UP address of the next hop
>- lookup to fing which interface the next-hop is connected to

>[!note]
>R1 knows the IP address of the next-hop, but R1 needs the next-hop's MAC address. To learn it R1 must send an ARP request to the next-hop
>
### Static route specifying the exit Interface
Instead of configuring the next-hop IP address of a route, you can specify the *exit interface*, which will be used to forward packets out of.
To configure this use 
```
ip route destination-network netmask exit-interface

R1(config)# ip route 192.168.3.0 255.255.255.0 g0/0 
```

A static route that specifies only the exit interface is called a *directly connected static route*, the reason being that the R1 will think that the destination network is directly connected to the interface. The router will then try to send packets directly to PC3 and an ARP request will be addressed to it instead of the next-hop. If the request does not reach PC3 R1 will not be able to forward packets to it.
If R2 uses *proxy ARP* it can reply on behalf of PC3 and responds to PC1's ARP request with the MAC of the connected interface. 

>[!note]
>A router will only use proxy ARP to reply to an ARP request if it has a route to the destination in its own routing table, otherwise it will ignore the request.

![[proxy ARP.png]]
>[!warning]
>The reliance of proxy ARP has some downsides:
>- on Cisco routers proxy ARP is enabled by default, but if it is disabled or the other router is not a Cisco router, the request will fail and no packets will be forwarded
>- The router will try to make a separate ARP entry for every host on the destination network, which can waste memory

### Static Routes specifying the exit interface and the next hop
You can also configure a *fully specified static route*, which contains the exit interface and the next-hop.
To configure this use:
```
ip route destination-network netmask exit-interface next-hop-ip

R1(config)# ip route 192.168.3.0 255.255.255.0 g0/0 192.168.12.2
```

This kind of static route has the benefit that the router will not need recursive lookups or to rely on proxy ARP. In reality there is no noticeable difference in performance between recursive and full specified static routes.

## Configuring a default route
The *default route* is a route to the least-specific destination possible: 0.0.0.0/0, which matches all possible IP addresses, from 0.0.0.0 through 255.255.255.255 and is only used when there aren't any more specific routes. 
A default route is often used to forward packets to the internet and to the Internet Service Provider (ISP).

>[!note]
>More specific routes can be used for destinations in the internal corporate network, and then all other traffic that does not match any other routes will be routed using the defualt route.

To configure a default route use
```
ip route 0.0.0.0 0.0.0.0 next-hop-ip
```

![[Default Route.png]]