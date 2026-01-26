#CCNA 
# Five Layer Model and OSI
The TCP/IP model defined in RFC 1122 has four layers, however network engineers reference a five-layer TCP model, as it is more useful for understanding networking.

![[TCP-IP model.png]]
# The Layers
Each layer provides an essential function to enable communication over a network.
The communication could be a web browser communicating with a web server.

![[Network communication.png]]
- Layer
## Layer 1: Physical Layer

> [!definition] Definition
> The **Physical Layer** defines the *physical requirements* for transmitting data from one node to another, such as ports, connectors, cables and how data should be encoded.
> 
> This layer is parallel to the [[(OSI) Open Systems Interconnection-Modell#1. Physische Schicht (Physical, Layer 1)|OSI Layer 1]]

In IEEE 802.3 [[Ethernet]] defines connectors and cable types, as well as how data should be encoded into electrical or light signals among other minutiae about how to communicate.
[[WLAN#IEEE 802.11 standards|IEEE 802.11]] defines what radio waves should be modulated to encode data, etc.
## Layer 2: Data Link Layer
>[!definition] Definition
>The **Data Link Layer** defines how data is addressed and is responsible for *hop-to-hop* delivery of messages, with the use of MAC addresses.
>
>This layer is parallel to the [[(OSI) Open Systems Interconnection-Modell#2. Datensicherungsschicht (Data Link, Layer 2)| OSI Layer 2]]. 

This Layer is responsible to prepare data for transmission over the physical medium to the next node, this node is a router or the destination device.
>[!info]
>Switches do not count as hops.

At each hop, the MAC address is changed to the MAC address of the next hop and sent to it.

![[Data-Link-Layer Hop-to-Hop.png]]

## Layer 3: Network Layer
>[!definition]
>The **Network Layer** uses IP addresses to provide *end-to-end* delivery of messages.
>
>This layer is parallel to the [[(OSI) Open Systems Interconnection-Modell#3. Netzwerk Schicht (Network, Layer 3)|OSI Layer 3]].

The Data Link Layer provides hop-to-hop delivery this layer is responsible for end-to-end delivery from the original source host to the final destination host. 
![[End-to-end delivery.png]]
## Layer 4: Transport Layer
>[!definition]
>The **Transport Layer** uses *port numbers* to ensure that data reaches the correct application process on the destination host. (*application-to-application* delivery)
>
>This layer is parallel to the [[(OSI) Open Systems Interconnection-Modell#4. Transportschicht (Transport, Layer 4)|OSI Layer 4]].

By addressing a message to a specific port, you can send data to specific application processes, which enables you to have multiple processes running simultaneously (f.e. web browser with multiple tabs, online games, etc.). 

![[Layer 4.png]]
## Layer 7: Application Layer
>[!definition]
>The **Application Layer** is the *interface* between applications and the network. Using Layer 7 protocols an application running on a computer can prepare a message to be sent over the network.
>
>This layer is parallel to the [[(OSI) Open Systems Interconnection-Modell#5. Sitzungsschicht (Session, Layer 5)|Layer 5]], [[(OSI) Open Systems Interconnection-Modell#6. Darstellungsschicht (Presentation, Layer 6)|Layer 6]] and [[(OSI) Open Systems Interconnection-Modell#7. Anwendungsschicht (Application, Layer 7)|Layer 7]].

![[Network communication.png]]

# Data encapsulation and de-encapsulation
## Data Encapsulation
To send data the host has to go through the process of *data encapsulation*, where each layer adds data before it is send.
This process can be broken up into 5 steps:
1. **Layer 7 (Application Layer)** protocol prepares data
2. **Layer 4 (Transport Layer)** encapsulates the data with a header addressed to a port number on the destination host
3. **Layer 3 (Network Layer)** encapsulates the data with a header addressed to the IP address of the destination host
4. **Layer 2 (Data Link Layer)** encapsulates data with a header addressed to the MAC address of the next hop and a trailer, used to check for errors.
5. The host transmits the bits of data over the physical medium (electrical/ light/ radio waves)

>[!definition]
>A *header* is supplemental data added to the front of a message that is to be transmitted over a network. A protocol's header contains the data used by that protocol.
>
>A *trailer* is supplemental data added to the end of a message that is to be transmitted over a network.

![[Data encapsulation.png]]

## Data De-encapsulation
When the destination host receives the message, it goes through the opposite process: *de-encapsulation*, which can also be broken down into five steps:
1. The destination host receives the message
2. It inspects Layer 2 (Data Link Layer) header and trailer, removes them and passes the message to Layer 3
3. It inspects the Layer 3 (Network Layer), removes it and passes the message to Layer 4
4. It inspects the Layer 4 header, removes it and sends the data to the appropriate application
5. The application receives and processes the data

![[Data de-encapsulation.png]]

# Protocol Data Units (PDU)
Each Layer has its own name for the combination of the data/ message and the Layers header/ trailer during the (de-)encapulation process:
- Layer 4 (Transport Layer): *segment* or L4PDU
- Layer 3 (Network Layer): *packet* or L3PDU
- Layer 2 (Data Link Layer): *frame* or L2PDU

>[!note]
>The content of the each PDU is called *payload*. 
>Meaning that a *frames* payload is a packet and a *packets* payload is a segment and the *segments* payload is the application data.

![[Protocol Data Unit (PDU).png]]

# Adjacent-Layer and Same-Layer Interactions
Each Layer of the TCP/IP model provides a service for the layer above it, this is called *adjacent-layer interaction*:
- Layer 4 (Transport Layer) provides a services to Layer 7 (Application Layer) by delivering the data to the appropriate application on the destination host
- Layer 3 (Network) provides a service to Layer 4 by delivering the segment to the correct destination host
- Layer 2 (Data Link) provides a service to Layer 3 by delivering the packets to the next hop
- Layer 1 (Physical) provides a service to Layer 2 by providing a physical medium for frames to travel over.

*Same-layer interaction* refers to the communication between the same layer on different computers:
- Application data from one computer is sent to an application on another computer
- The segment of Layer 4 (Transport) is addressed to Layer 4 of the destination host, where the information in the header will be inspected
- The packet of Layer 3 (Network) is addressed to Layer 3 of the destination host, where the information in the header will be inspected
- The frame of Layer 2 (Data Link) is addressed to the Layer 2 of the next hop, where the information in the header and trailer will be inspected
- Signals sent out of a physical port of one device are received by a physical port of another device.

![[Adjacent- and Same Layer Interactions.png]]
## Client-Server Relationship
## Network Application Services

| Protocol                                         | Description                                                                                         | Layer |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------- | ----- |
| Domain Name System (DNS)                         | Resolves internet names to IP addresses                                                             | 7     |
| Secure Shell (SSH)                               | Provides remote access to servers and networking devices                                            |       |
| Simple Mail Transfer Protocol (SMTP)             | Sends email messages and attachments from clients <br>to servers, and from servers to other servers |       |
| Post Office Protocol (POP)                       | Used by email clients to retrieve email and attachments<br>from a remote server                     |       |
| Internet Message Access Protocol <br>(IMAP)      | Used by email clients to retrieve email and attachments<br>from a remote server                     |       |
| Dynamic Host Configuration <br>Protocol (DHCP)   | Automatically configures devices with IP addressing and <br>other necessary information             |       |
| Hypertext Transfer Protocol (HTTP)               | Used by web browsers to request web pages and web <br>servers to transfer files (web pages)         | 7     |
| Hypertext Transfer<br>Protocol Secure<br>(HTTPS) | Same as HTTP, but encrypted                                                                         | 7     |
| File Transfer Protocol (FTP)                     | Used for interactive file transfer between systems                                                  |       |
# Protocols
![[TCP-IP Protocols.png]]
## Application Layer
- [[(DNS) Domain Name System]]: 
  Translates domain names to IP addresses
- [[(DHCPv4) Dynamic Host Configuration Protocol]]:
   dynamically assigns IP addressing information to clients at start up and re-uses them when no longer needed
- (SLAAC) Stateless Address Autoconfiguration:
  allows a device to obtain IPv6 addressing information without using a DHCPv6 server
- [[Email#(SMTP) Simple Mail Transfer Protocol|(SMTP) Simple Mail Transfer Protocol]]: 
  enables clients to send email to a mail server and enables mail server to send email to other mail servers
- [[Email#(POP3) Post Office Protocol version 3|(POP3) Post Office Protocol version 3]]:
  Enables clients to retrieve email from a mail server and download the email to the client's local mail application
- [[Email#(IMAP) Internet Message Access Protocol|(IMAP) Internet Message Access Protocol]]: 
   Enables clients to access email stored on a mail server as well as maintaining email on the server.
- [[(FTP) File Transfer Protocol]]:
  designed to transfer files from one host to another over a network
- (SFTP) SSH File Transfer Protocol:
  extension to the (SSH) Secure Shell protocol, which allows for encrypted file transfer
- (TFTP) Trivial File Transfer Protocol:
  connectionless file transfer protocol with best-effort, unacknowledged file delivery
- [[(HTTP-S) Hypertext Transfer Protocol - Secure]]:
   A set of rules for exchanging text, graphic images, sound, video, and other multimedia files on the World Wide Web. HTTPS encrypts data that is exchanged over the World Wide Web
- (REST) Representational State Transfer:
  A web service that uses application programming interfaces (APIs) and HTTP requests to create web applications.
## Transport Layer
- [[(TCP) Transmission Control Protocol]]:
  used for reliable, connection-oriented communications between processes running on separate hosts
- [[(UDP) User Datagram Protocol]]:
  used for connection-less communications between processes running on separate hosts, with less overhead than TCP but it does not ensure successful delivery
## Internet Layer
- [[IPv4]]:
  Receives message segments from the transport layer, packages messages into packets, and addresses packets for end-to-end delivery over a network. IPv4 uses a 32-bit address.
- [[IPv6]]:
  Similar to IPv4 but uses a 128-bit address.
- (NAT) Network Address Translation:
  Translates IPv4 addresses from a private network into globally unique public IPv4 addresses.
- (ICMP) Internet Control Message Protocol:
  Provides feedback from a destination host to a source host about errors in packet delivery.
- (ICMPv6 ND) ICMPv6 Neighbor Discovery:
   Includes four protocol messages that are used for address resolution and duplicate address detection.
- (OSPF) Open Shortest Path First
  Link-state routing protocol that uses a hierarchical design based on areas. OSPF is an open standard interior routing protocol.
- (EIGRP) Enhanced Interior Gateway Routing Protocol:
  An open standard routing protocol developed by Cisco that uses a composite metric based on bandwidth, delay, load and reliability.
- (BGP) Border Gateway Protocol
   An open standard exterior gateway routing protocol used between Internet Service Providers (ISPs
   (BGP is also commonly used between ISPs and their large private clients to exchange routing information.)
## Network Access
- [[(ARP) Address Resolution Protocol]]:
  Provides dynamic address mapping between an IPv4 address and a hardware address.
- [[Ethernet]]:
  Defines the rules for wiring and signaling standards of the network access layer.
- WLAN: 
  Defines the rules for wireless signaling across the 2.4 GHz and 5 GHz radio frequencies