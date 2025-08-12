The steps for DHCPv6 are:
1. The host sends an RS message 
2. The router responds with an RA message 
3. The host sends a DHCPv6 SOLICIT message
4. The host responds to the DHCPv6 server
5. The DHCPv6 server  sends a REPLY message

![[DHCPv6 steps.png]]
# Stateless DHCP Operation
The stateless DHCPv6 server is only providing information that is identical for all devices on the network, f.e. the IP of the DNS server.
>[!note]
>This process is known as stateless DHCPv6 because the server is not maintaining any client state information (i.e., a list of available and allocated IPv6 addresses).

## Enable Stateless DHCPv6 on an Interface
#Cisco_CLI 
Stateless DHCP is enabled on a router interface using the `ipv6 nd other-config-flag` 
```
R1(config-if)# ipv6 nd other-config-flag
```
# Stateful DHCPv6 Operation
The RA message tells the client to obtain all addressing information from a stateful DHCPv6 server, except the default gateway address which is the source IPv6 link-local address of the RA
>[!note]
>It is called *stateful* because the DHCPv6 server maintains IPv6 state information.
## Enable Stateful DHCPv6 on an Interface
#Cisco_CLI 

Stateless DHCPv6 is enabled on a router interface using the `ipv6 nd managed-config-flag` interface configuration command.
The `ipv6 nd prefix default no-autoconfig`
```
R1(config)# int g0/0/1
R1(config-if)# ipv6 nd managed-config-flag
R1(config-if)# ipv6 nd prefix default no-autoconfig
R1(config-if)# end
```
# Configure DHCPv6 
#Cisco_CLI 
## Configure a Stateless DHCPv6 Server
1. Enable IPv6 routing
   use the `ipv6 unicast-routing` command to enable IPv6 routing
```
R1(config)# ipv6 unicast-routing
```
2. Define a DHCPv6 pool name
   Use `ipv6 dhcp pool POOL-NAME` to create a pool name
```
R1(config)# ipv6 dhcp pool IPV6-STATELESS
```
3. Configure the DHCPv6 pool
   Configure additional information including DNS server address and domain name
```
R1(config-dhcpv6)# dns-server 2001:db8:acad:1::254
R1(config-dhcpv6)# domain-name example.com
```
4. Bind the DHCPv6 pool to an interface
   use `ipv6 dhcp server pool-name` 
   The `O` flag needs to be manually changed from `0` to `1` using `ipv6 nd other-config-flag`
```
R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# description Link to LAN
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64

-> R1(config-if)# ipv6 nd other-config-flag 
-> R1(config-if)# ipv6 dhcp server IPV6-STATELESS 

R1(config-if)# no shut
```

## Configure a Stateless DHCPv6 Client
1. Enable IPv6 routing
```
R3(config)# ipv6 unicast-routing
```
2.  Configure client router to create an LLA
```
R3(config)# interface g0/0/1
R3(config-if)# ipv6 enable
```
3. Configure client router to use SLAAC
```
R3(config-if)# ipv6 address autoconfig
```
4. Verify client router is assigned a GUA
```
R3# show ipv6 interface brief
```

## Configure a Stateful DHCPv6 Server
1. Enable IPv6 routing
```
R1(config)# ipv6 unicast-routing
```
2. Define a DHCPv6 pool name
   Use `ipv6 dhcp pool POOL-NAME` to create a pool name
```
R1(config)# ipv6 dhcp pool IPV6-Stateful
```
3. Configure the DHCPv6 pool
   stateful DHCPv6, all addressing and other configuration parameters must be assigned by the DHCPv6 server 
```
R1(config-dhcpv6)# address prefix 2001:db8:acad:1::/64
R1(config-dhcpv6)# dns-server 2001:4860:4860:8888
R1(config-dhcpv6)# domain-name example.com
```
4. Bind the pool to an interface
	- The M flag is manually changed from 0 to 1 using the interface command `ipv6 nd managed-config-flag`.
	- The A flag is manually changed from 1 to 0 using the interface command `ipv6 nd prefix default no-autoconfig`. The A flag can be left at 1, but some client operating systems such as Windows will create a GUA using SLAAC and get a GUA from the stateful DHCPv6 server. Setting the A flag to 0 tells the client not to use SLAAC to create a GUA.
	- The `ipv6 dhcp server` command binds the DHCPv6 pool to the interface. R1 will now respond with the information contained in the pool when it receives stateful DHCPv6 requests on this interface.
```
R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# description Link to LAN
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64

-> R1(config-if)# ipv6 nd managed-config-flag
-> R1(config-if)# ipv6 nd prefix default no-autoconfig
-> R1(config-if)# ipv6 dhcp server IPV6-STATEFUL

R1(config-if)# no shut
```

## Configure a Stateful DHCP6 Client
1. Enable IPv6 routing
```
R3(config)# ipv6 unicast-routing
```
2. Configure client router to create an LLA
   the `ipv6 enable` command is configured on the interface, so the router creates a LLA without needing a GUA
```
R3(config)# interface g0/0/1
R3(config-if)# ipv6 enable
```
3. Configure client router to use DHCPv6
   use the `ipv6 address dhcp` command so the router solicits IPv6 addressing information from a DHCPv6 server
```
R3(config-if)# ipv6 address dhcp
```
4. Verify client router is assigned a GUA
```
R3# show ipv6 interface brief
```
5. Step 5. Verify client router received other DHCPv6 information.
```
R3# show ipv6 dhcp interface g0/0/1
```
## DHCPv6 Server Verification Commands
- The` show ipv6 dhcp pool` command verifies the name of the DHCPv6 pool and its parameters
- Use the `show ipv6 dhcp binding` command output to display the IPv6 link-local address of the client and the global unicast address assigned by the server.
## Configure DHCPv6 Relay Agent
If the DHCPv6 server is located on a different network than the client, the router can be configured as a DHCPv6 relay agent.
The command to configure the router is: `ipv6 dhcp relay destination ipv6-address [interface-type interface-number]`
```
R1(config)# interface gigabitethernet 0/0/1
R1(config-if)# ipv6 dhcp relay destination 2001:db8:acad:1::2 G0/0/0
R1(config-if)# exit
```

### Verify the DHCPv6 Relay Agent
- This command will verify that the interface is in relay mode
 ![[Relay Agent show interfaces.png]]
- This command is used to verify if any hosts have been assigned an IPv6 configuration
  ![[Relay Agent show binding.png]]
- This command is used on the client PCs to verify that it has received an IPV6 configuration from the DHCPv6 server
  ![[Relay Agent ipconfig all.png]]