# (SMTP) Simple Mail Transfer Protocol

> [!definition]
> The *Simple Mail Transfer Protocol (SMTP)* defines how a mail client talks with a mail server with another.
> It listens on TCP port 25.

Commands used by the email client to transfer an email to an SMTP server:
- `HELO`/ `EHLO`: initiates a SMTP session
- `MAIL FROM`: specifies the sender's email address
- `RCPT TO`: specifies the recipient's email address
- `DATA`: indicates that the client will begin sending the content of the email message
- `.`: sent on a line by itself to indicate the end of the email message

You can send an email via [[(Telnet) Teletype Network#Sending an email|Telnet]].

>[!note]
>The secure version of SMTP is SMTPS and uses TCP ports 465 and 587

# (POP3) Post Office Protocol version 3

> [!definition]
> The *Post Office Protocol version 3 (POP3)* is designed to allow clients to communicate with an email serve and retrieve email messages from it.
> It listens on TCP port 110

>[!note]
>Email clients rely on SMTP to **send** messages and rely POP3 to **retrieve** email messages from the server.

POP3 Commands are:
- `User username`: identifies the user
- `PASS password`: provides the user's password
- `STAT`: requests the number of messages and total size
- `LIST`: list all messages and their sizes
- `RETR message_number`: retrieves the specific message
- `DELE message_number`: marks a message for deletion
- `QUIT`: ends the POP3 session and applying changes (f.e. deletion)

You can establish a POP3 session over [[(Telnet) Teletype Network#Establish a POP3 session|telnet]] 
>[!note]
>The secure version of POP3 is POP3S and uses TCP port 995
# (IMAP) Internet Message Access Protocol

>[!note]
>IMAP uses the TCP port 143

This protocol is used to synchronize messages across multiple devices.
Commands used by IMAP are:
- `LOGIN <username> <password>` authenticates the user
- `SELECT <mailbox>` selects the mailbox folder to work with
- `FETCH <mail_number> <data_item_name>` Example `fetch 3 body[]` to fetch message number 3, header and body.
- `MOVE <sequence_set> <mailbox>` moves the specified messages to another mailbox
- `COPY <sequence_set> <data_item_name>` copies the specified messages to another mailbox
- `LOGOUT` logs out

>[!note]
>The secure version of IMAP is IMAPS and uses TCP port 993

