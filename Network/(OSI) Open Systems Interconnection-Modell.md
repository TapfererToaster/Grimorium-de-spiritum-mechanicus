#CCNA 

Das *Open Systems Interconnection (OSI)* Modell besteht aus einem Regelsatz, Prozeduren und Formaten, die Netzwerkkommunikation managen und beim designen von Netzwerksystemen helfen und erklären, wie diese funktionieren.
Das Modell basiert auf einem Vorschlag der *Internationalen Organisation für Normung (ISO)*.

![[OSI-Summery-table.png]]

# 1. Physische Schicht (Physical, Layer 1)
Die Physische Schicht ist die unterste Schicht im OSI Modell und bezieht sich auf das physische Medium das den Datenbitstrom überträgt, bzw. die 1 und 0 die über das Netzwerk gesendet werden.

>[!summary]
>This layer describes the mechanical, electrical, functional and procedural means to activate, maintain and de-activate physical connections for a bit transmission to and from a network device.

Zu den Hauptaufgaben der 1.Schicht gehören: 
- **Hardware Spezifikationen**:
   Fragen im Bezug auf Kabel (CAT 6/ Glasfaser), Stecker, Funktransceiver, Netzwerkkarten (NIC), und anderen Hardware Geräten
- **Kodierung und Signalisierung**:
   wandelt Daten/ Bits in Signale um die über das Netzwerk gesendet werden können
- **Übertragung und Empfang**:
   senden und empfangen der Daten nach der Kodierung
- **Topologie und physisches Netzwerkdesign:**

>[!example] **Protokolle**:
>Ethernet, USB

# 2. Datensicherungsschicht (Data Link, Layer 2)
Diese Schicht ist für die Fehlererkennung und die Flusskontrolle für Frames in der physischen Verbindung verantwortlich.

>[!summary]
>This layers protocols describe methods for exchanging data frames between devices (NIC-to-NIC) over a common media and is responsible for error checking.

This Layer does the following:
- Enables upper layers to access the media, while the upper layer protocol is unaware of the type of media that is used to forward the data.
- Controls how data is placed and received on the media
- Exchanges frames between endpoints over the network media
- Receives encapsulated data and de-encapsulates them (Layer 3 packets)

Folgende Aufgaben gehören zu dieser Schicht:
- **Logical Link:**
  stellt die Verbindung zwischen den lokalen Netzwerkgeräten her
- **Media Access Control (MAC)**
  definiert die Zugriffskontrolle der Netzwerkgeräte
  >[!info]
  >Schicht 2 Switches versenden Frames mithilfe der MAC Adresse der Geräte
- **Datenframing**
  encapsulating data (Layer 3 *packets*) into Layer 2 *frames*, which will be send in Layer 1
- **Fehlererkennung**
  nutzt das CRC-Feld um zu prüfen ob Frames korrekt empfangen wurden and rejects corrupt frames
>[!info] VLAN-Tagging
>Wenn ein Frame in einen VLAN-fähiges Netzwerk eintritt, wird in Schicht 2 dem Frame ein Tag angehängt in dem seine VLAN-ID vermerkt ist. Jeder Frame muss einem/ seinem VLAN exakt zugeordnet werden können. Ist ein Frame untagged wird er im nativen VLAN versendet.
>Beispiel:
>Ein Switch empfängt einen Frame, der für ein spezifisches VLAN getagged ist und sendet ihn weiter zu einem Switch in demselben VLAN. Nachdem der Switch den Frame erhält entfernt er den VLAN Tag bevor der Frame den Port verlässt.

>[!example] **Protokolle:**
>Point-to-Point (PPP), ATM, [[(ARP) Address Resolution Protocol]], [[Ethernet]], Wifi, High-Level Data Link Control (HDLC), Frame Relay, Asynchronous Transfer Mode (ATM), X.25

At each hop along the path, a router performs the following Layer 2 functions:
1. Accepts a frame from a medium
2. De-encapsulate the frame
3. Rr-encapsulate the packet into a new frame
4. Forward the new frame to the next hop
## Sublayers
![[Data-Link Sublayers.png]]
### Media Access Control (MAC) Sublayer

