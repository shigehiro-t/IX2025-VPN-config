hostname Router2
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
logging buffered 10000
logging subsystem ike warn
logging subsystem ip warn
logging timestamp datetime
!
!
ip route 10.255.100.0/24 FastEthernet0/0.0
ip access-list esp-list permit tcp src any sport any dest any dport eq 50
ip access-list flt-list permit ip src 10.255.0.0/16 dest 10.255.0.0/16
ip access-list sec-list permit ip src any dest any
ip access-list udp-list permit udp src any sport eq 500 dest any dport eq 500
ip access-list udp2-list permit udp src any sport eq 4500 dest any dport eq 4500
!
!
!
ike nat-traversal
!
ike proposal ikeprop encryption aes-256 hash sha
!
ike policy ike-policy peer 10.255.100.160 key secret-vpn ikeprop
ike remote-id ike-policy address 10.255.100.160
!
ipsec autokey-proposal secprop esp-aes-256 esp-sha
!
ipsec autokey-map ipsec-policy sec-list peer 10.255.100.160 secprop
ipsec local-id ipsec-policy 10.255.100.161
ipsec remote-id ipsec-policy 10.255.100.160
!
!
!
!
!
!
!
!
proxy-dns ip enable
!
telnet-server ip enable
!
ssh-server ip enable
!
http-server username admin
http-server ip enable
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
watch-group host 10
  event 10 ip unreach-host 10.255.100.160 Tunnel0.0
  probe-timer restorer 60
  probe-timer variance 60
!
network-monitor host enable
!
!
ip dhcp profile enable
!
ip dhcp profile lan1
  assignable-range 192.168.2.2 192.168.2.49
  default-gateway 192.168.2.248
  dns-server 192.168.2.248
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
  ip address 10.255.100.161/24
  ip tcp adjust-mss auto
  ip filter flt-list 1 in
  ip filter udp-list 2 in
  ip filter udp2-list 3 in
  ip filter esp-list 4 in
  no shutdown
!
interface FastEthernet0/1.0
  no ip address
  no shutdown
!
interface FastEthernet1/0.0
  ip address 192.168.2.248/24
  ip dhcp binding lan1
  no shutdown
!
interface BRI1/0.0
  encapsulation ppp
  no auto-connect
  no ip address
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
  ip unnumbered FastEthernet0/0.0
  ip tcp adjust-mss auto
  ipsec policy tunnel ipsec-policy out
  no shutdown