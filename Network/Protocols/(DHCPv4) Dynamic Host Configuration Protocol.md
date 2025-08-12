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
   When the client receives the DHCPOFFER from the server, it sends back a broadcast DHCPREQUEST, informing the server (and other DHCP server) about accepting the offer.
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
2. Define a DHCPv4 Pool Name
```
Router(config)# ip dhcp pool pool-name
Router(dhcp-config)#
```
3. Configure the DHCPv4 Pool
```
# Define the address pool.
network network-address [mask | / prefix-length]

# Define the default reouter or gateway
default-router address ([address2... .address8])

# Define DNS server
dns-server address ([address2...address8])

# Define the domain name
domain-name domain

# Define the duration of the DHCP lease
lease {days [hours [minutes]]| infinite}

# Define the NetBIOS WINS server
netbios-name-server address [address2...address8]
```

## Configuration Example
```
# Exclude IPv4 addresses
R1(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.9
R1(config)# ip dhcp excluded-address 192.168.10.254

# Definde a DHCPv4 Pool Name
R1(config)# ip dhcp pool LAN-POOL-1

# Define the address pool
R1(dhcp-config)# network 192.168.10.0 255.255.255.0

# Define the default router or gateway
R1(dhcp-config)# default-router 192.168.10.1

# Define the DNS server
R1(dhcp-config)# dns-server 192.168.11.5

# Define the domain name
R1(dhcp-config)# domain-name example.com

R1(dhcp-config)# end

R1#
```

## Verification Commands
-  Verify the DHCPv4 Configuration
```
R1# show running-config | section dhcp
```
- Verify DHCPv4 Bindings
```
R1# show ip dhcp binding
```
- Verify DHCPv4 Statistics
```
show ip dhcp server statistics
```
- Verify DHCPv4 Client Received IPv4 Addressing
```
C:\Users\User> ipconfig /all
```

## Disable the Cisco IOS DHCPv4 Server
The DHCPv4 service is enabled by default. To disable the service, use the `no service dhcp` command and to re-enable the server process use `service dhcp`.
```
R1(config)# no service dhcp
R1(config)# service dhcp
```

## DHCPv4 Relay
Usually clients are not in the same subnet as server, so they are unable to receive services like DHCP or DNS. The `ip helper-address` command allows these services to function even when in different subnets.

>[!note]
>The IP addresses of the Router serial interfaces that are attached to switch/router are used as the helper addresses. 

```
R1(config)# interface g0/0/0
R1(config-if)# ip helper-address 192.168.11.6
R1(config-if)# end
```

# Configure DHCPv4 Client
#Cisco_CLI 

To configure a router as a DHCPv4 client use the `ip address dhcp` command on the interface connected to the DHCP server.
- Configure an Ethernet interface as a DHCP client
```
SOHO(config)# interface G0/0/1
SOHO(config-if)# ip address dhcp
SOHO(config-if)# no shutdown
```