#### Ethernet Standards of the MAC sublayer
![[Data-link Ethernet MAC sublayer.png]]
**Data Encapsulation**
[IEEE 802.3](https://en.wikipedia.org/wiki/IEEE_802.3) data encapsulation includes the following:
- **Ethernet frame**:
   Internal structure of the [[Ethernet#Frames|Ethernet frame]], fields within the frame are delimited by delimiting bits that provide synchronization between the transmitting and receiving nodes 
- **Ethernet Addressing**:
   The frame includes a source and destination MAC address to deliver the Ethernet frame from one Ethernet NIC to another Ethernet NIC on the same LAN
- **Ethernet Error detection**: 
  The frame includes a *frame check sequence (FCS)* trailer for error detection

**Accessing Media**
IEEE 802.3 also includes the specification, how different Ethernet communications standards over different media (copper, fibre, ...)
- IEEE 802.3u Fast Ethernet
- IEEE 802.3z Gigabit Ethernet over Fiber
- IEEE 802.3ab Gigabit Ethernet over Copper
- IEEE 802.3ae 10 Gigabit Ethernet over Fiber
#### MAC Addresses
**Characteristics**
MAC addresses are 6 bytes or 48 bits long and are written using the [[Number Systems#Hexadecimal| Hexadecimal Number System]].
There are also different notational conventions:
-  0cf5.a452.b101 (Cisco IOS) 
- 0C-F5-A4-52-B1-01 (Windows) 
- 0c:f5:a4:52:b1:01 (macOS)

**Organizationally Unique Identifier (OUI)**
To ensure that a MAC address is unique the first half of the address (3 bytes) is an organizationally unique identifier assigned to the manufacturer by the IEEE. The second half, the Vendor-assigned address, is then used to assign a unique address to each device.

**Unicast MAC Addresses**
This address is used to deliver a frame from a single transmitting device to a single destination device.

**Broadcast MAC Addresses**
- It has a destination MAC address of FF-FF-FF-FF-FF-FF in hexadecimal (48 ones in binary).
- It is flooded out all Ethernet switch ports except the incoming port.
- It is not forwarded by a router

**Multicast MAC Addresses**
- There is a destination MAC address of 01-00-5E when the encapsulated data is an IPv4 multicast packet and a destination MAC address of 33-33 when the encapsulated data is an IPv6 multicast packet.
- There are other reserved multicast destination MAC addresses for when the encapsulated data is not IP, such as Spanning Tree Protocol (STP) and Link Layer Discovery Protocol (LLDP).
- It is flooded out all Ethernet switch ports except the incoming port, unless the switch is configured for multicast snooping.
- It is not forwarded by a router, unless the router is configured to route multicast packets.
### Logical Link Control (LLC) Sublayer
The logical link control sublayer, defined in [IEEE 802.2](https://en.wikipedia.org/wiki/IEEE_802.2), communicates between the networking software at the upper layers and the hardware at the lower layers.
It takes the network protocol data, typically an IPv4 or IPv6 packet and adds Layer 2 control information to help deliver that packet to the destination node. 
It also places information inside the frame that identifies which network protocol is being used for the fram, this allows multiple Layer 3 protocols(IPv4, IPv6) to use the same network interface or media.
## Access Control Methods

>[!note]
>Ethernet networks operate in full-duplex and do not require access method.
### Contention-based access
In *contention-based multiple access networks*, all nodes are operating in half-duplex, competing for the use of the medium, as only one device can send at a time
#### Carrier sense multiple access with collision detection (CSMA/CD)
This control method was used by legacy Ethernet LANs where hubs were employed.
The workings behind it are, that a computer determines if a device is currently transmitting data, by checking if the signal amplitude is higher than normal on the media or the NIC compares data transmitted with data received.
#### Carrier sense multiple access with collision avoidance (CSMA/CA)
This control method is used by [[WLAN]] and attempts to avoid collisions.
Each device that transmits includes the time duration that it needs for the transmission and all other wireless devices receive this information and know how long the the medium will be unavailable, and wait until it is available.
### Controlled access
In a *controlled-based multiple access network*, each node has its own time to use the medium. These deterministic types of legacy networks are inefficient because a device must wait its turn to access the medium.
Networks that use this access control are:
- Legacy Token Ring
- Legacy ARCNET
## Layer 2 Frame
![[Layer 2 Frame.png]]
- **Frame start and stop indicator flags**: Used to identify the beginning and end limits of the frame.
- **Addressing**: Indicates the source and destination nodes on the media.
- **Type**: Identifies the Layer 3 protocol in the data field.
- **Control**: Identifies special flow control services such as quality of service (QoS). QoS gives forwarding priority to certain types of messages. For example, voice over IP (VoIP) frames normally receive priority because they are sensitive to delay.
- **Data**: Contains the frame payload (i.e., packet header, segment header, and the data).
- **Error Detection**: Included after the data to form the trailer.
# 3. Netzwerk Schicht (Network, Layer 3)
>[!summary]
>This layer provides services to exchange the individual pieces of data over the network between identified end devices.
> 

Diese Schicht definiert wie miteinander verbundene Netzwerke folgende Punkte behandeln:
- **Logische IP-Adressierung**
  jedes Gerät das über das Netzwerk kommuniziert, benötigt eine manchmal Schicht/ Layer 3 Adresse
- **DNS-Server**
  wandelt einen URL Namen in eine IP-Adresse um
- **Fully Qualified Domain Name (FQDN)**
  spezifiziert den Namen einer Top-Level-Domain und eines lokalen Host; zeigt die Website als eine URL da, z.B. maps.google.com
- **Windows Internet Naming Service (WINS)**
  dient der Namensauflösung in TCP/IP Netzwerken
- **Routing**
  handhabt eingehende Pakete und bestimmt ihr Ziel/ Next-Hop um zu ihrer Zieladresse zu gelangen
- **Datagrammkapselung**
  verpackt Daten von höheren Schichten in Datagramme (Pakete)
- **Fehlerbehebung und Diagnose**
  ermöglicht Geräten, die Datenverkehr weiterleiten möchten, Informationen über den Status von Hosts auf dem Netzwerk oder dem Gerät selbst auszutauschen
>[!info] Zusätzliche Funktionen von Schicht 3
>- **Internet Protocol Security (IPSec)**
>  Protokoll-Suite besteht aus offenen Standards, die eine private, sichere Kommunikation über IP-Netzwerke mithilfe von Kryptography gewährleisten.
>- **Adress Resolution PRotocoll (ARP)**
>  befindet sich zwischen Schichten 2 und 3; ermittelt die zu einer Netzwerkadresse (IP-Adresse) zugehörige Hardwareadresse; dies ermöglicht Kommunikation über ein Netzwerk nur durch MAC-Adressen
>- **Internet Control Message Protocol (ICMP)**
>  dient meist in Betriebssystemen für vernetzte Computer zum Austausch von Informations- und Fehlermeldungen; z.B. ping

>[!info] Vergleich von Layer 2 und Layer 3 Switches
>**Layer 2 (MAC) Switch** 
> leitet Datenverkehr über die MAC Adresse innerhalb eines LAN, indem sie eine Tabelle mit gelernten MAC Adresse und dem Port an dem sie empfangen wurde angelehnt 
> 
> **Layer 3 (IP) Switch**
> 

>[!example] **Protokolle**
>IP, ICMP, VPN, SSL/TLS

## Forwarding Decision
### Host Forwarding Decision
A host can send a packet to:
- **Itself** - A host can ping itself by sending a packet to a special IPv4 address of 127.0.0.1 or an IPv6 address ::1, which is referred to as the loopback interface. Pinging the loopback interface tests the TCP/IP protocol stack on the host.
- **Local host** - This is a destination host that is on the same local network as the sending host. The source and destination hosts share the same network address.
- **Remote host** - This is a destination host on a remote network. The source and destination hosts do not share the same network address

## Default Gateway
To communicate with devices on remote networks you need a default gateway, which can route traffic to remote networks. 
On a network, a default gateway is usually a [[Router#Router as Gateways|Router]] with the following features:
- a local IP address in the same address range as other hosts on the local network.
- It can accept data into the local network and forward data out of the local network.
- It routes traffic to other networks.
# 4. Transportschicht (Transport, Layer 4)
>[!summary]
>This layer defines services to segment, transfer and reassemble the data for individual communications between the end devices.
>

Bietet einen End-to-end Kommunikationspfad zwischen den niedrigeren Schichten und den Anwendungen der höheren Schichten.
(Enables end-to-end communication between running applications on different hosts.)
Protokolle dieser Schicht:
- **Real-Time Transport Protocol (RTP)**
  spezifiziert wie Programme die Übertragung von Multimedia-Daten in Echtzeit über Unicast- oder Multicast-Netzwerkdienste verwalten
- **Real-Time Streaming Protcol (RTSP)**
  Steuert Streaming Medien Server und Sitzungen zwischen Endpunkten
- **Real Time Messaging Protocol (RTMP)**
  Protokoll zum streamen von Audio, Video und Daten über das Internet zwischen einem Flash-Player und einem Server

>[!example] **Protokolle**
>[[(TCP) Transmission Control Protocol]],  [[(UDP) User Datagram Protocol]]
 
# 5. Sitzungsschicht (Session, Layer 5)
Ermöglicht Geräten, Sitzungen aufzubauen, zu verwalten und zu beenden.
Anhaltende logische Verbindung von zwei Software-Anwendungsprozessen, über die Daten über einen längeren Zeitraum austauschen können.
>[!summary]
>Is responsible for establishing, maintaining and synchronizing communication between applications running on different hosts. 

>[!example] **Protokolle**
>NetBIOS, PPTP, Network File System (NFS), Remote Procedure Call (RPC)

# 6. Darstellungsschicht (Presentation, Layer 6)
Wandelt systemabhängige Darstellungen der Daten in eine unabhängige Form um und ermöglicht einen korrekten Datenaustausch zwischen unterschiedlichen Systemen. 
(Ensures that data is delivered in a form that the application layer can understand.)

>[!summary]
>Provides a common representation of the data transferred between applications services.

The presentation layer has three primary functions:

- Formatting, or presenting, data at the source device into a compatible format for receipt by the destination device.
- Compressing data in a way that can be decompressed by the destination device.
- Encrypting data for transmission and decrypting data upon receipt.

As shown in the figure, the presentation layer formats data for the application layer, and it sets standards for file formats. Some well-known standards for video include Matroska Video (MKV), Motion Picture Experts Group (MPG), and QuickTime Video (MOV). Some well-known graphic image formats are Graphics Interchange Format (GIF), Joint Photographic Experts Group (JPG), and Portable Network Graphics (PNG) format.

>[!info] Zusätzliche Funktionen der Darstellungsschicht
>- Übersetzung zur Verbindungen von verschiedenen Computertypen miteinander, Windows-, macOS und Linux-Server sowie Mainframes können auf demselben Netzwerk kommunizieren
>- Kompression und Dekompression zur Verbesserung des Datendurchsatz
>- Verschlüßelung gewährleistet Datensicherheit während sie den Protokollstapel passiert

>[!example] Protokolle
>JPEG, MPEG, ASCII, Unicode


# 7. Anwendungsschicht (Application, Layer 7)

>[!summary]
>Contains protocols used for process-to-process communitation

This layer provides the interface between the applications used to communicate, and the underlying network over which messages are transmitted. Application layer protocols are used to exchange data between programs running on the source and destination hosts.

Stellt Anwendungsdienste für den Nutzerzugriff zur Verfügung. 
Protokolle dieser Schicht sind:
- **File Transfer Protcol (FTP)**
  Computerdateien können über ein Netzwerk übertragen werden
- **Hypertext Transfer Protocol (HTTP)**
  Wird zum Austausch oder zur Übertragung von strukturierten Netzwerktext genutzt
- **Domain Name System (DNS)**
  dezentrales und hierarchisches Bennenungssystem auf einem Netzwerk für Computer, Dienste und andere Resourcen

>[!example] **Protokolle**
>FTP, HTTP, SMTP

# Protocol Data Units (PDU)
The data and header information that are built in the bottom four layers have special names:

|       Layer       |   PDU    |
| :---------------: | :------: |
| Transport Layer 4 | Segments |
|  Network Layer 3  | Packets  |
| Data Link Layer 2 |  Frames  |
| Physical Layer 1  |   Bits   |



