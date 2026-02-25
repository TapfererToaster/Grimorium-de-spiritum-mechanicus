#CCNA 
# Transmission
WLAN uses the air as an *unbound medium*, unlike a *bound media* like copper or fiber-optic cable, meaning that the wireless signals propagate freely in open space, radiating in all directions from the source. This unbound nature allows greater mobility and flexibility, as devices do not need to be physically connected with a network cable. 
However it also introduces some challenges:
- All devices within range of a wireless device receive that device's signals
  data privacy within a wireless LAN is a great concern and communication is usually encrypted
- wireless devices must contend for airtime
  Devices must operate in half-duplex, only one device can transmit at a time and CSMA/CD or CSMA/CA must be implemented
- signal interference can be a major issue 
  Other WLAN signals, microwave ovens, bluetooth devices can disrupt and interfere with your WLAN signals
- Wireless communication are regulated by various international and national bodies
- Wireless signal coverage area must be considered
  as wireless signals travel through space, they lose strength, this is called *free-space path loss (FSPL)*; additionally the electromagnetic waves that are used as signals are affected by objects that are in their way

## Radio waves
To send a wireless signal, a device applies an alternating electric current to an antenna, which produces fluctuating electromagnetic fields that radiate out as waves, which are called electromagnetic waves.
### Wave behaviors
The electromagnetic waves that are used to encode wireless signals are influenced by the media they pass through and objects they encounter.
- *Absorption*:
  a wave passes through a medium, f.e. a wall and is converted into heat, weakening the signal
- *Reflection*:
  a wave bounces off a surface rather than passing through it, like light reflecting off a mirror; metal surfaces are highly reflective of radio waves.
- *Refraction*: 
  a wave passes from one medium to another one with a different density, altering the waves speed and causing it to bend
- *Diffraction*:
  the bending and spreading of waves around the edge of an object
- *Scattering*:
  a wave encounters a surface or medium with irregularities that cause the wave to spread out erratically, causes can be dust, smog, water vapor and textured surfaces
### Amplitude and frequency
>[!note]
Amplitude and frequency are two fundamental characteristics of electromagnetic waves. 

**Amplitude** 
The maximum strength of a signal, the higher the amplitude, the stronger the signal. If the signal is to weak the receiver might not be able to understand it
![[Radio Frequency Amplitude.png|522x146]]

**Frequency**
The number of oscillations per second, measured in *hertz (Hz)*. 
![[Radio frequency frequency.png|522x140]]

Higher frequencies support higher data transfers rates and are less crowded with other wireless devices. However, they are more susceptible to absorption by obstacles.

Lower frequencies penetrate better through obstacles and can cover larger distances. However, they support lower data transfer rates and more crowded with other wireless devices.

**Period**
The amount of time it takes for one full oscillation.
![[Radio Frequency period.png]]

### RF bands and channels
The *electromagnetic spectrum* is the entire range of electromagnetic radiation.
![[Radio frequency electromagnetic spectrum.png]]
Radio frequency (RF) is a segment defined as ranging from around 20 kHz to around 300 GHz. RF is used for radios, microwaves and radar, but also 802.11 WLAN. 
Three bands are used for WLAN:
- 2.4 GHz
- 5 GHz
- 6 GHz

>[!note]
>A band is a specific range of frequencies, such as the AM radio band, FM radio band and 802.11 WLAN band.

### 2.4 GHz band
This band spans from 2.4 to 2.495 GHz and is used for WLAN , but also for Bluetooth, microwave ovens, cordless telephones and many other technologies, leading to congestion and interference.
It is divided into 14 individual *channels*, which are like bands a range of frequencies. 
![[2.4 GHz band channels.png]]

> [!NOTE]
> For communication to work both the transmitting and receiving device must use the same channel.

Each channel has a defined center frequency and a standard width of 20 MHz, older standards use 22 MHz.
Because of the overlap between the channels it is important to use non-overlapping channels to avoid interference ind a WLAN that has multiple access points. Recommended are channels 1,6,11 or 1,5,9,11 in countries that allow up to 13 channels.
![[2.4 GHz band AP layout.png]]

