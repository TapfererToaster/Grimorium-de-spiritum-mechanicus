# Static and Dynamic [[IPv4]] Addressing
With a static address assignment a network administrator manually configures the network information needed for communication.
The minimum needed is:
- **IP address**
  Identifies the host on the network
- **Subnet mask**
  Identifies the the network portion of the IPv4 address
- **Default gateway** 
  Identifies the networking device that allows the host to access the internet or anther remote network

Because static addresses do not change, static assignment is useful for printers, servers and other devices that are accessed by clients at a particular IPv4 address.

The other option is the use of the DHCPv4, which automatically assigns new hosts an address until it disconnects from the network and the address is returned to the pool for others to use.

## DHCP Server
When you enter a location with a wireless hotspot or access point, the DHCP client of your device contacts the local DHCP server.
In home networks and small businesses wireless router are used and is both DHCP client and server. The router receives the public IPv4 address and configuration from the ISP and distributes addresses to internal hosts.