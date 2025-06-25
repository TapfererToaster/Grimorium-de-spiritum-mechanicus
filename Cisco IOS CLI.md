#CCNA
#Cisco_CLI 

The *Cisco Command Line Interface* is a text-based interface that allows you to configure Cisco [[Router#Router Configuration|Router]] and [[Switches]].

# Accessing the CLI
To access the CLI you can:
- connect your PC to the Console port (USB/ RJ45 [[Cables, Connectors and Ports#Rollover|Rollover cable]])
- connect your device over the network with Telnet or SSH

## Terminal emulator
After you connected your PC with the console port, you will need to use a *terminal emulator*. 

>[!tip]
>You can use [PuTTY](http://www.putty.org/) 

When using a terminal emulator you have to configure a few settings:
- *Speed*:
  The rate at which data is sent
- *Data bits*:
  The number of bits of information used for each character of text sent to the device
- *Stop bits*:
  Sent after every character to allow the receiving device to detect the end of the character
- *Parity*:
  An extra bit sent with each character to be used for error detection
- *Flow control*:
  Provides support for circumstances where a device sends data faster than the receiver can handle

>[!note]
>You have to check the manufacturer's settings for the device you are configurating

> [!example]
> The settings for PuTTY, when configuring a Cisco device are:
> - Speed: 9600 bits per second
> - Data bits: 8
> - Stop bits: 1
> - Parity: None
> - Flow Control: None
> 

# Navigating
>[!attention] 
>Networking is not just theory, it is also a practical skill, which you have to practice.

> [!tldr]
> ![[Cisco EXEC global config mode navigation.png]]

## EXEC modes
>[!note]
>The EXEC mode is indicated by the hostname followed by a greater-than sign: `Router>`
>
>It can also be referred to as *"view-only" mode*.

The *user EXEC* mode is the least privileged mode in the IOS. In it you are only allowed to enter basic commands to view information about the device configuration and status, but you can not change the device's configuration. 
For example you could display the current time of the device with:
```
show clock
```

The *privileged EXEC mode* allows you to use all `show` commands as well as execute operational commands like restart the device, save the configuration, move and delete files, etc. and other commands to control various features of the device.

> [!important]
> To access the privileged EXEC mode use the command `enable`
> ```
> Router> enable
> Router#
> ```
> 
>To return to the user EXEC use the `disble` command.
>```
>Router# disable
>Router>
>```

>[!note]
>The privileged EXEC mode is indicated by the hostname followed by a hashtag sign: `Router#`
>
>The privileged EXEC mode can also be called *enable mode*.

Both EXEC modes are limited in that they do not allow you to make changes to the device's status and configuration.
## Global configuration mode

> [!important]
> To access the global configuration mode use the command `configure terminal` from privileged EXEC mode.
> ```
> Router# configure terminal
> Router(config)#
> ```
> 
> To return to the privileged EXEC mode you can use:
> - `end`
> - `CTRL-C`
> - `CTRL-Z`
> - `exit`

>[!note]
>The global configuration mode is indicated by the hostname followed by `(config)#`: `Router(config)#`

The *global configuration mode* allows us to make changes to a device's configuration like the hostname and passwords, but also to access the other configuration modes.

>[!important]
>The `do` command lets you execute EXEC commands like `show`, which do not work in the configuration mode.
>```
>Router(config)# do show clock
>```

## Keyboard Shortcuts

![[Shortcuts.png]]
![[Exit Shortcuts.png]]
## Context-sensitive help features

![[Cisco IOS context-sensitive help.png]]
# Commands
To find all all information about Cisco IOS commands use the [Cisco IOS Command Reference](https://www.cisco.com/c/en/us/td/docs/ios/fundamentals/command/reference/cf_book.html).
You can also type `?` to get a list of commands 
# IOS Configuration Files
**Running-config**
- The current operations of the device are stored here and when you modify this file the changes take instant effect.
- The running-config file is stored in the RAM and is lost when the device is powered off.
- To store the configuration you must store it in the *startup-config* file, otherwise the device will have a *factory-default* configuration every time it boots up

>[!note]
>*Factory-default* refers to the original state of the device as it sent from the factory.

**Startup-config**
- This file contains the configuration that is loaded by the device when it boots-up.
- The startup-config file is stored in *nonvolatile* RAM

To store the current config you can use the commands:
- `write`
- `write memory`
- `copy runnin-config startup-config`

To return the device to factory-default use the commands:
- `write erase`
- `erase nvram:`
- `erase startup-config`
# Password-protecting 
## Configure the enable password
The *enable password* is needed when you want to enter the privileged EXEC mode.
It is a legacy feature and is replaced by `enable secret`, but is still supported in Cisco IOS.
To configure the enable password use the command `enable password`:
```
Router(config)# enable password ccna
Router(config)# exit
```
>[!warning]
>The enable password is stored in cleartext, so you need to use the command `service password-encryption` to store the password in ciphertext (encrypted text).
>The password will be encrypted using *type 7* encryption, a very weak form of encryption.
## Configuring the enable secret
The *enable secret* is a more secure command and is used to protect access to privileged EXEC mode.
The password is stored as a hash.
>[!note]
>The hashing algorithm depends on the IOS version of the device.

To configure the enable secret use the command `enable secret`
```
Router(config)# confienable secret cisco
```

## Configure vty password
To configure the password for the vty you start in global configuration mode:
```
Router(config)# line vty 0 15
Router(config)# password cisco
Router(config)# login
Router(config)# exit
```

# MAC Address Table
To view the [[Switches#MAC Address learning| MAC address table]] of a Cisco Switch you can use the command:
```
SW1# show mac address-table
```

>[!note]
>You must be in user EXEC or privileged EXEC mode.

After entering the command the following will be displayed:
![[MAC address table.png]]
The `type` column indicates whether the MAC address was learned dynamically or statically and the `Vlan` column indicates in which virtual LAN the MAC address was learned in.

You can also:
- delete an dynamic entry with: 
  `clear mac address-table dynamic address MacAddress` 
- Clear all dynamic MAC addresses learned on a specific interface:
  `clear mac address-table dynamic interface interfaceName`

# Ping
To test connectivity you can use the [[(ICMP) Internet Control Message Protocol#Ping|ping]] command:
```
PC1# ping 10.0.0.12
```

![[Ping command.png]]

>[!note]
> The five exclamation marks (!!!!!) indicate that the ping was successful, if a request did not receive a reply a period (.) will be displayed, f.e. `.!!!!`

# Filter show Command 
- **`section`**
  Shows the entire section that starts with the filtering expression
```
R1# show running-config | section line vty
line vty 0 4
 password 7 110A1016141D
 login
 transport input all
```
- **`include`**
  Includes all output that match the filtering expression
```
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0/0   192.168.10.1    YES manual up                    up
GigabitEthernet0/0/1   192.168.11.1    YES manual up                    up
Serial0/1/0            209.165.200.225 YES manual up                    up
Serial0/1/1            unassigned      NO  unset  down                  down
R1#
R1# show ip interface brief | include up
GigabitEthernet0/0/0   192.168.10.1    YES manual up                    up
GigabitEthernet0/0/1   192.168.11.1    YES manual up                    up
Serial0/1/0            209.165.200.225 YES manual up                    up
```
- **`exclude`**
  Excludes all output lines that match the filtering expression
```
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0/0   192.168.10.1    YES manual up                    up
GigabitEthernet0/0/1   192.168.11.1    YES manual up                    up
Serial0/1/0            209.165.200.225 YES manual up                    up
Serial0/1/1            unassigned      NO  unset  down                  down
R1#
R1# show ip interface brief | exclude unassigned
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0/0   192.168.10.1    YES manual up                    up
GigabitEthernet0/0/1   192.168.11.1    YES manual up                    up
Serial0/1/0            209.165.200.225 YES manual up                    up
```
- **`begin`**
  Shows all output lines from a certain point, starting with the line that matches the filtering expression
```
R1# show ip route | begin Gateway
Gateway of last resort is not set
      192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.10.0/24 is directly connected, GigabitEthernet0/0/0
L        192.168.10.1/32 is directly connected, GigabitEthernet0/0/0
      192.168.11.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.11.0/24 is directly connected, GigabitEthernet0/0/1
L        192.168.11.1/32 is directly connected, GigabitEthernet0/0/1
      209.165.200.0/24 is variably subnetted, 2 subnets, 2 masks
C        209.165.200.224/30 is directly connected, Serial0/1/0
L        209.165.200.225/32 is directly connected, Serial0/1/0
```

>[!note]
>Output filters can be used in combination with any `show` command.
# Command History Feature
- To recall commands in the history buffer, press **Ctrl**+**P** or the **Up Arrow** key.
- To return to more recent commands in the history buffer, press **Ctrl**+**N** or the **Down Arrow** key.
- Use the **show history** privileged EXEC command to display the contents of the buffer.
- Use the **terminal history size** user EXEC command to increase or decrease the size of the buffer
```
R1# terminal history size 200
R1# show history
  show ip int brief
  show interface g0/0/0
  show ip route
  show running-config
  show history
  terminal history size 200
```


# Interface errors
Various counters are used to keep track of errors encountered when sending or receiving data. If an error occures the specific counter is incremented.
You can view these in the `show interface` command:
```
SW1# show interfaces f0/1 
. . .
 164850273 packets input, 138587749740 bytes, 0 no buffer 
 Received 606 broadcasts (0 multicasts) 
 0 runts, 0 giants, 0 throttles 
 0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored 
 0 watchdog, 0 multicast, 0 pause input 
 0 input packets with dribble condition detected 
 165209751 packets output, 180164587250 bytes, 0 underruns 
 0 output errors, 0 collisions, 0 interface resets 
 0 unknown protocol drops 
 0 babbles, 0 late collision, 0 deferred 
 0 lost carrier, 0 no carrier, 0 pause output 
 0 output buffer failures, 0 output buffers swapped out
```

Some errors that are important for CCNA:
- **Runts**:
  received frames that are smaller than the minimum frame size (64 bytes)
  can be caused by collisions
  >[!info]
  >When hosts want to send a frame that is smaller than 64 bytes, it is supposed to add padding (bytes of all 0)
  
- **Giants**:
  received frames with a payload greater than the interface´s MTU (maximum transmission unit), which is typically 1,500 bytes
  can be a sign of misconfiguration
- **Input errors**:
  counter for all errors of received frames
- **Cyclic Redundancy Check (CRC)**:
  counts frames that failed the FCS (Frame Check Sequence) 
  can be caused by EMI (electromagnetic interference)
- **Output errors**:
  counter for all errors of transmitted frames
- **Collisions**:
  counter for all collisions that happened when the device is transmitting a frame
  >[!note]
  >In a switched LAN collisions should not occur.

 - **Late Collisions**:
   counter for collisions that happened after the 64th byte of the frame has been transmitted.
   >[!note]
   >Can be a sign that the other device is not using CSMA/CD, which could indicate a duplex mismatch.
   
## Speed mismatches
Speed mismatches occur when the devices are using different speeds (f.e. 1000 and 100). The two interfaces will be not operational.

![[Speed Mismatch.png]]
```
R1# show ip interface brief
Interface IP-Address OK? Method Status Protocol
GigabitEthernet0/1 192.168.1.1 YES NVRAM down down 
. . . 
SW1# show ip interface brief
Interface IP-Address OK? Method Status Protocol
FastEthernet0/1 Unassigned YES NVRAM down down
```
## Duplex Mismatch
When two interfaces have different duplex settings (full duplex and half duplex) the performance of the link will be greatly reduced. If the full duplex device transmits a frame while the half duplex device is also transmitting a frame, the half duplex device will interpret that as a collision (although a collisions has not occurred).