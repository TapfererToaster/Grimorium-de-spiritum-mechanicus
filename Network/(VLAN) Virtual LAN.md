#CCNA 
*VLANs* provide segmentation and organizational flexibility in a switched network and are based on logical connections instead of physical connections.
They allow admins to separate the network based on factors such as team or application.
Benefits of VLANs are:
- **Smaller broadcast domains**
	- Dividing a network into VLANs reduces the number of devices in the broadcast domain
- **Improved security**
	- Only users in the same VLAN can communicate together
	- Only users in the same VLAN can communicate without the services of a router. The router may have a security feature such as an access control list to restrict communications between VLANs
- **Improved IT efficiency**
	- simpler network management as users with similar network requirements can be configured on the same VLAN
	- VLANs can be named to make them easier to identify
- **Reduced cost**
	- reduce the need for expensive network upgrades and use the existing bandwidth and uplinks more efficiently
- **Better performance**
	- smaller broadcast domains reduce unnecessary traffic on the network and improve performance
- **Simpler project and application management**
	- VLANs aggregate users and network devices to support business or geographic requirements

# Types of VLAN
## Default VLAN
The default VLAN on a Cisco switch is VLAN 1 and unless explicitly configured all switch ports are on VLAN1.
Important:
- All ports are assigned to VLAN 1 by default
- The native VLAN is VLAN 1 
- The management VLAN is VLAN 1
- VLAN 1 cannot be renamed or deleted

## Data VLAN 
Data VLANs are configured to separate user-generated traffic into groups of users or devices.
Voice and management traffic should not be permitted on data VLANs.
## Native VLAN
>[!note] Remember
>Access Ports (untagged ports) send and receive frames without 802.1Q tags
>Trunk Ports (tagged ports) send and receive frames with 802.1Q tags to indicate which VLAN each frame belongs to.

Untagged traffic that is received on a trunk port (tagged port) is assigned to the *native VLAN* and is forwarded without a tag by that trunk port. 

This was developed to accommodate for devices that do not support 802.1Q tagging such as hubs, but since hubs have become obsolete there is no need for the Native VLAN anymore and it can make a network vulnerable to security exploits.

>[!note] Disabling the Native VLAN
>The Native VLAN can not be disabled, so an unused VLAN (that is not VLAN 1) should be assigned as the Native VLAN on trunk ports.
### Native VLAN Mismatch
When the ports on each end of a link have different native VLANs configured, a mismatch occurs and frames might not reach their destination.
![[Native VLAN Mismatch.png]]
In this Example the frame sent from PC1 to PC10 will be forwarded untagged as VLAN10 is configured as the native VLAN on `SW1 G0/0` port. When port `SW2 G0/0` receives the untagged frame it assigns it to its native VLAN, VLAN 30. This results that SW2 cannot forward the frame to PC2 as it is in VLAN10.
## Management VLAN
This is a data VLAN configured specifically for network management traffic including SSH, Telnet, HTTPS, HTTP and SNMP.
By default VLAN 1 is configured as the management VLAN on a Layer 2 switch.
## Voice VLAN
To support Voice over IP (VoIP) a separate VLAN with the following characteristics is required:
- Assured bandwidth to ensure voice quality
- Transmission priority over other types of network traffic
- Ability to be routed around congested areas on the network
- Delay of less than 150 ms across the network

To meet these requirements the entire network has to be designed to support VoIP.

# VLAN in a Multi-Switched Environment
## Defining VLAN Trunks
*VLAN trunks* allow VLAN traffic to propagate between switches, which enables devices connected to different switches but in the same VLAN to communicate without a router.

>[!definition]
>A trunk is a point-to-point link between two network devices that carries more than one VLAN.

