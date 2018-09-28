## IP Address Ranges

* IPv4 classes
  - Class A (very large networks): ``0.0.0.0`` to ``127.255.255.255``
  - Class B (medium networks): ``128.0.0.0`` to ``191.255.255.255``
  - Class C (small networks): ``192.0.0.0`` to ``223.255.255.255``
  - Class D (multicasting): ``224.0.0.0`` to ``239.255.255.255``
  - Class E (experimental): ``240.0.0.0`` to ``255.255.255.254``

* Private IPv4 addresses
  - ``10.0.0.0/8``
  - ``172.16.0.0/12``
  - ``192.168.0.0/16``

* Special IPv4 user addresses
  - Loopback: ``127.0.0.0/8``
  - Link-local: ``169.254.0.0/16``
  - TEST-NET: ``192.0.2.0/24``

* IPv6 special addresses
  - Default route: ``::/0``
  - Localhost loopback: ``::1/128``
  - Unique local: ``fc00::/7``
  - Link-local: ``fe80::/10``
  - Multicast: ``ff00::/8``

* Protocol-specific addresses
  - IPv4 EIGRP multicast: ``224.0.0.10``
  - IPv6 EIGRP multicast: ``FF02::A``
  - IPv4 OSPF multicast: ``224.0.0.5``
  - IPv6 OSPF multicast: ``FF02::5``


## Basic Configuration

* Router basic security
```
R1(config)# enable secret password
R1(config)# line con 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# exit
R1(config)# line vty 0 15
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# exit
R1(config)# service password-encryption
```

* Configure / show command history
```
S1(config)# terminal history size 200  ! <-- takes effect on all input lines
S1(config)# line con 0
S1(config-line)# history size 100  ! <-- takes effect on console input line
S1(config-line)# line vty 0 15
S1(config-line)# history size 100  ! <-- takes effect on VTY input lines
S1(config-line)# end
S1# show history
```

* Securely configure SSH
```
S1(config)# ip domain-name company.com
S1(config)# crypto key generate rsa
S1(config)# username admin secret password privilege 15
S1(config)# line vty 0 15
S1(config-line)# transport input ssh
S1(config-line)# login local
S1(config-line)# exit
S1(config)# ip ssh version 2
```

* Configure router IPv4 interface
```
R1(config)# int gigabitethernet 0/0
R1(config-if)# description Link to LAN 1
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shut
```

* Configure router IPv6 interface
```
R1(config)# ipv6 unicast-routing
R1(config)# int gigabitethernet 0/0
R1(config-if)# description Link to R2
R1(config-if)# ipv6 enable
R1(config-if)# ipv6 address 2001:db8:acad:3::1/64
R1(config-if)# ipv6 address FE80::260:3EFF:FE11:6770 link-local
R1(config-if)# no shut
```

* Enable IP on a switch
```
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.10.2 255.255.255.0
S1(config-if)# no shut
S1(config-if)# exit
S1(config)# ip default-gateway 192.168.10.1
```


## Static Routing

* Exit interface static route
```
R1(config)# ip route 172.16.1.0 255.255.255.0 s0/0/0
```

* Next-hop static route
```
R1(config)# ip route 172.16.1.0 255.255.255.0 172.16.2.2
```

* Fully-specified static route
```
R1(config)# ip route 172.16.1.0 255.255.255.0 s0/0/0 172.16.2.2
```

* Default IPv4 static route
```
R1#(config) ip route 0.0.0.0 0.0.0.0 172.16.2.2
```

* Floating IPv4 static route
```
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
R1(config)# ip route 0.0.0.0 0.0.0.0 10.10.10.2 5
```

* Default IPv6 static route
```
R1#(config) ipv6 route ::/0 2001:db8:acad:4::2
```


## Dynamic Routing

* Enable and configure basic RIPv2
```
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 192.168.1.0
R1(config-router)# network 192.168.2.0
R1(config-router)# redistribute static   <-- put this on edge routers
R1(config-router)# default-information originate   <-- put this on edge routers
```

* Configure a passive interface
```
R1(config-router)# passive-interface g0/0
```

* Debug protocols
```
R1# show ip protocols
```

## Routing Debugging

* Show IPv4 routes
```
R1# show ip route
```

* Show specific entry in routing table
```
R1# show ip route 192.168.2.1
```

