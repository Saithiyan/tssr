**[Sommaire](https://github.com/Saithiyan/tssr)**
- [Routeur](https://github.com/Saithiyan/tssr/blob/main/servicefiles/routeur.md)
- [DHCP](https://github.com/Saithiyan/tssr/blob/main/servicefiles/dhcp.md)
- [DNS](https://github.com/Saithiyan/tssr/blob/main/servicefiles/dns.md)
- [DDNS](https://github.com/Saithiyan/tssr/blob/main/servicefiles/ddns.md)
- [SPLIT](https://github.com/Saithiyan/tssr/blob/main/servicefiles/split.md)
- [Serveur-Web](https://github.com/Saithiyan/tssr/blob/main/servicefiles/serveur-web.md)

note : [Config nftables](https://github.com/Saithiyan/tssr/blob/main/servicefiles/note-nftables.md)

---
# DHCP : Configuration

Configurer l'interfaces de sortie sur lesquelles s'applique le service **DHCP-server** :
Editer dans le ***fichier /etc/default/isc-dhcp-server*** :
```sh
nano /etc/default/isc-dhcp-server
```
Exemple:
```
INTERFACESv4="enp0s3 enp0s8"
```
Sauvegarder en backup le fichier **dhcpd.conf** pardefault :
```sh
mv /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.back
```
OU Effacer :
```sh
rm /etc/dhcp/dhcpd.conf
```
Configurer le le service **DHCP-server** dans le fichier ***/etc/dhcp/dhcpd.conf*** :
```sh
nano /etc/dhcp/dhcpd.conf
```

```
authoritative;
default-lease-time 36000;
max-lease-time 72000;

subnet 192.168.100.0 netmask 255.255.255.0 {
        range 192.168.100.1 192.168.100.20;
        option routers 192.168.100.254;
        option domain-name-servers 192.168.100.250,192.168.100.251;
        option domain-name "one.piece";
}
```
Redémarrer le service **DHCP-server** :
```sh
systemctl restart isc-dhcp-server
```

### Exemple :
#####  DDNS : coté DHCP + Class

```
authoritative;
default-lease-time 36000;
max-lease-time 72000;

ddns-updates on;
ddns-update-style standard;
ddns-domainname "chat.on";
ddns-rev-domainname "in-addr.arpa";
include "/etc/dhcp/secure.key";

zone chat.on {
	primary 192.168.100.250;
	key dns-secret;
}
zone 100.168.192.in-addr.arpa {
	primary 192.168.100.250;
	key dns-secret;
}

class "comptable" {
    match if substring (option user-class,0,9)="comptable";
}
class "tech" {
    match if substring (option user-class,0,4)="tech";
}
class "dmzclient"{
    match if substring (option user-class,0,9)="dmzclient";
}
subnet 192.168.100.0 netmask 255.255.255.0 {
	option routers 192.168.100.254;
	option domain-name-servers 192.168.100.250, 192.168.100.251;
	option domain-name "chat.on";

	pool { allow members of "comptable";
		range 192.168.100.10 192.168.100.49;
	}
	pool { allow members of "tech";
		range 192.168.100.50 192.168.100.99;
	}
	pool { allow unknown-clients;
		deny members of "comptable";
		deny members of "tech";
		range 192.168.100.100 192.168.100.150;
		option domain-name "visit.eurs";
	}
}

subnet 192.168.200.0 netmask 255.255.255.0 {
	option routers 192.168.200.254;
	option domain-name-servers 192.168.200.254;
	option domain-name "chat.on";

	pool { allow members of "dmzclient";
		range 192.168.200.100 192.168.200.199;
	}
	pool { allow unknown-clients;
		deny members of "dmzclient";
		range 192.168.200.200 192.168.200.230;
		option domain-name "visiteurs.dmzclient";
	}
}
```