# VLAN Tagging
The standard [[Ethernet#Frames|Ethernet frame header]] does not contain information about the VLAN to which the frame belongs. To add these information the IEEE 802.1Q header is used in a process called *tagging*. This header includes a 4-byte tag inserted into the original Ethernet frame between the Source and EtherType field, specifying the VLAN.
## VLAN tag fields
![[VLAN tag fields.png]]

The fields of the VLAN tag are the following
- *Tag Protocol ID (TPID)*: 2 byte long and contains the value 0x8100, indicates that the frame is 802.1Q tagged
- *Tag Control Information (TCI)*
	- Priority Code Point: 3 bits in length and is used for Quality of Service by markinng frames as higher or lower priority
	- Drop Eligible Indicator: 1 bit long and indicates frames that can be dropped if the network is congested
	  >[!note]
	  > Previously known as **Canonical Format Identifier (CFI)** which was used to enable Token Ring frames to be carried across Ethernet links

	- VLAN Identifier (VID): 12 bit long and indicates which VLAN the frame is in, suppurts up to 4096 VLAN IDs

>[!note]
>After the header is inserted the FCS is recalculated.

## Native VLANs and 802.1Q Tagging
### Tagged Frames on the Native VLAN
- Some devices add a VLAN tag to native VLAN traffic
- Control traffic sent on the native VLAN should not be tagged
- When a 802.1Q trunk port receives a tagged frame with the VLAN ID that is the same as the native VLAN, it drops the frame.
- When configuring a switch port on a Cisco switch configure the devices so they do not send tagged frames on the native VLAN
### Untagged Frames on the Native VLAN
- When a Cisco switch trunk port receives untagged frames, it forwards those to the native  VLAN
- If there are no devices associated with the native VLAN and there are not other trunk ports the frame will be dropped.
- The default native VLAN is VLAN 1
- When configuring an 802.1Q trunk port, a default Port VLAN ID (PVID) is assigned the value of the native VLAN ID
- All untagged traffic coming in or out of the 802.1Q port is forwarded based on the PVID value.
## Voice VLAN Tagging
# VLAN Configuration
#Cisco_CLI 
## VLAN Ranges on Catalyst Switches
### Normal Range VLANs
The following are characteristics of normal range VLANs:
- They are used in all small- and medium sized business and enterprise networks 
- They are identified by a VLAN ID between 1 and 1005
- IDs 1002 through 1005 are automatically created and cannot be removed
- Configurations are stored in the switch flash memory in a VLAN database file called `vlan.dat`
- When configured, *VLAN trunking protocol (VTP)*; helps synchronize the VLAN database between switches

### Extended Range VLANs
The following are characteristics of extended range VLANs:
- They are used by service providers to service multiple customers and by global enterprises large enough to need extended range VLAN IDs
- They are identified by a VLAN id between 1006 and 4094
- Configurations are saved, by default in the running configuration
- They support fewer VLAN features than normal range VLANs
- Requires VTP transparent mode configuration to support extended range VLANs

## VLAN creation
```
Switch# configure terminal

# Create a VLAN with valid ID
Switch(config)# vlan vlan-id

# Specify a unique name 
Switch(config-vlan)# name vlan-name

Switch(config-vlan)# end
```

**Example**
```
S1# configure terminal
S1(config)# vlan 20
S1(config-vlan)# name student
S1(config-vlan)# end
```

## Access Port Assignment
```
Switch# configure terminal
Switch(config)# interface interface-id

# Set the port to access mode 
Switch(config-if)# switchport mode access

# Assign the port to a VLAN
Switch(config-if)# switchport access vlan vlan-id

Switch(config-if)# end
```

**Example**
```
S1# configure terminal
S1(config)# interface fa0/6
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 20
S1(config-if)# end
```
## Data and Voice VLAN

Use the `switchport voice vlan vlan-id` command to assign a voice VLAN to a port.
Use the `mls qos trust [cos | device cisco-phone | dscp | ip-precedence]` interface configuration command to set the trusted state of an interface, and to indicate which fields of the packet are used to classify traffic.

```
S3(config)# vlan 20
S3(config-vlan)# name student
S3(config-vlan)# vlan 150
S3(config-vlan)# name VOICE
S3(config-vlan)# exit
S3(config)# interface fa0/18
S3(config-if)# switchport mode access
S3(config-if)# switchport access vlan 20
S3(config-if)# mls qos trust cos
S3(config-if)# switchport voice vlan 150
S3(config-if)# end
S3#
```
## Verify VLAN Information

Use `show vlan [brief | id vlan-id | name vlan-name | summary]` to view information about the VLAN
```
# Display VLAN name, status and its ports one VLAN per line
brief

# Display information about hte identified VLAN ID number
id vlan-id

# Display information about the identified VLAN name
name vlan-name

# Display VLAN summary information
summary
```

## Change VLAN Port Membership
To change the VLAN port membership re-enter the `switchport access vlan vlan-id` command with the correct or new VLAN ID.
## Delete VLANs
To delete a VLAN use the `no vlan vlan-id` command.
>[!warning]
>Before deleting a VLAN, reassign all member ports to a different VLAN first. Any ports that are not moved to an active VLAN are unable to communicate with other hosts after the VLAN is deleted and until they are assigned to an active VLAN.

To delete the entire `vlan.dat` file use `delete flash:vlan.dat`. This will delete all VLANs and their configuration.

>[!tip]
>To restore a Catalyst switch to its factory default condition, unplug all cables except the console and power cable from the switch. Then enter the `erase startup-config` privileged EXEC mode command followed by the `delete vlan.dat` command.

# Trunk Configuration
#Cisco_CLI 
>[!important]
>Remember a VLAN trunk is a Layer 2 link between two switches that carries traffic for all VLANs (unless the allowed VLAN list is restricted manually or dynamically).

```
Switch# configure terminal
Switch(config)# interface interface-id

# Set the port to permanent trunking mode
Switch(config-if)# switchport mode trunk

# Set the native VLAN to something other than VLAN 1
Switch(config-if)# switchport trunk native vlan vlan-id

# Specify the list of VLANs to be allowed on the trunk link
Switch(config-if)# switchport trunk allowed vlan vlan-list

Switch(config-if)# end
```

**Example**
```
S1(config)# interface fastEthernet 0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 99
S1(config-if)# switchport trunk allowed vlan 10,20,30,99
S1(config-if)# end
```

>[!note] Modifying the list of allowed VLANs
>There are some options to the `switchport trunk allowed vlan vlan-id` command:
>- `add`: add VLANs to the current list
>  `SW1(config-if)# switchport trunk allowed vlan add 1` 
>- `all`: all VLANs are allowed
>- `except`: all VLANs except the following
>  `SW1(config-if)# switchport trunk allowed vlan except 1-9,11-19,21-29,31-4094`
>- `none`: no VLANs
>- `remove`: remove VLANs from the current list

## Verify Trunk Configuration
To verify the Trunk configuration use `show interface interface-id switchport` or `show interfaces trunk`

## Reset the Trunk to the Default State
To remove the allowed VLANs and reset the native VLAN of the trunk use the commands `no switchport trunk allowed vlan` and `no switchport trunk native vlan`
```
S1(config)# interface fa0/1
S1(config-if)# no switchport trunk allowed vlan
S1(config-if)# no switchport trunk native vlan
S1(config-if)# end
```

# Dynamic Trunking Protocol (DTP)
The *Dynamic Trunking Protocol (DTP)* is a Cisco proprietary protocol that automatically negotiates trunking with a neighboring device.
To enable trunking from a Cisco switch to a device that does not support DTP, use the `switchport mode trunk` and `switchport nonegotiate` command, this causes the interface to become a trunk, but it will not generate DTP frames.
```
S1(config-if)# switchport mode trunk
S1(config-if)# switchport nonegotiate
```

To re-enable dynamic trunking protocol use the `switchport mode dynamic auto` command.
```
S1(config-if)# switchport mode dynamic auto
```

If the ports connecting two switches are configured to ignore all DTP advertisements with the `switchport mode trunk` and the `switchport nonegotiate` commands, the ports will stay in trunk port mode. If the connecting ports are set to dynamic auto, they will not negotiate a trunk and will stay in the access mode state, creating an inactive trunk link.
>[!tip]
>When configuring a port to be in trunk mode, use the `switchport mode trunk` command. Then there is no ambiguity about which state the trunk is in; it is always on.


## Negotiated Interface Modes
The `switchport mode` command has additional options for negotiating the interface mode.
```
Switch(config-if)# switchport mode {  access | dynamic {  auto | desirable } |  trunk }
```

The options are:
- **access**:
	- Puts the interface (access port) into permanent nontrunking mode and negotiates to convert the link into a nontrunk link
	- The interface becomes a nontunk interface, regardless of whether the neighboring interface is a trunk interface
- **dynamic auto**:
	- Makes the interface able to convert the link to a trunk link.
	- The interface becomes a trunk interface if the neighboring interface is set to trunk or desirable mode.
	- The default switchport mode for all Ethernet interfaces is `dynamic auto`
- **dynamic desirable**:
	- Makes the interface actively attempt to convert the link to a trunk link.
	- The interface becomes a trunk interface if he neighboring interface is set to trunk, desirable or dynamic auto mode
- **trunk**:
	- Puts the interface into permanent trunking mode and negotiates to convert the neighboring link into a trunk link.
	- The interface becomes a trunk interface even if the neighboring interface is not a trunk interface.

>[!tip]
>Use the `switchport nonegotiate` command to stop DTP negotiation. 
>This command can only be used when the interface switchport mode is `access` or `trunk`. 
>You must manually configure the neighboring interface as a trunk interface to establish a trunk 
>link.

![[Results of DTP Configuration.png]]
## Verify DTP Mode
To verify the DTP mode use `show dtp interface interface-id`.

>[!note]
>A general best practice is to set the interface to `trunk` and `nonegotiate` when a trunk link is required. On links where trunking is not intended, DTP should be turned off.

## DTP as a Vulnerability
DTP is a security vulnerability as an attacker can exploit it to form a trunk link to a switch and gaining access to the VLANs in a LAN. 
Although end host do not use DTP messages a malicious user can use tools like [Yersinia](https://www.kali.org/tools/yersinia/) to send DTP messages.
![[DTP Yersinia Hack.png]]

To prevent this you can either:
- Manually configure the port an an access port with `switchport mode access`
- Explicitly disable DTP with `switchport nonnegotiate`

>[!note]
>It is a security best practice to use `switchport nonegotiate` to disable DTP on switch ports, even if you configure the port in trunk mode.

# VLAN Trunking Protocol (VTP)
The *VLAN Trunking Protocol (VTP)* is another Cisco-proprietary protocol enables switches to synchronize their VLAN databases, the file in that stores information about existing VLANs. This is important as switches will drop packets from VLANs they do not know.
![[VLAN Trunking Protocol.png]]

>[!note] 
>The VLAN database is stored is stored in the `vlan.dat` file, which can be viewed with the `dir flash:` or `show flash:` commands.

## VTP Revision Number and Domain
The *VTP revision number* is used to track the latest version of a VLAN database, it starts with 0 and is incremented when the database is changed (a VLAN is created, deleted, renamed). This also causes the switch to inform other switches in the *VTP domain* of the changes so they can update their databases.

>[!warning]
>If you add a switch to a VTP domain make sure that its VTP revision number is 0.
>If it is not 0 you have to reset it, which can be done by:
>- Change the VTP domain name and then back to the original name.
>- Change the VTP mode to transparent and back to server or client (works only in Version 1 and 2)
>- Change the VTP mode to off and then back to server or client (works only in Version 1 and 2)
>  
>  If you do not do this the newly added switch might have a higher revision number if it was already used in another VTP domain, which leads the other switches to synchronize their databases to the new one. This can cause Network problems and outages.

A *VTP domain* is a group of switches in a LAN that share the same *VTP domain name*. 
By default, switches do not have a VTP domain name so you have to configure one with the command `vtp donmain domain-name`:
```
SW1(config)# vtp domain Domain1
```
After that SW1 will send VTP messages to other switches in the LAN and all switches without a VTP domain name will adopt the domain name from SW1.
## VTP Modes
A switch can operate in one of four different modes, which can be configured with the `vtp mode mode` command.
- **Server**:
    - default mode 
  - switch can create, modify and delete VLANs
  - will advertise VLAN database changes to other switches
  - will synchronize its own database upon receiving an advertisement with a higher revision number
  - recommended mode for all switches in the domain
  ```
	SW1(config)# vtp mode server
	```

- **Client**:
  - Switch cannot create, modify or delete VLANs, otherwise behaves like server
  ```
	SW1(config)# vtp mode client 
	```
  
- **Transparent**:
	- Switch can create, modify and delete VLANs, but will not advertise changes or synchronize its own database
	- will forward VTP messages between switches in the same domain.
	- recommended for independently managed switches without interrupting VTP operation on other switches
- **Off**:
	- switch can create, modify and delete VLANs
	- will not advertise or synchronize changes
	- does not forward VTP messages
	- recommended mode for all switches if VTP is not used in the LAN

## VTP Versions
There are three versions of VTP available, but Version 1 and 2 are outdated.
Version 3 introduced the off mode, the primary/secondary server concept and extended-range VLAN support.
You can set the VTP version with the `vtp version version` command:
```
SW1(config)# vtp version 3 
```
### Primary/ Secondary Server
The *Primary Server* in VTP version 3 is the only server in a VTP domain that can create, modify and delete VLANs. You can set the primary server with the `vtp primary` command:
```
SW1# vtp primary
```

The other switches in server mode are now *Secondary Servers* and are functionally clients, but they become the primary server when the `vtp primary` command is executed on it.
### Extended-Range VLANs
Before Version 3 only the normal-range VLANs 1-1005 were available for use, the extended-range VLANs 1006-4094 were reserved for internal application.

## VTP 

# Inter-VLAN Routing
Hosts in one VLAN cannot communicate with hosts in other VLANs, unless there is a router or a Layer 3 switch.
*Inter-VLAN routing* is the process of forwarding network traffic from one VLAN to another VLAN.
## Legacy Inter-VLAN routing
![[Legacy Inter-VLAN routing.png]]
A legacy method of inter VLAN routing was using a Router and connecting it with each VLAN.

## Router on a Stick
 ![[Router on a Stick.png]]
 The *router on a stick* method only needs one Ethernet port, which is configured as an 802.1Q trunk and connected to a trunk port on a Layer 2 switch.
### Subinterfaces 
This method requires the creation of *subinterfaces*, which send and receive tagged frames, for each VLAN to be routed.
To create a subinterface use `interface interface-id.subinterface-id`.
After that you configure the subinterface to respond to 802.1Q encapsulated traffic from a specified VLAN with `encapsulation dot1q vlan-id [native]`. The `native` keyword is used to set the native VLAN to something other than VLAN 1.
You also have to set a IP address to the subinterface, which will serve as a default gateway.

**Example**
#Cisco_CLI 
```
# R1(config)# interface g0/0 
# R1(config-if)# no shutdown

R1(config)# interface G0/0/1.10
R1(config-subif)# description Default Gateway for VLAN 10

# set the VLAN associated with the subinterface
R1(config-subif)# encapsulation dot1Q 10

R1(config-subif)# ip add 192.168.10.1 255.255.255.0
R1(config-subif)# exit
```

## Layer 3 Switch/ SVI inter-VLAN routing
![[Layer 3 Switch VLAN routing.png]]
This method uses a *Layer 3 switch* and *switched virtual interfaces (SVI)*.
The advantages of using Layer 3 switches :
- much faster than router-on-a-stick 
- no need for external links from the switch to the router
- not limited to one link because Layer 2 EtherChannels can be used as trunk links between the switches to increase bandwidth.
- lower latency because data does not need to leave the switch in order to be routed to a different network.
- They more commonly deployed in a campus LAN than routers.

### Switch Configuration
#Cisco_CLI 
1. Create VLANs
```
D1(config)# vlan 10
D1(config-vlan)# name LAN10
D1(config-vlan)# vlan 20
D1(config-vlan)# name LAN20
```
2. Create the SVI VLAN interfaces.
```
D1(config)# interface vlan 10
D1(config-if)# description Default Gateway SVI for 192.168.10.0/24
D1(config-if)# ip add 192.168.10.1 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
D1(config)#
D1(config)# int vlan 20
D1(config-if)# description Default Gateway SVI for 192.168.20.0/24
D1(config-if)# ip add 192.168.20.1 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
```
3. Configure access ports
```
D1(config)# interface GigabitEthernet1/0/6
D1(config-if)# description Access port to PC1
D1(config-if)# switchport mode access
D1(config-if)# switchport access vlan 10
D1(config-if)# exit
D1(config)#
D1(config)# interface GigabitEthernet1/0/18
D1(config-if)# description Access port to PC2
D1(config-if)# switchport mode access
D1(config-if)# switchport access vlan 20
D1(config-if)# exit
```
4. Enable IP routing
```
D1(config)# ip routing
```

### Routing
![[Layer 3 Switch Routing.png]]
If VLANs are to be reachable by other Layer 3 devices, they mus be advertised using static or dynamic routing. To enable routing on a Layer 3 switch, a *routed port* must be configured.

A routed port (G0/0/1) is created by disabling the switchport feature on a Layer 2 port with `no switchport`, which converts it into a Layer 3 interface.

#### Configuration
#Cisco_CLI 
1. Configure the routed port
```
D1(config)# interface GigabitEthernet0/0/1
D1(config-if)# description routed Port Link to R1
D1(config-if)# no switchport
D1(config-if)# ip address 10.10.10.2 255.255.255.0
D1(config-if)# no shut
D1(config-if)# exit
```
2. Enable routing
```
D1(config)# ip routing
```
3. Configure routing
   Configure the OSPF routing protocol to advertise VLAN 10 and VLAN 20 netoworks
```
D1(config)# router ospf 10
D1(config-router)# network 192.168.10.0 0.0.0.255 area 0
D1(config-router)# network 192.168.20.0 0.0.0.255 area 0
D1(config-router)# network 10.10.10.0 0.0.0.3 area 0
```
4. Verify routing
```
D1# show ip route | begin Gateway
```

## Troubleshooting

![[VLAN Troubleshooting.png]]