* Show IPv6 routes
```
R1# show ipv6 route
```

* Show default gateway information
```
R1# show ip route static | begin Gateway
```

* Show static route configuration
```
R1# show run | section ip route
```

* Show IPv4 default static route
```
R1# show ip route static
```

* Show IPv6 default static route
```
R1# show ipv6 route static
```

* Show interface information
```
R1# show ip int b
```

* Show neighbor details
```
R1# show cdp neighbors detail
```

* Check connectivity from specific interface
```
R1# ping 192.168.2.1 source g0/0
```


## Switchport Configuration

* Basic port-security configuration
```
S1(config)# int fa0/19
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
S1(config-if)# switchport port-security maximum 10
S1(config-if)# switchport port-security violation restrict
S1(config-if)# switchport port-security mac-address sticky
```

* Show learned MAC addresses
```
S1# show port-security address
```

* Show port status
```
S1# show int fa0/18 status
```

* Show interface port-security information
```
S1# show port-security interface fastethernet 0/18
```


## VLANs

* Create a VLAN
```
S1(config)# vlan 22
S1(config-vlan)# name Faculty
```

* Assign a port to a VLAN
```
S1(config)# int fa0/18
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 22
```

* Configure trunk links
```
S1(config)# int fa0/18
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 99
S1(config-if)# switchport trunk allowed vlan 10,20,30
```

* Configure legacy inter-VLAN routing (must configure router interfaces, too)
```
S1(config)# vlan 10
S1(config-vlan)# vlan 30
S1(config-vlan)# int f0/11
S1(config-if)# switchport access vlan 10
S1(config-if)# int f0/4
S1(config-if)# switchport access vlan 10
S1(config-if)# int f0/6
S1(config-if)# switchport access vlan 30
S1(config-if)# int f0/5
S1(config-if)# switchport access vlan 30
```

* Configure router-on-a-stick inter-VLAN routing (requires trunking connected switch ports)
```
R1(config)# int g0/0.10
R1(config-subif)# encapsulation dot1q 10
R1(config-subif)# ip address 172.17.10.1 255.255.255.0
R1(config-subif)# int g0/0.30
R1(config-subif)# encapsulation dot1q 30
R1(config-subif)# ip address 172.17.30.1 255.255.255.0
R1(config)# int g0/0
R1(config-if)# no shut
```

* Reset trunk to default state
```
S1(config)# int fa0/18
S1(config-if)# no switchport trunk allowed vlan
S1(config-if)# no switchport trunk native vlan 
```

* Remove VLAN port membership
```
S1(config)# int fa0/18
S1(config-if)# no switchport access vlan
```

* Show VLAN assignment
```
S1# show vlan brief
```


## Access Lists

* Create and apply a numbered standard ACL
```
R1(config)# no access-list 1
R1(config)# access-list 1 deny host 192.168.10.10
R1(config)# access-list 1 permit any
R1(config)# int g0/0
R1(config-if)# ip access-group 1 in
```

* Create and apply a named standard ACL
```
R1(config)# ip access-list standard NO_ACCESS
R1(config-std-nacl)# deny host 192.168.11.10
R1(config-std-nacl)# permit any
R1(config-std-nacl)# exit
R1(config)# int g0/0
R1(config-if)# ip access-group NO_ACCESS out
```

* Configure a named IPv4 extended ACL to allow HTTP traffic
```
R1(config)# ip access-list extended WEB_SURF
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 80
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 443
R1(config-ext-nacl)# exit
R1(config)# ip access-list extended WEB_BROWSE
R1(config-ext-nacl)# permit tcp any 192.168.10.0 0.0.0.255 established
R1(config-ext-nacl)# exit
R1(config)# interface g0/0
R1(config-if)# ip access-group WEB_SURF in
R1(config-if)# ip access-group WEB_BROWSE out
```

* Configure a numbered IPv4 ACL to filter certain FTP traffic between `192.168.11.0/24` and `192.168.10.0/24` networks
```
R1(config)# access-list 101 deny tcp 192.168.11.0 0.0.0.255 192.168.10.0 0.0.0.255 eq ftp
R1(config)# access-list 101 deny tcp 192.168.11.0 0.0.0.255 192.168.10.0 0.0.0.255 eq ftp-data
R1(config)# access-list 101 permit ip any any
R1(config)# interface g0/1
R1(config-if)# ip access-group 101 in
```

