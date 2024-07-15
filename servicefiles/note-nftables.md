# NOTE : nftables

#### CONFIG NAT-PAT avec nftables
```sh
table ip NAT-PAT {
	chain nat {
		type nat hook postrouting priority srcnat; policy accept;
		oifname "enp0s3" masquerade;
	}
	chain pat {
		type nat hook prerouting priority dstnat; policy accept;
		iif "enp0s3" tcp dport {80,443} dnat to 192.168.100.252;
		iif "enp0s3" udp dport 53 dnat to 192.168.100.250:53;

#		iif "enp0s3" tcp dport {80,443} ip saddr {192.168.100.33,192.168.100.22} dnat to 192.168.100.252;
#	        iif "enp0s3" tcp dport {80,443} ip saddr != 192.168.100.33 drop;
	}
}
```
###### Remarque : 
- Protocole tcp pour :
	1. http : 80
	2. https : 443 
	3. ssh : 22
	4. telnet : 23
- Protocole udp pour : 
	1. dns : 53
	2. dhcp : 67/68