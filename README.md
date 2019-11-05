# IX2025-VPN-config
IX Series IX2025 (magellan-sec) Software, Version 9.5.20, RELEASE SOFTWARE

***

Using IX2025, NEC router and connecting on IPsec. 

To connect Router1 with Router2, I use IPsec. 
- Router1 IP-ADDR 10.255.100.160
- Router2 IP-ADDR 10.255.100.161



Router1 is also dhcp server and dispensing IP-ADDR 192.168.0.1/28 - 192.168.0.15/28 .

Router2 is also dhcp server and dispensing IP-ADDR 192.168.0.1/28 - 192.168.0.15/28 .

***
IX2025を使った拠点間VPNのコンフィグです.

10.255.100.0/24のネットワーク内にある2つの拠点をIPsecでつなぎます.
- Router1 IP-ADDR 10.255.100.160
- Router2 IP-ADDR 10.255.100.161



Router1の配下に192.168.0.1/28 - 192.168.0.15/28までを配る.

Router2の配下に192.168.0.1/28 - 192.168.0.15/28までを配る.