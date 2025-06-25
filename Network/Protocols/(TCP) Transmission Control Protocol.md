This Protocol operates at the [[(OSI) Open Systems Interconnection-Modell#4. Transportschicht (Transport, Layer 4)|transport layer (Layer 4)]] of the OSI model and is used for reliable, connection-oriented communications. 
In order to be reliable it adds overhead und thus some inefficiencies.
TCP enables:
- Error recovery
- Flow control using windowing
- Connection establishment and termination
- Ordered data transfer
- Data segmentation

Applications that rely on TCP are:
- HTTP
- FTP
- Telnet
- SSH
- SMTP

# Protocols and their Ports
Protocols and services are identified by a port number, which are numeric identifier used to keep track of conversations between a client and server. Every message contains a source and destination port.
Clients are preconfigured to use a port that is registered for each service, which are assigned and managed by the *Internet Corporation for Assigned Names and Numbers (ICANN)*.
The ports are split into three categories:
- **Well-Known Ports**
  Destination ports that are associated with common network application; Ports 1-1023
- **Registered Ports**
  These ports can be used by organization to register specific applications; Ports 1024-49151
- **Private Ports**
  Often used as source ports and can be used by any application; Ports 49152-65535
>[!note]
>IP packets contain the source and destination ports as well as the source and destination IP addresses. 
>The combination of the source/ destination IP address and source/ destination port is known as a *socket*.

# Header
![[bilder/tcp/TCP Header.png]]

| Port | Protocol    |
| ---- | ----------- |
| 20   | FTP data    |
| 21   | FTP control |
| 22   | SSH         |
| 23   | Telnet      |
| 25   | SMTP        |
| 53   | DNS         |
| 80   | HTTP        |
| 110  | POP3        |
| 143  | IMAP        |
| 443  | HTTPS       |



