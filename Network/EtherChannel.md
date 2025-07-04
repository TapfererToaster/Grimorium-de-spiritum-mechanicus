#CCNA 

![[EtherChannel.png]]

*EtherChannel* was originally developed by Cisco as a LAN switch-to-switch technique of grouping several Fast Ethernet or Gigabit Ethernet ports into one logical channel. When an EtherChannel is configured, the resulting virtual interface is called a *port channel*. The physical interfaces are bundled together into a port channel interface.
EtherChannel is a link aggregation technology that groups multiple physical Ethernet links together into one single logical link. It is used to provide fault-tolerance, load sharing, increased bandwidth, and redundancy between switches, routers, and servers.

**Advantages**
- Most configuration tasks can be done on the EtherChannel interface instead of on each individual port, ensuring configuration consistency throughout the links.
- EtherChannel relies on existing switch ports. There is no need to upgrade the link to a faster and more expensive connection to have more bandwidth.
- Load balancing takes place between links that are part of the same EtherChannel. Depending on the hardware platform, one or more load-balancing methods can be implemented. These methods include source MAC and destination MAC load balancing, or source IP and destination IP load balancing, across the physical links.
- EtherChannel creates an aggregation that is seen as one logical link. When several EtherChannel bundles exist between two switches, STP may block one of the bundles to prevent switching loops. When STP blocks one of the redundant links, it blocks the entire EtherChannel. This blocks all the ports belonging to that EtherChannel link. Where there is only one EtherChannel link, all physical links in the EtherChannel are active because STP sees only one (logical) link.
- EtherChannel provides redundancy because the overall link is seen as one logical connection. Additionally, the loss of one physical link within the channel does not create a change in the topology. Therefore, a spanning tree recalculation is not required. Assuming at least one physical link is present; the EtherChannel remains functional, even if its overall throughput decreases because of a lost link within the EtherChannel.

**Implementation Restrictions**
- Interface types cannot be mixed (Fast Ethernet and Gigabit Ethernet cannot be mixed)
- each EtherChannel can consist of up to eight compatibly-configured Ethernet ports; EtherChannel provides full-duplex bandwidth up to 800 Mbps (Fast EthernetChannel) or 8 Gbps (Gigabit EtherChannel)
- the individual EtherChannel group member port configuration must be consistent on both devices. If the physical ports of one side are configured as trunks, the physical ports of the other side must also be configured as trunks within the same native VLAN. Additionally, all ports in each EtherChannel link must be configured as Layer 2 ports.
- Each EtherChannel has a logical port channel interface and a configuration applied to the port channel interface affects all physical interfaces that are assigned to that interface.

# AutoNegotiation Protocols
EtherChannels can be formed through negotiation using one of two protocols:
- Port Aggregation Protocol (PAgP)
- Link Aggregation Control Protocol (LACP)

>[!note]
>It is possible to configure a static or unconditional EtherChannel without PAgPk or LACP

## Port Aggregation Protocol (PAgP)
- Cisco proprietary protocol 
- PAgP packets are sent between EtherChannel-capable ports to negotiate the forming of a channel
- groups Ethernet links into an EtherChannel and adds it to the spanning tree as a single port
- manages EtherChannel:
	- PAGP packets are sent every 30 seconds
	- Checks for configuration consistency and manages link additions and failures between two switches
	- ensures EtherChannel is created, all ports have the same type of configuration
- helps to create the EtherChannel link by detecting the configuration of each side and ensuring that links are compatible so that the EtherChannel link can be enabled

### Modes
 - **On**:
   Forces the interface to channel without PAgP. Interfaces configured in the on mode do not exchange PAgP packets
 - **PAgP desirable**:
   places an interface in an active negotiating state in which the interface initiates negotiations with other interfaces by sending PAgP packets
 - **PAgP auto**:
   places an interface in a passive negotiating state in which the interface responds to the PAgP packets that it receives but does not initiate PAgP negotiation

>[!note]
>The modes must be compatible on each side.
>If one side is configured to auto mode, it waits for the other side to initiate the EtherChannel negotiation. If the other side is also set to auto, the negotiation never starts.
>![[Pasted image 20250704085611.png]]

## Link Aggregation Control Protocol (LACP)
- part of IEEE 802.3ad allowing several physical ports to be bundled to form a single logical channel
- allows a switch to negotiate an automatic bundle by sending LACP packets to the other switch
- allows for eight active links and eight standby links; a standby link will become active should one of the active links fail
### Modes
- **On**:
  forces the interface to channel without LACP. Interfaces configured in the on mode do not exchange LACP packets
- **LACP active**:
  places a port in an active negotiating state. In this state, the port initiates negotiations with other ports by sending LACP packets
- **LACP passive**:
  places a port in a passive negotiating state. In this state, the port responds to the LACP packets that it receives but does not initiate LACP packet negotiating

>[!note]
>The modes must be compatible for a EtherChannel to form.
>![[Pasted image 20250704094021.png]]

# LACP Configuration 
>[!warning] Guidlines and Restrictions
>- **EtherChannel support**
>  all Ethernet interfaces must support EtherChannel with no requirement that interfaces be physically contiguos
>- **Speed and duplex**:
>  Configure all interfaces in an EtherChannel to operate at the same speed and in the same duplex mode
>- **VLAN match**:
>  All interfaces in the EtherChannel bundle must be assigned to the same VLAN or be configured as a trunk
>- **Range of VLANs**:
>  EtherChannel supports the same allowed range of VLANs on all the interfaces in a trunking EtherChannel. If the allowed range of VLANs is not the same, the interfaces do not form an EtherChannel, even when they are set to `auto` or `desirable` mode.

#Cisco_CLI 
1. Specify the interface that compose the EtherChannel group using the `interface range interface` command
2. Create the port channel interface with the `channel-group identifier mode active` command
3. Use the `interface port-channel` command
```
S1(config)# interface range FastEthernet 0/1 - 2
S1(config-if-range)# channel-group 1 mode active
Creating a port-channel interface Port-channel 1
S1(config-if-range)# exit
S1(config)# interface port-channel 1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan 1,2,20
```

## Verify
```
S1# show interfaces port-channel 1
S1# show etherchannel summary
S1# show etherchannel port-channel
S1# show interfaces f0/1 etherchannel
```

## Common Issues with EtherChannel Configurations
- Assigned ports in the EtherChannel are not part of the same VLAN, or not configured as trunks. Ports with different native VLANs cannot form an EtherChannel.
- Trunking was configured on some of the ports that make up the EtherChannel, but not all of them. It is not recommended that you configure trunking mode on individual ports that make up the EtherChannel. When configuring a trunk on an EtherChannel, verify the trunking mode on the EtherChannel.
- If the allowed range of VLANs is not the same, the ports do not form an EtherChannel even when PAgP is set to the `auto` or desirable` mode.
- The dynamic negotiation options for PAgP and LACP are not compatibly configured on both ends of the EtherChannel.

## Troubleshoot EtherChannel Example
1. View the EtherChannel Summary Information
```
S1# show etherchannel summary
```
2. View Port Channel Configuration
```
S1# show run | begin interface port-channel
```
3. Correct the Misconfiguration
4. Verify EtherChannel is Operational