hostname Router2



username admin password plain admin administrator



ssh-server ip enable

http-server ip enable

telnet-server ip enable



interface FastEthernet0/0.0

  ip address 10.255.100.161/24

  ip napt service ssh 10.255.100.161

  ip napt service http 10.255.100.161

  ip napt service telnet 10.255.100.161

  no shutdown

configure



pki ...