### 5 GHz band
This band spans from 5.150 to 5.895 GHz and is less crowded with other technologies. 
The channels of the 5 GHz band do not overlap and can be combined to support higher data transfers rates, although resulting in fewer available channels.
![[5 GHz band channels.png]]
### 6 GHz band
The 6 GHz band was adopted in 2020 in the 802.11ax (Wi-Fi 6E) and 802.11be (Wi-Fi 7) standards, it ranges from 5.925 GHz to 7.125 GHz. It provides more non-overlapping channels than the 5GHz band and can also be combined to form wider channels, enhancing data transfer rates.
![[6 GHz band channels.png]]

# IEEE 802.11 standards
![[IEEE 802.11 standards.png]]

>[!note]
>In the 802.11 standard the term station is used for wireless devices connected to the WLAN, instead of clients.

## Frame Format
>[!note]
>[[TCP-IP Modell#Protocol Data Units (PDU)|Frames]] are the Protocol Data Units (PDU) of Layer 2.

![[802.11 Frame Format.png]]

- *Frame Control*:
  Provides information such as message type and subtype
- *Duration/ID*:
  Depending on the message type:
	- time in milliseconds that the channel will be dedicated to transmission of a frame
	- An identifier for the association between client and AP
- *Sequence Control*:
  Used to reassemble a fragmented message and identify retransmitted frames
- *Quality of Service (QoS) Control*:
  Used to prioritize certain traffic types
- *High Throughput (HT) Control*:
  Used to support higher data transfer rates of 802.11n, 802.11ac and later standard
- *Frame Body*:
  The message encapsulated inside of the frame
- *Frame Check Sequence (FSC)*:
  Allows the receiving device to check if the frame was corrupted in transit

Communicating via an AP over the air adds complexity that requires additional address fields which can be a combination of the following addresses:
- *Basic Service Set Identifier (BSSID)*:
  Identifies the AP
- *Destination address (DA)*:
  The final recipient of the frame
- *Source address (SA)*:
  The original sender of the frame
- *Receiver address (RA)*:
  The immediate recipient of the frame
- *Transmitter address (TA)*:
  The immediate sender of the frame

>[!example]
>In this example PC1 communicates with PC2 via the AP.
>![[802.11 Frame Format example.png]]

## Message Types
802.11 defines three main message types:
- **Management**: Used to establish communications in the wireless LAN
  - *Probe*
  - *Beacon*
  - *Authentication*
  - *Association*
  - *Disassociation*
- **Control**: Used to facilitate the delivery of frames over the medium
  - *Request-to-send (RTS)*
  - *Clear-to-send (CTS)*
  - *Acknowledgement (Ack)*

>[!note]
>RTS and CTS are used for asking and granting permission to transmit a frame, which helps to reduce collisions. 
>Modern networks usually do not use it as it adds additional overhead; for each data frame a RTS and CTS frame must be transmitted.
>![[RTS-CTS Example.png]]

- **Data**: Used to carry data payloads, typically IPv4/IPv6 packets
### Client Association Process
Before a client can send and receive traffic from an AP it must associate itself with the AP. And before the client can associate itself with the AP, the client has to authenticate itself. 

![[Client Association Process.png]]

For communication between the client and AP to beginn they have to know the other is within range.
There are two ways:
- *Active scanning*
 The client constantly sends probe requests and listens for probe responses from the AP
 - *Passive scanning*
   The client listens for beacon messages from the AP; the AP sends beacon messages periodically to advertise each BSS

# Service Sets
A *service set* is a group of devices that operate on the same WLAN, sharing the same *service set identifier (SSID)*, a human-readable label that identifies the service set.
There are four main types of service sets:
- *Independent basic service set (IBSS)*
  A peer-to-peer network where devices communicate directly 
- *Basic service set (BSS)*
  Devices communicate through a single access point
- *Extended service set (ESS)*
  Multiple linked BSSs
- *Mesh basic service set (MBSS)*
  A network of interconnected APs 

## Independent Basic Service Set (IBSS)
A IBSS, also called ad-hoc WLAN, consists of wireless devices communicating directly with each other. 
![[IBSS.png]]
>[!note]
>These are useful for quick and temporary communication setups, but do not scale beyond a few devices.

## Basic Serve Set (BSS)
In a BSS wireless clients connect to a *wireless access point (WAP or AP)*, which coordinates the communication between the devices and serves as the gateway to other networks.
![[BSS.png]]
>[!note]
>The SSID is a human-readable name and does not need to be unique, instead a unique *basic service set identifier (BSSID)* is used to uniquely identify the BSS.

### Multiple BSSs
A single AP is capable of providing more than one BSS, for example a home BSS and one for guests, applying different security policies.
However the BSS can only serve one channel, meaning that both BSS and the connected devices must use this one channel, which could lead to congestion.
![[Multiple BSSs.png]]
>[!note]
>Many modern APs have multiple radios/antennas, allowing them to serve on different channels, typically 2.4 GHz and 5 GHz.

### Distribution System (DS)
WLANs are not a standalone network, there is always a wired network infrastructure, which enables the clients to communicate with other BSSs, stations in wired the LAN, WAN, over the internet, etc.
![[BSS Distribution System.png]]

## Extended Service Set (ESS)
An *ESS* is used when the range of one AP is not enough to provide signal coverage of an entire site or building, which is often the case in enterprise LANs.
An ESS links multiple BSSs through a DS, while each ESS is identified by its own BSSID (MAC address of the AP), but all operate using the same SSID.
![[ESS.png]]

To the user, the ESS appears as a single wireless network, even when moving through the building. The client device will automatically switch to the nearest AP when the signal of the currently connected AP weakens.
>[!tip]
>To provide seamless roaming each AP's cell should overlap by 10% to 20%.
## Mesh Basic Service Set (MBSS)
In a *MBSS* multiple WAPs form a mesh that can relay data between the APs and the DS without each AP requiring a wired connection to the DS. 
![[MBSS.png]]

>[!note]
> An AP connected to the DS is called a *root access point (RAP)*
> An AP not connected to the DS is called a *mesh access point (MAP)*

>[!tip]
>Ideally, each AP should have two radios tuned to different channels, one dedicated for for clients and one dedicated to relay frames to other APs in the mesh.

## Additional AP Operational Modes
### Repeater
A repeater is an AP that extends the range of a wireless network by transmitting the signal of the main AP. Due the retransmitting of the signal the repeater reduces the networks *throughput*, the rate at which data can be transferred over the network, as the repeater occupies additional airtime for each packet.
![[Repeater.png]]

>[!attention]
>A repeater might resemble a MBSS, but they serve different [[Network topologies]] and function differently.
>- A repeater AP is a standalone device that extends the signal of a single AP without special configuration or infrastructure.
>- A mesh AP is part of a larger mesh network where each AP routes traffic through the best path withing the mesh.

### Workgroup Bridge
A workgroup bridge is connected to a device without wireless capabilities via an [[Ethernet]] cable  and allows it access to the WLAN.
![[Workgroup Bridge.png]]

### Outdoor Bridge
An outdoor bridge connects multiple remote sites with wireless APs using directional antennas. These are used in campus or metropolitan areas connecting buildings without physical cabling.
![[Outdoor bridge.png]]

# AP architecture
## Autonomous APs
![[Autonomous AP.png]]
*Autonomous APs* are self-contained units, that have the required hard- and software to handle all aspects of wireless networking. 
Each AP is standalone and must be individually configured and managed, which makes them useful in small networks or areas that do not need a large number of APs.
Autonomous APs are connected to the wired LAN via [[(VLAN) Virtual LAN#Trunks|trunk links]], as they support multiple SSIDs each mapped to a to an [[Ethernet]] [[(VLAN) Virtual LAN|VLAN]].
To configure an Autonomous AP you can use the CLI, Telnet, SSH or a web based GUI.
## Lightweight APs
![[Lightweight AP.png]]

*Lightweight APs (LWAP)* offload many of their more complex, non-real-time functions to a *wireless LAN controller (WLC)*, while the LWAP performs [[(OSI) Open Systems Interconnection-Modell#Media Access Control (MAC) Sublayer|media access control (MAC) operations]], this separation is called *split-MAC architecture*.
The WLC, which is used to centralize control and simplify operations, is responsible for management functions such as RF management (setting the channel and transmit power), client authentication and association, Quality of Service and Security. This  allows for *self-healing coverage*, if one LWAP stops working the WLC can increase the transmit power of a nearby LWAP to maintain the coverage area.
The LWAP handles real-time functions like transmitting and receiving RF signals, encryption of frames, sending beacon messages and responding to client's probe requests.

LWAPs are connected to the wired LAN via access ports. Because access ports only support a single VLAN, the ports are set to the [[(VLAN) Virtual LAN#Management VLAN|management VLAN]]. Translating between SSIDs and VLANs is offloaded to the WLC. The *Control and Provisioning of Wireless Access Points (CAPWAP)* protocol is used to establish tunnels through which LWAPs send frames from clients to the WLC, which translates them to Ethernet frames [[(VLAN) Virtual LAN#VLAN Tagging|tagged]] to the appropriate VLAN.
![[CAPWAP.png]]
>[!note]
>CAPWAP establishes two tunnels, a data tunnel, used to tunnel traffic to and from the clients, and a control tunnel, used for communication, configuration and management between the LWAP and the WLC.
>![[CAPWAP tunnels.png]]
>The data tunnel is by default not encrypted, but can be enabled.  
>CAPWAP uses *Datagram Transport Layer Security (DTLS)*, a TLS protocol that uses UDP instead of TCP.

### WLC Deployment
![[WLC Deployment.png|542x228]]

In a network with more than 10 APs it should be considered to use a WLC, but because there is a difference between a network with 30 APs and a network with thousands of APs, there are multiple WLC deployment options. 

- **Unified WLC**
  A *unified WLC* is a dedicated hardware appliance, that can support up to 6.000 LWAPs and 64.000 clients with a single WLC.
- **Cloud WLC**
  A *cloud WLC* is a virtual machine deployed on a server in a [[Cloud]]. Depending on the hardware resources available to the VM the WLC can support up to 6.000 LWAPs and 64.000 clients.
- **Embedded WLC**
  An *embedded WLC* is integrated into a switch, which support up to 200 APs and 4.000 clients.
- **Cisco Mobility Express**
  *Cisco Mobility Express* is integrated into an AP, supporting up to 100 LWAPs and 2.000

### Client-Serving LWAP Modes
**Local**
This is the default LWAP operational mode, offering BSSs for clients and tunneling the traffic through the CAPWAP to the WLC.

**FlexConnect**
![[FlexConnect.png|482x180]]

In this mode the LWAP can locally switch client traffic between the wired and wireless LANs, without tunneling the data to the WLC, resulting in reduced latency and conserving bandwidth. FlexConnect can be configured on a per-SSID basis and can locally switch traffic between enabled SSIDs and the wired LAN even when the connection to the WLC is lost.

**Bridge**
In this mode the LWAP can form a wireless mesh with other LWAPs, without every LWAP being directly connected to the wired network.

### Network Management LWAP Modes
These modes assist in network management tasks.
- **Monitor**
  The LWAP is used to analyse the RF environment and detect unauthorized "rogue" devices; it does not transmit data from its radio
- **Rogue Detector**
  The radios are disabled and it is dedicated to analyze traffic on the wired LAN for rogue devices
- **Sniffer**
  The LWAP captures all wireless traffic on a specific channel. The captured messages are then sent to the WLC and can then be redirected to tools like [[Wireshark]] for inspection
- **SE-Connect**
  The LWAP is dedicated to RF spectrum analysis on all channels and can send these to tools like Cisco Spectrum Expert for evaluation of the RF landscape.

## Cloud-based APs
*Cloud-based architecture* uses a cloud platform to manage the APs, it could be described as a public SaaS cloud service. 
Cloud-based APs use cloud computing to offer scalable, easy to manage wireless networks. 
### Cisco Meraki
![[cloud based AP.png|558x203]]

Cisco offers Cisco Meraki as their cloud-based APs.
The Meraki APs communicate through an encrypted tunnel over the internet with the Meraki cloud platform, sending RF spectrum information, client statistics and other information to the cloud. The APs can be managed with the Meraki dashboard, a browser based tool. User data is is not tunneld, but is send directly to the wired LAN. 

Cisco Meraki can be used with switches, firewalls, IoT devices, etc. 
### Cloud Managed Network Devices
Cloud-managed devices connect to a SaaS cloud service for configuration and management, simplifying tasks such as deploying new APs, configurations, monitoring and troubleshooting.  This is also the key characteristic of cloud-managed devices, instead of using a CLI you can use the GUI.
The benefits of this are:

**Rapid and simplified deployment**
Cloud management platforms use *zero-touch provisioning (ZTP)*. Newly connected devices automatically connect to Meraki and download their configuration without requiring manual configuration of each device via a console port.

**Simplified management**
Admins can apply configuration changes to all devices once through the cloud, spending less time on routine management tasks.

**Enhanced visibility and analytics**
Cloud platforms provide detailed analytics and reporting tools that can help with optimizing the network.

**Automated updates**
Network device firmware updates and security patches can be automatically deployed to all devices.

**Operational efficiency**
By simplifying the processes *operating expenses (OpEx)*, the costs of maintaining the network, are reduced. 

# Security
>[!note]
>Imagine you are in a room full of people and you want to have a private conversation with your friend at the other side of the room, but all you can do is shout at the top of your lungs.
>That how communication in a wireless LAN works.

## The CIA triad in wireless LANs

**Confidentiality**:  "Data and systems should only be accessible by authorized entities."
Wireless signals can be picked up by any receiver within range of the transmitter. 
Encryption is used to maintain confidentiality, turning unencrypted cleartext messages into unintelligible cyphertext.

**Integrity**: "Data and systems should not be altered during storage and transmission, except by authorized entities, and should always be complete."
Even when data is encrypted it is not guaranteed that the data is not altered. To ensure the integrity, the sender calculates a *checksum* of the message before sending it, the receiver will also calculate a checksum of the message, if both checksums are the same the message has not been altered.

**Availability**: "Data and systems should be accessible and usable by authorized entities when required."
To ensure the availability of a wireless LAN, devices should be maintained and replaced if faulty. To prevent against attacks physical security is key.

## Wired Equivalent Privacy (WEP)
The original 802.11 standard defined the *Wired Equivalent Privacy (WEP)* security protocol, which promised privacy to wireless communications equivalent to that of wired communication. It is now obsolete and a *legacy protocol*, as it has various vulnerabilities and is insecure.
 
### Open System and Shared Key Authentication
*Open System Authentication* and *Shared Key Authentication* are the two methods of authentication defined by WEP. 

Open System Authentication does not exchange credentials, a client simply sends an authentication request and the AP sends a authentication response unless the request itself is faulty (f.e. invalid formatting). This is done to authenticate that both devices are valid 802.11 devices.  
>[!note]
>OSA is used in the *Client Association Process*.

Shared Key Authentication uses a static WEP key (Wi-Fi password) on the AP, that each client has to know to authenticate itself. 
The process used for it is:
1. The client sends an authentication request
2. The AP sends a response and a unencrypted *challenge phrase*
3. The client uses the WEP key to encrypt the challenge phrase and sends the ciphertext back to the AP
4. The AP uses the WEP key to decrypt the challenge phrase and compares it to the original challenge phrase
5. If the decrypted and the original challenge phrase match the AP sends back a final authentication response indicating a successful authentication

![[Shared Key Authentication.png|609x217]]

>[!note]
>A static WEP key can be either 40 or 104 bits long. In addition the device generates a random 24 bit number called an *initialization vector (IV)* that is generated for each packet and combined with the WEP key.

### WEP Encryption and Integrity
WEP uses the *Rivest Cipher 4* encryption algorithm and the same key used in Shared Key Authentication  can be used to to encrypt data messages from and to the wireless clients. 
Integrity is ensured by appending a 32 bit checksum called the *integrity check value (ICV)* to each plaintext message before it is encrypted.

## Wi-Fi Protected Access (WPA)
![[WPA Genarations.png]]
WPA was developed by the Wi-Fi Alliance as an interim enhancement after the vulnerabilities of WEP were discovered, until the IEEE developed a more permanent solution, the 802.11i standard.
WPA3 is as of 2020 the latest WPA standard and offers:
- superior security protocols
- *Protected Management Frames (PMF)*
  ensures the authenticity and integrity of 802.11 management frames (authentication and association)
- *Forward Secrecy (FS)*
  also known as *Perfect Forward Secrecy (PFS)*; ensures that the encrypted data remains encrypted even if the PSK is compromised, meaning you cannot decrypt past messages with a PSK, as for every session a new encryption key is derived from the PSK


WPA defines two authentications methods:
- *WPA-Personal*: designed for small office / home office (SOHO) networks, it uses a pre shared key authentication
- *WPA-Enterprise*: designed for enterprise LANs, it uses individual credentials for useres or devices

### WPA-Personal
WPA-Personal uses a *pre-shared key (PSK)* either a static 256 bit string used to generate secure encryption keys or a 8 to 63 character passphrase (Wi-Fi Password) that is automatically converted into a 256 bit PSK.  

![[PSK four way handshake.png]]

>[!note]
>Note that the authentication occurs after the client has associated itself with the AP.

The *four-way handshake* is the process used by WPA and WPA2 for authentication, it is used to confirm that the client and the AP have the same PSK and to generate a unique encryption keys to encrypt and decrypt data sent to and from the client.  

>[!note]
>This method is more secure than WEP shared key authentication, but if an attacker manages to capture the four-way handshake the attacker could brute-force the PSK.

*Simultaneous Authentication of Equals (SAE)* is used by WPA3, which takes place before the association and allows the client and AP to verify that they have matching PSKs without the vulnerability of a brute-force attack to learn the PSK, even though SAE also uses a PSK.

![[SAE.png|597x246]]

### WPA-Enterprise
WPA-Enterprise authenticates each individual user or device through the IEEE 802.1X standard, which uses the *Extensible Authentication Protocol (EAP)* framework. EAP defines various *EAP methods*, some of them are:

- *Lightweight EAP (LEAP)*
The client authenticates with a username and password, the client and authentication server exchange challenge phrases. 
  >[!note]
  >LEAP was developed by Cisco to address WEP's vulnerabilities, but uses WEP encryption and is no longer secure.

- *EAP-Flexible Authentication vie Secure Tunneling (EAP-FAST)*
To authenticate the client with the authentication server a Transport Layer Security (TLS) tunnel using a Protected Access Credential (PAC), a shared secret, is established through which the credentials are exchanged.
>[!note]
>EAP-FAST was developed by Cisco to replace LEAP.

- *Protected EAP (PEAP)*
The authentication server has a digital cerificate that the client uses for authentication and to establish a TLS tunnel through which credentials are exchanged.

- *EAP-Transport Layer Security (EAP-TLS)*
The authentication server and the client each have a digital certificate that they use for authentication.

Although EAP defines these methods it does not define how these messages should be send over the network. This is done by 802.1X, which defines how to encapsulate EAP messages over the LAN, called *EAP over LAN (EAPoL)*.
EAPoL transports EAP messages between the devices roles: 
- *Supplicant*: Client device that wants to connect to the network
- *Authenticator*: The network device that the client connects to (AP)
- *Authentication server*: The server that verifies the supplicant's credentials 

![[EAPoL.png|552x222]]

>[!note]
>This set of protocols form the "802.1X/EAP protocol stack" used to enable authentication vie EAP.
>![[802.1X-EAP protocol stack.png|525x217]]

## Wireless Encryption and Integrity
### Temporal Key Integrity Protocol (TKIP)
*TKIP* was developed as an enhancement to WEP after the vulnerabilities were discovered, until new protocols and hardware to use these protocols was developed.
> [!NOTE]
> WEP's main flaw was that it used static encryption keys, all clients used the same key and it remained the same unless an admin manually configured a new passphrase.


The improvements TKIP offers are:
- *per-packet key mixing*:
  a new key is is generated for each packet making it harder to brute force the PSK
- *message integrity check (MIC)*
  a stronger integrity checksum
- adding a sequence number to each frame to mitigate replay attacks
>[!note]
>A *replay attack* is when an attacker captures valid messages and retransmits them later, gaining unauthorized access to a system.

### Counter Mode with Cipher Block Chaining Message Authentication Code Protocol (CCMP)
*CCMP* was defined in the 802.11i standard, which formed the basis for WPA2, providing message encryption and integrity.
It uses the *Advanced Encryption Standard (AES)*, specifically AES *counter mode* encryption, which uses a counter value that is incremented and encrypted with each block, ensuring that each block of data is encrypted with a unique key.
*Cipher Block Chaining Message Authentication Code (CBC-MAC)* a more robust data integrity checksum than TKIP's MIC.
### Galois/Counter Mode Protocol (GCMP)
*GCMP* is a used by WPA3 and offers improved security and efficiency, which is necessary for the faster data rates of the newer 802.11 standards.
It uses AES counter mode encryption with a key length of 128 or 256 bits and *Galois Message Authentication Code (GMAC)* for its data integrity checksum.
# Configuration on Cisco Devices
#Cisco_CLI 
