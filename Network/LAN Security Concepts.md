#CCNA 
# Endpoint Security
![[Endpoint Security.png]]
- *VPN-enabled Router*
- *Nex-generation Firewall*: provides stateful packet inspection, application visibility and control, *next generation intrusion prevention system (NGIPS)*, *advanced malware protection (AMP)* and URL filtering
- *Network Access Control*: devices include authentication, authorization and accounting (AAA) services
- *Email Security Appliance (ESA)*:  monitors *Simple Mail Transfer Protocol (SMTP)* traffic
	- Block unknown threats
	- Remediate against stealth malware that evaded initial detection
	- Discard emails with bad links
	- Block access to newly infected sites
	- Encrypt content in outgoing email to prevent data loss
- *Web Security Appliance (WSA)*: Complete control of how users or applications access the internet
	- URL blacklisting
	- URL filtering
	- malware scanning
	- URL categorization
	- Web application filtering
	- de-/encrypting web traffic
# Access Control
## Authentication
>[!note]
>Authentication: Who you are
### Local Authentication
Local authentication stores usernames and passwords in a local database on a network device, such as a router. 
### Server-Based Authentication
The usernames and passwords are stored on a AAA server and the router uses *Remote Authentication Dial-in User Service (RADIUS)* or *Terminal Access Controller Access Control System (TACACS+)* to communicate with the server and authenticate the entered username and passwords.

## Authorization
>[!note]
>Authorization: What you can do

## Accounting
>[!note]
>Accounting: What you did

# IEEE 802.1X
This standard is a port-based access control and authentication protocol that denies unauthorized workstations from connecting to a LAN through a publicly accessible switch port. Before the client has access to the LAN a authentication server must first authenticate the client.
![[IEEE 802.1X.png]]
- *Supplicant* (Client): a device running IEEE 802.1X-compliant client software
- *Authenticator* (Switch): requests identifying information from the client, relays that information to the authentication server and sends it's response back to the client; a wireless accesspoint can also act as an Authenticator
- *Authentication Server*: validates the identity of the client and notifies the Authenticator to grant or deny LAN access to the client

# Layer 2 / Switch Vulnerabilities
>[!note]
>- Always use secure variants of these protocols such as SSH, Secure Copy Protocol (SCP), Secure FTP (SFTP), and Secure Socket Layer/Transport Layer Security (SSL/TLS).
>- Consider using out-of-band management network to manage devices.
>- Use a dedicated management VLAN where nothing but management traffic resides.
>- Use ACLs to filter unwanted access.
## Switch Attacks
- *MAC Table Attacks*: MAC address flooding
- *VLAN Attacks*: VLAN hopping, VLAN double-tagging
- *DHCP Attacks*: DHCP starvation, DHCP spoofing
- *ARP Attacks*: ARP spoofing, ARP poisoning
- *Address Spoofing*: MAC and IP address spoofing
- *STP Attacks*: STP manipulation
## Switch Attack Mitigation
- *Port Security*: prevents MAC address flooding, DHCP starvation
- *DHCP Snooping*: prevents DHCP spoofing, DHCP starvation
- *Dynamic ARP Inspection (DAI)*: prevents ARP spoofing, ARP poisoning
- *IP Source Guard (IPSG)*: prevents MAC and IP address spoofing

## MAC Address Table Attacks
- MAC flooding: use a tool like [macof](https://www.kali.org/tools/dsniff/#macof) to generate fake MAC addresses and flood the switch until the MAC table "overflows", all traffic will be broadcasted allowing an attacker to capture all frames sent on the LAN/VLAN
## VLAN
### VLAN Hopping
This attack enables the attacker to see traffic from other VLANs without a router, by configuring a host to spoof 802.1Q and DTP signaling to trunk with a connected switch.

### VLAN Double-Tagging
In this attack a 802.1Q tag is hidden inside a frame that already has an 802.1Q tag. This allows the frame to be sent to a VLAN that the original tag did not specify.
1. The attacker sends a frame to the switch which has a tag to the native VLAN and a inner tag for the victims VLAN
2. The switch receives the frame and inspects the first tag and forwards the frame out of all native VLAN ports after stripping the tag. 
3. When the frame is received on another switch it again checks the tag, which is addressed to the victims VLAN and forwards the frame to the victim or floods the VLAN.

### VLAN Attack Mitigation
- Disabling trunking on all access ports
- Disable auto trunking on trunk links so that trunks must be manually enabled
- Be sure that the native VLAN is only used for trunk links

## DHCP Attacks
### DHCP Starvation
In this attack a attacker uses tools like [Gobbler](https://sourceforge.net/projects/gobbler/) to try to lease the entire IP pool with fake MAC addresses. This creates a DoS for connecting clients as they are unable to receive a IP address.
### DHCP Spoofing
This attack is done by connecting a rouge DHCP server to a network, which provides false IP configuration parameters to connecting clients:
- default gateway: the default gateway given to the host can be used for a man-in-the-middle attack
- DNS server: the rouge DNS server point the clients to malicious websites
- IP: provides a fake IP address creating a DoS attack on the client

## Address Spoofing
An attacker can spoof the IP and MAC address of his computer
## STP Attack
Attackers can manipulate the STP to disrupt the network or make their system appear as a root bridge.

## Cisco Discovery Protocol (CDP)
This protocol is a Layer 2 link discovery protocol, which is used for auto-configuration of connections and troubleshoot networks.
The information sent can also be used by an attacker as the messages are broadcasted unencrypted and unauthenticated.