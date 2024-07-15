# DNS

### DNS primary / master :

##### Etape 1 :
Commenter la 3ème ligne ( include ), dans le fichier __named.conf__ dans ***/etc/bind/named.conf*** :
```sh
nano /etc/bind/named.conf
```

##### Etape 2 :
Dans le ***fichier /etc/bind/named.conf.options*** : 
```sh
nano /etc/bind/named.conf.options
```
Décommenter :
```
forwarders {
    1.0.0.1;1.1.1.1;8.8.8.8;
};
```
Facultatif (en fonction des cas) :
```
listen-on-v6 { none; };
listen-on { 127.0.0.1 ; 192.168.100.0/24; };
```
##### Etape 3 :
Dans le ***fichier /etc/bind/named.conf.local*** :
```sh
nano /etc/bind/named.conf.local
```
Création / Déclaration de la __zone directe__ :
__( type primary ou type master )__
```
zone "one.piece" {
	type primary; 
    file "interne-directe-one.piece";

	notify yes;
    also-notify { 192.168.100.251; };
    allow-transfer { 192.168.100.251; };
};
```
Création / Déclaration de la __zone inverse__ :
```
zone "100.168.192.in-addr.arpa" {
    type master;
    file "interne-inverse-one.piece";

	notify yes;
    also-notify { 192.168.100.251; };
    allow-transfer { 192.168.100.251; };
};
```

##### Etape 4 :

-  **FICHIER ZONE DIRECTE :** 
Copier le Model __db.empty__ qui se trouve dans ***/etc/bind/db.empty*** pour créer les fichiers de zone

Créer le fichier de zone directe dans ***/var/cache/bind/zone-directe.dns*** :
```sh
cp /etc/bind/db.empty /var/cache/bind/interne-directe-one.piece
```

```sh
nano /var/cache/bind/interne-directe-one.piece
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
@       IN      A       192.168.100.252
ddns-srv1       A       192.168.100.250
dns-srv2        A       192.168.100.251
web-srv3        A       192.168.100.252
www             CNAME   web-srv3
```


-  **FICHIER ZONE INVERSE :**

Créer le fichier de zone inverse dans ***/var/cache/bind/zone-inverse.dns*** :
```sh
cp /var/cache/bind/interne-directe-one.piece /var/cache/bind/interne-inverse-one.piece
```

```sh
nano /var/cache/bind/interne-inverse-one.piece 
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

---
### DNS secondary / slave : 
##### Permet la haute disponibilités grâce à un cluster/grappe de server

Même procédure pour le fichier __named.conf__ et __named.conf.options__ dans ***/etc/bind/*** :

Dans ***/etc/binf/named.conf.local*** :
```sh 
nano /etc/bind/named.conf.local 
```
__( type secondary ou type slave )__
__( masters ou primaries )__
```
zone "one.piece" {
    type secondary;
    file "/var/cache/bind/slave-directe-one.piece";
    masters { 192.168.100.250; };
};

zone "100.168.192.in-addr.arpa" {
    type slave;
    file "/var/cache/bind/slave-inverse-one.piece";
    primaries { 192.168.100.250; };
#   masters { 192.168.100.250; };
};
```

```sh
systemctl restart named
```
```sh
named-checkconf -z
```
