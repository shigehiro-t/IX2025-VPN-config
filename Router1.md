hostname Router1



username admin password plain admin administrator



ssh-server ip enable

http-server ip enable

telnet-server ip enable



interface FastEthernet0/0.0

  ip address 10.255.100.160/24

  ip napt service ssh 10.255.100.160

  ip napt service http 10.255.100.160

  ip napt service telnet 10.255.100.160

  no shutdown

configure



pki ...