**[Sommaire](https://github.com/Saithiyan/tssr)**
- [Routeur](https://github.com/Saithiyan/tssr/blob/main/servicefiles/routeur.md)
- [DHCP](https://github.com/Saithiyan/tssr/blob/main/servicefiles/dhcp.md)
- [DNS](https://github.com/Saithiyan/tssr/blob/main/servicefiles/dns.md)
- [DDNS](https://github.com/Saithiyan/tssr/blob/main/servicefiles/ddns.md)
- [SPLIT](https://github.com/Saithiyan/tssr/blob/main/servicefiles/split.md)
- [Serveur-Web](https://github.com/Saithiyan/tssr/blob/main/servicefiles/serveur-web.md)

note : [Config nftables](https://github.com/Saithiyan/tssr/blob/main/servicefiles/note-nftables.md)

---
# Routeur : Configuration

### Interfaces :

Configurer les interfaces :
```sh
nano /etc/network/interfaces
```
Modifier le fichier avec :
```
auto enp0s3
iface enp0s3 inet DHCP

auto enp0s8
iface enp0s8 inet static
	address 192.168.100.254/24
```
Redémarrer les services networking.service :
```sh
systemctl restart networking.service
```
Pour vérifier la configuration :
```sh
ip -c -br a
```

### Routage :

Il y a deux façons d'activer le routage sur linux :
**1. SOIT :**
Décommenter la ***ligne 28 du fichier /etc/sysctl.conf*** :

Persiste pour activer le routage :
```sh
sysctl -p
```

**2. SOIT :**
Remplacer le **0** par un **1** dans le ***fichier /proc/sys/net/ipV4/ip_forward*** :
```sh
echo 1 > /proc/sys/net/ipV4/ip_forward
```


### NAT avec nftables

Activation et Configuration du Nat :
Il y a deux façons de configurer le NAT sur linux avec nftables :
**1. SOIT _via les commandes_ :**
Création d'une table nommé **" NAT-PAT "** :
```sh
nft add table ip NAT-PAT
```
Création d'une chaine nommé **" nat "** dans la table **" NAT-PAT "** :
```sh
nft add chain NAT-PAT nat "{ type nat hook postrouting priority srcnat;}"
```
Création d'une règle dans la chaine **" nat "** de la table **" NAT-PAT "** :
Masque la sortie sur l'interface enp0s3
```sh
nft add rule NAT-PAT nat oifname enp0s3 masquerade
```
Ajout des règles défini à la suite, dans le fichier ***/etc/nftable.conf*** :
```sh
nft list ruleset >> /etc/nftables.conf
```

###### Remarque : 
Pour lister les règles définis :
```sh
nft list ruleset
```
Pour effacer les règles définis en le mémoire :
```sh
nft flush ruleset
```

**1. SOIT _manuellement_ :**
Editer le ***fichier /etc/nftfables.conf*** :
```sh
nano /etc/nftables.conf
```
Ajout des **règles NAT** :
```sh
table ip NAT-PAT {
	chain nat {
		type nat hook postrouting priority srcnat; policy accept;
		oifname "enp0s3" masquerade;
	}
}
```
