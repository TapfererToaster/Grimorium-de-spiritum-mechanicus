# Ping

>[!note]
>Ping is a component of the Internet Control Message Protocol (ICMP), which plays a supporting role for the Internet Protocol (IP).

Ping is a software utility that tests the reachability of hosts over a network and is a common diagnostic and troubleshooting tool. It can be used to f.e. to test whether two hosts can reach each other.
Ping can also be used to measure the *round-trip time (RTT)* between two hosts, which is the time it takes a message to travel from one host to another and back.

>[!definition]
>Ping consists of two messages:
>- *ICMP echo request*
>- *ICMP echo reply*
# Destination or Service Unreachable
Some of the Destination Unreachable codes for ICMPv4 are as follows:
- 0 - Net unreachable
- 1 - Host unreachable
- 2 - Protocol unreachable
- 3 - Port unreachable

Some of the Destination Unreachable codes for ICMPv6 are as follows:
- 0 - No route to destination
- 1 - Communication with the destination is administratively prohibited (e.g., firewall)
- 2 - Beyond scope of the source address
- 3 - Address unreachable
- 4 - Port unreachable