* Configure an IPv6 ACL to deny access for a specific LAN
```
R1(config)# ipv6 access-list NO-R3-LAN-ACCESS
R1(config-ipv6-acl)# deny ipv6 2001:db8:cafe:30::/64 any
R1(config-ipv6-acl)# permit ipv6 any any
R1(config-ipv6-acl)# exit
R1(config)# interface s0/0/0
R1(config-if)# ipv6 traffic-filter NO-R3-LAN-ACCESS in
```

* Replace an entry in an ACL
```
R1(config)# ip access-list standard 1
R1(config-std-nacl)# no 10
R1(config-std-nacl)# 10 deny host 192.168.10.10
```

* Apply ACL to VTY ports
```
R1(config)# line vty 0 4
R1(config-line)# login local
R1(config-line)# transport input ssh
R1(config-line)# access-class 21 in
R1(config-line)# exit
R1(config)# access-list 21 permit 192.168.10.0 0.0.0.255
R1(config)# access-list 21 deny any
```

* Debug ACLs
```
R1# show access-lists
```


## Dynamic Host Configuration Protocol (DHCP)

* Configure a basic DHCPv4 server
```
R1(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.9
R1(config)# ip dhcp excluded-address 192.168.10.254
R1(config)# ip dhcp pool LAN-POOL-1
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# dns-server 192.168.11.5
R1(dhcp-config)# domain-name example.com
```

* Configure a router as a DCHPv4 relay
```
R1(config)# int g0/0
R1(config-if)# ip helper-address 192.168.11.6
```

* Configure a router as a DHCPv4 client
```
R1(config)# int g0/1
R1(config-if)# ip address dhcp
R1(config-if)# no shut
```

* Configure a router as a stateless DHCPv6 server
```
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 dhcp pool IPV6-STATELESS
R1(config-dhcpv6)# dns-server 2001:db8:cafe:aaaa::5
R1(config-dhcpv6)# domain-name example.com
R1(config-dhcpv6)# exit
R1(config)# int g0/1
R1(config-if)# ipv6 address 2001:db8:cafe:1::1/64
R1(config-if)# ipv6 dhcp server IPV6-STATELESS
R1(config-if)# ipv6 nd other-config-flag
```

* Configure a router as a DHCPv6 relay agent
```
R1(config)# int g0/0
R1(config-if)# ipv6 dhcp relay destination 2001:db8:cafe:1::6
```

* Configure a router as a stateless DHCPv6 client
```
R1(config)# int g0/1
R1(config-if)# ipv6 enable
R1(config-if)# ipv6 address autoconfig
```

* Configure a router as a stateful DHCPv6 server
```
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 dhcp pool IPV6-STATEFUL
R1(config-dhcpv6)# address prefix 2001:db8:cafe:1::/64 lifetime infinite
R1(config-dhcpv6)# dns-server 2001:db8:cafe:aaaa::5
R1(config-dhcpv6)# domain-name example.com
R1(config-dhcpv6)# exit
R1(config)# int g0/1
R1(config-if)# ipv6 address 2001:db8:cafe:1::1/64
R1(config-if)# ipv6 dhcp server IPV6-STATEFUL
R1(config-if)# ipv6 nd managed-config-flag
```

* Configure a router as a stateful DHCPv6 client
```
R1(config)# int g0/1
R1(config-if)# ipv6 enable
R1(config-if)# ipv6 address dhcp
```

* Debugging DHCPv4 server
```
R1# show run | section dhcp
R1# show run | include no service dhcp
R1# show ip dhcp binding
R1# show ip dhcp server statistics
R1# show ip dhcp conflict
R1# debug ip dhcp server events
```

* Debugging DHCPv6 server
```
R1# show ipv6 dhcp pool
R1# show ipv6 interface
R1# debug ipv6 dhcp detail
```


## NAT / PAT

* Configure static NAT
```
R1(config)# ip nat inside source static 192.168.11.99 209.165.201.15
R1(config)# int s0/0/0
R1(config-if)# ip address 10.1.1.2 255.255.255.252
R1(config-if)# ip nat inside
R1(config-if)# exit
R1(config)# int s0/1/0
R1(config-if)# ip address 209.165.200.1 255.255.255.252
R1(config-if)# ip nat outside
```

