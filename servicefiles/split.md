**[Sommaire](https://github.com/Saithiyan/tssr)**
- [Routeur](https://github.com/Saithiyan/tssr/blob/main/servicefiles/routeur.md)
- [DHCP](https://github.com/Saithiyan/tssr/blob/main/servicefiles/dhcp.md)
- [DNS](https://github.com/Saithiyan/tssr/blob/main/servicefiles/dns.md)
- [DDNS](https://github.com/Saithiyan/tssr/blob/main/servicefiles/ddns.md)
- [SPLIT](https://github.com/Saithiyan/tssr/blob/main/servicefiles/split.md)
- [Serveur-Web](https://github.com/Saithiyan/tssr/blob/main/servicefiles/serveur-web.md)

note : [Config nftables](https://github.com/Saithiyan/tssr/blob/main/servicefiles/note-nftables.md)

---
# SPLIT: interne et externe

Dans le fichier ***/etc/bind/named.conf.local***
Filtrage par ACL :
```
acl LAN { 127.0.0.0/8; 192.168.100.0/24; };
acl WAN { 0.0.0.0/0; };
```
Pour __zone directe__ dans la __view interne__ :
```
view "interne" {
	match-clients { LAN; };
	recursion yes;
	
	zone "one.piece" {
		type primary;
		file "interne-directe-one.piece";
	
		notify yes;
		also-notify { 192.168.100.251; };
		allow-transfer { 192.168.100.251; };
		allow-update { 127.0.0.1; 192.168.100.0/24; key "dns-secret"; };
	
	};
	
	zone "100.168.192.in-addr.arpa" {
		type primary;
		file "interne-inverse-one.piece";
	
		notify yes;
		also-notify { 192.168.100.251; };
		allow-transfer { 192.168.100.251; };
		allow-update { 127.0.0.1; 192.168.100.0/24; key "dns-secret"; };
	};
};
```

```
view "externe" {
    match-clients { WAN; };
    recursion no;

    zone "one.piece" {
        type master;
        file "externe-directe-one.piece";

		notify no;
#		also-notify { none; };
        allow-transfer { none; };
		allow-update { none; };
    };
};
```


On crée les fichiers de config des __zones directes__ et __inverses__ :
-  __FICHIER ZONE EXTERNE-DIRECTE__ :
```
 cp /etc/bind/interne-directe-one.piece /var/cache/bind/externe-directe-one.piece
```

```sh
nano /var/cache/bind/externe-directe-one.piece 
```

```
$ORIGIN @
$TTL    86400
@       IN      SOA     ddns-srv1 root.localhost. (
                        2406171         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                          86400 )       ; Negative Cache TTL

@       IN      NS      ddns-srv1
@       IN      NS      dns-srv2
@       IN      A       192.168.XX.XX
ddns-srv1       A       192.168.XX.XX
dns-srv2        A       192.168.XX.XX
web-srv3        A       192.168.XX.XX
www             CNAME   web-srv3
```


-  __FICHIER ZONE EXTERNE-INVERSE__ :

```sh
cp /var/cache/bind/interne-inverse-one.piece /var/cache/bind/externe-inverse-one.piece
```

```sh
nano /var/cache/bind/externe-inverse-one.piece 
```

```
$ORIGIN @
$TTL    86400
@       IN      SOA     ddns-srv1 root.localhost. (
                        2406171         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                          86400 )       ; Negative Cache TTL
;
@       IN      NS      ddns-srv1.one.piece.
@       IN      NS      dns-srv2.one.piece.
252             PTR     one.piece.
250             PTR     ddns-srv1.one.piece.
251             PTR     dns-srv2.one.piece.
252             PTR     web-srv3.one.piece.
```

```sh
systemctl restart named
```

```sh
named-checkconf -z
```
