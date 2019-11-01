! NEC Portable Internetwork Core Operating System Software

! IX Series IX2025 (magellan-sec) Software, Version 9.5.20, RELEASE SOFTWARE]

! Compiled Mar 06-Wed-2019 17:03:09 JST #2

! Current time Oct 31-Thu-2019 18:42:26 JST

!

!hostname Router2timezone +09 00

!

!

!

!

username admin password hash 0C34240482 administrator

!

!

!

!

!logging buffered 100000 cyclic

logging subsystem ike warn

logging subsystem ip warn

logging timestamp datetime

!

!

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

ike proposal ikeprop encryption aes-256 hash md5 group 1024-bit

!

ike policy ike-policy peer any key secret-vpn mode aggressive ikeprop

ike commit-bit ike-policy

ike remote-id ike-policy keyid router2-vpn

!

ipsec autokey-proposal secprop esp-aes-256 esp-md5 lifetime time 3600

!

ipsec dynamic-map auto sec-list secprop ike ike-policy

ipsec commit-bit auto quick-mode

ipsec local-id auto 10.255.100.161/24

ipsec remote-id auto 10.255.100.160/24

!

telnet-server ip enable

!

http-server ip enable

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

  ip address dhcp

  ip filter flt-list 1 in

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

  ip tcp adjust-mss auto

  ipsec policy tunnel auto out

  no shutdown