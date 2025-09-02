#CCNA 
![[Logical Physical Topology.png]]
The *Spanning Tree Protocol (STP)* is a loop-preventing protocol that allows for redundancy in a Layer 2 topology. 
>[!note]
>Unlike Layer 3 protocols (IPv4, IPv6), Layer 2 Ethernet has no mechanism to recognize and eliminate endlessly looping frames.
>Looping frames can also cause *MAC address flapping*, which is when a switch learns the same MAC address on multiple ports.

- IEEE 802.D is the original IEEE MAC Bridging standard for STP.
- When a redundant link fails STP recalculates the best link
- Path redundancy provides multiple networks services by eliminating a single point of failure 
- *Broadcast Storms* can be caused by faulty NIC or a Layer 2 loop

# Spanning Tree Algorithm (STA)
![[Scenario Topology.png]]

1. **Select the Root Bridge**
   The STA begins by selecting a root bridge and each switch determines a single least cost path from itself to the root bridge
   ![[STA Select Root Bridge.png]]
2. **Block Redundant Paths**
   STP ensures that there is only one logical path between all destinations on the network by blocking redundant paths that could cause a loop.
   ![[STA Block Redundant Paths.png]]
3. **Loop-Free Topology**
   The topology is now loop free but has also redundant paths in case of a failure
   ![[STA Loop free Topology.png]]
4. **Link Failure Causes Recalculation**
   If a blocked path is needed to compensate for a switch or cable failure, STP recalculates the paths and unblocks the necessary ports
   ![[STA Recalculation.png]]

# STP Algorithm
Switches use *Bridge Protocol Data Units (BPDUs)* to share information about themselves and their connections, the *bridge identifier (BID)*, which identifies the switch in the LAN and the BID of the root bridge are important for the root bridge election.
## The BID
![[Bridge Identifier (BID).png]]

- **Bridge Priority**
  The default value for all Cisco switches is the decimal value 32768. And the range is 0 to 61440 in increments of 4096. A lower bridge priority is preferable with 0 taking precedence over all other bridge priorities.
- **Extended System ID**
  A decimal value added to the bridge priority to identify the VLAN for his BPDU. The extended system ID allows later implementations of STP to have different root bridges for different sets of VLANs. 

>[!note]
>The bridge priority and the extended system ID are added together to create the bridge priority.

- **MAC address**
  MAC address that identifies the switch not its ports.
  When two switches are configured with the same priority and have the same extended system ID, the switch having the lowest MAC address expressed in hexadecimal will have the lower BID. 
## Elect the Root Bridge
![[Elect the root bridge.png]]
- Switches exchange BPDUs to build a loop-free topology 
- After a switch boots it begins to send out BPDU frames, containing the BID of the sending switch and the BID of the root bridge (Root ID), every two seconds
- At first all switches declare themselves as the root bridge with their own BID set as the Root ID, through BPDU exchange the switches learn which one has the lowest BID and agree on one root bridge

>[!note] Impact of default BIDs
>Because the default priority is 32768, two or more switches can have the same priority. If that happens the switch with the lowest MAC address will become the root bridge.
>
>It is recommended that the administrator should configure the desired root bridge switch with a lower priority to ensure the root bridge decision best meets network requirements

## Determine the Root Path Cost
When the root bridge has been elected for a given spanning tree instance, the STA starts the process of determining the best paths to the root bridge from all destinations in the broadcast domain. The path information, known as the internal root path cost, is determined by the sum of all the individual port costs along the path from the switch to the root bridge.

The default port costs are defined by the speed at which the port operates.
![[Root Path Cost.png]]
## Elect the Root Ports
After the root bridge has been determined, the STA algorithm is used to select the root port, with information found in the BPDUs.
The root port is selected using the following parameters:
1. **Lowest root path cost**
   Determines the most efficient path to the root bridge. When a switch forwards a BPDU it adds the cost of the port it received the BPDU.
   ![[Lowest root path cost.png]]

2. **Lowest neighbor BID**
   If multiple ports on a switch have the same root path cost, the port connected to the neighbor with the lowest BID is selected as the root port.
   ![[Lowest neighbor BID.png]]
3. **Lowest neighbor port ID**
   If multiple ports have the same root path cost and are connected to the same neighbor, the port connected to the neighboring port with the lowest port ID will be the root port.  
## Elect Designated Ports
A *Designated Port* is a port on which traffic from the root bridge is received. The port is determined by the lowest internal routing cost to the rooting bridge (root port).
![[Designated Ports.png]]
>[!note] Designed Ports on Root Bridge
>All ports on the root bridge are designated ports.

## Elect Alternate (Blocked) Ports
If a port is not a root or a designated port, it becomes an alternate or backup port. These ports are in discarding or blocking state to prevent loops.
All other inter-switch ports are in forwarding state. 
![[Elect alternate ports.png]]
# STP Timers and Port States
STP convergence requires three timers:
- **Hello Timer**: interval between BPDUs
  The default is 2 seconds but can modified to between 1 and 10 seconds.
- **Forward Delay Timer**: time that is spend in the listening and learning state
  The default is 15 seconds but can be modified to between 4 and 30 seconds.
- **Max Age Timer**: maximum time a switch waits before attempting to change the STP topology
  The default is 20 seconds but can be modified to between 6 and 40 seconds.

## Port States
- **Blocking**
  - Port is an alternate port an does not participate in frame forwarding
  - port receives BPDU frames to determine the location and root ID of the root bridge and which role each port should assume in the final active STP topology.
  - When expected BPDU were not received from neighbor switch during the Max Age Timer it will go into the blocking state 
- **Listening**
	- After the blocking state, a port will move to the listening state
	- receives BPDUs to determine the path to the root
	- the switch port transmits its own BPDU frames and informs adjacent switches that the switch port is preparing to participate in the active topology
- **Learning**
	- After the Listening state, a port will move to the learning state
	- receives and processes BPDUs and prepares to participate in frame forwarding
	- begins to populate the MAX address table
	- user frames are not forwarded to the destination in this state
- **Forwarding**
	- In this state, a switch port is considered part of the active topology
	- forwards user traffic and sends and receives BPDU frames
- **Disabled**
	- A port in this state does not participate in spanning tree and does not forward frames
	- The disabled state is set when the port is administratively disabled

![[Operation Port states.png]]

# Different Versions of STP

![[STP variations.png]]
## Rapid Spanning Tree Protocol
The *Rapid Spanning Tree Protocol (RSTP)* defined in IEEE 802.1w supersedes the original, while retaining backward compatibility, as most parameters have been left unchanged.
RSTP increases the speed of recalculation of the spanning tree when the layer 2 network topology changes.
### RSTP Port States
![[RSTP Port States.png]]

### RSTP Port Roles
![[RSTP Port Roles.png]]

# PortFast and BPDU Guard
When a device is connected to a switch or a switch is powered up it goes through the listening and learning states, each time waiting for the Forward Delay timer to expire.
This delay can prevent DHCP clients from reaching a DHCP server as the DHCP messages will not be forwarded.
When a switch port is configured with *PortFast*, it will change from blocking to forwarding state immediately. 
>[!warning]
>PortFast is only for use on switch ports that connect to end devices.

