The *Address Resolution Protocol (ARP)* maps [[(OSI) Open Systems Interconnection-Modell#MAC Addresses|MAC addresses]] with their assigned [[IPv4]] addresses.

When a host wants to send a message it needs to know the IP address and the MAC address of the destination device, if it does not know the MAC address it uses ARP.
The process of ARP:
1. *ARP request*:
   The host creates and sends a frame addressed to a [[(OSI) Open Systems Interconnection-Modell#MAC Addresses|broadcast MAC address]], containing a message with the IPv4 address of the intended destination host.
2. Each host on the network receives the broadcast frame and compares the IPv4 address inside the message with its own Ipv4 address.
3. *ARP reply*:
   If the addresses match, device sends back its MAC address.
4. The sending host stores the the MAC and IPv4 address in its ARP table

![[ARP process.png]]

>[!note]
>Due to the ARP request-reply exchange, SW1 and SW2 have learned PC1 and PC3 MAC addresses.
# Destination on the same Network
When a host wants to send a message to another host on the same network it needs both the MAC and IP address. If the host only knows the IP address, it uses ARP to discover the MAC address.

# Destination on a Remote Network
When the destination IP address is on a remote network the destination MAC address used in the packet is the MAC address of the local default gateway.
When the router receives the packet it de-encapsulates the [[(OSI) Open Systems Interconnection-Modell#2. Datensicherungsschicht (Data Link, Layer 2)|Layer 2]] information. It then examines the IP address to determine the best next-hop device and encapsulates the IPv4 packet in a new data link frame. The destination of the frame is the MAC address of the next-hop device and the source is the MAC address of the interface facing the internet of the router.
When the packet reaches the network of the destination device the router it again de-encapsulates the packet and encapsulates the packet with the destination MAC of the device and the source MAC of the router.
# Broadcast Domain
W