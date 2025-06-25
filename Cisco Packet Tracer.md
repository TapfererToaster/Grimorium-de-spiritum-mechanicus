# Setup a Home Network
## Part 1: Connect the Devices
### Step 1: Connect the coaxial cables

- We connect the cable modem to the cable splitter with a coaxial cable, by plugging the cable into the coaxial1 port on the cable splitter and into the port 0 of the cable modem.
- We connect another coaxial cable to the coaxial2 port of the cable splitter and plug it into port 0 of the TV
### Step 2: Connect the network cables
There are two PCs in the house, which do not have a wireless LAN adapter, so they must be connected with Ethernet cables
- We connect Port 1 on the Cable Modem to the Internet port of the Home Wireless Router with a Copper Straight-Through
- We connect the FastEthernet0 port on the Office PC to the GigabitEthernet1 port on the Wireless Router
- We connect the FastEthernet0 port on the Bedroom PC to the GigabitEthernet2 port on the Wireless Router

## Part 2: Configure the Wireless Router
### Step 1: Access the home wireless router GUI
- We set the Office PC to DHCP in IP Configuration under the Desktop tab
- We go to the Web Browser, enter the IP of the Home Wireless Router and enter "admin" as the username and password, the GUI of the router should be displayed
### Step 2: Configure the basic settings
The house is located in a densely populated area, so many people could see the wireless network. As it is not expected that more than 10 devices will be connected at the same time we set the number of users to 10.
- We click on the Administration tab, change the default password to "MyPassword1!" and click Save Settings
### Step 3: Configure a wireless LAN
- We  click on the Wireless tab in the GUI of the Home Wireless Router
- We enable the SSID Broadcast and change the Network Name (SSID) to "MyHome" and save the settings
- We navigate to the "Wireless Security" tab, select "WPA2 Personal" in the "2,4 GHz" drop down menu, set "MyPassPhrase1!" as the passphrase, save the settings and close the browser
## Part 3: Configure IP Addressing and Test Connectivity
### Step 1: Connect the laptop to the wireless network
- We click on the Laptop in the living room and go to "PC Wireless" in the "Desktop" tab
- We click on the "Connect" tab, "MyHome" should appear 
- We select "MyHome" and enter the passphrase; the laptop should be connected
### Step 2: Configure the bedroom PC
- We open "IP Configuration" on the "Bedroom PC" and set it to "DHCP"
- The pc should receive an IP address, try to access the internet with the web browser to test the connectivity
# Configure DHCP on a Wireless Router
## Part 1: Set up the Network Topology
- We add three PCs and connect them to the Ethernet ports of the Router using straigth-through cables.
## Part 2: Observe the default DHCP Settings
- We access the "IP Configuration" on the desktop of a PC and enable DHCP
- After a IP address is assigned, we enter the GUI of the router through the web browser and enter "admin" as both the username and password
- Look at the default settings of the router, notice that DHCP is enabled and the starting range of possible addresses for clients starts with `192.168.0.100`
## Part 3: Change the default DHCP Range of Addresses
- Change the IP address of the router from `192.168.0.1` to `192.168.5.1` and save the settings
- The web page should display an error message
- Return to the "IP Configuration", click "Static" and then "DHCP" again to receive a new IP address
- Open the web browser again, enter the new IP of the router `192.168.5.1` and enter "admin" as the username and password
## Part 4: Change the default DHCP Range of Addresses
- Change the "Start IP Address" from `192.168.5.100` to `192.168.5.126` and the "Maximum number of Users" to 75
- After saving the settings the web page should show another error message
- Return to the "IP Configuration" and turn DHCP off and on again to receive a new address
## Part 5: Enable DHCP on the other PCs
- Enter "IP Configuration" and enable DHCP on the other two PCs
## Part 5: Verify Connectivity
- Select "Command Prompt" on the desktop of "PC2"
- Test the connection by pinging the other PCs and the router with
```
  ping 192.168.5.1
  ping 192.168.5.127
  ping 192.168.5.128
```

# Configure Basic Router Settings
1. Console into the router and enable privileged EXEC mode.
```
Router> enable
```
2. Enter configuration mode.
```
Router# configure te
```
3. Assign a device name to the router.
```
Router(config)# hostname R1
```
4.  Set the router’s domain name as ccna-lab.com.
```
R1(config)# ip domain-name ccna-lab.com
```
5. Encrypt the plaintext passwords.
```
R1(config)# serice password-encryption 
``` 
6. Configure the system to require a minimum 12-character password.
```
R1(config)# security passwords min-length 12
```
7. Configure the username SSHadmin with an encrypted password of 55Hadm!n2020.
```
R1(config)# username SSHadmin secret 55Hadm!n2020
```
8. Generate a set of crypto keys with a 1024 bit modulus.
```
crypto key generate  rsa general-keys modulus 1024
```
9. Assign $cisco!PRIV* as the privileged EXEC password.
```
R1(config)# enable secret $cisco!PRIV*
```
10. Assign $cisco!!CON* as the console password. Configure sessions to disconnect after four minutes of inactivity, and enable login.
```
R1(config)# line console 0
R1(config-line)# password $cisco!!CON* 
R1(config-line)# exec-timeout 4
R1(config-line)# login
R1(config-line)# exit
R1(config)#
```
11. Assign $cisco!!VTY* as the vty password. Configure the vty lines to accept SSH connections only. Configure sessions to disconnect after four minutes of inactivity, and enable login using the local database.
```
R1(config)# line vty 0 15
R1(config-line)# password $cisco!!VTY*
R1(config-line)# transport input ssh
R1(config-line)#login local
R1(config-line)# exit
R1(config)#
```
12. Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
```
R1(config)# banner motd $ something something $
```
13. Enable IPv6 routing.
```
R1(config)# ipv6 unicast-routing
```
14. Configure all three interfaces on the router with the IPv4 and IPv6 addressing information from the addressing table above. Configure all three interfaces with descriptions. Activate all three interfaces.
```
R1(config)# interface g0/0/0
R1(config-if)# description something
R1(config-if)# ip address 192.168.0.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)#exit

R1(config)# interface g0/0/1
R1(config-if)# description something
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)#exit

R1(config)# interface loopback 0
R1(config-if)# description something
R1(config-if)# ip address 10.0.0.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad:1::2/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)#exit
```
15. The router should not allow vty logins for two minutes if three failed login attempts occur within 60 seconds.
```
R1(config)# login block-for 120 attempts 3 within 60
```
16. Set the clock on the router.
```
R1(config)# clock set 13:00:00 20 jun 1999
```
17. Save the running configuration to the startup configuration file.
```
R1(config)# exit
R1# copy running-config startup-config
```
