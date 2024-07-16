# All notes some cmd

Changement mot de passe sous Linux :
sudo passwd root

En Root : passwd root

ID : donne le 'UID' et notre 'GID'
	  0 étant l'identifiant le plus fort dans le système (admin)
	  de 0 à 999 == les identifiants ‘systèmes’
	  à partir de 1000 == es identifiants ‘utilisateurs’ 

Pour voir les interfaces : 	
→ ip -br -c -4 a && ip -c -br  -4 link
→ ip -4 -br
→ ip -4 -br mr
→ ip -4 -br ma

→ ip a flush enp0s8   initialisez la carte réseau
→ ip ad flush dev enp0s3
→ dhclient -v enp0s3 

→ init 0 == éteindre le srv linux
→ init 6 == restart le srv Linux

ps aux : la commande pour voir les taches/ processus
ps aux | less : si tu veux naviguer avec les flèches
ps aux | grep <word> : pour avoir le processus recherché par un mot
kill <PID du processus> : pour désactiver ou Kill le processus

######################################################################
ZONE 

zone directe :  permet d'associer un nom de domaine à une ip;
zone inverse : permet d'associer une ip à nom de  domaine
le split dns : permet d'attendre le dns depuis l'extérieur

######################################################################


Videz le cache ARP : arp -d
→ root@router:/# hostnamectl set-hostname router

Redémmarrez le service réseau et mettre en UP si la carte de réseau est en DOWN:

systemctl restart networking
ip link set up dev ens33 ----> le mettre en UP

	
avahi-daemon : le programme qui permet de gerer la gestion multicast DNS sous Linux.
               Mets la machine en écoute sur le port UDP 5353, sur le réseau 224.0.0.251

ss -anu		: Socket Statistic → avoir les connexions réseau, en donnant Addres et Porrt qui sont en activités
					: l'option anu = All Numeric U → pour protocol UD
					: en faisant cette commande on peut voir quelle service tourne, exemple = dchp avec le port 67

