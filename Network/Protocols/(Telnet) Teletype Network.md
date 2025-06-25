# Connecting to ports
The *telnet* protocol is a network protocol for remote terminal connection, meaning we can use the `telnet` command to connect and communicate with remote systems that listens on TCP ports and issue text commands.
Examples for services that could be running and how to connect with them_
- **Echo server**: echoes everything you send; listens on port 7  
```
telnet ip-address 7
```
- **Daytime server**: replies with the current day and time; listens on port 13
```
telnet ip-address 13
```
- **Web (HTTP) server**: serves web pages; listens on port 80
```
telnet ip-address 80
...
GET / HTTP/1.1
Host: telnet.thm

...
```
>[!note]
>You need to issue the `GET / HTTP/1.1` command and identify the host such as `Host: tlenet.thm/anything` and press `Enter` twice.

## Connecting to Web Servers
>[!note]
>Web server listen on port 80 and serve web pages using [[(HTTP-S) Hypertext Transfer Protocol - Secure|HTTP(S)]].

To connect to a web server you use:
```
telnet ip-address 80
...
GET / HTTP/1.1
Host: telnet.thm

...
```
>[!info]
>You need to issue the `GET / HTTP/1.1` command and identify the host such as `Host: tlenet.thm/anything` and press `Enter` twice.
>
>`/` gets you the default page

To access a file/ web page:
```
telnet ip-address 80
...
GET /file.html HTTP/1.1
(Host: anything)
...
```
>[!info]
>On some servers, you might get the file without sending `Host: anything`
>It might also work to use `GET /file.html` depending on the server.

## Sending an email
```
telnet ip-address 25
...
HELO client.com
...
MAIL FROM: <user@client.com>
...
RCPT TO: <server@server.com
...
DATA
...
From: user@client.com
To: server@server.com
Subject: Telnet email

Hello. I am using telnet to send you this email.
.
...
QUIT
...
```

## Establish a POP3 session
```
telnet ip-address 110
...
AUTH
...
USER user
...
PASS password
...
STAT
...
LIST
...
RETR email_number
...
QUIT
```
