 UDP is a simple connectionless protocol that operates at the transport layer, i.e., layer 4. Being connectionless means that it does not need to establish a connection. UDP does not even provide a mechanism to know that the packet has been delivered.

# Header 
![[UDP Header.png]]
# Protocols and their Ports
- **Live video and multimedia applications** - These applications can tolerate some data loss, but require little or no delay. Examples include VoIP and live streaming video.
- **Simple request and reply applications** - Applications with simple transactions where a host sends a request and may or may not receive a reply. Examples include DNS and DHCP.
- **Applications that handle reliability themselves** - Unidirectional communications where flow control, error detection, acknowledgments, and error recovery is not required, or can be handled by the application. Examples include SNMP and TFTP.

| Port      | Protocol        |
| --------- | --------------- |
| 53        | DNS             |
| 67 and 68 | DHCP            |
| 69        | TFTP            |
| 80        | HTTP            |
| 110       | POP3            |
| 161       | SNMP            |
| 443       | SSL/TLS (HTTPS) |
| 514       | Syslog          |
| 520       | RIP             |
