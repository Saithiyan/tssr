# DDNS

DDNS : Le DHCP et le DNS deviennent associés, et échanges entre eux afin d'enregistrer l'ip et le nom de la machine dans la zone directe.

Création de la clé __TSIG__ pour sécuriser les échanges :
```sh
tsig-keygen -a HMAC-SHA512 dns-secret > /etc/bind/secure.key
```
Ajouter le chemin de la secure.key dans le fichier __named.conf__ dans ***/etc/bind/named.conf*** :
```
include "/etc/bind/secure.key";
```
Ajouter dans __DNS master__ dans ***/etc/bind/named.conf.local*** : 
```
allow-update { 127.0.0.1; 192.168.100.0/24; key "dns-secret"; };
```
Copier la clé dans le fichier clé __secure.key__ dans ***/etc/dhcp/secure.key*** :
```sh
 cp /etc/bind/secure.key /etc/dhcp/secure.key
```
Ajouter dans __DHCP__ dans ***/etc/dhcp/dhcpd.conf*** : 
```sh
nano /etc/dhcp/dhcpd.conf
```

```
ddns-updates on;
ddns-update-style standard;
ddns-domainname "one.piece";
ddns-rev-domainname "in-addr.arpa";
include "/etc/dhcp/secure.key";

zone one.piece {
	primary 192.168.100.250;
	key dns-secret;
}

zone 100.168.192.in-addr.arpa {
	primary 192.168.100.250;
	key dns-secret;
}
```

```sh
systemctl restart isc-dhcp-server && systemctl restart named
```

Tester le fonctionnement en faisant la cmd : 
```sh
journalctl -f
```
Puis de changer le nom du pc client.
