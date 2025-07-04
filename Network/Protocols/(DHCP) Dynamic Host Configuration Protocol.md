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

# DHCP Server and Client
A DHCP server dynamically assigns, or leases, an IPv4 address from a pool of addresses for a limited period of time or until the client no longer needs the address (or disconnects).

A client leases the information from the server for an administratively defined period of time, after which the client has to request a new one.

When you enter a location with a wireless hotspot or access point, the DHCP client of your device contacts the local DHCP server.
In home networks and small businesses wireless router are used and is both DHCP client and server. The router receives the public IPv4 address and configuration from the ISP and distributes addresses to internal hosts.

# Operations
1. **DHCP Discover (DHCPDISCOVER)**
   The client broadcasts DHCPDISCOVER messages with its own MAC to discover available DHCPv4 servers.
2. **DHCP Offer (DHCPOFFER)**
   When the DHCPv4 server receives a DHCPDISCOVER message, it reserves an available IPv4 address to lease to the client. The server also creates an ARP entry consisting of the MAC address of the requesting client and the leased IPv4 address of the client. The DHCPv4 server sends the DHCPOFFER message to the requesting client.
3. **DHCP Request (DHCPREQUEST)**
   When the client receives the DHCPOFFER from the server, it sends back a DHCPREQUEST, informing the server (and other DHCP server) about accepting the offer.
4. **DHCP Acknowledgement (DHCPACK)**
   When receiving the DHCPREQUEST the server may verify the lease information with an ICMP ping to the assigned address and create a new ARP entry.

## Steps to Obtain a Lease
1. DHCP Discover (DHCPDISCOVER)
2. DHCP Offer (DHCPOFFER)
3. DHCP Request (DHCPREQUEST)
4. DHCP Acknowledgment (DHCPACK)

## Steps to Renew a Lease
1. DHCP Request (DHCPREQUEST)
2. DHCP Acknowledgement (DHCPACK)

# Configure DHCPv4 Server
#Cisco_CLI 
1. Exclude IPv4 Addresses
```
Router(config)# ip dhcp excluded-address low-address [high-address]
```