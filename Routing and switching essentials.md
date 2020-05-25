# Routing and switching essentials
### 1. Basic Device Configuration
##### 1.1  Configure a switch with initial settings
(images of configuring the interface and default gateway here)

##### 1.2 Configure Switch Ports


The table summarizes some of the more useful switch verification commands.

| TaskIOS | Commands |
| ------ | ------- |
| Display interface status and configuration. | S1# show interfaces [interface-id] |
| Display current startup configuration. |S1# show startup-config|
|Display current operating config. | S1# show running-config
|Display information about flash file system. | S1# show flash
|Display system hardware and software status.| S1# show version
|Display history of command entered.| S1# show history |
|Display IP information about an interface. | S1# show ip interface [interface-id] OR S1# show ipv6 interface [interface-id]|
|Display the MAC address table. | S1# show mac-address-table  OR  S1# show mac address-table


##### 1.4 Basic Router Settings

Configure terminal

```sh
Router#configure terminal
```

Name the router R2

```sh
Router(config)#hostname R2
```
configure class as secret password
```sh
R1(config)#enable secret class
```

Configure cisco as the console line password and require users to login. Then exit line configuration mode.
```sh
R1(config)#line console 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
```

Configure cisco as the vty password for lines 0 through 4 and require users to login.
```sh
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
```

Exit line configuration mode and encrypt all plaintext passwords.
```sh
R1(config-line)#exit
R1(config)#service password-encryption
```
Enter the banner Authorized Access Only! and use # as the delimiting character.
```sh
R1(config)#banner motd #Authorized Access Only!#
```
Exit global configuration mode and save the configuration.
```sh
R1(config)#exit
R1#copy running-config startup-config
Destination filename \[startup-config\]?   
Building configuration...  
\[OK\]
```

Configure GigabitEthernet 0/0/1.

Use g0/0/1 to enter interface configuration mode.
Configure the IPv4 address 10.1.2.1 and subnet mask 255.255.255.0.
Configure the IPv6 address 2001:db8:acad:5::1/64.
Describe the link as Link to LAN 4.
Activate the interface.
```sh
Router(config-if)#interface g0/0/1
Router(config-if)#ip address 10.1.2.1 255.255.255.0
Router(config-if)#ipv6 address 2001:db8:acad:5::1/64
Router(config-if)#description Link to LAN 4
Router(config-if)#no shutdown
%LINK-3-UPDOWN: Interface GigabitEthernet0/0/1, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
```
##### 1.6.1 Packet tracer excersize
a.     Configure basic settings.
1)    Hostname as shown in the Addressing Table.
2)    Configure Ciscoenpa55 as the encrypted password.
3)    Configure Ciscolinepa55 as the password on the lines.
4)    All lines should accept connections.
5)    Configure an appropriate message of the day banner.

```sh
Router>enable
Router#configure t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname RTA
Router(config)#enable secret Ciscoenpa55
Router(config)#service password-encryption
Router(config)#line console 0
Router(config-line)#password Ciscolinepa55
Router(config-line)#login
Router(config-line)#exit
Router(config)#banner motd test
Router(config)#line vty 0 4
Router(config-line)#password Ciscolinepa55
Router(config-line)#login
Router(config-line)#
Router(config-line)#exit
RTA(config)#int g0/0
RTA(config-if)#no shutdown

RTA(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
RTA(config-if)#exit
RTA(config)#int g0/1
RTA(config-if)#no shutdown

RTA(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
Router#
```

b.     Configure interface settings.
1)    Addressing.
2)    Descriptions on the interfaces.
3)    Save your configuration.

```sh
Router#configure t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface g0/0
Router(config-if)#description Connected to SW1o
Router(config-if)#ip address 10.10.10.1 255.255.255.0
Router(config-if)#int g0/1
Router(config-if)#ip address 10.10.20.1 255.255.255.0
Router(config-if)#description Connected to SW2
Router(config-if)#end
Router#
%SYS-5-CONFIG_I: Configured from console by console
Router#copy running-config  startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
Router#
```
Step 2: Configure switch SW1 and SW2.

 a.     Configure the default management interface so that it will accept connections over the network from local and remote hosts. Use the values in the addressing table.
b.     Configure an encrypted password using the value in step 1a above.
c.     Configure all lines to accept connections using the password from step 1a above.
d.     Configure the switches so that they can send data to hosts on remote networks.
e.     Save your configuration.

```sh
Switch>enable
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#enable secret Ciscoenpa55
Switch(config)#interface vlan1
Switch(config-if)#ip address 10.10.10.2 255.255.255.0
Switch(config-if)#no shutdown
Switch(config-if)#ip default-gateway 10.10.10.1
Switch(config)#line con 0
Switch(config-line)#password Ciscolinepa55
Switch(config-line)#login
Switch(config-line)#line vty 0 4
Switch(config-line)#password Ciscolinepa55
Switch(config-line)#exit
Switch(config)#line con 0
Switch(config-line)#password Ciscolinepa55
Switch(config)#exit
Switch#
%SYS-5-CONFIG_I: Configured from console by console
copy run start
Destination filename [startup-config]? 
Building configuration...
[OK]
Switch#
```



### 2. Switching Concepts
### 3. VLANs
### 4. Inter-VLAN Routing
### 5. STP Concepts
### 6. EtherChannel
### 7. DHCPv4
### 8. SLAAC and DHCPv6
### 9. FHRP Concepts
### 10. LAN Security Concepts
### 11. Switch Security Configuration
### 12. WLAN Concepts
### 13. WLAN Configuration
### 14. Routing Concepts
### 15. IP Static Routing