* Configure dynamic NAT
```
R1(config)# ip nat pool NAT-POOL1 209.165.200.256 209.165.200.240 netmask 255.255.255.224
R1(config)# access-list 1 permit 192.168.0.0 0.0.255.255
R1(config)# ip nat inside source list 1 pool NAT-POOL1
R1(config)# int s0/0/0
R1(config-if)# ip nat inside
R1(config)# int s0/1/0
R1(config-if)# ip nat outside
```

* Configure PAT address pool
```
R1(config)# ip nat pool NAT-POOL1 209.165.200.256 209.165.200.240 netmask 255.255.255.224
R1(config)# access-list 1 permit 192.168.0.0 0.0.255.255
R1(config)# ip nat inside source list 1 pool NAT-POOL1 overload
R1(config)# int s0/0/0
R1(config-if)# ip nat inside
R1(config)# int s0/1/0
R1(config-if)# ip nat outside
```

* Debugging NAT
```
R1# show ip nat translations
R1# show ip nat translations verbose
R1# clear ip nat statistics
R1# show ip nat statistics
```


## Cisco Discovery Protocol (CDP)

* Enable CDP on an interface
```
S1(conf)# int g 0/1
S1(config-if)# cdp enable
```

* Globally disable CDP
```
R1(conf)# no cdp run
```

* CDP debugging
```
R1# show cdp neighbors
R1# show cdp neighbors detail
R1# show cdp interface
```

## Link Layer Discovery Protocol (LLDP)

* Configure and verify LLDP
```
S1(config)# lldp run
S1(config)# int g 0/1
S1(config-if)# lldp transmit
S1(config-if)# lldp receive
S1# show lldp
```

* Debugging LLDP
```
S1# show lldp neighbors
S1# show lldp neighbors detail
```


## Big Data Protocol (BDP) - A Brian Proprietary Protocol

* Basic configuration
```
R1# conf t
R1(config)# interface big-data
R1(config-if-bd)# blockchain enable
R1(config-if-bd)# blockchain ledger-flags IMMUTABLE DECENTRALIZED THE-FUTURE-IS-NOW
R1(config-if-bd)# blockchain block-size extra-wide-to-handle-big-data
R1(config-if-bd)# exit
R1(config)# service the-uber-of-finance
R1(config)# service iot-devices
R1(config)# service the-cloud
R1(config)# service turing-complete-smart-contracts
R1(config)# banner motd *The Next Big Thing™ is here*
```

* Configuring your VLANs to handle BDP
```
S1(config)# int bd0/18
S1(config-if)# switchport mode incromprehensibly-large-big-data
S1(config-if)# switchport incromprehensibly-large-big-data allowed "synergy >= 5"
S1(config-if)# switchport consensus byzantine 
```

* Debugging your BDP configuration
```
R1# show big-data details | section synergy
```
**Important:** If your synergy levels are too low, try applying Machine Learning.


## Network Time Protocol (NTP)

* Setting the system clock
```
R1# clock set 20:36:00 dec 11 2015
```

* Configure NTP server
```
R1# show clock detail
R1(config)# ntp server 209.165.200.225
R1(config)# end
R1# show clock detail
```

* Set a server as the authoritative server (sets to stratum level 1)
```
R1(config)# ntp master 1
```

* Debugging NTP configuration
```
R1# show ntp associations
R1# show ntp status
```


## Syslog

* Setting log message time stamp
```
R1(config)# service timestamps log datetime
```

