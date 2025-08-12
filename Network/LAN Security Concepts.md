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

# VLAN Hopping