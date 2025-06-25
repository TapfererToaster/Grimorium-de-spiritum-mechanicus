# The need for IPv6
IPv6 is designed as a successor to IPv4, because IPv4 does not have enough addresses. This shortage is because the IPv4 has 4.3 billion possible addresses, but there are a lot more devices than that. The use of private addresses and *Network Address Translation (NAT)* slowed down the depletion, but it also created its own problems. 
With the rise of IoT the need for more addresses became clearer, which IPv6 with its 128-bits and 340 undecillion (10^36) possible addresses could provide.

# IPv4 and IPv6 Coexistence
Both IPv4 and IPv6 are used today and will be for the near future so the IETF created protocols that allows for IPv6 migrations.
- **Dual Stack**
  Dual stack allows devices to run IPv4 and IPv6 protocol stacks simultaneously.
- **Tunneling**
  Tunneling encapsulates IPv6 packets inside an IPv4 packet, so it can be transported over an IPv4 network.
- **Translation**
  IPv6 packets are translated to IPv4 packets, using *Network Address Translation 64 (NAT64)* and allows IPv6-enabled devices to communicate with IPv4-enabled devices. 
# Packets
[Cisco Networking Academy](https://www.netacad.com/launch?id=75cbc2e3-bab4-484c-a925-ad839c127c20&tab=curriculum&view=43f9c41c-73eb-5d8e-afef-3b78facacf9f)
# IPv6 Addresses
## Unicast
- **Global Unicast Address (GUA)** - This is similar to a public IPv4 address. These are globally unique, internet-routable addresses. GUAs can be configured statically or assigned dynamically.
- **Link-local Address (LLA)** - This is required for every IPv6-enabled device. LLAs are used to communicate with other devices on the same local link. With IPv6, the term link refers to a subnet. LLAs are confined to a single link. Their uniqueness must only be confirmed on that link because they are not routable beyond the link. In other words, routers will not forward packets with a link-local source or destination address.

# Header
![[IPv6 Header.png]]
- **Version** - This field contains a 4-bit binary value set to 0110 that identifies this as an IP version 6 packet.
- **Traffic Class** - This 8-bit field is equivalent to the IPv4 Differentiated Services (DS) field.
- **Flow Label** - This 20-bit field suggests that all packets with the same flow label receive the same type of handling by routers.
- **Payload Length** - This 16-bit field indicates the length of the data portion or payload of the IPv6 packet. This does not include the length of the IPv6 header, which is a fixed 40-byte header.
- **Next Header** - This 8-bit field is equivalent to the IPv4 Protocol field. It indicates the data payload type that the packet is carrying, enabling the network layer to pass the data to the appropriate upper-layer protocol.
- **Hop Limit** - This 8-bit field replaces the IPv4 TTL field. This value is decremented by a value of 1 by each router that forwards the packet. When the counter reaches 0, the packet is discarded, and an ICMPv6 Time Exceeded message is forwarded to the sending host,. This indicates that the packet did not reach its destination because the hop limit was exceeded. Unlike IPv4, IPv6 does not include an IPv6 Header Checksum, because this function is performed at both the lower and upper layers. This means the checksum does not need to be recalculated by each router when it decrements the Hop Limit field, which also improves network performance.
- **Source IPv6 Address** - This 128-bit field identifies the IPv6 address of the sending host.
- **Destination IPv6 Address** - This 128-bit field identifies the IPv6 address of the receiving host.