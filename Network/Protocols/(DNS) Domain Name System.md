*Domain Name System (DNS)* is responsible for mapping a domain name to an IP address. It operates at the [[(OSI) Open Systems Interconnection-Modell#7. Anwendungsschicht (Application, Layer 7)| Layer 7 (Application)]] of the OSI model and uses UDP port 53 and TCP port 53 as a fallback.
There are many different types of DNS records:
- **A record**: maps a hostname to one or more [[IPv4]] addresses, f.e. `example.com` to `172.17.2.172`
- **AAA record**: maps a hostname to one or more [[IPv6]] addresses
- **CNAME (Canonical Name) record**: maps a domain name to one or more other domain names, f.e. `www.example.com` to `example.com` or `example.org`
- **MX (Mail Exchange) record**: specifies the mail server responsible for handling emails for a domain.

This means that when you type `example.com` in your browser, it tries to resolve this domain name by querying the DNS server for the *A record* and when you try to send an email to `test@example.com`, the mail server queries the DNS server for the *MX record*.
# Nslookup
If you want to look up the IP address of a domain from the command line, you can use `nslookup`
```
nslookup google.com

Server:  unifi.localdomain
Address:  192.168.11.1

Nicht autorisierende Antwort:
Name:    google.com
Addresses:  2a00:1450:4001:81c::200e
          142.250.186.142
```