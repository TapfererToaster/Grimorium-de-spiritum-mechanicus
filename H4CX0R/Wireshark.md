Wireshark is an open-source, cross-platform network packet analyser tool capable of sniffing and invertigating live traffic and inspecting packet captures (PCAP).

# Merge PCAP Files
Wireshark can combine two pcap files into one. You can use the `File --> Merge` menu path

# Viewing File Details
To view the file details (hash, capture time, file comments, interface, statistics) follow `Statistics --> Capture File Properties` or click the pcap icon on the bottom left.

# Dissecting Packets
Wireshark decodes the available protocols and fields.
When you click on a packet its details will open in the bottom half of the screen.
The details consist of 5 to 7 layers based on the [[(OSI) Open Systems Interconnection-Modell|OSI]] model.
![[Packet Details.png]]
[[(OSI) Open Systems Interconnection-Modell#1. Physische Schicht (Physical, Layer 1)|Layer 1]]: The Frame
- details about the physical layer and what frame/packet you are looking at
![[packet details layer 1.png]]
[[(OSI) Open Systems Interconnection-Modell#2. Datensicherungsschicht (Data Link, Layer 2)|Layer 2]]: 
- shows you the source and destination MAC addresses
![[packet details layer 2.png]]
[[(OSI) Open Systems Interconnection-Modell#3. Netzwerk Schicht (Network, Layer 3)|Layer 3]]:
- shows source and destination IPv4 address
![[packet details layer 3.png]]
[[(OSI) Open Systems Interconnection-Modell#4. Transportschicht (Transport, Layer 4)|Layer 4]]:
- shows you details of the protocol (TCP/UDP) and the source and destination ports
![[packet details layer 4.png]]
- Wireshark also shows segments from TCP that had to be reassembled:
![[packet details protocol errors.png]][[(OSI) Open Systems Interconnection-Modell#5. Sitzungsschicht (Session, Layer 5)|Layer 5]]:
- shows details to the protocol being used ([[(HTTP-S) Hypertext Transfer Protocol - Secure|HTTP]], [[(FTP) File Transfer Protocol|FTP]], SMB, ...)
![[packet details layer 5.png]]
- Wireshark also shows the application specific data:
![[packet details application data.png]]
# Packet Navigation
## Go to Packet
Wireshark calculates the number of captured packets and assigns a number to each packet.
To go to a specific packet select `Go->Go to Packet` on the toolbar at the top of the screen.

## Find Packets
You can use `Edit->Find Packet` on the toolbar to search for packets based on their content or an event of interest.
Two crucial points about finding packets are:
- know the input type (Display filter, Hex, String and Regex)
  String and regex are the most common search types but are case sensitive
- choose the search field
  you can search in packet list, packet details and packet bytes

## Mark Packets
You can mark packets in `Edit` on the toolbar
## Comment Packets
You can comment packets under `Edit->Packet Comment`
## Export Packets
Sometimes you have to dig deeper into packets so you can export packets under `File-> Export Specific Packets...`
## Export Files
You can export files under `File->Export Objects`
This can be used when searching for downloaded files and needing to inspect them
## Change Time Display Format
By default Wireshark displays the time as "seconds since beginning of capture" to change it go to `View->Time Display Format`
## Expert Info
Wireshark detects specific states of protocols to help analysis.
Expert info provides a group of categories
![[Expert info categories.png]]To enable Expert info go to `Analyse->Expert Information`
# Packet Filtering
>[!note]
>Wireshark has a Capture filter for capturing only the packets you want and a Display filter for displaying only the packets you want
>
## Apply as Filter
Under `Analyse->Apply as Filter` you can filter the packets you want
## Conversation filter
When you want to investigate a packet and all linked packets you can go to `Analyse->Converation Filter` to display the packets of that conversation
>[!note]
>You can colourise the conversations under `View->Colorise Conversation` and reset it under `View->Colorise Conversation->Reset Colourisation` 

## Prepare as Filter
This lets you filter packets, but it adds the required query to the pane and waits for the execution command (enter) or another filter by using `... and/or ...`
## Apply as Column
You can add columns to the packet list that display specific values.
Go to `Analyse->Apply as Column`
## Follow Stream
You can display the raw traffic and reconstruct the stream to recreate application-level data, which could reveal unencrypted protocol data like usernames, passwords and other transferred data.
Go to `Analyse-> Follow TCP/UDP/HTTP Stream`