→ scp root@192.168.100.251:/etc/bind/secure.key .
→ scp stagiare@172.23.12.65:/home/stagiaire/Bureau/Circus/* .
→ scp -r stagiaire@172.23.12.65:/home/stagiaire/Bureau/Circus/* .


################# #################

<VirtualHost *:80>
ServerName www.pluton.labo
ServerAlias pluton.labo
DocumentRoot /var/www/html/pluton
</VirtualHost>

www.pluton.labo 

################# #################

######################################
##     Résolution Bullshit          ##
######################################

journalctl -xe					==== on observe l'heure précise (18:31) s'il y a des erreurs ou pas
journalctl -u isc-dhcp-server.service -e
journalctl -u named.service -e
journalctl -u named.service -f                  ==== suivre l'action en temps réel
journalctl -u bind9.service

######################################

Le multicast DNS va diffuser le hostname avec le suffixe ‘local’
	
→ /etc/services → List des port 
→ lsblk
→ htop
→ df -h

→ su - root
→ su -l
→ ssh -l pollux x.x.x.x
→ ssh pollux@x.x.x.x p2222
→ ssh -P222 nom_user@ip public
→ installation du paquet net-tools qui n'est maintenant plus installé par défaut: sudo apt-get install net-tools

→ touch : permet de créer un fichier vide, touch tako.txt
→ echo bonjour > lettre.txt : va remplacer tout le texte contenu dans le fichier, par la chaine de caractère ‘bonjour’, si le texte n'existe pas il sera créé

→ echo monsieur>> lettre.txt : va ajouter la chaine de caractère ‘monsieur, à la suite du contenu du fichier lettre.txt’
→ mkdir photo.hivers -p : permet de créer l'arborescence photo, contenant le dossier enfant ‘hivers’

→ rm : supprimer un dossier ou fichier
→ rm -r : supprimer un dossier ou fichier avec tout son contenu ( -r = recursif )
 
→ groupadd : groupadd stagiaires
→ chgrp : permet changement sur le fichier ou dossier == chgrp stagiaire courrier.txt,, le fichier txt ou dosser sera donné à stagiaire == c'est un transfert
→ chown : permet changement de propriétaire sur le fichier ou dossier
                chown jean:stagiaire courrier-jean.txt : change le propriétaire, en 'jean' et le groupe en ‘stagiaire’ de l'objet ‘courrier’

→ which  : which --> voir le chemin entier de la commande tapé
→ whoami : Qui suis-je
→ logout : femer la session de travail (local ou distant) = exit = CTRL D
→ last : on peut voir qui s'était connecté, quand et par quel moyen
→ lastlog : savoir qui s'était connecté la dernière fois, parmis tous les utilisateurs
→ En mode console on peut ouvrir plusieurs session, de faire un changement de session sans se déconnecter : ALT F2 ou ALT F3
→ pts0 : (pseudo terminal) c'est une connexion à distance, accès par réseau “ssh ou putty”
→ tty1 (terminal type console) : connexion en local directment sur la machine
→ who : pour savoir qui est connecté
             
→ Autorisation connexion ssh root →  vi /etc/ssh/sshd_config :  ajoutez PermitRootLogin yes , quittez sauvegarde et relancez le service : systemctl restart ssh.sevrice
→ Sudo : Substitute / Switch User & do = Basculer en user root pour la durée d'une action, exemple sudo nano /etc/.....
→ SU : Switch User & do = Basculer en user root, tout le temps nécessaire. Pour quittez tapez exit ou logout 

→ apt-get install “nom du paquet à installer : sudo”
→ adduser “sudoers” : adduser pollux sudo, pollux appartient au membre du groupe sudo, se déconnecter et reconnecter
→ adduser : adduser tako pollux, tako fait parti du membre pollux,, id tako === pollux apparait
→ groups : sur le compte user faire cette commande et si sudo apparait cela signifie que vous êtes membre du groupe du sudo ou pour savoir à quel grp il appartient
→ id "id user" : un user a un uid unique, en revanche il peut appartenir ou être membre du gid (groupeid)
→ /etc/skel : est un modèle de configuration (compte)
→ addgroup : création d'un group exemple : addgroup labo
→ ajout un user au groupe : adduser tako labo. Pour savoir si tako appartient au groupe labo : groups tako 
→ usermod : modification d'un user, mettre / affecté celui-ci dans son groupe pincicipal labo, en gros le user change de groupe principal
	usermod -g labo tako
	exemple : id tako → gid1001(membre du grp) puis gid1002

→ chmod
→ chmod 777 * -R stagiaire/
  
  R W X	  |  R - X  | R--
  4 2 1      4   1    4
    7          5      4

→ Files      666
→ Directory  777
	
→ remettre tako dans son groupe : adduser tako tako
→ lister les user : cat /etc/passwd
→ lister les user : cat /etc/group
→ cat /etc/passwd | awk -F: '{print $ 1}'

→ deluser tako , le compte est supprimer mais pas son groupe
→ delgroup tako, le groupe est supprimer

→ modification de groupe / renommer le group : groupmod -n stagiaires labo → le groupe labo est renommé le groupe stagiaires

→ root@kai:/#passwd → attention on est entrain de changer notre mot de passe donc faire Attention !!!!!!
→ root@kai:/#passwd pollux → changement password du user

→ root@kai:/#passwd -S → savoir l'état du compte du user
→ root@kai:/#chfn pollux → changé les paramètres du user, son bureau, téléphone etc..
→ 


##################################################################################  
Création de la clé secrète / process Obsolète :

→ dnssec-keygen    ========== création une paire de clé
→ /etc/bind
→ dnssec-keygen -a HMAC-MD5 -b 128 -r /dev/urandom -n USER maj-key  ===== resultat Kmaj-key.+157+04870
→ maj-key === est le nom de clé  

##################################################################################    

Création d'une clé servant à valider les mises à jours dynamiques :
→ tsig-keygen update > /etc/bind/update.key


Copy clé du srv1 vers srv2 :
→ /etc/bind/scp root@192.168.100.250:/etc/bind/secure.key .


in soa = start of authority
serial = numéro de version (besoin d'incrémenter le premier pour maj le second serveur)
refresh =  actualise tous les x temps en seconde
retry =  quand dns1 est down temps avant tentative de contact 
expire =  temps au bout duquel la zone secondaire arrete de repondre si le primaire est down
ttl = time to live (trop court beaucoup de requête, trop long risque en cas de changement d’ip  moyenne <24h)
(ces valeurs sont à remplir qu’en cas de haute dispo)

in ns = name server
in mx= messagerie
in a = adresse ipv4      aaaa = ipv6
in cname = alias, repointe sur la ligne désigné
in ptr = pointeur

-------------------------------------------------------------------------------------

################################################################# 
 NEXTCLOUD
 sudo curl -sSL https://raw.githubusercontent.com/nextcloud/nextcloudpi/master/install.sh | sudo bash    # 
 #################################################################

Connaître la version de debian :
lsb_release -a

Connaître :
uname -a

Connaitre IP
ip a

Paquet : iftop
affichage d'informations sur l'utilisation de la bande passante d'une interface réseau
Le programme iftop est à l'utilisation du réseau ce que top(1) est à l'utilisation du processeur.
Il écoute le trafic sur une interface réseau et affiche une table de l'utilisation de la bande passante par paires d'hôtes.
Il est ainsi facile de répondre à la question : « Pourquoi la connexion Internet est-elle si lente ? »

Comment savoir la version linux: 
lsb_release -a

Utilisez la commande ps pour répertorier tous les processus et filtrer la sortie à l'aide de grep pour vérifier si le processus SSH est en cours d'exécution:
ps aux | grep sshd

vérifier les ports ouverts consiste à vérifier le fichier de port:
lsof -i

→ lsblk
→ parted /dev/sda “rm 1”
→ lsblk
→ parted /dev/sda “mklabel gpt”
→ parted /dev/sda “mkpart primary ext4 1M -1”
→ mkfs -t ext4 /dev/sda1
→ mkdir /mnt/ssd
→ blkid → relevé le  copié et collé dans fstab
→ nano /etc/fstab → ajouté →  /mnt/ssd ext4 defaults, nofail 0 1 
→ mount -a
→ systemctl -daemon-reload
→ df -Th
 
→  apt install docker.io docker-compose -y
→  
 
→  route print -4 (FW)
→  netsh firewall set opmode disable ou enable
→   
 
→ sudo systemctl status mariadb
→ sudo systemctl status nginx
→ sudo whereis nginx
→ sudo systemctl stop
→ sudo systemctl start


#########################################################################################

DHCP / NAT-PAT :

/etc/network/interfaces:

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto enp0s3
iface enp0s3 inet static
        address 192.168.100.254/24

# Network DMZ
auto enp0s8
iface enp0s8 inet static
        address 192.168.200.254/24

# Network WAN HOMEWORK
auto enp0s9
iface enp0s9 inet dhcp

# Activation du routage IPV4
up echo 1 > /proc/sys/net/ipv4/ip_forward

--------------------------------------------------------------------------------

                               Activation du NAT:
nft add table NAT
nft add chain NAT INTERNET "{ type nat hook postrouting priority -100 ; }"
nft add rule NAT INTERNET ip saddr 192.168.100.0/24 iif enp0s3 oif enp0s9

nft list ruleset

nft list ruleset > /etc/nftables.conf
systemctl enable nftables.service

--------------------------------------------------------------------------------
                              Installation du DHCP
Une verification :
ip -c -br -4 address && ip -c -br -4 link
Deux commande à la fois seulement si la première est réussie il exécutera la deuxième, && est un
élément de contrôle

apt install isc-dhcp-server -y


Message d'erreur, c'est normale

Pour ne pas avoir un conflit de dhcp étant donné que la carte enp0s9 reçoit son adresse ip via dhcp
de la box on active le service dhcp que sur l’enp0s3 qui donne vers notre LAN ou sur toutes nos
interfaces LAN si on le souhaite en précisant le DHCP Pool :

nano /etc/default/isc-dhcp-server

INTERFACEv4="enp0s3"
#INTERFACEv6=""
--------------------------------------------------------------------------------

                                    Création des Pool dhcp
nano /etc/default/isc-dhcp-server
cd /etc/dhcp pour accéder au dossier DHCP

rm dhcpd.conf il faut pas modifier une config déjà mise en place, vaut mieux supprimer et refaire
notre propre directory avec notre config

nano dhcpd.conf

authoritative ;
#Pool d'adresse pour la LAN 'Rouge'
subnet 192.168.100.0 netmask 255.255.255.0 {
range 192.168.100.10 192.168.100.50;
option routers 192.168.100.254;
option domain-name-servers 8.8.8.8;
option domain-name "tssr.labo";

}


systemctl restart isc-dhcp-server

En cas d’erreur on peut vérifier avec la commande :

journalctl -xe et on peut constater les erreurs détecter sont indiquées par Num de ligne
nano dhcpd.conf -l pour voir les num de ligne et on rectifie l’erreur

--------------------------------------------------------------------------------

                                       Regle PAT

nft add chain NAT APPLICATION "{type nat hook prerouting priority 0;}"

nft add rule NAT APPLICATION iif enp0s9 tcp dport 3389 dnat to 192.168.100.250:3389

Puis on vérifier on se connectant sur l’adresse IP de l’interface WAN de notre routeur debian
distribuer par le DHCP de notre box
Connexion à distance sur 192.168.1.51 :3389 = 192.168.100.250



--------------------------------------------------------------------------------

                                       Regle interdiction accès à IP de la machine physique

flush ruleset

table inet filter {
	chain input {
		type tilter hook input priority filter;
		ip saddress (ip physique de la maison) tcp dport 22 iif "enp0s9" accept;
		tcp dport 22 iif "enp0s9" drop; 
	}
	chain forward {
		type tilter hook forward priority filter;
		tcp dport 22 iif "enp0s9" drop; 
	
	}
	chain output {
		type tilter hook output priority filter;
	}
}

table ip NAT-PAT
	chain NAT {
		type nat hook postrouting priority srcnat; Policy accept;
		oif "enp0s9" masquerade		
 	}
	chain PAT {
		type nat hook prerouting priority dstnat; Policy accept;
		iif "enp0s9" tcp dport 80 dnat to 192.168.200.250:80;
		iif "enp0s9" tcp dport 2222 dnat to 192.168.200.250:22;
		iif "enp0s9" tcp dport 3389 dnat to 192.168.100.250:3389; 
}

--------------------------------------------------------------------------------------------

Putty === /dev/ttyUSB0

pour se connecter au SWITCH
--------------------------------------------------------------------------------------------

Fonctionne par des échanges en diffusion (Broadcast) - presque tout le temps

Comment ça fonctionne :

Un client a besoin : Faire une requête
Port 68 du client



x.x.x.:67 ====> 255.255.255.255:68

D : Discover
O : Offer
R : Rquest
A : Aknoxledgement


Il s'agit d'une offre limité dans le temp : Le bail à une durée de vie, 
=== à 87,5% du temps du bail, le client devrait faire le renouvellement
=== à 50% le client devrait faire le renouvellement (par anticipitation)
=== c'est un unicast

Qu'est ce qu'on faire avec un serveur DHCP?

 -- IP
   -- Mask
     -- Gateway
       -- DNS

Les OPTIONS : 255 options pré-définis
              la plus connue  : n°3 => le route par défaut 
== on peut aussi créer ses propres options

#########################################################################################

lsblk
>> mkfs. 
formatage : mkfs -t ext4 /dev/sdb1    ---> ext4 est le système de fichier par défaut sur Linux 
>> mount /dev/sdb1 /mnt --> monter temporairement -- 
>> ls -l /mnt
>>mkdir /mnt/doc
>> ls -l /media

<<   server/#umount /dev/sdb1 



-------------------------------------------------------------------------------
>> pour savoir et listé  ce qui a été installé
-------------------------------------------------------------------------------

dpkg -l
ss -anu

#########################################################################################

Mise en place d'un routeur + srv (ddns + split) ou srv web

----------------------------------------------
Routeur :

cmd: nano /etc/network/interfaces  -->

auto enp0s3
iface enp0s3 inet DHCP

auto enp0s8
iface enp0s8 inet static
	address 192.168.100.254/24

cmd: systemctl restart networking.service
cmd: ip -c -br a

Activation du routage  -->
SOIT:
Décommenter la ligne 28 du fichier /etc/sysctl.conf
cmd: sysctl -p

SOIT:
cmd: echo 1 > /proc/sys/net/ipV4/ip_forward

Activation et Configuration du Nat  -->

SOIT:
cmd: nano /etc/nftables.conf  -->
table ip NAT-PAT {
	chain nat {
		type nat hook postrouting priority srcnat; policy accept;
		oifname "enp0s3" masquerade;
	}
}

SOIT:
cmd: nft add table ip NAT-PAT
cmd: nft add chain NAT-PAT nat "{ type nat hook postrouting priority srcnat;}"
cmd: nft add rule NAT-PAT nat oifname enp0s3 masquerade    :masque la sortie sur l'interface enp0s3 
cmd: nft list ruleset >> /etc/nftables.conf   :ajoute les règles à la suite dans le fichier /etc/nftable.conf

cmd: nft list ruleset   :pour lister les règles données
cmd: nft flush ruleset   :pour effacer les règles dans le mémoire


apt update; apt install isc-dhcp-server -y; apt install bind9 -y; apt install apache2 -y

----------------------------------------------
DHCP :

cmd: nano /etc/default/isc-dhcp-server  -->  INTERFACESv4="enp0s3"

cmd: nano /etc/dhcp/dhcpd.conf  -->  
authoritative;
default-lease-time 36000;
max-lease-time 72000;

subnet 192.168.100.0 netmask 255.255.255.0 {
        range 192.168.100.1 192.168.100.20;
        option routers 192.168.100.254;
        option domain-name-servers 192.168.100.250,192.168.100.251;
        option domain-name "one.piece";
}

cmd: systemctl restart isc-dhcp-server

----------------------------------------------

DNS master :

cmd: nano /etc/bind/named.conf  --> commenter la 3ème ligne (include)

cmd: nano /etc/bind/named.conf.options  -->
decommenter :

forwarders {
                1.0.0.1;
        };

listen-on-v6 { none; };
listen-on { 127.0.0.1 ; 192.168.100.0/24; };

cmd nano /etc/bind/named.conf.local  -->  
zone "one.piece" {
        type master;
        file "interne-directe-one.piece";

	notify yes;
        also-notify { 192.168.100.251; };
        allow-transfer { 192.168.100.251; };
};

zone "100.168.192.in-addr.arpa" {
        type master;
        file "interne-inverse-one.piece";

	notify yes;
        also-notify { 192.168.100.251; };
        allow-transfer { 192.168.100.251; };
};

FICHIER ZONE DIRECTE
cmd: cp /etc/bind/db.empty /var/cache/bind/interne-directe-one.piece
cmd nano /var/cache/bind/interne-directe-one.piece  -->
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

FICHIER ZONE INVERSE
cmd: cp /var/cache/bind/interne-directe-one.piece /var/cache/bind/interne-inverse-one.piece
cmd nano /var/cache/bind/interne-inverse-one.piece  --> 
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

cmd: systemctl restart named
cmd: named-checkconf -z

----------------------------------------------
DNS slave : la haute disponibilités grâce à un cluster/grappe de server

cmd: nano /etc/bind/named.conf.local  -->
zone "one.piece" {
        type slave;
        file "/var/cache/bind/slave-directe-one.piece";
        masters { 192.168.100.250; };
};

zone "100.168.192.in-addr.arpa" {
        type slave;
        file "/var/cache/bind/slave-inverse-one.piece";
        masters { 192.168.100.250; };
};

cmd: systemctl restart named
cmd: named-checkconf -z

----------------------------------------------
DDNS : Le DHCP et le DNS deviennent associés, et échanges entre eux

Création de la clé de sécurisation : clé TSIG :
cmd: tsig-keygen -a HMAC-SHA512 dns-secret > /etc/bind/secure.key
cmd cp /etc/bind/secure.key /etc/dhcp/secure.key

Ajouter dans dns master, /etc/bind/named.conf.local  -->  allow-update { 127.0.0.1; 192.168.100.0/24; key "dns-secret"; };

DHCP:--> on rajoute
cmd: nano /etc/dhcp/dhcpd.conf  -->

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

cmd: systemctl restart isc-dhcp-server ; systemctl restart named

tester en faisant la cmd: journalctl -f
puis de changer le nom du pc client


----------------------------------------------
WEB-SERVER:

apt update; apt install apache2 openssl -y

Dans le dossier /var/www/html/ créer un dossier site, et un fichier index.html  -->
cmd: cd /var/www/html/
cmd: mkdir one
cmd: echo "<html><body>Welcome www.one.piece</body></html>" > one/index.html

On crée des certificats et clés ssl avec openssl  -->
cmd: mkdir /etc/ssl/mescertifs
cmd: openssl req -new -x509 -nodes -out /etc/ssl/mescertifs/one.crt -keyout
/etc/ssl/mescertifs/one.key

On créer les fichiers de configuration de chaque site dans /etc/apache2/sites-available  -->
cmd: cd  /etc/apache2/sites-available; nano one.conf

<VirtualHost *:80>
	ServerName one.piece
	ServerAlias www.one.piece
	Redirect permanent / https://one.piece
	DocumentRoot /var/www/html/one
</VirtualHost>

<VirtualHost *:443>
   	ServerName one.piece
	ServerAlias www.one.piece
	DocumentRoot /var/www/html/one
 
	SSLEngine on
	SSLCertificateFile /etc/ssl/mescertifs/one.crt
	SSLCertificateKeyFile /etc/ssl/mescertifs/one.key
</VirtualHost>

Ensuite il faut activer le site et le ssl dans /etc/apache2/sites-enabled  -->
cmd: a2enmod ssl
cmd: a2ensite one.conf (si plusieurs sites: cmd: a2ensite one.conf two.piece)
cmd: systemctl enable apache2
cmd: systemctl restart apache2

----------------------------------------------
SPLIT: interne et externe

Dans le fichier /etc/bind/named.conf.local -->

Filtrage par ACL :
acl LAN { 127.0.0.0/8; 192.168.100.0/24; };
acl WAN { 0.0.0.0/0; };


Pour les clients en zone interne :
view "interne" {
    match-clients { LAN; };
    recursion yes;

    zone "one.piece" {
        type master;
        file "interne-directe-one.piece";

	notify yes;
        also-notify { 192.168.100.251; };
        allow-transfer { 192.168.100.251; };
	allow-update { 127.0.0.1; 192.168.100.0/24; key "dns-secret"; };

    };

    zone "100.168.192.in-addr.arpa" {
        type master;
        file "interne-inverse-one.piece";

	notify yes;
        also-notify { 192.168.100.251; };
        allow-transfer { 192.168.100.251; };
	allow-update { 127.0.0.1; 192.168.100.0/24; key "dns-secret"; };
    };

};

view "externe" {
    match-clients { WAN; };
    recursion no;

    zone "one.piece" {
        type master;
        file "externe-directe-one.piece";

	notify no;
 #      also-notify { none; };
        allow-transfer { none; };
	allow-update { none; };

    };

    zone "100.168.192.in-addr.arpa" {
        type master;
        file "externe-inverse-one.piece";

	notify no;
 #      also-notify { none; };
        allow-transfer { none; };
	allow-update { none; };
    };

};

On crée les fichiers de config des zones directes et inverses -->
FICHIER ZONE EXTERNE-DIRECTE
cmd: cp /etc/bind/interne-directe-one.piece /var/cache/bind/externe-directe-one.piece
cmd nano /var/cache/bind/externe-directe-one.piece  -->
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

FICHIER ZONE EXTERNE-INVERSE
cmd: cp /var/cache/bind/interne-inverse-one.piece /var/cache/bind/externe-inverse-one.piece
cmd nano /var/cache/bind/externe-inverse-one.piece  --> 
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

cmd: systemctl restart named
cmd: named-checkconf -z

---CONFIG NAT-PAT avec nftables---
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


Remarque : 
- protocole tcp pour : http:80, https:443, ssh:22, telnet:23
- protocole udp pour : dns:53, dhcp:67/68

#########################################################################################

``` ROUTE ''''

# Afficher toutes les routes
ip route

# Afficher les routes pour une table spécifique
ip route show table [nom_de_la_table]

# Ajouter une route par défaut
ip route add default via [adresse_ip_du_gateway]

# Ajouter une route spécifique
ip route add [destination] via [adresse_ip_du_gateway]

# Ajouter une route pour un réseau spécifique
ip route add [réseau]/[masque] via [adresse_ip_du_gateway]

# Ajouter une route via une interface
ip route add [destination] dev [interface]

# Ajouter une route dans une table de routage spécifique
ip route add [destination] via [adresse_ip_du_gateway] table [nom_de_la_table]

# Supprimer une route spécifique
ip route del [destination] via [adresse_ip_du_gateway]

# Supprimer une route pour un réseau spécifique
ip route del [réseau]/[masque] via [adresse_ip_du_gateway]

# Supprimer une route par défaut
ip route del default

# Modifier une route existante
ip route change [destination] via [nouvelle_adresse_ip_du_gateway]

# Modifier une route pour un réseau spécifique
ip route change [réseau]/[masque] via [nouvelle_adresse_ip_du_gateway]

# Ajouter une route avec une métrique spécifique
ip route add [destination] via [adresse_ip_du_gateway] metric [valeur]

# Changer la métrique d'une route existante
ip route change [destination] via [adresse_ip_du_gateway] metric [valeur]

# Créer une nouvelle table de routage (ajoute un nouvel identifiant de table)
echo '200 my_table' >> /etc/iproute2/rt_tables

# Ajouter une route dans une table spécifique
ip route add [destination] via [adresse_ip_du_gateway] table [nom_de_la_table]

# Afficher une table de routage spécifique
ip route show table [nom_de_la_table]

# Ajouter une route de type 'blackhole' (pour bloquer le trafic vers une destination)
ip route add blackhole [destination]/[masque]

# Ajouter une route de type 'prohibit' (pour interdire explicitement le trafic vers une destination)
ip route add prohibit [destination]/[masque]

# Ajouter une route de type 'unreachable' (pour indiquer que la destination est inatteignable)
ip route add unreachable [destination]/[masque]

