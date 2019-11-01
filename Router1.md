! NEC Portable Internetwork Core Operating System Software

! IX Series IX2025 (magellan-sec) Software, Version 9.5.20, RELEASE SOFTWARE]

! Compiled Mar 06-Wed-2019 17:03:09 JST #2

! Current time Oct 31-Thu-2019 18:42:26 JST

!

!

hostname Router1

timezone +09 00

!

!

!

!

username admin password hash 0C34240482 administrator

!

!

!

!

!

logging buffered 131072

logging subsystem ike debug

logging subsystem ip info

logging timestamp datetime

!

!       

ip route default Tunnel0.0

ip route 10.255.100.0/24 Tunnel0.0

ip access-list flt-list permit udp src any sport any dest any dport eq 500

ip access-list flt-list permit udp src any sport any dest any dport eq 4500

ip access-list flt-list permit udp src any sport any dest any dport eq 1701

ip access-list flt-list permit ip src any dest any

ip access-list sec-list permit ip src any dest any

!

!

!

ike nat-traversal

!

ike initial-contact always

!

ike proposal ikeprop encryption aes-256 hash md5 group 1024-bit

!

ike policy ike-policy peer 49.135.45.135 key secret-vpn mode aggressive ikeprop

ike keepalive ike-policy 10 3

ike local-id ike-policy keyid router2-vpn

!

ipsec autokey-proposal secprop esp-aes-256 esp-md5 lifetime time 3600

!

ipsec autokey-map ipsec-policy sec-list peer 49.135.45.135 default

ipsec local-id ipsec-policy 10.255.100.160/24

ipsec remote-id ipsec-policy 10.255.100.161/24

!

!

!

!

!

!

!

!

!

telnet-server ip enable

!

!

!

!

!

!

!

!

!

!

!       

!

ip router ospf 1

  router-id 10.255.100.160

  passive-interface FastEthernet1/0.0

  area 0

  network Tunnel0.0 area 0

  network FastEthernet1/0.0 area 0

!

device FastEthernet0/0

!

device FastEthernet0/1

!

device FastEthernet1/0

!

device BRI1/0

  isdn switch-type hsd128k

!

interface FastEthernet0/0.0

  ip address dhcp receive-default

  ip filter flt-list 30 out

  no shutdown

!

interface FastEthernet0/1.0

  no ip address

  shutdown

!

interface FastEthernet1/0.0

  no ip address

  shutdown

!

interface BRI1/0.0

  encapsulation ppp

  no auto-connect

  no ip address

  shutdown

!

interface FastEthernet0/0.1

  encapsulation pppoe

  auto-connect

  no ip address

  ip filter udplist 1 in

  shutdown

!

interface Loopback0.0

  no ip address

!       

interface Null0.0

  no ip address

!

interface Tunnel0.0

  tunnel mode ipsec

  ip unnumbered FastEthernet1/0.0

  ip mtu 1500

  ip tcp adjust-mss auto

  ipsec policy tunnel ipsec-policy out

  no shutdown