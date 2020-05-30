# Routing and switching essentials
Setting the clock 
```sh
S1#show clock 
*0:15:3.563 UTC Mon Mar 1 1993
S1#clock set 11:14:00 28 May 2020
S1#show clock
11:14:4.626 UTC Thu May 28 2020
S1#
```
[deel1](#test)


Subnetmask table
| Prefix | Subnetmask |
| --- | --- |
| /24 | 255.255.255.0 |
| /25 | 255.255.255.128 |
| /26 | 255.255.255.192 |
| /27 | 255.255.255.224 |
| /28 | 255.255.255.240 |
| /29 | 255.255.255.248 |
| /30 | 255.255.255.252 |
| /31 | 255.255.255.254 |
| /32 | 255.255.255.255 |

### 1. Basic Device Configuration
##### 1.1  Configure a switch with initial settings
(images of configuring the interface and default gateway here)

##### 1.2 Configure Switch Ports 


The table summarizes some of the more useful switch verification commands.

| Task | IOS Commands |
| ------ | ------- |
| Display interface status and configuration. | S1# show interfaces [interface-id] |
| Display current startup configuration. |S1# show startup-config|
|Display current operating config. | S1# show running-config
|Display information about flash file system. | S1# show flash
|Display system hardware and software status.| S1# show version
|Display history of command entered.| S1# show history |
|Display IP information about an interface. | S1# show ip interface [interface-id] OR S1# show ipv6 interface [interface-id]|
|Display the MAC address table. | S1# show mac-address-table  OR  S1# show mac address-table

Use the duplex interface configuration mode command to manually specify the duplex mode for a switch port. Use the speed interface configuration mode command to manually specify the speed. For example, both switches in the topology should always operate in full-duplex at 100 Mbps.
|Task	|IOS Commands|
|-----| -------|
|Enter global configuration mode.	|S1# configure terminal|
Enter interface configuration mode.	|S1(config)# interface FastEthernet 0/1| 
Configure the interface duplex.	|S1(config-if)# duplex full
|Configure the interface speed.	|S1(config-if)# speed 100
|Return to the privileged EXEC mode.|S1(config-if)# end
|Save the running config to the startup config.	|S1# copy running-config startup-config
##### 1.3 Secure Remote Acces
This is an example of how to acces your switch by using ssh

```sh
Switch>show ip ssh
SSH Disabled - version 1.99
Switch(config)#ip domain-name cisco.com
Switch(config)#crypto key generate rsa
% Please define a hostname other than Switch.
Switch(config)#hostname s1
s1(config)#crypto key generate rsa
s1(config)#username admin secret ccna
*Mar 1 0:1:14.396: %SSH-5-ENABLED: SSH 1.99 has been enabled
s1(config)#line vty 0 15
s1(config-line)#transport input ssh
s1(config-line)#login local
s1(config-line)#exit
s1(config)#ip ssh version 2
```
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

Configure GigabitEthernet 0/0/1:
- Use g0/0/1 to enter interface configuration mode.
- Configure the IPv4 address 10.1.2.1 and subnet mask 255.255.255.0.
- Configure the IPv6 address 2001:db8:acad:5::1/64.
- Describe the link as Link to LAN 4.
- Activate the interface.
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
##### 3.3 VLAN Configuration
This command gives a brief overview of all the Vlans and their ports
```sh
show vlan brief
```
###### 3.3.2 VLAN Creation Commands
The table displays the Cisco IOS command syntax used to add a VLAN to a switch and give it a name. Naming each VLAN is considered a best practice in switch configuration.

|Task	|IOS Command|
| ----- | ------ |
|Enter global configuration mode.	|Switch# configure terminal
|Create a VLAN with a valid ID number.	|Switch(config)# vlan vlan-id
|Specify a unique name to identify the VLAN.|Switch(config-vlan)# name vlan-name |
Return to the privileged EXEC mode.	|Switch(config-vlan)# end

After creating a VLAN, the next step is to assign ports to the VLAN.

The table displays the syntax for defining a port to be an access port and assigning it to a VLAN. The switchport mode access command is optional, but strongly recommended as a security best practice. With this command, the interface changes to permanent access mode.

|Task |	IOS Command |
| ----- | ----- |
|Enter global configuration mode.	|Switch# configure terminal |
|Enter interface configuration mode.	|Switch(config)# interface interface-id
|Set the port to access mode.|Switch(config-if)# switchport mode access 
| Assign the port to a VLAN.	|Switch(config-if)# switchport access vlan vlan-id
|Return to the privileged EXEC mode.	|Switch(config-if)# end |
Note: Use the interface range command to simultaneously configure multiple interfaces.

Remove the data VLAN from the port.
```sh
S1(config-if)#no switchport access vlan
```
###### 3.3.8 Verify VLAN Information
```sh
show vlan [brief | id vlan-id | name vlan-name | summary].
```

###### 3.3.9 Change VLAN Port Membership
 For instance, assume Fa0/18 was incorrectly configured to be on the default VLAN 1 instead of VLAN 20. To change the port to VLAN 20, simply enter switchport access vlan 20.
 To change the membership of a port back to the default VLAN 1, use the no switchport access vlan int
###### 3.3.10 Delete VLANs
The no vlan vlan-id global configuration mode command is used to remove a VLAN from the switch vlan.dat file.

Caution: Before deleting a VLAN, reassign all member ports to a different VLAN first. Any ports that are not moved to an active VLAN are unable to communicate with other hosts after the VLAN is deleted and until they are assigned to an active VLAN.

##### 3.4.6 Configure VLAN and trunking exersize
an example of how to shut down a specific range of ports of a switch (here fast ethernet 2 -5, 7 - 24 and gigabit 1 and 2 get shut down)
```sh
S1(config)#int range fa0/2 - 5, fa0/7- 24, g0/1-2
S1(config-if-range)#shutdown

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down
```
##### 3.4 VLAN Trunks

|Task |	IOS Command |
| ---- | ---- |
|Enter global configuration mode.	|Switch# configure terminal
|Enter interface configuration mode. |Switch(config)# interface interface-id
|Set the port to permanent trunking mode.	|Switch(config-if)# switchport mode trunk
|Sets the native VLAN to something other than VLAN 1.	|Switch(config-if)# switchport trunk native vlan vlan-id
|Specify the list of VLANs to be allowed on the trunk link.	|Switch(config-if)# switchport trunk allowed vlan vlan-list
|Return to the privileged EXEC mode.	|Switch(config-if)# end
##### 3.5 Dynamic Trunking Protocol
The switchport mode command has additional options for negotiating the interface mode. The full command syntax is the following:
```sh
Switch(config)# switchport mode { access | dynamic { auto | desirable } | trunk }
```

### 4. Inter-VLAN Routing
##### 4.2 Router-on-a-stick Inter-VLAN Routing
In addition to using **ping** between devices, the following **show** commands can be used to verify and troubleshoot the router-on-a-stick configuration.
- show ip route
- show ip interface brief
- show interfaces
- show interfaces trunk

###### 4.2.7 Packter tracer excersize
**Configuration of router on a stick interVlan routing**
Steps:
1. Create VLANs on switches
``` sh
S1(config)#vlan 10
```
2. Assign VLANs to ports
```sh
S1(config)#int f0/11
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 10
```

3. Configure subinterfaces on router using 802.1Q encapsulation
- Create the subinterface G0/0.10.
- Set the encapsulation type to 802.1Q and assign VLAN 10 to the subinterface.
- Refer to the Address Table and assign the correct IP address to the subinterface.
```sh
R1(config)# int g0/0.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 172.17.10.1 255.255.255.0
```

4. Enable trunking
```sh
S1(config)#int g0/1
S1(config-if)#switchport mode trunk
```
##### 4.3 Layer 3 Switch Inter-VLAN Routing
###### 4.3.8 Layer 3 Switcing packet tracer
Steps:
1. Configure Layer 3 Switching
a. On MLS, configure G0/2 as a routed port and assign an IP address according to the Addressing Table.
```sh
MLS(config)# interface g0/2
MLS(config-if)# no switchport
MLS(config-if)# ip address 209.165.200.225 255.255.255.252
```
b.  Verify connectivity to Cloud by pinging 209.165.200.226.

MLS# ping 209.165.200.226
2. Add VLANS
3. Configure SVI on MLS
Configure and activate the SVI interfaces for needed VLANs according to the Addressing Table. 

```sh
MLS(config)# interface vlan 10
MLS(config-if)# ip address 192.168.10.254 255.255.255.0
```
4. Configure trunking on MLS
    - On MLS, configure interface g0/1.
    - Make the interface a static trunk port.
    ```sh
    MLS(config-if)# switchport mode trunk
    ```
c.     Specify the native VLAN as 99.

```sh
MLS(config-if)# switchport trunk native vlan 99
```


d.     Encapsulate the link with the dot1q protocol.
```sh
MLS(config-if)# switchport trunk encapsulation dot1q
```

5. Configure trunking on switch
    ```sh
    S1(config)#int g0/1
    S1(config-if)#switchport mode trunk
    S1(config-if)#switchport trunk native vlan 99
    ```
6. Enable routing
    ```sh
    MLS(config)# ip routing
    ```
7. Enable ipv6 routing
    Enter the ipv6 unicast-routing command to enable IPv6 routing in global configuration mode.
    ```sh
    MLS(config)# ipv6 unicast-routing
    ```
8. configure svi for ipv6 on MLS
    Configure IPv6 addressing on SVI for VLANs 10, 20, and 30 according to the Addressing Table. The configuration for VLAN 10 is shown below.
    ```sh
    MLS(config)# interface vlan 10
    ```
MLS(config-if)# ipv6 address 2001:db8:acad:10::1/64
9. Configure G0/2 with ipv6 on MLS
    a. Configure IPv6 addressing on G0/2.
    ```sh
    MLS(config)# interface G0/2
    MLS(config-if)# ipv6 address 2001:db8:acad:a::1/64
    ```
    b. Use the show ipv6 route command to verify IPv6 connected networks.

### 5. STP Concepts (Spanning Tree Protocol)
### 6. EtherChannel
##### 6.2 Configure EtherChannel
Enter interface range mode for FastEthernet0/1 and FastEthernet0/2. Use fa 0/1 - 2 as the interface designation.

 ```sh
  S1(config)#interface range fa 0/1 - 2
  ```
Configure the channel-group to use LACP unconditionally.
 ```sh
  S1(config-if-range)#channel-group 1 mode active
  ```
Configure the switchport settings for the port-channel that was created:
- Enter interface configuration mode for port-channel 1
- Configure port-channel 1 as a trunk
- Allow VLANS 1,2,20 to cross the trunk link. Enter the VLANs as shown with no spaces.
 ```sh
S1(config-if-range)#interface port-channel 1
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk allowed vlan 1,2,20
  ```
###### 6.2.4 Configuring EtherChannel Packet Tracer
1. Configure Basic Switch settings
    - Assign each switch a hostname according to the topology diagram.
    -   Configure all ports that are required for the EtherChannels as static trunk ports. (this for all needed ports for the different etherchannels. in this example 21-22 are a channel and 23-24 are a channel)
 ```sh
S3(config)#int ra f0/21-22, f0/23-24
S3(config-if-range)#switchport mode trunk
  ```
2. Configure an EtherChannel
2.1 With Cisco PAgP
 On S1 and S3, add ports F0/21 and F0/22 to Port Channel 1 with the channel-group 1 mode desirable command. The mode desirable option enables the switch to actively negotiate to form a PAgP link. Note: Interfaces must be shutdown before adding them to the channel group.
 ```sh
S1(config)# interface range f0/21 – 22
S1(config-if-range)# shutdown
S1(config-if-range)# channel-group 1 mode desirable
S1(config-if-range)# no shutdown
S3(config)# interface range f0/21 - 22
S3(config-if-range)# shutdown
S3(config-if-range)# channel-group 1 mode desirable
S3(config-if-range)# no shutdown
  ```
The message “Creating a port-channel interface Port-channel 1” should appear on both switches when the channel-group is configured. This interface designation will appear as Po1 in command output.

 Configure the logical interface to become a trunk by first entering the interface port-channel number command and then the switchport mode trunk command. Add this configuration to both switches.
 ```sh
S1(config)# interface port-channel 1
S1(config-if)# switchport mode trunk
S3(config)# interface port-channel 1
S3(config-if)# switchport mode trunk
  ```
2.3 With 802.3ad LACP
 ```sh
S1(config)# interface range g0/1 - 2
S1(config-if-range)# shutdown
S1(config-if-range)# channel-group 2 mode active
S1(config-if-range)# no shutdown
S1(config-if-range)# interface port-channel 2
S1(config-if)# switchport mode trunk
  ```
2.3 with redundant EtherChannel Link
 ```sh
S2(config)# interface range f0/23 - 24
S2(config-if-range)# shutdown
S2(config-if-range)# channel-group 3 mode passive
S2(config-if-range)# no shutdown
S2(config-if-range)# interface port-channel 3
S2(config-if)# switchport mode trunk
  ```
On S3, add ports F0/23 and F0/24 to Port Channel 3 with the channel-group 3 mode active command. The active option indicates that you want the switch to use LACP unconditionally. Statically configure Port Channel 3 as a trunk interface.
Port Channel 2 is not operative because Spanning Tree Protocol placed some ports into blocking mode. Unfortunately, those ports were the Gigabit ports. In this topology, you can restore these ports by configuring S1 to be primary root for VLAN 1. You could also set the priority to 24576.
 ```sh
S1(config)# spanning-tree vlan 1 root primary
  ```
or
 ```sh
S1(config)# spanning-tree vlan 1 priority 24576
  ```
  
this command is to remove a channel group when you made a mistake
 ```sh
S1(config)# no interface port-channel 1
  ```
### 7. DHCPv4
|Task|	IOS Command|
| ----- | ----- |
|Define the address pool.|	network network-number [mask \| / prefix-length]
|Define the default router or gateway.	|default-router address [ address2….address8]
|Define a DNS server.	|dns-server address [ address2…address8]
|Define the domain name.	|domain-name domain
|Define the duration of the DHCP lease.	|lease {days [hours [ minutes]] \| infinite}
|Define the NetBIOS WINS server.	|netbios-name-server address [ address2…address8]|

| Command |Description |
| --- | --- |
|show running-config | section dhcpDisplays the DHCPv4 commands configured on the router. 
|show ip dhcp binding | Displays a list of all IPv4 address to MAC address bindings provided by the DHCPv4 service. 
|show ip dhcp server statistics |Displays count information regarding the number of DHCPv4 messages that have been sent and received.
##### 7.2 Configure a Cisco IOS DHCPv4 Server
- Exclude the following addresses from the 192.168.11.0/24 address range:
    - Exclude the .1 through the .9 address.
    - Exclude the .254 address.
 ```sh
R1(config)#ip dhcp excluded-address 192.168.11.1 192.168.11.9
R1(config)#ip dhcp excluded-address 192.168.11.254
  ```
- Configure the DHCPv4 pool with the following requirements:
    - Define the pool name of LAN-POOL-2.
    - Configure the network address.
    - Configure the default gateway address.
    - Configure the DNS server address.
    - Configure example.com as the domain name.
    - Return to privileged EXEC mode.
     ```sh
    R1(config)#ip dhcp pool LAN-POOL-2
    R1(dhcp-config)#network 192.168.11.0 255.255.255.0
    R1(dhcp-config)#default-router 192.168.11.1
    R1(dhcp-config)#dns-server 192.168.11.6
    R1(dhcp-config)#domain-name example.com
    R1(dhcp-config)#end
     ```
The DHCPv4 service is enabled by default. To disable the service, use the no service dhcp global configuration mode command. Use the service dhcp global configuration mode command to re-enable the DHCPv4 server process
   ```sh
R1(config)# no service dhcp
R1(config)# service dhcp
R1(config)# 
 ```
###### 7.2.10 Configure DHCPv4 Packet tracer 

|Device |Interface |IPv4 Address |Subnet Mask |Default Gateway|
| --- | --- | --- | --- | --- |
R1 | G0/0 | 192.168.10.1 | 255.255.255.0|N/A
|  | S0/0/0 |10.1.1.1 |255.255.255.252 |N/A
R2 |G0/0 | 192.168.20.1 | 255.255.255.0 | N/A
|R2 |G0/1 | DHCP Assigned | DHCP Assigned | N/A
| | S0/0/0 | 10.1.1.2 | 255.255.255.252 | N/A
| | S0/0/1 | 10.2.2.2 | 255.255.255.252 | N/A
R3 | G0/0 | 192.168.30.1 | 255.255.255.0 | N/A
| | S0/0/1 | 10.2.2.1 | 255.255.255.0 | N/A
PC1 | NIC | DHCP Assigned | DHCP Assigned | DHCP Assigned
PC2 | NIC | DHCP Assigned | DHCP Assigned | DHCP Assigned
DNS Server | NIC | 192.168.20.254 |255.255.255.0 | 192.168.20.1

1. Configure a Router as a DHCP Server
- Configure the excluded IPv4 addresses
     - Configure R2 to exclude the first 10 addresses from the R1 LAN.
 ```sh
 R2(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
```
- Create a DHCP pool on R2 for R1 LAN
    a. Create a DHCP pool named R1-LAN (case-sensitive).
 ```sh
R2(config)# ip dhcp pool R1-LAN
```
b. Configure the DHCP pool to include the network address, the default gateway, and the IP address of the DNS server.
```sh
R2(dhcp-config)# network 192.168.10.0 255.255.255.0
R2(dhcp-config)# default-router 192.168.10.1
R2(dhcp-config)# dns-server 192.168.20.254
```
(do the same for all other LANs that need a dhcp pool fe. R3-LAN)
2. Configure DHCP Relay
- Configure R1 and R3 as a DHCP relay agent
a. Configure the helper address for the LAN interface on R1.
```sh
R1(config)# interface g0/0
R1(config-if)# ip helper-address 10.1.1.2
```
b. Configure the helper address for the LAN interface on R3.
- Configure hosts to receive IP addressing information from DHCP
Go to the pc, then ip config and change from static to dhcp
3. Configure a Router as a DHCP Client
Just as a PC is able to receive an IPv4 address from a server, a router interface has the ability to do the same. Router R2 needs to be configured to receive addressing from the ISP.
a.     Configure the Gigabit Ethernet 0/1 interface on R2 to receive IP addressing from DHCP and activate the interface.
```sh
R2(config)# interface g0/1
R2(config-if)# ip address dhcp
R2(config-if)# no shutdown
```
b. Use the show ip interface brief command to verify that R2 received an IP address from DHCP.
##### 7.3 Cisco Router as a DHCPv4 Client
- Enter interface configuration mode. Use g0/0/1 as the interface name.
```sh
SOHO(config)#interface g0/0/1
```
- Configure the interface to acquire IPv4 addressing information from the ISP.
- Activate the interface.
```sh
SOHO(config-if)#ip address dhcp
SOHO(config-if)#no shutdown
Sep 12 10:01:25.773: %DHCP-6-ADDRESS\_ASSIGN: Interface GigabitEthernet0/0/1 assigned DHCP address 209.165.201.12, mask 255.255.255.224, hostname SOHO
```
- Return to privileged EXEC mode.
- Use the **show ip interface g0/0/1** command to verify the IPv4 information.
```sh
SOHO(config-if)#end
SOHO#show ip interface g0/0/1
GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 209.165.201.12/27
  Broadcast address is 255.255.255.255
  Address determined by DHCP
(output omitted)
```
### 8. SLAAC and DHCPv6
##### 8.4 Configure DHCPv6 Server
###### 8.4.2 Configure a Stateless DHCPv6 Server
Step 1. Enable IPv6 routing.
```sh
R1(config)# ipv6 unicast-routing
```
Step 2. Define a DHCPv6 pool name.
```sh
R1(config)# ipv6 dhcp pool IPV6-STATELESS
R1(config-dhcpv6)#
```
Step 3. Configure the DHCPv6 pool.
```sh
R1(config-dhcpv6)# dns-server 2001:db8:acad:1::254
R1(config-dhcpv6)# domain-name example.com
R1(config-dhcpv6)# exit
```
Step 4. Bind the DHCPv6 pool to an interface.
```sh
R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# description Link to LAN
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 nd other-config-flag
R1(config-if)# ipv6 dhcp server IPV6-STATELESS
R1(config-if)# no shut
R1(config-if)# end
R1#
```
Step 5. Verify that the hosts have received IPv6 addressing information.
To verify stateless DHCP on a Windows host, use the ipconfig /all command. 
###### 8.4.3 Configure a Stateless DHCPv6 Client
Step 1. Enable IPv6 routing.
```sh
R3(config)# ipv6 unicast-routing
R3(config)#
```
Step 2. Configure the client router to create an LLA.
The client router needs to have a link-local address. An IPv6 link-local address is created on a router interface when a global unicast address is configured. It can also be created without a GUA using the ipv6 enable interface configuration command.
```sh
R3(config)# interface g0/0/1
R3(config-if)# ipv6 enable
R3(config-if)# 
```
Step 3. Configure the client router to use SLAAC.
The client router needs to be configured to use SLAAC to create an IPv6 configuration. The ipv6 address autoconfig command enables the automatic configuration of IPv6 addressing using SLAAC.
```sh
R3(config-if)# ipv6 address autoconfig
R3(config-if)# end
```
Step 4. Verify that the client router is assigned a GUA.
Use the show ipv6 interface brief command to verify the host configuration as shown.
```sh
R3# show ipv6 interface brief
GigabitEthernet0/0/0   [up/up]
    unassigned
GigabitEthernet0/0/1   [up/up]
    FE80::2FC:BAFF:FE94:29B1
    2001:DB8:ACAD:1:2FC:BAFF:FE94:29B1
Serial0/1/0            [up/up]
    unassigned
Serial0/1/1            [up/up]
    unassigned
R3#
```
Step 5. Verify that the client router received other necessary DHCPv6 information.
The show ipv6 dhcp interface g0/0/1 command confirms that the DNS and domain names were also learned by R3.
```sh
R3# show ipv6 dhcp interface g0/0/1
GigabitEthernet0/0/1 is in client mode
  Prefix State is IDLE (0)
  Information refresh timer expires in 23:56:06
  Address State is IDLE
  ...
```
###### 8.4.4 Configure a StateFull DHCPv6 Server
Step 1. Enable IPv6 routing.
The ipv6 unicast-routing command is required to enable IPv6 routing.
```sh
R1(config)# ipv6 unicast-routing
R1(config)# 
```
Step 2. Define a DHCPv6 pool name.
Create the DHCPv6 pool using the ipv6 dhcp pool POOL-NAME global config command.
```sh
R1(config)# ipv6 dhcp pool IPV6-STATEFUL
R1(config-dhcpv6)#
```
Step 3. Configure the DHCPv6 pool.
R1 will be configured to provide IPv6 addressing, DNS server address, and domain name, as shown in the command output. With stateful DHCPv6, all addressing and other configuration parameters must be assigned by the DHCPv6 server. The address prefix command is used to indicate the pool of addresses to be allocated by the server. Other information provided by the stateful DHCPv6 server typically includes DNS server address and the domain name, as shown in the output.

Note: This example is setting the DNS server to Google's public DNS server.
```sh
R1(config-dhcpv6)# address prefix 2001:db8:acad:1::/64
R1(config-dhcpv6)# dns-server 2001:4860:4860::8888
R1(config-dhcpv6)# domain-name example.com
R1(config-dhcpv6)#
```
Step 4. Bind the DHCPv6 pool to an interface.
The example shows the full configuration of the GigabitEthernet 0/0/1 interface on R1.

The DHCPv6 pool has to be bound to the interface using the ipv6 dhcp server POOL-NAME interface config command.
```sh
R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# description Link to LAN
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 nd managed-config-flag
R1(config-if)# ipv6 nd prefix default no-autoconfig
R1(config-if)# ipv6 dhcp server IPV6-STATEFUL
R1(config-if)# no shut
R1(config-if)# end
R1#
```
Step 5. Verify that the hosts have received IPv6
addressing information.
To verify on a Windows host, use the ipconfig /all command to verify the stateless DHCP configuration method.
```sh
C:\PC1> ipconfig /all
```
###### 8.4.5 Configure a StateFul DHCPv6 Client
Step 1. Enable IPv6 routing.
The DHCPv6 client router needs to have ipv6 unicast-routing enabled.
```sh
R3(config)# ipv6 unicast-routing
R3(config)#
```
Step 2. Configure the client router to create an LLA.
In the output, the ipv6 enable command is configured on the R3 Gigabit Ethernet 0/0/1 interface. This enables the router to create an IPv6 LLA without needing a GUA.
```sh
R3(config)# interface g0/0/1
R3(config-if)# ipv6 enable
R3(config-if)# 
```
Step 3. Configure the client router to use DHCPv6.
The ipv6 address dhcp command configures R3 to solicit its IPv6 addressing information from a DHCPv6 server.
```sh
R3(config-if)# ipv6 address dhcp
R3(config-if)# end
R3#
```
Step 4. Verify that the client router is assigned a GUA.
Use the show ipv6 interface brief command to verify the host configuration as shown.
```sh
R3# show ipv6 interface brief
GigabitEthernet0/0/0   [up/up]
    unassigned
GigabitEthernet0/0/1   [up/up]
    FE80::2FC:BAFF:FE94:29B1
    2001:DB8:ACAD:1:B4CB:25FA:3C9:747C
Serial0/1/0            [up/up]
    unassigned
Serial0/1/1            [up/up]
    unassigned
R3#
```
Step 5. Verify that the client router received other necessary DHCPv6 information.
The show ipv6 dhcp interface g0/0/1 command confirms that the DNS and domain names were learned by R3.
```sh
R3# show ipv6 dhcp interface g0/0/1
GigabitEthernet0/0/1 is in client mode
  Prefix State is IDLE
  Address State is OPEN
```
###### 8.4.7 Configure a DHCPv6 Relay Agent
The command syntax to configure a router as a DHCPv6 relay agent is as follows:
```sh
Router(config-if)# ipv6 dhcp relay destination ipv6-address [interface-type interface-number]
```
This command is configured on the interface facing the DHCPv6 clients and specifies the DHCPv6 server address and egress interface to reach the server, as shown in the output. The egress interface is only required when the next-hop address is an LLA.
```sh
R1(config)# interface gigabitethernet 0/0/1
R1(config-if)# ipv6 dhcp relay destination 2001:db8:acad:1::2 G0/0/0
R1(config-if)# exit
R1(config)#
```
### 9. FHRP Concepts
##### 9.3 Hsrp Configuration guide (PT)
//TODO
### 10. LAN Security Concepts
### 11. Switch Security Configuration
You are currently logged into S1. Configure FastEthernet 0/5 for port security by using the following requirements:
- Use the interface name fa0/5 to enter interface configuration mode.
- Enable the port for access mode.
- Enable port security.
- Set the maximum number of MAC address to 3.
- Statically configure the MAC address aaaa.bbbb.1234.
- Configure the port to dynamically learn additional MAC addresses and dynamically add them to the running configuration.
- Return to privileged EXEC mode.
```sh
S1(config)#interface fa0/5
S1(config-if)#switchport mode access
S1(config-if)#switchport port-security
S1(config-if)#switchport port-security maximum 3
S1(config-if)#switchport port-security mac-address aaaa.bbbb.1234
S1(config-if)#switchport port-security mac-address sticky
S1(config-if)#end
```
- Enter the command to verify port security for all interfaces.
```sh
S1#show port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                (Count)       (Count)          (Count)
---------------------------------------------------------------------------
      Fa0/5              3            2                  0         Shutdown
---------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 8192
```
- Enter the command to verify port security on FastEthernet 0/5. Use fa0/5 for the interface name.
```sh
S1#show port-security interface fa0/5
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 3
Total MAC Addresses        : 2
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 1
Last Source Address:Vlan   : 0090.2135.6B8C:1
Security Violation Count   : 0
```
- Enter the command that will display all of the addresses to verify that the manually configured and dynamically learned MAC addresses are in the running 
```sh
S1#show port-security address
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan    Mac Address       Type                          Ports   Remaining Age
                                                                   (mins)    
----    -----------       ----                          -----   -------------
   1    0090.2135.6b8c    SecureSticky                  Fa0/5        -
   1    aaaa.bbbb.1234    SecureConfigured              Fa0/5        -
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 8192
```

###### 11.1.10 Implement port security (PT)
Step 1: Configure Port Security
- Access the command line for S1 and enable port security on Fast Ethernet ports 0/1 and 0/2.
```sh
S1(config)# interface range f0/1 – 2
S1(config-if-range)# switchport port-security
```
- Set the maximum so that only one device can access the Fast Ethernet ports 0/1 and 0/2.
```sh
S1(config-if-range)# switchport port-security maximum 1
```
- Secure the ports so that the MAC address of a device is dynamically learned and added to the running configuration.
```sh
S1(config-if-range)# switchport port-security mac-address sticky
```
- Set the violation mode so that the Fast Ethernet ports 0/1 and 0/2 are not disabled when a violation occurs, but a notification of the security violation is generated and packets from the unknown source are dropped.
```sh
S1(config-if-range)# switchport port-security violation restrict
```
- Disable all the remaining unused ports. Use the range keyword to apply this configuration to all the ports simultaneously.
```sh
S1(config-if-range)# interface range f0/3 - 24 , g0/1 - 2
S1(config-if-range)# shutdown
```
##### 11.2 Mitigate VLAN Attacks
You are currently logged into S1. The ports status of the ports are as follows:
- FastEthernet ports 0/1 through 0/4 are used for trunking with other switches.
- FastEthernet ports 0/5 through 0/10 are unused.
- FastEthernet ports 0/11 through 0/24 are active ports currently in use.
Use range fa0/1 - 4 to enter interface configuration mode for the trunks.
```sh
S1(config)#interface range fa0/1 - 0/4
```
- Configure the interfaces as nonnegotiating trunks assigned to default VLAN 99.
```sh
S1(config-if-range)#switchport mode trunk
S1(config-if-range)#switchport nonegotiate
S1(config-if-range)#switchport trunk native vlan 99
S1(config-if-range)# exit
```
- Use range fa0/5 - 10 to enter interface configuration mode for the trunks.
```sh
S1(config)#interface range fa0/5 - 10
```
- Configure the unused ports as access ports, assign them to VLAN 86, and shutdown the ports.
```sh
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 86
% Access VLAN does not exist. Creating vlan 86
S1(config-if-range)#shutdown
```
- Use range fa0/11 - 24 to enter interface configuration mode for the active ports and then configure them to prevent trunking.
```sh
S1(config)#interface range fa0/11 - 24
S1(config-if-range)#switchport mode access
S1(config-if-range)# end
S1#
```
##### 11.3 Mitigate DHCP Attacks
- You are currently logged into S1. Enable DHCP snooping globally for the switch.
```sh
S1(config)#ip dhcp snooping
```
- Enter interface configuration mode for g0/1 - 2, trust the interfaces, and return to global configuration mode.
```sh
S1(config)#interface range g0/1 - 2
S1(config-if-range)#ip dhcp snooping trust
S1(config-if-range)#exit
```
Enter interface configuration mode for f0/1 - 24, limit the DHCP messages to no more than 10 per second, and return to global configuration mode.
```sh
S1(config)#interface range f0/1 - 24
S1(config-if-range)#ip dhcp snooping limit rate 10
S1(config-if-range)#exit
```
- Enable DHCP snooping for VLANs 10,20,30-49.
```sh
S1(config)#ip dhcp snooping vlan 10,20,30-49
S1(config)# exit
```
- Enter the command to verify DHCP snooping.
```sh
S1#show ip dhcp snooping
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10,20,30-49
DHCP snooping is operational on following VLANs:
none
DHCP snooping is configured on th.....
```
- Enter the command to verify the current DHCP bindings logged by DHCP snooping
```sh
S1#show ip dhcp snooping binding
MacAddress         IpAddress       Lease(sec) Type          VLAN Interface
------------------ --------------- ---------- ------------- ---- --------------------
00:03:47:B5:9F:AD  10.0.0.10       193185     dhcp-snooping 5    FastEthernet0/1
S1#
```
##### 11.4 Mitigate ARP Attacks
- Implementing DAI for a switch
- You are currently logged into S1. Enable DHCP snooping globally for the switch.
```sh
S1(config)#ip dhcp snooping
```
- Enter interface configuration mode for g0/1 - 2, trust the interfaces for both DHCP snooping and DAI, and then return to global configuration mode.
```sh
S1(config)#interface range g0/1 - 2
S1(config-if-range)#ip dhcp snooping trust
S1(config-if-range)#ip arp inspection trust
S1(config-if-range)#exit
```
Enable DHCP snooping and DAI for VLANs 10,20,30-49.
```sh
S1(config)#ip dhcp snooping vlan 10,20,30-49
S1(config)#ip arp inspection vlan 10,20,30-49
S1(config)#
```
You have successfully configured DAI for the switch.
##### 11.5 Mitigate STP Attacks
- You are currently logged into S1. Complete the following steps to implement PortFast and BPDU Guard on all access ports:
- Enter interface configuration mode for fa0/1 - 24.
Configure the ports for access mode.
Return to global configuration mode.
Enable PortFast by default for all access ports.
Enable BPDU Guard by default for all access ports.
```sh
S1(config)#interface range fa0/1 - 24
S1(config-if-range)#switchport mode access
S1(config-if-range)#exit
S1(config)#spanning-tree portfast default
S1(config)#spanning-tree portfast bpduguard default
S1(config)# exit
```
- Verify that PortFast and BPDU Guard is enabled by default by viewing STP summary information.
```sh
S1#show spanning-tree summary
Switch is in pvst mode
Root bridge for: none
Extended system ID           is enabled
Portfast Default             is enabled
PortFast BPDU Guard Default  is enabled
Portfast BPDU Filter Default is disabled
Loopguard Default            is disabled
EtherChannel misconfig guard is enabled
UplinkFast                   is disabled
BackboneFast                 is disabled
Configured Pathcost method used is short
(output omitted)
S1#
```
You have successfully configured and verified PortFast and BPDU Guard for the switch.
##### 11.6 Switch Security Configuration (PT)
this is a combination of all the above security measures
1. Create a secure trunk
a.     Connect the G0/2 ports of the two access layer switches.
use a cable to connect the ports
b. Configure ports G0/1 and G0/2 as static trunks on both switches.
```sh
SW-1(config)#int range g0/1-2
SW-1(config-if-range)#switchport mode trunk
```
c. Disable DTP negotiation on both sides of the link.
```sh
SW-1(config-if-range)#switchport nonegotiate
```
d. Create VLAN 100 and give it the name Native on both switches.
```sh
SW-1(config-if)#vlan 100
SW-1(config-vlan)#
SW-1(config-vlan)#name Native
```
e. Configure all trunk ports on both switches to use VLAN 100 as the native VLAN.
```sh
SW-1(config-if-range)#switchport trunk native vlan 100
```
2. Secure Unused Switchports
a. Shutdown all unused switch ports on SW-1.
```sh
SW-1(config)#int range f0/3 - 9, f0/11- 24
SW-1(config-if-range)#shutdown
```
b. On SW-1, create a VLAN 999 and name it BlackHole. The configured name must match the requirement exactly.
```sh
SW-1(config)#vlan 999
SW-1(config-vlan)#name BlackHole
```
c. Move all unused switch ports to the BlackHole VLAN.
```sh
SW-1(config-vlan)#int range f0/3 - 9, f0/11- 24
SW-1(config-if-range)#switchport acces vlan 99
```
3. Implement Port Security.
a. Activate port security on all the active access ports on switch SW-1.
```sh
SW-1(config-if-range)#int range f0/1 -2, f0/10, g0/1-2
SW-1(config-if-range)#switchport port-security
```
b. Configure the active ports to allow a maximum of 4 MAC addresses to be learned on the ports.
```sh
SW-1(config-if)#switchport port-security maximum 4
```
c. For ports F0/1 on SW-1, statically configure the MAC address of the PC using port security.
```sh
SW-1(config-if-range)#switchport port-security mac-address 0010.11E8.3CBB
```
d. Configure each active access port so that it will automatically add the MAC addresses learned on the port to the running configuration.
```sh
SW-1(config-if)#switchport port-security mac-address sticky
```
e. Configure the port security violation mode to drop packets from MAC addresses that exceed the maximum, generate a Syslog entry, but not disable the ports.
```sh
SW-1(config-if)#switchport port-security violation restrict 
```
4. Configure DHCP Snooping.

a. Configure the trunk ports on SW-1 as trusted ports.
```sh
SW-1(config-if-range)#int range g0/2 
SW-1(config-if-range)#ip dhcp snooping trust
```
b. Limit the untrusted ports on SW-1 to five DHCP packets per second.
```sh
SW-1(config-if-range)#int range f0/3 - 9, f0/11- 24
SW-1(config-if-range)#ip dhcp snooping limit rate 5
```
c. On SW-2, enable DHCP snooping globally and for VLANs 10, 20 and 99.
```sh
SW-2(config)#ip dhcp snooping vlan 10,20,99
```
5. Configure PortFast, and BPDU Guard.
a. Enable PortFast on all the access ports that are in use on SW-1.
```sh
SW-1(config)#int range f0/1-2, f0/10,f0/24
SW-1(config-if-range)#spanning-tree portfast
```
b. Enable BPDU Guard on all the access ports that are in use on SW-1.
```sh
SW-1(config-if-range)#spanning-tree bpduguard enable 
```
c. Configure SW-2 so that all access ports will use PortFast by default.
```sh
SW-2(config)#spanning-tree portfast default 
SW-2(config)#
```
### 12. WLAN Concepts

### 13. WLAN Configuration
##### 13.1 Remote Site WLAN Configuration
##### 13.2 Configure a Basic WLAN on the WLC
1. Create the WLAN

In the figure, the administrator is creating a new WLAN that will use Wireless_LAN as the name and service set identifier (SSID). The ID is an arbitrary value that is used to identify the WLAN in display output on the WLC.
2. Apply and Enable the WLAN

After clicking Apply, the network administrator must enable the WLAN before it can be accessed by users, as shown in the figure. The Enable checkbox allows the network administrator to configure a variety of features for the WLAN, as well as additional WLANs, before enabling them for wireless client access. From here, the network administrator can configure a variety of settings for the WLAN including security, QoS, policies, and other advanced settings.
3. Select the Interface

When you create a WLAN, you must select the interface that will carry the WLAN traffic. The next figure shows the selection of an interface that has already been created on the WLC. We will learn how to create interfaces later in this module.
4. Secure the WLAN

Click the Security tab to access all the available options for securing the LAN. The network administrator wants to secure Layer 2 with WPA2-PSK. WPA2 and 802.1X are set by default. In the Layer 2 Security drop down box, verify that WPA+WPA2 is selected (not shown). Click PSK and enter the pre-shared key, as shown in the figure. Then click Apply. This will enable the WLAN with WPA2-PSK authentication. Wireless clients that know the pre-shared key can now associate and authenticate with the AP.
5. Verify the WLAN is Operational

Click WLANs in the menu on the left to view the newly configured WLAN. In the figure, you can verify that WLAN ID 1 is configured with Wireless_LAN as the name and SSID, it is enabled, and is using WPA2 PSK security.
6. Monitor the WLAN

Click the Monitor tab at the top to access the advanced Summary page again. Here you can see that the Wireless_LAN now has one client using its services, as shown in the figure.
7. View Wireless Client Details

Click Clients in the left menu to view more information about the clients connected to the WLAN, as shown in the figure. One client is attached to Wireless_LAN through AP1 and was given the IP address 192.168.5.2. DHCP services in this topology are provided by the router.
#####13.3 Configure a WPA2 Enterprise WLAN on the WLC
VLAN interface configuration on the WLC includes the following steps:

- Create a new interface.
To add a new interface, click CONTROLLER > Interfaces > New..., as shown in the figure.
- Configure the VLAN name and ID.
 the figure, the network administrator configures the interface name as vlan5 and the VLAN ID as 5. Clicking Apply will create the new interface.
- Configure the port and interface address.
On the Edit page for the interface, configure the physical port number. G1 in the topology is Port Number 1 on the WLC. Then configure the VLAN 5 interface addressing. In the figure, VLAN 5 is assigned IPv4 address 192.168.5.254/24. R1 is the default gateway at IPv4 address 192.168.5.1.
- Configure the DHCP server address.
In larger enterprises, WLCs will be configured to forward DHCP messages to a dedicated DHCP server. Scroll down the page to configure the primary DHCP server as IPv4 address 192.168.5.1, as shown in the figure. This is the default gateway router address. The router is configured with a DHCP pool for the WLAN network. As hosts join the WLAN that is associated with the VLAN 5 interface, they will receive addressing information from this pool.
- Apply and Confirm.
Scroll to the top and click Apply, as shown in the figure. Click OK for the warning message.
- Verify Interfaces.
Click Interfaces. The new vlan5 interface is now shown in the list of interfaces with its IPv4 address, as shown in the figure.
### 14. Routing Concepts
##### 14.3 Basic Router Configuration Review
The following examples show the full configuration for R1.
```sh
Router> enable
Router# configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)# hostname R1
R1(config)# enable secret class 
R1(config)# line console 0  
R1(config-line)# logging synchronous
R1(config-line)# password cisco 
R1(config-line)# login 
R1(config-line)# exit 
R1(config)# line vty 0 4 
R1(config-line)# password cisco 
R1(config-line)# login 
R1(config-line)# transport input ssh telnet 
R1(config-line)# exit 
R1(config)# service password-encryption 
R1(config)# banner motd #
Enter TEXT message. End with a new line and the #
***********************************************
WARNING: Unauthorized access is prohibited!
***********************************************
#
R1(config)# ipv6 unicast-routing
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)# description Link to LAN 1
R1(config-if)# ip address 10.0.1.1 255.255.255.0 
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64 
R1(config-if)# ipv6 address fe80::1:a link-local
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface gigabitethernet 0/0/1
R1(config-if)# description Link to LAN 2
R1(config-if)# ip address 10.0.2.1 255.255.255.0 
R1(config-if)# ipv6 address 2001:db8:acad:2::1/64 
R1(config-if)# ipv6 address fe80::1:b link-local
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface serial 0/1/1
R1(config-if)# description Link to R2
R1(config-if)# ip address 10.0.3.1 255.255.255.0 
R1(config-if)# ipv6 address 2001:db8:acad:3::1/64 
R1(config-if)# ipv6 address fe80::1:c link-local
R1(config-if)# no shutdown
R1(config-if)# exit
R1# copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
R1#
```
###### 14.3.5 Basic Routing Configuration Review (PT)
Part 1: Configure Devices and Verify Connectivity
- Step 1: Configure the PC interfaces.
    a. Configure the IPv4 and IPv6 addresses on PC3 as listed in the Addressing Table.
    b. Configure the IPv4 and IPv6 addresses on PC4 as listed in the Addressing Table.
go to the Ip conf settings on the pc and give in the correct ip-addresses
- Step 2: Configure the router.
a.     On the R2 router, open a terminal. Move to privileged EXEC mode.
b.     Enter configuration mode.
c.     Assign a device name of R2 to the router.
```sh
Router(config)#hostname R2
```
d.     Configure c1sco1234 as the encrypted privileged EXEC mode password.
```sh
R2(config)#enable secret c1sco1234
```
e.     Set the domain name of the router to ccna-lab.com.
```sh
R2(config)#ip domain-name ccna-lab.com
```
f.      Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
```sh
R2(config)#no ip domain lookup
```
g.     Encrypt the plaintext passwords.
```sh
R2(config)#service password-encryption
```
h.     Configure the username SSHadmin with an encrypted password of 55Hadm!n.
```sh
R2(config)#username SSHadmin secret 55Hadm!n
```
i.      Generate a set of crypto keys with a 1024 bit modulus.
```sh
R2(config)#crypto key generate rsa
The name for the keys will be: R2.ccna-lab.com
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
```
j.      Assign cisco as the console password, configure sessions to disconnect after six minutes of inactivity, and enable login. To prevent console messages from interrupting commands, use the logging synchronous command.
```sh
R2(config)#line console 0
*Mar 1 6:8:20.568: %SSH-5-ENABLED: SSH 1.99 has been enabled
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exec-timeout 6 0
R2(config-line)#logging synchronous 
```
k.     Assign cisco as the vty password, configure the vty lines to accept SSH connections only, configure sessions to disconnect after six minutes of inactivity, and enable login using the local database.
```sh
R2(config-line)#line vty 0 4
R2(config-line)#password cisco
R2(config-line)#transport input ssh
R2(config-line)#exec-timeout 0
R2(config-line)#exec-timeout 6 0
R2(config-line)#login local
```
l.      Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
```sh
R2(config)#banner motd #Warning Unauth acc is prohib!#
```
m.   Enable IPv6 Routing.
```sh
R2(config)#ipv6 unicast-routing 
```
n.     Configure all four interfaces on the router with the IPv4 and IPv6 addressing information from the addressing table above. Configure all four interfaces with descriptions. Activate all four interfaces.
```sh
R2(config-if)#int g0/0/1
R2(config-if)#description Connects to S4
R2(config-if)#ip address 10.0.5.1 255.255.255.0
R2(config-if)#ipv6 address 2001:db8:acad:5::1/64
R2(config-if)#ipv6 address fe80::2:b link-local 
R2(config-if)#no shutdown
R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
```
***Repeat this for each interface that needs to be configured***
o.     Save the running configuration to the startup configuration file.
```sh
R2#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
R2#
```
##### 14.4 IP Routing Table
To see the full routing table: 
```sh
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS......
```
### 15. IP Static Routing
##### 15.1 Static Routing
- IPv4 static routes are configured using the following global configuration command:
```sh
Router(config)# ip route network-address subnet-mask { ip-address | exit-intf [ip-address]} [distance]
```
- IPv6 static routes are configured using the following global configuration command:
```sh
Router(config)# ipv6 route ipv6-prefix/prefix-length {ipv6-address | exit-intf [ipv6-address]} [distance]
```
##### 15.2 Configure IP Static Routes
- Configure an IPv4 next-hop static route on R2 to the 192.168.2.0/24 network using the next-hop address 192.168.1.1.
```sh
R2(config)#ip route 192.168.2.0 255.255.255.0 192.168.1.1
```
- Configure a fully specified IPv4 static route on R2 to the 172.16.3.0/24 network using the exit interface/next-hop pair: g0/0/1 172.16.2.1
```sh
R2(config)#ip route 172.16.3.0 255.255.255.0 g0/0/1 172.16.2.1
```
- Configure an IPv6 next-hop static route on R2 to the 2001:db8:cafe:2::/64 network using the next-hop address 2001:db8:cafe:1::1.
```sh
R2(config)#ipv6 route 2001:db8:cafe:2::/64 2001:db8:cafe:1::1
```
- Configure a fully specified IPv6 static route on R2 to the 2001:db8:acad:3::/64 network using the exit interface/next-hop pair: g0/0/1 / fe80::1
```sh
R2(config)#ipv6 route 2001:db8:acad:3::/64 g0/0/1 fe80::1
```
- Issue the command to display only the IPv4 static routes in the routing table of R2.
```sh
R2#show ip route static
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, \* - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 5 subnets, 2 masks
S        172.16.3.0/24 \[1/0\] via 172.16.2.1, GigabitEthernet0/0/1
S     192.168.2.0/24 \[1/0\] via 192.168.1.1
```

- Issue the command to display only the IPv6 static routes in the routing table of R2.
```sh
R2#show ipv6 route static
IPv6 Routing Table - default - 9 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, R - RIP, H - NHRP, I1 - ISIS L1
       I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary, D - EIGRP
       EX - EIGRP external, ND - ND Default, NDp - ND Prefix, DCE - Destination
       NDr - Redirect, RL - RPL, O - OSPF Intra, OI - OSPF Inter
       OE1 - OSPF ext 1, OE2 - OSPF ext 2, ON1 - OSPF NSSA ext 1
       ON2 - OSPF NSSA ext 2, a - Application
S   2001:DB8:ACAD:3::/64 \[1/0\]
     via FE80::1, GigabitEthernet0/0/1
S   2001:DB8:CAFE:2::/64 \[1/0\]
     via 2001:DB8:CAFE:1::1
```

==============================================================
You are now logged into R3:

Configure a directly connected IPv4 static route on R3 to the 172.16.3.0/24 network using exit interface S0/1/1.
```sh
R3(config)#ip route 172.16.3.0 255.255.255.0 s0/1/1
```
- Configure a directly connected IPv4 static route on R3 to the 172.16.1.0/24 network using exit interface S0/1/1.
```sh
R3(config)#ip route 172.16.1.0 255.255.255.0 s0/1/1
```
- Configure a directly connected IPv4 static route on R3 to the 172.16.2.0/24 network using exit interface S0/1/1.
```sh
R3(config)#ip route 172.16.2.0 255.255.255.0 s0/1/1
```
- Configure a directly connected IPv6 static route on R3 to the 2001:db8:acad:1::/64 network using exit interface S0/1/1.
```sh
R3(config)#ipv6 route 2001:db8:acad:1::/64 s0/1/1
```
- Configure a directly connected IPv6 static route on R3 to the 2001:db8:acad:3::/64 network using exit interface S0/1/1.
```sh
R3(config)#ipv6 route 2001:db8:acad:3::/64 s0/1/1
```
- Configure a directly connected IPv6 static route on R3 to the 2001:db8:acad:2::/64 network using exit interface S0/1/1.
```sh
R3(config)#ipv6 route 2001:db8:acad:2::/64 s0/1/1
```
- Issue the command to display only the IPv4 static routes in the routing table of R3.
```sh
R3#show ip route static
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, \* - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is not set
      172.16.0.0/24 is subnetted, 3 subnets
S        172.16.1.0 is directly connected, Serial0/1/1
S        172.16.2.0 is directly connected, Serial0/1/1
S        172.16.3.0 is directly connected, Serial0/1/1
```
Issue the command to display only the IPv6 static routes in the routing table of R3.
```sh
R3#show ipv6 route static
IPv6 Routing Table - default - 8 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, R - RIP, H - NHRP, I1 - ISIS L1
       I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary, D - EIGRP
       EX - EIGRP external, ND - ND Default, NDp - ND Prefix, DCE - Destination
       NDr - Redirect, RL - RPL, O - OSPF Intra, OI - OSPF Inter
       OE1 - OSPF ext 1, OE2 - OSPF ext 2, ON1 - OSPF NSSA ext 1
       ON2 - OSPF NSSA ext 2, a - Application
S   2001:DB8:ACAD:1::/64 \[1/0\]
     via Serial0/1/1, directly connected
S   2001:DB8:ACAD:2::/64 \[1/0\]
     via Serial0/1/1, directly connected
S   2001:DB8:ACAD:3::/64 \[1/0\]
     via Serial0/1/1, directly connected
You have successfully configured and verified IPv4 and IPv6 static routes.
```
##### 15.3 Configure IP Default Static Routes
The basic command syntax for an IPv4 default static route is as follows:
```sh
Router(config)# ip route 0.0.0.0 0.0.0.0 {ip-address | exit-intf}
```
Note: An IPv4 default static route is commonly referred to as a quad-zero route.
The basic command syntax for an IPv6 default static route is as follows:
```sh
Router(config)# ipv6 route ::/0 {ipv6-address | exit-intf}
```
###### 15.3.4 Configure Default static Routes
- Configure an IPv4 default static route on R3 to reach all remote networks. Use the next-hop IPv4 address argument.

R3(config)#ip route 0.0.0.0 0.0.0.0 192.168.1.2
- Configure an IPv6 default static route on R3 to reach all remote networks. Use the next-hop IPv6 address argument.

R3(config)#ipv6 route ::/0 2001:db8:cafe:1::2
#####15.4 Configure Floating Static Routes
- Configure an IPv4 default static route on R3 using the next-hop address 192.168.1.2.
```sh
R3(config)#ip route 0.0.0.0 0.0.0.0 192.168.1.2
```
- Configure an IPv4 default static route on R3 using the next-hop address 10.10.10.1 with an administrative distance of 5.
```sh
R3(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.1 5
```
- Configure an IPv6 default static route on R3 using the next-hop address 2001:db8:cafe:1::2
```sh
R3(config)#ipv6 route ::/0 2001:db8:cafe:1::2
```
- Configure an IPv6 default route on R3 using the next-hop address 2001:db8:feed:10::1 with an administrative distance of 5.
```sh
R3(config)#ipv6 route ::/0 2001:db8:feed:10::1 5
```
##### 15.5 Configure Static Host Routes
Enter Global Configuration mode to configure the following:
- A static IPv4 route to a host at address 209.165.200.238 and an exit interface of s0/1/0.
- A static IPv6 route to a host at address 2001:db8:acad::2 and an exit interface of s0/1/0.
Note: Be sure to use s0/1/0 as the interface designation.
```sh
Branch#configure terminal
Branch(config)#ip route 209.165.200.238 255.255.255.255 s0/1/0
Branch(config)#ipv6 route 2001:db8:acad:2::238 s0/1/0
```
Part 1: Configure IPv4 Static and Floating Static Default Routes
Open configuration window

The PT network requires static routes to provide internet access to the internal LAN users through the ISPs. In addition, the ISP routers require static routes to reach the internal LANs. In this part of the activity, you will configure an IPv4 static default route and a floating default route to add redundancy to the network.
###### 15.6.1 Configure static and default ipv4 and ipv6 routes (PT)
DeviceInterfaceIP Address / Prefix
Edge_Router S0/0/0 10.10.10.2/30
Edge_Router S0/0/0 2001:db8:a:1::2/64
Edge_Router S0/0/1 10.10.10.6/30
Edge_Router S0/0/1 2001:db8:a:2::2/64
Edge_Router G0/0 192.168.10.17/28
Edge_Router G0/0 2001:db8:1:10::1/64
Edge_Router G0/1 192.168.11.33/27
Edge_Router G0/1 2001:db8:1:11::1/64
ISP1 S0/0/0 10.10.10.1/30
ISP1 S0/0/0 2001:db8:a:1::1/64
ISP1 G0/0 198.0.0.1/24
ISP1 G0/0 2001:db8:f:f::1/64
ISP2 S0/0/1 10.10.10.5/30
ISP2 S0/0/1 2001:db8:a:2::1/64
ISP2 G0/0 198.0.0.2/24
ISP2 G0/0 2001:db8:f:f::2/64
PC-A NIC 192.168.10.19/28
PC-A NIC 2001:db8:1:10::19/64
PC-B NIC 192.168.11.4/27
PC-B NIC 2001:db8:1:11::45
Customer Server NIC 198.0.0.10
Customer Server NIC 2001:db8:f:f::10


- Step 1: Configure an IPv4 static default route.
On Edge_Router, configure a directly connected IPv4 default static route. This primary default route should be through router ISP1.

- Step 2: Configure an IPv4 floating static default route.
On Edge_Router, configure a directly connected IPv4 floating static default route. This default route should be through router ISP2. It should have an administrative distance of 5.

- Part 2: Configure IPv6 Static and Floating Static Default Routes
In this part of the activity, you will configure IPv6 static default and floating static default routes for IPv6.

- Step 1: Configure an IPv6 static default route.
On Edge_Router, configure a next hop static default route. This primary default route should be through router ISP1.

- Step 2: Configure an IPv6 floating static default route.
On Edge_Router, configure a next hop IPv6 floating static default route. The route should be via router ISP2. Use an administrative distance of 5.

Close configuration window

- Part 3: Configure IPv4 Static and Floating Static Routes to the Internal LANs
In this part of the lab you will configure static and floating static routers from the ISP routers to the internal LANs.

- Step 1: Configure IPv4 static routes to the internal LANs.
Open configuration window

a.     On ISP1, configure a next hop IPv4 static route to the LAN 1 network through Edge_Router.

b.     On ISP1, configure a next hop IPv4 static route to the LAN 2 network through Edge_Router.

Step 2: Configure IPv4 floating static routes to the internal LANs.
a.     On ISP1, configure a directly connected floating static route to LAN 1 through the ISP2 router. Use an administrative distance of 5.

b.     On ISP1, configure a directly connected floating static route to LAN 2 through the ISP2 router. Use an administrative distance of 5.

Part 4: Configure IPv6 Static and Floating Static Routes to the Internal LANs.
Step 1: Configure IPv6 static routes to the internal LANs.
c.     On ISP1, configure a next hop IPv6 static route to the LAN 1 network through Edge_Router.

d.     On ISP1, configure a next hop IPv6 static route to the LAN 2 network through Edge_Router.

- Step 2: Configure IPv6 floating static routes to the internal LANs.
a.     On ISP1, configure a next hop IPv6 floating static route to LAN 1 through the ISP2 router. Use an administrative distance of 5.

b.     On ISP1, configure a next hop IPv6 floating static route to LAN 2 through the ISP2 router. Use an administrative distance of 5.

If your configuration has been completed correctly, you should be able to ping the Web Server from the hosts on LAN 1 and LAN 2. In addition, if the primary route link is down, connectivity between the LAN hosts and the Web Server should still exist.

Close configuration window

Part 5: Configure Host Routes
Users on the corporate network frequently access a server that is owned by an important customer. In this part of the activity, you will configure static host routes to the server. One route will be a floating static route to support the redundant ISP connections.

Step 1: Configure IPv4 host routes.
Open configuration window

a.     On Edge Router, configure an IPv4 directly connected host route to the customer server.

b.     On Edger Router, configure an IPv4 directly connected floating host route to the customer sever. Use an administrative distance of 5.

Step 2: Configure IPv6 host routes.
a.     On Edge Router, configure an IPv6 next hop host route to the customer server through the ISP1 router.

b.     On Edger Router, configure an IPv6 directly connected floating host route to the customer sever through the ISP2 router. Use an administrative distance of 5.
Edge_Router>en
Edge_Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Edge_Router(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.1
Edge_Router(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.5 5
Edge_Router(config)#ipv6 route ::/0 2001:db8:a:1::1
Edge_Router(config)#ipv6 route ::/0 2001:db8:a:2::1 5
Edge_Router(config)#


Edge_Router>en
Edge_Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Edge_Router(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.1
Edge_Router(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.5 5
Edge_Router(config)#ipv6 route ::/0 2001:db8:a:1::1
Edge_Router(config)#ipv6 route ::/0 2001:db8:a:2::1 5
Edge_Router(config)#








Edge_Router con0 is now available






Press RETURN to get started.













Edge_Router>en
Edge_Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Edge_Router(config)#ip route 198.0.0.10 255.255.255.0 s0/0/0
%Inconsistent address and mask
Edge_Router(config)#ip route 198.0.0.10 255.255.255.255 s0/0/0
Edge_Router(config)#ip route 198.0.0.10 255.255.255.255 s0/0/1 5
Edge_Router(config)#ipv6 route 2001:db8:f:f::10/128 2001:db8:a:1::
Edge_Router(config)#ipv6 route 2001:db8:f:f::10/128 2001:db8:a:1::/64
                                                    ^
% Invalid input detected at '^' marker.
	
Edge_Router(config)#ipv6 route 2001:db8:f:f::10/128 2001:db8:a:1::1
Edge_Router(config)#ipv6 route 2001:db8:f:f::10/128 2001:db8:a:2::1 5
Edge_Router(config)#





Edge_Router con0 is now available






Press RETURN to get started.













Edge_Router>en
Edge_Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Edge_Router(config)#ip route 198.0.0.10 255.255.255.0 s0/0/0
%Inconsistent address and mask
Edge_Router(config)#ip route 198.0.0.10 255.255.255.255 s0/0/0
Edge_Router(config)#ip route 198.0.0.10 255.255.255.255 s0/0/1 5
Edge_Router(config)#ipv6 route 2001:db8:f:f::10/128 2001:db8:a:1::
Edge_Router(config)#ipv6 route 2001:db8:f:f::10/128 2001:db8:a:1::/64
                                                    ^
% Invalid input detected at '^' marker.
	
Edge_Router(config)#ipv6 route 2001:db8:f:f::10/128 2001:db8:a:1::1
Edge_Router(config)#ipv6 route 2001:db8:f:f::10/128 2001:db8:a:2::1 5
Edge_Router(config)#

### 16. Troubleshoot Static and Default Routes

Send a ping from R1 to the G0/0/0 interface on R3.

R1#ping 192.168.2.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.1, timeout is 2 seconds:
U.U.U
Success rate is 0 percent (0/5)
Test the next hop gateway by sending a ping from R1 to the S0/1/0 interface of R2.

R1#ping 172.16.2.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.2.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
Review the routing table on R1.

R1#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, \* - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 5 subnets, 2 masks
S        172.16.1.0/24 \[1/0\] via 172.16.2.2
C        172.16.2.0/24 is directly connected, Serial0/1/0
L        172.16.2.1/32 is directly connected, Serial0/1/0
C        172.16.3.0/24 is directly connected, GigabitEthernet0/0/0
L        172.16.3.1/32 is directly connected, GigabitEthernet0/0/0
S     192.168.1.0/24 \[1/0\] via 172.16.2.2
S     192.168.2.0/24 \[1/0\] via 172.16.2.2
Review the routing table on R2.

R2#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, \* - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 5 subnets, 2 masks
C        172.16.1.0/24 is directly connected, GigabitEthernet0/0/0
L        172.16.1.1/32 is directly connected, GigabitEthernet0/0/0
C        172.16.2.0/24 is directly connected, Serial0/1/0
L        172.16.2.2/32 is directly connected, Serial0/1/0
S        172.16.3.0/24 \[1/0\] via 172.16.2.1
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, Serial0/1/1
L        192.168.1.2/32 is directly connected, Serial0/1/1
Enter configuration mode and configure a static route on R2 to reach the R3 LAN.

R2#configure terminal
R2(config)#ip route 192.168.2.0 255.255.255.0 192.168.1.1
Exit configuration mode and review the routing table on R2.

R2(config)#exit
\*Sep 20 03:10:34.913: %SYS-5-CONFIG\_I: Configured from console by console
R2#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, \* - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 5 subnets, 2 masks
C        172.16.1.0/24 is directly connected, GigabitEthernet0/0/0
L        172.16.1.1/32 is directly connected, GigabitEthernet0/0/0
C        172.16.2.0/24 is directly connected, Serial0/1/0
L        172.16.2.2/32 is directly connected, Serial0/1/0
S        172.16.3.0/24 \[1/0\] via 172.16.2.1
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, Serial0/1/1
L        192.168.1.2/32 is directly connected, Serial0/1/1
S     192.168.2.0/24 \[1/0\] via 192.168.1.1
Send a ping from R1 to the G0/0/0 interface on R3.

R1#ping 192.168.2.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 3/3/4 ms
You have successfully performed troubleshooting on IPv4 static and default routes.Send a ping from R1 to the G0/0/0 interface on R3.

R1#ping 192.168.2.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.1, timeout is 2 seconds:
U.U.U
Success rate is 0 percent (0/5)
Test the next hop gateway by sending a ping from R1 to the S0/1/0 interface of R2.

R1#ping 172.16.2.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.2.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
Review the routing table on R1.

R1#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, \* - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 5 subnets, 2 masks
S        172.16.1.0/24 \[1/0\] via 172.16.2.2
C        172.16.2.0/24 is directly connected, Serial0/1/0
L        172.16.2.1/32 is directly connected, Serial0/1/0
C        172.16.3.0/24 is directly connected, GigabitEthernet0/0/0
L        172.16.3.1/32 is directly connected, GigabitEthernet0/0/0
S     192.168.1.0/24 \[1/0\] via 172.16.2.2
S     192.168.2.0/24 \[1/0\] via 172.16.2.2
Review the routing table on R2.

R2#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, \* - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 5 subnets, 2 masks
C        172.16.1.0/24 is directly connected, GigabitEthernet0/0/0
L        172.16.1.1/32 is directly connected, GigabitEthernet0/0/0
C        172.16.2.0/24 is directly connected, Serial0/1/0
L        172.16.2.2/32 is directly connected, Serial0/1/0
S        172.16.3.0/24 \[1/0\] via 172.16.2.1
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, Serial0/1/1
L        192.168.1.2/32 is directly connected, Serial0/1/1
Enter configuration mode and configure a static route on R2 to reach the R3 LAN.

R2#configure terminal
R2(config)#ip route 192.168.2.0 255.255.255.0 192.168.1.1
Exit configuration mode and review the routing table on R2.

R2(config)#exit
\*Sep 20 03:10:34.913: %SYS-5-CONFIG\_I: Configured from console by console
R2#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, \* - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 5 subnets, 2 masks
C        172.16.1.0/24 is directly connected, GigabitEthernet0/0/0
L        172.16.1.1/32 is directly connected, GigabitEthernet0/0/0
C        172.16.2.0/24 is directly connected, Serial0/1/0
L        172.16.2.2/32 is directly connected, Serial0/1/0
S        172.16.3.0/24 \[1/0\] via 172.16.2.1
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, Serial0/1/1
L        192.168.1.2/32 is directly connected, Serial0/1/1
S     192.168.2.0/24 \[1/0\] via 192.168.1.1
Send a ping from R1 to the G0/0/0 interface on R3.

R1#ping 192.168.2.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 3/3/4 ms
You have successfully performed troubleshooting on IPv4 static and default routes.
# 17. Single-Area OSPF 
#17.1 Skills integration Challenge (PT)
***Picture of addressing table here plz***
 

Background

In this Skills Integration Challenge, your focus is OSPFv2 and OSPFv3 configurations. You will configure IP addressing for all devices. Then you will configure OSPFv2 routing for the IPv4 portion of the network and OSPFv3 routing for the IPv6 portion of the network. One router will be configured with both IPv4 and IPv6 configurations. Finally, you will verify your configurations and test connectivity between end devices.

Note: This activity is graded using a combination of assessment items and connectivity tests. The instructions window will not show your score. To see your score, click Check Results > Assessment Items. To see the results of a specific connectivity test, click Check Results > Connectivity Tests.

Requirements

- Use the following requirements to configure ***RA*** addressing and OSPFv2 routing:
    - IPv4 addressing according to the Addressing Table
```sh
RA(config)#int g0/0
RA(config-if)#ip address 172.31.0.1 255.255.254.0
RA(config-if)#no shutdown
RA(config)#int s0/0/0
RA(config-if)#ip address 172.31.4.1 255.255.255.252
RA(config-if)#no shutdown
```
‏‏‎ ‎
    - Process ID 1
```sh
RA(config)#router ospf 1
```
‏‏‎ ‎
    - Router ID 1.1.1.1
```sh
RA(config-router)#router-id 1.1.1.1
```
‏‏‎ ‎
    - Network address for each interface
```sh
RA(config-router)#network 172.31.0.0 0.0.1.255 area 0
RA(config-router)#network 172.31.4.0 0.0.0.3 area 0
```
‏‏‎ ‎
    - LAN interface set to passive (do not use the default keyword)
```sh
RA(config-router)#passive-interface g0/0
```
‏‏‎ ‎
- Use the following requirements to configure RB addressing, OSPFv2 routing and OSPFv3 routing:
    - IPv4 and IPv6 addressing according to the Addressing Table
```sh
RB(config)#ipv6 unicast-routing 
RB(config)#int g0/0
RB(config-if)#ip address 172.31.2.1 255.255.254.0
RB(config-if)#ipv6 address 2001:db8:1::1/64
RB(config-if)#no shutdown
RB(config-if)#int s0/0/0
RB(config-if)#ip address 172.31.4.2 255.255.255.252
RB(config-if)#int s0/0/1
RB(config-if)#ipv6 address 2001:db8:2::1/64
RB(config-if)#no shutdown
```
‏‏‎ ‎
        - Set the Gigabit Ethernet 0/0 Link Local address to FE80::1  
```sh
RB(config-if)#int g0/0
RB(config-if)#ipv6 address fe80::1 link-local
```
‏‏‎ ‎
    - OSPFv2 routing requirements:
        - Process ID 1
```sh
RB(config)#router ospf 1
```
‏‏‎ ‎
        - Router ID 2.2.2.2
```sh
RB(config-router)#router-id 2.2.2.2
```
‏‏‎ ‎
        - Network address for each interface
```sh
RB(config-router)#network 172.31.2.0 0.0.1.255 area 0
RB(config-router)#network 172.31.4.0 0.0.0.3 area 0
```
‏‏‎ ‎
        - LAN interface set to passive (do not use the default keyword)
```sh
RB(config-router)#passive-interface g0/0
```
‏‏‎ ‎
    - OSPFv3 routing requirements:
        - Enable IPv6 routing (already done in step above)
```sh
RB(config)#ipv6 unicast-routing 
```
‏‏‎ ‎
        - Process ID 1
```sh
RB(config-router)#ipv6 router ospf 1
```
‏‏‎ ‎
        - Router ID 2.2.2.2
```sh
RB(config-rtr)#router-id 2.2.2.2
```
‏‏‎ ‎
        - Enable OSPFv3 on each interface
```sh
RB(config)#int s0/0/1
RB(config-if)#ipv6 ospf 1 area 0
```
‏‏‎ ‎
- Use the following requirements to configure RC addressing and OSPFv3 routing:
    - IPv6 addressing according to the Addressing Table
```sh
RC(config)#int g0/0
RC(config-if)#ipv6 address 2001:db8:3::1/64
RC(config-if)#int s0/0/0
RC(config-if)#ipv6 address 2001:db8:2::2/64
```
‏‏‎ ‎
        - Set the Gigabit Ethernet 0/0 Link Local address to FE80::3
```sh
RC(config-if)#ipv6 address fe80::3 link-local
```
‏‏‎ ‎
    - OSPFv3 routing requirements:
        - Enable IPv6 routing
‏‏‎ ‎
        - Process ID 1
```sh
```
‏‏‎ ‎
        - Router ID 3.3.3.3
```sh
```
‏‏‎ ‎
        - Enable OSPFv3 on each interface
```sh
RB(config-if)#int g0/0
RC(config-if)#ipv6 ospf 1 area 0
```
‏‏‎ ‎
- Configure PCs with appropriate addressing.
    - PCA and PCB IPv4 addressing must use the last assignable address in the IPv4 subnet.
    - PCB and PCC IPv6 addressing must use the second assignable address in the IPv6 network and the link-local FE80 address as the default gateway.
    - Finish the Addressing Table documentation

Note: If OSPFv3 has not converged, check the status of interfaces using the ***show ip ospf interface command.*** Sometimes, the OSPFv3 process needs to deleted from the configuration and reapplied to force convergence.
# test