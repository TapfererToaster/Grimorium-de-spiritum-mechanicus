#CCNA 
# Unicast, Broadcast and Multicast
## Unicast
In a unicast transmission one device sends a message to one other device in a one-to-one communication. 
A unicast packet has a destination IP address of a single recipient. 
>[!info]
>The source IP address in a packet can only be a unicast address, as the packet originates from one device.

## Broadcast
In a broadcast transmission one device sends a message to all other devices on a network in an one-to-all communication.

A broadcast packet must be processed by all devices in the same broadcast domain, which identifies all hosts on the same network.

> [!important]
> A broadcast packet has a destination IP address with all ones in the host portion, or 32 one bits.
> There are two types of broadcasts:
> - *limited:* is send to 255.255.255.255
> - *directed:* is send to all hosts on a specific network and uses the highest address in the network f.e. `172.16.4.255` in `172.16.4.0/24`
## Multicast
In a multicast transmission one device sends a message to a selected set of other devices that are part of a multicast group.

> [!important]
> A multicast packet has a destination IP address that is a multicast address, the addresses from `224.0.0.0` to `239.255.255.255` are reserved as a multicast range.

>[!info]
>Routing protocols like *Open Shortest Path First (OSPF)* use multicast transmissions, enabling routers to communicate with each other using the reserved address `224.0.0.5`

# Types of IPv4 Addresses
## Public and Private Addresses

