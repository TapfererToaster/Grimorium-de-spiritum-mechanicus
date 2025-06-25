The *File Transfer Protocol (FTP)* is designed to transfer files and listen on TCP port 21 by default.
Commands used by the FTP are:
- `USER`: input the username
- `PASS`: used to enter the password
- `RETR`: (retrieve)/download a file from the FTP server
- `STOR`: (store)/upload a file to the FTP server

# Connecting to the Server
```
ftp ip-address
```

After the connection was successful you are prompted to enter a username and a password.
>[!note]
>You can try to use the username `anonymous` and provide no password

When the username and password are accepted you could use `ls` to list the available files.
>[!note]
>When there are text files you can use `type ascii` to switch to ASCII mode and then use `get file.txt` to retrieve the file.
>
