Nmap is a network scanner, which can be used to discover IP addresses, open ports and what services are running.
>[!important]
>You should use Nmap as `root` or with `sudo`, to not restrict its scanning abilities.
# Discovering
## Hosts
There are different ways to specify your target:
- IP range `-`:
  You can scan a range of IP addresses, f.e. 192.168.0.1-192.168.0.10
```
192.168.0.1-10
```
- Subnets `/`:
  You can scan a subnet, f.e. 192.169.0.0-255
```
192.168.0.1/24
```
- Hostname:
  You can also specify your target by hostname, f.e. `example.com`

>[!note]
>The `-sn` ping scan aims to discover live hosts on a network without attempting to discover the services running on them.

### On a Local Network
To scan a local network that we are already connected to we use:
```
nmap -sn 192.168.1.0/24
```
This command tells Nmap to scan all possible IP addresses (192.168.1.0-192.168.1.255) and report if the host is "up".
>[!note]
>Nmap sends an ARP request to all IP addresses and if a device response, Nmap labels it "Host is up".

>[!tip]
>As we are on a local network we can look up the MAC addresses, which can held to figure out what type of devices are up.
>Remember that the first half of a MAC addresses is a Vendor specific number, which you could look up.
>

### On a Remote Network
>[!note] 
>Remote means that at least one router is between our device and the target host.

We are connected to the `192.168.66.0/24` network and scan the target network `192.168.11.0/24`
```
nmap -sn 192.168.11.0/24
```
To discover which hosts are up Nmap will send two ICMP echo (ping) requests, two ICMP timestamp requests, two TCP packets to port 443 with the SYN flag set, and two TCP packets to port 80 with the ACK flag set.

## Services
>[!note]
>Web server usually listen on TCP ports 80 and 443
>DNS server usually listen on UDP and TCP port 53

### Scanning TCP Ports
#### Connect Scan (-sT)
The connect scan tries to complete a TCP three-way handshake with every target TCP port.
Will be used if Nmap is run without `sudo` privileges. 

#### SYN Scan (-sS)
Also called stealth scan, this scan only performs the first step of the three-way handshake, meaning it sends a TCP SYN packet, if the target port awnsers with an TCP SYN-ACK packet the port is marked as open and sends a TCP RST packet instead of finishing the handshake.

### Scanning UDP Ports
Many services use UDP instead of TCP, f.e. DNS, DHCP, NTP.
To scan for UDP services you can use `-sU`.

### Limiting Target Ports
Nmap scans the most common 1.000 ports by default, but there are other options:
- `-F`: scans the 100 most common ports
- `-p[range]`: sets the range, f.e. `-p10-1024`, `-p-25` (= p1-25), `-p-` (= p1-65535, scans all ports)

## Version Detection
### OS Detection
To enable OS detection use the `-O` option.
```
root@tryhackme:~# nmap -sS -O 192.168.124.211
```
### Service and Version Detection
After discovering several open ports you can use `-sv` to en able version detection.
```
root@tryhackme:~# nmap -sS -sV 192.168.124.211
```
### Forcing the Scan
After we run a port scan some host do not reply to the ICMP request, to force NMap to treat the host as online and scan the ports you can add `-Pn`
```

```

## Timeing
Nmap gives you the ability to control the time it takes to complete a scan.
### Templates
It offers six templates:
- *T0*: paranoid (9.8 hours to scan the 100 most common ports)
- *T1*: sneaky (27.53 minutes)
- *T2*: polite (20.56 seconds)
- *T3*: normal (0.15 seconds)
- *T4*: aggressive (0.13 seconds)

>[!note]
>To scan the 100 most common ports use `-F`
>```
>nmap -sS 192.168.0.1 -F
>```

You can either add the number `-T0` or the name `-T paranoid`
```
nmap -sS 192.168.0.1 -T0
nmap -sS 192.168.0.1 -T paranoid
```
### Parallel Service probes
Ýou can set the minimum and maximum number of simultaneously active UDP and TCP port probes with:
- `--min-parallelism number`
- `--max-paralellelism number`

By default, nmap controlls the number of probes automatically, meaning if the network performs good/bad the number of probes is increased/ decreased.

### Packet rates
To control the rate at which nmap sends packets you can use:
- `--min-rate number`
- `--max-rate number`

>[!important]
>The rate is measured as numbers per second.

## Output
To get more information about the scan you can use `-v` to enable verbose output. You can increase the verbosity by adding `v` to a maximum of 4 `v`: 
- `-v4` or `-vvvv`

You can enable debugging-level output with `-d` for even more information. Like above you can add more `d` to increase the information output, with the maximum being 9 `d`:
- `-d9`

## Saving Scan Reports
To save the output of a scan you can use:
- `-oN filename`: normal output
- `-oX filename`: XML output
- `-oG filename`: `grep`-able output
- `-oA basename`: output in all major forms

```
nmap -sS 192.168.0.1 -oN scanFile
```
## Summary
![[Nmap scan summery.png]]
- *TCP FIN Scan (-sF)*:
  SYN scans can be noticed by a firewall, that is when you should use a FIN scan
  `-sF -p ports ip-address`
- *-sC*
  With this command runs the most common NSE scripts based on the open ports with which you can get information about the system, f.e. what OS is running on the target
  `nmap -sC ip-address`
# Enummerating SMB

## User enumeration
```
nmap --script smb-enum-users.nse ip-address
```
## Group Enumeration
```
nmap --script smb-enum-group-nse -p445 host
```
- You can search what groups a user is part of 
```
# nmap --script smb-enum-groups.nse --script-args smbusername=vagrant,smbpass=vagrant 192.168.56.3
```
## Network Share Enumeration
Identify systems that share files, folders, printers and more. The `smb-enum-shares` NSE script uses Microsoft Remote Procedure Call (MSRPC).
```
nmap --script smb-enum-shares.nse -p 445 host-ip

# nmap --script smb-enum-shares.nse -p 445 192.168.88.251
```
