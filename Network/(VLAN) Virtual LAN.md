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
The standard [[Ethernet#Frames|Ethernet frame header]] does not contain information about the VLAN to which the frame belongs. To add these information the IEEE 802.1Q header is used in a process called *tagging*. This header includes a 4-byte tag inserted into the original Ethernet frame, specifying the VLAN.
## VLAN tag fields

![[VLAN tag fields.png]]

The fields of the VLAN tag are the following
- **Type**: 2 byte value called *tag protocol ID (TPID)*. For Ethernet it is set to hex 0x8100
- **User priority**: 3 bit value that supports level or service implementation.
- **Canonical Format Identifier (CFI)**: 1 bit identifier that enables Token Ring frames to be carried across Ethernet links
- **VLAN ID (VID)**: 12 bit VLAN identification number that supports up to 4096 VLAN IDs

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

## Port Assignment
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

## Verify Trunk Configuration
To verify the Trunk configuration use `show interface interface-id switchport`

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