enable-config

hostname Router2

username admin password plain admin administrator

logging buffered 10000
logging subsystem ike warm
logging subsystem ip warm
logging timestamp datetime

ip route 192.168.1.248/29 Tunnel0.0
ip route 10.255.100.160/24 FastEthernet0/0.0

ip access-list flt-list permit ip src 10.255.100.160/24 dest 192.168.2.248/29
ip access-list sec-list permit ip src any dest any

ike proposal ikeprop encryption aes-256 hash sha
ike policy ike-policy peer 10.255.100.160 key secret-vpn ikeprop
ipsec autokey-proposal secprop esp-aes-256 esp-sha
ipsec autokey-map ipsec-policy sec-list peer 10.255.100.160 secprop
ipsec local-id ipsec-policy 192.168.2.248/29
ipsec remote-id ipsec-policy 192.168.1.248/29

ssh-server ip enable
http-server ip enable
telnet-server ip enable

http-server username admin

ip dhcp profile enable

ip dhcp profile lan1
  dns-server 192.168.2.248
  exit

proxy-dns ip enable

interface FastEthernet1/0.0
  ip address 192.168.2.248/29
  ip dhcp binding lan1
  no shutdown
  exit
  
interface FastEthernet0/0.0
  ip address 10.255.100.161/24
  ip tcp adjust-mss auto
  ip filter flt-list 1 in
  no shutdown
  exit

interface Tunnel0.0
  tunnel mode ipsec
  ipsec policy tunnel ipsec-policy
  ip unnumbered FastEthernet1/0.0
  ip tcp adjust-mss auto
  no shutdown
  exit
!