> [!important]
> *Public addresses* are globally routed between ISP routers.
> 
> *Private addresses* are blocks of addresses that are used to address internal hosts within a network. These were introduced in 1996 with the [RFC 1918: Address Allocation for Private Internets](https://www.rfc-editor.org/rfc/rfc1918).
> 
> | Network Address and Prefix | RFC 1918 Private Address Range |
| -------------------------- | ------------------------------ |
| 10.0.0.0/8                 | 10.0.0.0 - 10.255.255.255      |
| 172.16.0.0/12              | 172.16.0.0 - 172.31.255.255    |
| 192.168.0.0/16             | 192.168.0.0 - 192.168.255.255  |

>[!info] [[Router#Routing|Routing]] to the internet
>When a packet is send to a destination that is outside the network, the source IP address is a private address, which must be translated to a public address before forwarded to an ISP.
>The address is translated using *Network Address Translation (NAT)*.

## Special Use IPv4 Addresses
Addresses that can not be assigned to hosts, or with restrictions on how the devices interact within a network are called special use addresses.

**Loopback addresses**
The address block `127.0.0.0/8 (127.0.0.1-127.255.255.254)` are used by a host to direct traffic to itself. This can be used to test the IP configuration of a device by using the `ping` command.

**Link-Local addresses**
The address block `169.254.0.0/16 (169.254.0.1-169.254.255.254)`, commonly known as *Automatic Private IP Adressing (APIPA)* addresses, are used by Windows clients when they can not obtain an IP address through other means to self-configure an address.

## Legacy Classful Addressing
In 1981, IPv4 addresses were assigned using classful addressing as defined in [RFC 790 - Assigned numbers](https://datatracker.ietf.org/doc/html/rfc790) dividing the unicast ranges into specific classes:
- **Class A (0.0.0.0/8 - 127.0.0.0/8)**
  designed for extremly large networks; used a fixed /8 bit predix, indicating the network address and the remaining three octets as host addresses, resulting in more than 16 million hos addresses per network.
- **Class B (128.0.0.0/16 - 192.255.0.0/16)**
  designed for moderate to large networks; used a fixed /16 prefix indicating the network address and the remaining two octets as host addresses, resulting in more than 65,000 host addresses per network.
- **Class C (192.0.0.0/24 - 223.255.255.0/24)**
  designed for small networks; used fixed /24 prefix indicating the network address and the remaining octet as the host addresses, resulting in 254 host addresses.
- **Class D (224.0.0.0 - 239.0.0.0)**
  used for multicasting
- **Class E (240.0.0.0 - 255.0.0.0)**
  experimental address block

![[IP Legacy Classes.png]]

>[!note]
>The Class was determined by the first 1 to 4 bits of the address.

With the introduction of the world wide web, classful addressing was replaced by classless addressing and is thus deprecated.

# Assignment of IP Addresses
IPv4 and IPv6 addresses are managed by the *Internet Assigned Numbers Authority (IANA)*, which manages and allocates blocks of IP addresses to the *Regional Internet Registries (RIRs)*.
There are five RIRs, who have the responsability to allocate IP addresses to ISPs:
- **African Network Information Centre (AfriNIC)**
- **Asia Pacific Network Information Centre (APNIC)**
- **Regional Latin-American and Caribbean IP Address Registry (LACNIC)**
- **Reseaux IP Europeens Network Coordination Centre (RIPE NCC)**

# Network Segmentation
Subnetting reduces the overall network traffic, which improves network performance, it also allows for better security implementation.
What devices belong in which subnet can be determined by:
- **Location**
  The devices on each floor of a building are grouped into a subnet for each floor
- **Group or Function**
  Each unit (Administration, Human Resources, ...) are divided into different subnets
- **Device Type**
  Different devices (server, printer, hosts, ...) are divided into different subnets
  
  
## Broadcast Domains 
In an Ethernet LAN devices use *[[(ARP) Address Resolution Protocol]]* to locate other devices on the network. ARP sends [[(OSI) Open Systems Interconnection-Modell#2. Datensicherungsschicht (Data Link, Layer 2)|Layer 2]] broadcasts to known IPv4 addresses on the local network to discover the associated MAC address. 
Devices typically acquire IPv4 address configuration using the *[[(DHCPv4) Dynamic Host Configuration Protocol]], sending a broadcast on the local network to a DHCP server.
Switches send broadcasts out all interfaces except the interface on which it was received.
>[!warning] Large Broadcast Domains
> The hosts in a large broadcast domain can generate excessive broadcasts, which can slow down network operations, due to high network traffic. 

# Packets
[Cisco Networking Academy](https://www.netacad.com/launch?id=75cbc2e3-bab4-484c-a925-ad839c127c20&tab=curriculum&view=8647a713-1af7-56f2-aad8-6a839871467f)

# Addressing
The IPv4 address is a 32-bit number that identifies a host at [[TCP-IP Modell#Layer 3 Network Layer|TCP/IP Layer 3]].
## Address Structure
An IPv4 address is made up of 32-bits and divided into two parts, *network* and *host* portion.  
It is a string of 0s and 1s, but to be more readable the *dotted decimal notation* is used, which splits the IP address into four groups of 8 bit, separates them with a dot and each octet is  converted to decimal.
![[IPv4 dotted decimal.png]]

> [!nfo] Octet and byte
>A *byte* does not necessarily have to be 8 bits long, as it is defined as the minimum unit of data a computer can read. Although it is almost always 8 bits long there have been computers that use 6,7 and 9 bit bytes.
>A *octet* always refers to 8 (bits) and is technically more precise in the context of IPv4 addresses.
 
### Suffix length
The *Suffix length*  or *CIDR-Notation* is used to separate the *Network portion* from the *Host portion*, by indicating the length of the Network portion. If the Prefix is /24 then the Network portion is 24 bits long.

>[!note] 
>All hosts in the same network will share the same Network portion, but have a unique Host portion.
>![[Example Host-Network Portion.png]]
>
### Netmasks
*Netmasks* seperate the Network portion from the Host portion, which identifies the subnet of the host. This 32 bit string is written in dotted decimal notation, like IPv4 addresses.
The netmask is always a series of 1s followed by a series of 0s.

![[IPv4 address structure.png]]
# Header
The IPv4 header consists of 14 fields (the "Option" field is optional) and has a length of at least 20 bytes (without the "Option" field) and a maximum 60 bytes.

![[IPv4 Header.png]]
## Version field
This field indicates the version of IP being used:
- 0b0100 (0d4) indicates IPv4
- 0b0110 (0d6) indicates IPv6
## Internet Header Length (IHL) field
This field indicates the length of the IPv4 header and is 4 bits long.
>[!remember]
>The length of an IPv4 header is variable in length, depending if the Option field is present, which itself is also variable in length.

The length of IPv4 header is indicated in 4-byte increments, if the value of this field is 5, it means that the header is 20 bytes long. A value greater than 5 indicates that the option field is present in the header. The maximum value of this field is 15 indicating that the header is 60 bytes long (the complete 40 bytes of the Option field are used).
## DSCP and ECN fields
The *Differentiated Services Code Point (DSCP)*, which is 6 bits long, and *Explicit Congestion Notification (ECN)*, which is 2 bits in length, use the field that used to be called *Type of Service*. 
These fields are used for *Quality of Service (QoS)*, which is used to prioritize specific types of network traffic over other types, f.e. prioritize delay-sensitive network traffic like voice and video calls.
## Total Length field
The *Total Length* field indicates the length of the packet -the IPv4 header and the payload - in bytes. For example, a value of 100 in this fiels means the packet is 100 bytes long, a value of 1000 means the packet is 1000 bytes in length.

>[!caution]
>Do not confuse this field with the *IHL* field, which indicates just the length of the IPv4 header

## Identification, Flags and Fragment Offset fields
These three fields are used together to support packet fragmentation and are 32 bits in total length.

>[!info] Fragmentation
>IPv4 uses *maximum transmission unit (MTU)*, packets larger than the MTU will be fragmented and reassembled at the destination host.
>
>The typical MTU is 1500 bytes

### Identification Field
This field is 16 bits in length and is used to identify which original packet a fragment belongs to When a packet is fragmented, all of its fragments must have the same value in this field.
### Flags Field
This field is 3 bits in length and is used to control and identify fragments. The 3 bits are defined as follows:
- Bit *0*: *Reserved*
   this bit is not used an always set to 0
- Bit *1*: *Don't Fragment (DF)*
  If this bit is set to 1, the packet should not be fragmented. If the packet's size is greater than the MTU, it will be discarded.
- Bit *2*: *More Fragments (MF)*
  If this bit is set to 1, it means there are more fragments remaining. The final fragment of the  packet will have a value of 0 in this field. Unfragmented packets will always have a value of 0 for this bit.
### Fragment Offset Field
This field is 13 bits long and is used to indicate the position of the fragment within the original packet. This allows fragmented packets to be reassembled, even if the fragments arrive out of order. 

## Time to Live (TTL)
This field is 8 bits long and is used to prevent looping packets. When a host sends a packet, it will set an initial value (a common value is 64) and then each router that forwards the packet will decrease the value in this field by 1. If the value reaches 0, the router will drop the packet.

>[!note] Looping
>A *loop* is when a message travels around the network without being able to find its destination.
>
>For example, there are three routers R1, R2 and R3, a looping packet could be passed from R1 to R2, from R2 to R3 and then from R3 to R1 and so on.
>![[Looping packet.png]]

## Protocol field
The *Protocol Field* is 8 bits long and indicates what kind of message is encapsulated inside of the packet, by a specific value.
Some protocols field values are:
- *1*: [[(ICMP) Internet Control Message Protocol]]
- *6*: [[(TCP) Transmission Control Protocol]]
- *17*: [[(UDP) User Datagram Protocol]]
- *89*: Open Shortest Path First  
## Header Checksum field
This field is 16 bits long and is used to check for errors in the IPv4 header (not in the entire packet)
## Source Address and Destination Address fields
These two fields are both 32 bit long and contain the IP address of the host sending the packet (*Source Address*) and the intended recipient of the packet (*Destination Address*)
## Option field
The final field of the IPv4 header is the *Option field*. This field is optional and variable in length, from 0 bytes to 40 bytes. 
## IPv4 Header and IPv6 Header changes
![[IPv4 Header changes.png]]
- **Version** - Contains a 4-bit binary value set to 0100 that identifies this as an IPv4 packet.
- **Differentiated Services or DiffServ (DS)** - Formerly called the type of service (ToS) field, the DS field is an 8-bit field used to determine the priority of each packet. The six most significant bits of the DiffServ field are the differentiated services code point (DSCP) bits and the last two bits are the explicit congestion notification (ECN) bits.
- **Time to Live (TTL)** - TTL contains an 8-bit binary value that is used to limit the lifetime of a packet. The source device of the IPv4 packet sets the initial TTL value. It is decreased by one each time the packet is processed by a router. If the TTL field decrements to zero, the router discards the packet and sends an Internet Control Message Protocol (ICMP) Time Exceeded message to the source IP address. Because the router decrements the TTL of each packet, the router must also recalculate the Header Checksum.
- **Protocol** - This field is used to identify the next level protocol. This 8-bit binary value indicates the data payload type that the packet is carrying, which enables the network layer to pass the data to the appropriate upper-layer protocol. Common values include ICMP (1), TCP (6), and UDP (17).
- **Header Checksum** - This is used to detect corruption in the IPv4 header.
- **Source IPv4 Address** - This contains a 32-bit binary value that represents the source IPv4 address of the packet. The source IPv4 address is always a unicast address.
- **Destination IPv4 Address** - This contains a 32-bit binary value that represents the destination IPv4 address of the packet. The destination IPv4 address is a unicast, multicast, or broadcast address.

# Subnetting

# IPv4 Network Attributes
## Network Address
- used to identify the network
- can not be assigned to a host
- all host bits are set to 0

![[Network Address.png]]
## Broadcast Address
- last address of the network
- can not be assigned to a host
- used to address a message to all hosts
- all host bits are set to 1

![[Broadcast Address.png]]
## First and Last Usable Addresses
- the first and last address that can be assigned to a host
- the first address after the network address
  ![[First Usable Address.png]]
- the last address before the broadcast address
  ![[Last Usable Address.png]]

>[!note] Maximum Number of Hosts
>The maximum number of hosts can be calculated with the formula:
> $2^{x}-2$
>, with x being the number of Hostsbits.  



