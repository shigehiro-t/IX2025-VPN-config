hostname Router1



username admin password hash 0C34240482 administrator



logging buffered 10000

logging subsystem ike debug

logging subsystem ip warn

logging timestamp datetime



ip route 10.255.100.0/24 Tunnel0.0

ip route 10.255.100.161/24 Fastether0/0.1



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

ipsec local-id ipsec-policy 192.168.255.1/28

ipsec remote-id ipsec-policy 192.168.255.2/28



telnet-server ip enable

telnet-server ip access-list console



http-server ip enable



interface FastEthernet1/0.0

  ip address 192.168.255.1/28

  no shutdown

configure



interface FastEthernet0/0.1

  ip filter ike-list 1 in

  ip filter ike2-list 1 in

  ip filter natt-list 1 in

  ip filter ipsec-list 1 in

  ip filter -list 1 in

  ip address 10.255.100.160/24

  ip napt enable

  ip napt service ipsec 10.255.100.160 none tcp 50

  ip napt service ping 10.255.100.160 none icmp any

  ip napt service telnet 10.255.100.160 none tcp 23

  ip napt service ike 10.255.100.160 none udp 500

  ip napt service ike2 10.255.100.160 none udp 4500

  ip napt service natt 10.255.100.160 none udp 1701

  ip filter flt-list 30 out

  no shutdown

configure



interface Tunnel0.0

  tunnel mode ipsec

  ip unnumbered FastEthernet0/0.1

  ip tcp adjust-mss auto

  ipsec policy tunnel ipsec-policy out

  no shutdown

configure



!assignable-range 192.168.255.2 192.168.255.7

!ip dhcp enable