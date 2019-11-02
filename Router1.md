! NEC Portable Internetwork Core Operating System Software

! IX Series IX2025 (magellan-sec) Software, Version 9.5.20, RELEASE SOFTWARE

! Compiled Mar 06-Wed-2019 17:03:09 JST #2

! Current time Nov 01-Fri-2019 19:15:34 JST

hostname Router1



username admin password hash 0C34240482 administrator



logging buffered 10000

logging subsystem ike debug

logging subsystem ip warn

logging timestamp datetime



ip access-list flt-list permit udp src any sport any dest any dport eq 500

ip access-list flt-list permit udp src any sport any dest any dport eq 4500

ip access-list flt-list permit udp src any sport any dest any dport eq 1701

ip access-list flt-list permit ip src any dest 50

ip access-list icmp permit icmp type 8

ip access-list sec-list permit ip src any dest any



ike proposal ikeprop encryption aes-256 hash md5 group 1024-bit

ike policy ike-policy peer 10.255.100.161 key secret-vpn mode aggressive ikeprop



ipsec autokey-proposal secprop esp-aes-256 esp-md5 lifetime time 3600

ipsec autokey-map ipsec-policy sec-list peer 10.255.100.161 default

ipsec local-id ipsec-policy 10.255.100.160/24

ipsec remote-id ipsec-policy 10.255.100.161/24



telnet-server ip enable



device FastEthernet0/0

device FastEthernet0/1

device FastEthernet1/0

device BRI1/0

  isdn switch-type hsd128k



interface FastEthernet0/0.0

  ip address dhcp receive-default

  ip napt enable

  ip napt service ipsec 10.255.100.160 none tcp 50

  ip napt service ping 10.255.100.160 none icmp any

  ip napt service telnet 10.255.100.160 none tcp 23

  ip napt service ike 10.255.100.160 none udp 500

  ip napt service ike2 10.255.100.160 none udp 4500

  ip napt service natt 10.255.100.160 none udp 1701

  ip filter flt-list 30 out

  no shutdown



interface FastEthernet0/1.0

  no ip address

  shutdown



interface FastEthernet1/0.0

  ip address 192.168.255.1/28

  no shutdown



interface BRI1/0.0

  encapsulation ppp

  no auto-connect

  no ip address

  shutdown



interface FastEthernet0/0.1

  encapsulation pppoe

  auto-connect

  no ip address

  ip filter udplist 1 in

  shutdown



interface Loopback0.0

  no ip address

interface Null0.0

  no ip address



interface Tunnel0.0

  tunnel mode ipsec

  ip unnumbered FastEthernet1/0.0

  ip tcp adjust-mss auto

  ipsec policy tunnel ipsec-policy out

  no shutdown