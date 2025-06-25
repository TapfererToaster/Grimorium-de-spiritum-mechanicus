>[!note]
>You can run `tcpdump` without any arguments, but this is only useful to check if you have it installed.

>[!important]
>You must be logged in as `root` or use `sudo` to capture packets.

# Commands
## Specify the Network Interface
To specify on which network interface you want to listen use `-i interface`:
- `-i any`: listen on all available interfaces
- `-i eth0`: listen on the Ethernet interface

To display the available interfaces use `ip address show` or `ip a s`
## Save the Captured Packets
To save the captured packets use `-w File`. The file extension is commonly `.pcap`. 
>[!tip]
>After the packets are saved you could inspect them with tool like [[Wireshark]].
## Read Captured Packets from a File
To read the packets from a file use `-r File`.
## Limit the Number of Captured Packets
To specify the number of packets to capture use `-c Count`, otherwise Tcpdump will continue to capture packets until you interrupt it.  
## Resolving IP Addresses and Port Numbers
When possible Tcpdump will resolve IP addresses with Domain Names. To prevent this use `-n`.
To prevent port numbers to be resolved, f.e. port `80` to `http`, use `-nn`
to stop DNS and port number lookups.
## Verbose Output
For more details use `-v`, `-vv` or `-vvv`. Check the manual page for more information.
## Summary
![[Basic Commands.png]]
# Filtering
## By Host
If you want to capture the packet exchange of specific host, f.e. network printer or server, you can use `host IP` or `host Hostname`.
You can further specify if you only want packets from a host with `src host IP/Hostname` or send to a host with `dst host IP/Hostname`.
## Logical Operators
You can use logical operators to combine multiple filters
- *and*: f.e. `tcpdump host 1.1.1.1 and tcp`
- *or*: f.e. `tcpdump udp or icmp`
- *not*: f.e. `tcpdump not tcp`
## Summary
![[Basic Filter Commands.png]]
>[!tip]
>Wenn du nach bestimmten Protokollen suchst denk daran auch nach den ports zu filtern
# Advanced Filtering
## Binary Operations
Binary operators are:
- `&` and
- `|` or
- `!` not
## Header Bytes
You can also filter packets based on the contents of a header byte. 
With `proto[expr:size]` you can refer to the contents of any byte in the header.
- `proto`: refers to the protocol 
  - `arp`: [[(ARP) Address Resolution Protocol|ARP]]
  - `ether`: [[Ethernet]]
  - `icmp`: icmp
  - `ip`: [[IPv4]]
  - `ip6`: [[IPv6]]
  - `tcp`: [[(TCP) Transmission Control Protocol|TCP]]
  - `udp`: [[(UDP) User Datagram Protocol|UDP]]
- `expr`: indicates the byte offset (0 refers to the first byte)
- `size`: indicates the number of bytes (one is the default)

**Examples**
- `ether[0] & 1 != 0` takes the first byte in the Ethernet header and the decimal number 1 (i.e., 0000 0001 in binary) and applies the `&` (the And binary operation). It will return true if the result is not equal to the number 0 (i.e., 0000 0000). The purpose of this filter is to show packets sent to a multicast. A multicast Ethernet address is a particular address that identifies a group of devices intended to receive the same data.
- `ip[0] & 0xf != 5` takes the first byte in the IP header and compares it with the hexadecimal number F (i.e., 0000 1111 in binary). It will return true if the result is not equal to the (decimal) number 5 (i.e., 0000 0101 in binary). The purpose of this filter is to catch all IP packets with options.

## TCP flags
You can also refer to TCP flag field with `tcp[flags]`:
- `tcp-syn`: TCP SYN (Synchronize)
- `tcp-ack`: TCP ACK (Acknowledge)
- `tcp-fin`: TCP FIN (Finish)
- `tcp-rst`: TCP RST (Reset)
- `tcp-push`: TCP Push

**Example**
- `tcpdump "tcp[tcpflags] == tcp-syn"` to capture TCP packets with only the SYN (Synchronize) flag set, while all the other flags are unset.
- `tcpdump "tcp[tcpflags] & tcp-syn != 0"` to capture TCP packets with at least the SYN (Synchronize) flag set.
- `tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"` to capture TCP packets with at least the SYN (Synchronize) or ACK (Acknowledge) flags set
# Displaying Packets
You have the following options:
- `-q`: quick output
- `-e`: print the link-level header
- `-A` : Show packet data in ASCII
- `-xx`: Show packet data in hexadecimal format
- `-X`: Show packet header and data in hex and ASCII
## Link-Level Header
If you want to include the MAC address on Ethernet or WiFi network you can use `-e`