* Configuring a syslog client (send levels 4 and lower to syslog server at 192.168.1.3 from interface g0/0's source IP)
```
R1(config)# logging 192.168.1.3
R1(config)# logging trap 4
R1(config)# logging source-interface g0/0
```

* Debugging syslog
```
R1# show logging | include changed state to up
R1# show logging | begin Jun 12 22:35
```


## HTTP Server

* Setup a basic HTTP server using local user databse
```
R1(config)# ip http server
R1(config)# ip http authentication local
```

* Specify an access list to restrict access to the HTTP server (this example only permits host 209.165.202.130)
```
R1(config)# ip access-list standard 20
R1(config-std-nacl)# permit host 209.165.202.130
R1(config-std-nacl)# deny any
R1(config-std-nacl)# exit
R1(config)# ip http access-class 20
```


## Router file systems

* Navigate the file system
```
R1# show file systems
R1# dir
R1# cd nvram:
R1# pwd
```

* Backup running config to a USB
```
R1# dir usbflash0:/
R1# copy run usbflash0:
```

* Copy an IOS image to device
```
R1# show flash0:
R1# copy tftp: flash0:
R1# conf t
R1(config)# boot system flash0://c1900-universalk9-mz.SPA.152-4.M3.bin
R1(config)# exit
R1# copy run start
R1# reload
```

* Password recovery
```
%% break during boot sequence 
rommon 1 > confreg 0x2142
rommon 2 > reset
%% allow boot to complete
Router> en
Router# copy start run
Router# conf t
Router(config)# enable secret mynewpassword
Router(config)# config-register 0x2102
Router(config)# end
Router# copy run start
```


## VLAN Trunk Protocol (VTP)

* **Important note**: ensure that connections between VTP servers/clients/relays are trunked

* Configure a switch as a VTP server
```
S1(config)# vtp mode server
S1(config)# vtp version 2
S1(config)# vtp domain BRIAN
S1(config)# vtp password brians-pw
```

* Configure a switch as a VTP client
```
S2(config)# vtp mode client
S2(config)# vtp domain BRIAN  ! set to the same domain as server
S2(config)# vtp password brians-pw
```

* Configure extended VLAN to be advertised via VTP
```
S1(config)# vtp mode transparent
S1(config)# vlan 2000  ! creating an extended VLAN as an example; this is not required
S1(config-vlan)# end
```

* Check VTP status
```
S1# show vtp status
S1# show vtp password
```


## Dynamic Trunking Protocol (DTP)

* Show DTP information about an interface
```
S1# show dtp interface f0/1
```


## Spanning Tree Protocol (STP)

* Modify the root path cost
```
S1(config)# int fa0/1
S1(config-if)# spanning-tree cost 35  ! to set
S1(config-if)# no spanning-tree cost  ! to removed
```

* Configure PVST+
```
S1(config)# spanning-tree vlan 1 root primary
S2(config)# spanning-tree vlan 1 priority 24576
S3(config)# spanning-tree vlan 1 root secondary
```

* Configure PortFast and BDPU
```
S2(config)# int fa0/11
S2(config-if)# spanning-tree portfast  ! required for proper DHCP operation
S2(config-if)# spanning-tree bpduguard enable  ! disables PortFast on a port if a BPDU is received
```

* Configure the spanning tree mode
```
S1(config)# spanning-tree mode rapid-pvst
S1(config)# int fa0/2
S1(config-if)# spanning-tree link-type point-to-point
S1(config-if)# end
S1# clear spanning-tree detected-protocols
```

* Debugging
```
S1# show spanning-tree
S1# show spanning-tree active
S1# show spanning-tree vlan 99
```


## EtherChannel

* Configure EtherChannel with LACP
```
S1(config)# int range fa0/1-2
S1(config-if-range)# channel-group 1 mode active  ! <-- this mode can be "active" or "passive" on the other end of the channel 
S1(config-if-range)# interface port-channel 1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan 1,2,20
```

* Configure EtherChannel with PAgP: follow LACP configuration, but make one end of the channel ``desirable`` and the other ``auto``

* Debugging EtherChannel
```
S1# show interfaces port-channel 1
S1# show interfaces fa0/1 etherchannel
S1# show etherchannel summary
```


## HSRP

* Configure HSRP on two routers
```
!! main router
R1(config)# int g0/1
R1(config-if)# ip address 172.16.10.2 255.255.255.0
R1(config-if)# standby version 2
R1(config-if)# standby 1 ip 172.16.10.1
R1(config-if)# standby 1 priority 150
R1(config-if)# standby 1 preempt
R1(config-if)# no shut

!! secondary router
R2(config)# int g0/1
R2(config-if)# ip address 172.16.10.3 255.255.255.0
R2(config-if)# standby version 2
R2(config-if)# standby 1 ip 172.16.10.1
R2(config-if)# no shut
```

* Debugging
```
S1# show standby brief
S1# debug standby ?
```


## EIGRP

* Configure EIGRP with IPv4
```
R1(config)# router eigrp 1  ! 1 specifies the process-id that must be same on all routers
R1(config)# eigrp router-id 1.1.1.1  ! if this is not specified, highest address from loopbacks
                                     ! and then physical interfaces will be chosen
R1(config-router)# network 172.16.0.0  ! advertise a network; optional wildcard/subnet mask allowed
R1(config-router)# passive-interface g 0/0  ! don't advertise from this interface; note that this stops
                                            ! both outgoing and incoming traffic, which prevents routers
                                            ! from becoming neighbors
R1(config-router)# auto-summary  ! summarize networks
R1(config-router)# redistribute static  ! tell EIGRP to include static in its EIGRP updates to other routers
```

* Configure EIGRP with IPv6
```
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 router eigrp 2
R1(config-rtr)# eigrp router-id 2.0.0.0
R1(config-rtr)# no shut  ! EIGRP IPv6 is down by default
R1(config-rtr)# exit
R1(config)# int g0/0
R1(config-if)# ipv6 eigrp 2  ! EIGRP IPv6 must be enabled on each interface
```

* Configure load balancing
```
!! Equal cost
R1(config-router)# maximum-paths 2  ! change the default of four equal cost paths; valid range is 1-16

!! Unequal cost
R1(config-router)# variance 2  ! include routes with a metric of less than 2 times the min metric route
                               ! for that destination
R1(config-router)# traffic-share balanced  ! distribute traffic proportionally to the ratios of metrics
                                           ! associated with different routes
R1(config-router)# traffic-share min across-interfaces  ! traffic is sent only across the minimum-cost
                                                        ! path, even with multiple paths in the routing table;
                                                        ! when paired with the variance command, all feasible 
                                                        ! routes are installed in the routing table, which 
                                                        ! decreases convergence times
```

* Modify bandwith metric
```
R1(config)# int s0/0/0
R1(config-if)# bandwith 64  ! bandwith in kilobits
```

* Configure bandwith utilization
```
!! for IPv4
R1(config)# int s0/0/0
R1(config-if)# ip bandwith-percent eigrp 1 40  ! EIGRP uses 50% by default

!! for IPv6
R1(config)# int s0/0/0
R1(config-if)# ipv6 bandwith-percent eigrp 1 40 
```

* Configure Hello and Hold timers
```
!! for IPv4
R1(config)# int s0/0/0
R1(config-if)# ip hello-interval eigrp 1 50
R1(config-if)# ip hold-time eigrp 1 150

!! for IPv6
R1(config)# int s0/0/0
R1(config-if)# ipv6 hello-interval eigrp 1 50
R1(config-if)# ipv6 hold-time eigrp 1 150
```

* Debugging; below commands also have ``ipv6`` variants
```
R1# show run | section eigrp 1
R1# show ip route eigrp  ! verify that a router learned the route to a remote network
R1# show ip eigrp interfaces  ! verify interfaces are enabled for EIGRP
R1# show ip eigrp neighbors  ! verify that a router recognizes its neighbors
R1# show ip eigrp topology
R1# show ip eigrp topology all-links
R1# show ip protocols  ! check the passivity of configured interfaces
```


## OSPF

* Configure for IPv4
```
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1# clear ip ospf process  ! make the router ID change take effect
```

* Using a loopback interface as the Router ID; this is a technique for older IOS versions
```
R1(config)# int loopback 0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
!! DO NOT advertise this network
```

* Methods for adding networks
```
!! specify the wildcard mask
R1(config)# router ospf 10
R1(config-router)# network 172.16.1.0 0.0.0.255 area 0
R1(config-router)# network 172.16.3.0 0.0.0.255 area 0

!! add the ip address of specific interfaces
R1(config)# router ospf 10
R1(config-router)# network 172.16.1.1 0.0.0.0 area 0
R1(config-router)# network 172.16.3.1 0.0.0.0 area 0

!! specify routes to be summarized in advertisements
R1(config)# router ospf 10
R1(config-router)# network 172.16.1.0 0.0.0.255 area 0
R1(config-router)# network 172.16.3.0 0.0.0.255 area 0
R1(config-router)# area 0 range 172.16.0.0 255.255.252.0
```

* Manually configure OSPF cost
```
R1(config)# int s0/0/0
R1(config-if)# ip ospf cost 15625
```

* Configure reference bandwidth
```
R1(config-router)# auto-cost reference-bandwidth 10000 ! default is 100 Mbps; this makes all greater bandwiths
                                                       ! appear to be the same within best-path calculations
!! above line causes the router to distinguish between 100Mbps and Gigabit ethernet
```

* Debugging
```
R1# show ip protocols | section Router ID  ! check for updated RID
R1# show ip ospf interface s0/0/0  !  examine calculated costs
R1# show ip ospf neighbor  ! verify a router has formed an adjacency with a directly-connected router
R1# show ip route ospf
R1# show int s0/0/0 | include BW  !  check for configured bandwith
R1# show int s0/0/0 | include Cost:  ! check for configured cost
R1# show ip ospf interface  ! see details for every OSPFv2-enabled interface
R1# show ip ospf interface brief  ! similar to above, but just shows key information
```


## SNMP

* Basic SNMP configuration
```
R1(config)# snmp-server community communitystring ro SNMP_ACL  ! `ro` specifieis read-only, use `rw` for read-write;
                                                               ! this line also restricts access NMS hosts permitted
                                                               ! by the specified ACL
R1(config)# snmp-server NOC_SNMP_MANAGER  ! document the location of the device
R1(config)# snmp-server contact Wayne World  ! document the system contact
R1(config)# snmp-server host 192.168.1.3 version 2c communitystring  ! specify the recipient of the SNMP trap operations;
                                                                     ! version may also specify auth/noauth/priv
R1(config)# snmp-server enable traps  ! enable traps on this agent; trap level may also be specified here
R1(config)# ip access-list standard SNMP_ACL
R1(config-std-nacl)# permit 192.168.1.3
```

* Securely configure SNMP v3
```
R1(config)# ip access-list standard PERMIT-ADMIN
R1(config-std-nacl)# permit 192.168.1.0 0.0.0.255
R1(config-std-nacl)# exit
R1(config)# snmp-server view SNMP-RO iso included  ! create a view to identify which MIB OIDs the manager can read;
                                                   ! note that this is a requirement to limit messages to read-only;
                                                   ! this example includes the entire ISO tree from the MIB
R1(config)# snmp-server group ADMIN v3 priv read SNMP-RO access PERMIT-ADMIN  ! create a group associated with the created view
R1(config)# snmp-server user BOB ADMIN v3 auth sha authpw priv aes 128 encpw  ! create a user associated with the created
                                                                              ! group; unsecure alternative to `sha` is `md5`;
                                                                              ! snmp management software only supports AES 128
                                                                              ! despite 256-bit support on the router
 
```

* Configure an SNMP manager
```
R1# snmp-server manager
R1# snmp-server manager session-timeout 10  ! destroy non-active sessions after 10 seconds
```

* Verifying SNMP configuration
```
R1# show snmp
R1# show snmp community
R1# show management event  ! show event values that have been configured
R1# show snmp sessions  ! display current SNMP sessions
```


## SPAN

* Configuring local SPAN
```
S1(config)# monitor session 1 source interface fa0/1  ! these commands accept VLANs in place of interfaces, too
S1(config)# monitor session 1 destination interface fa0/2
```

* Verifying local SPAN
```
S1# show monitor
```


## Point-to-Point Connections

* Configure HDLC encapsulation
```
R1(config)# int s0/0/0
R1(config-if)# encapsulation hdlc
```

* Configuring basic PPP
```
R1(config)# int s0/0/0
R1(config-if)# encapsulation ppp
R1(config-if)# compress predictor  ! use predictor compression algorithm, alternative is `stac`
                                   ! compressing data may degrade performance and should not be
                                   ! configured if traffic is already compressed (.zip, .tar.gz, etc.)
R1(config-if)# ppp quality 80  ! link will close if it does not meet 80% quality requirement
```

* Configuring PPP multilink
```
R1(config)# int multilink 1  ! we must create our multilink interface before we can assign a serial interface to it
R1(config-if)# exit
R1(config)# int s0/0/0
R1(config-if)# ppp multilink  ! enable multilink
R1(config-if)# ppp multilink group 1  ! add this interface to the multilink group specified;
                                      ! this must be specified on multiple physical interfaces to 
                                      ! have an effect
```

* Configuring PPP PAP authentication
```
R1(config)# username R2 password myr1pw  ! note that these are the credentials R2 will use to authenticate on R1
R1(config)# int s0/0/0
R1(config-if)# ppp authentication pap  ! `pap`, `chap`, `pap chap`, and `chap pap` are all valid here;
                                       ! the order DOES matter
R1(config-if)# ppp sent-username R1 password myr2pw  ! the R1/myr2pw account MUST be configured on R2
```

* Configuring PPP CHAP authentication
```
R1(config)# int s0/0/0
R1(config-if)# ppp authentication chap
R1(config-if)# ppp chap password mypassword  ! note that the CHAP protocol will implicitly authenticate using this password
                                             ! paired with this router's hostname; as such, we must create a local account
                                             ! on the destination router matching the R1/mypassword username/password combo;
                                             ! if you would like to explicitly set which username is sent via CHAP, execute
                                             ! the `ppp chap hostname newhostname` command in the interface configuration
```

* Troubleshooting PPP serial encapsulation
```
debug ppp
debug ppp packet
debug ppp negotiation
debug ppp error
debug ppp authentication
debug ppp compression
debug ppp cbcp
```

* Troubleshooting serial connections
```
R1# show interfaces serial 0/0/0  ! look at `Encapsulation:` and `Open:` entries
```


## PPPoE

* Configuring basic PPPoE; assume that our ISP router has user `Fred` and password `Barney` configured and is using CHAP auth
```
R1(config)# interface dialer 2  ! ppp tunnel takes place over a virtual dialer interface
R1(config-if)# ip address negotiated
R1(config-if)# encapsulation ppp
R1(config-if)# ppp authentication chap callin
R1(config-if)# ppp chap hostname Fred  ! authentication occurs one-way; the ISP authenticates us
R1(config-if)# ppp chap password Barney
R1(config-if)# ip mtu 1492  ! this is lowered from the default ppp mtu of 1500 to accommodate the pppoe headers
R1(config-if)# dialer pool 1  ! this is used for binding our virtual dialer interface to a physical interface
R1(config-if)# no shut
R1(config-if)# int g0/1
R1(config-if)# no ip address
R1(config-if)# pppoe enable
R1(config-if)# pppoe-client dial-pool-number 1  ! this must match the pool number specified on the dialer interface
R1(config-if)# no shut
```

* Verifying PPPoE
```
! ppp debug commands are still applicable
R1# show run | section interface Dialer2
```


## Generic Routing Encapsulation (GRE)

* Configuring GRE; in the below example, router `R2` must be configured with corresponding commands
```
R1(config)# interface Tunnel0  ! configure the virtual tunnel interface 
R1(config-if)# tunnel mode gre ip
R1(config-if)# ip address 192.168.2.1 255.255.255.0  ! tunnel interface address, typically private
R1(config-if)# tunnel source 209.165.201.1  ! this is the address of the physical serial interface on this router
R1(config-if)# tunnel destination 198.133.219.87 ! this is the address of the physical serial interface on the router
                                                 ! we are tunnelling to
R1(config-if)# router ospf 1
R1(config-router)# network 192.168.2.0 0.0.0.255 area 0
```

* Verifying GRE configuration
```
R1# show ip int b | include Tunnel
R1# show interface Tunnel 0  ! look for `Tunnel source` and `Tunnel protocol/transport` lines
R1# show ip route | include Tunnel
```


## Border Gateway Protocol (BGP)

* Configuring BGP; this single-homed example involves a `Company-A` router and its ISP's router `ISP-1`
```
Company-A(config)# router bgp 65000  ! enable bgp and set the AS number; there can only be one AS number per router
Company-A(config-if)# neighbor 209.165.201.1 remote-as 65001  ! identify the BGP peer and its AS number
Company-A(config-if)# network 198.133.219.0 mask 255.255.255.0  ! enter this network to the BGP table so it is advertised;
                                                                ! mask must be applied when the network is different than
                                                                ! its classful equivalent

ISP-1(config)# router bgp 65001
ISP-1(config-router)# neighbor 209.165.201.2 remote-as 65000
ISP-1(config-router)# network 0.0.0.0  ! the ISP advertises a default route
```

* Verifying BGP configuration
```
R1# show ip route
R1# show ip bgp
R1# show ip bgp summary
```
