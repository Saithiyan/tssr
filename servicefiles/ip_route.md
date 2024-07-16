# IP Route :

```sh
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
```
Remplace les éléments entre crochets (`[ ]`) par les valeurs appropriées selon le contexte de ta configuration réseau. Utilise ces commandes avec des privilèges super-utilisateur (souvent avec `sudo`) pour qu'elles soient appliquées correctement.



---
Version plus complet avec définitions :

### Script de commandes `ip route` avec descriptions pour manipuler les routes sous Debian

```sh
#!/bin/bash

# Afficher la table de routage
# Affiche toutes les routes configurées sur le système.
ip route show

# Ajouter une route
# Ajoute une route statique pour atteindre un réseau spécifique.
# Exemple : ajouter une route pour le réseau 192.168.1.0/24 via la passerelle 192.168.0.1
ip route add 192.168.1.0/24 via 192.168.0.1

# Ajouter une route par interface
# Ajoute une route statique pour atteindre un réseau spécifique via une interface réseau.
# Exemple : ajouter une route pour le réseau 192.168.1.0/24 via l'interface eth0
ip route add 192.168.1.0/24 dev eth0

# Supprimer une route
# Supprime une route statique existante.
# Exemple : supprimer la route pour le réseau 192.168.1.0/24
ip route del 192.168.1.0/24

# Remplacer une route
# Remplace une route existante ou en ajoute une nouvelle si elle n'existe pas.
# Exemple : remplacer la route pour le réseau 192.168.1.0/24 avec une nouvelle passerelle
ip route replace 192.168.1.0/24 via 192.168.0.254

# Changer les attributs d'une route
# Modifie les attributs d'une route existante.
# Exemple : changer la route pour 192.168.1.0/24 pour utiliser une nouvelle interface
ip route change 192.168.1.0/24 dev eth1

# Ajouter une route par défaut
# Définit une route par défaut pour tout le trafic non correspondant à d'autres routes.
# Exemple : ajouter une route par défaut via la passerelle 192.168.0.1
ip route add default via 192.168.0.1

# Supprimer une route par défaut
# Supprime la route par défaut existante.
# Exemple : supprimer la route par défaut
ip route del default

# Afficher les routes pour une table de routage spécifique
# Affiche les routes configurées dans une table de routage spécifique.
# Exemple : afficher les routes dans la table "main"
ip route show table main

# Ajouter une route dans une table de routage spécifique
# Ajoute une route dans une table de routage spécifique.
# Exemple : ajouter une route pour 192.168.1.0/24 via 192.168.0.1 dans la table "main"
ip route add 192.168.1.0/24 via 192.168.0.1 table main

# Supprimer une route d'une table de routage spécifique
# Supprime une route d'une table de routage spécifique.
# Exemple : supprimer la route pour 192.168.1.0/24 de la table "main"
ip route del 192.168.1.0/24 table main

# Afficher les routes de toutes les tables
# Affiche toutes les routes configurées sur le système, y compris celles des tables de routage spéciales.
ip route show table all

# Ajouter une route avec une métrique
# Ajoute une route avec une métrique spécifique, utilisée pour la sélection de routes préférentielles.
# Exemple : ajouter une route pour 192.168.1.0/24 via 192.168.0.1 avec une métrique de 100
ip route add 192.168.1.0/24 via 192.168.0.1 metric 100

# Afficher la table de routage IPv6
# Affiche toutes les routes IPv6 configurées sur le système.
ip -6 route show

# Ajouter une route IPv6
# Ajoute une route IPv6 statique pour atteindre un réseau spécifique.
# Exemple : ajouter une route pour le réseau fd00::/64 via la passerelle fe80::1
ip -6 route add fd00::/64 via fe80::1

# Supprimer une route IPv6
# Supprime une route IPv6 statique existante.
# Exemple : supprimer la route pour le réseau fd00::/64
ip -6 route del fd00::/64

# Remplacer une route IPv6
# Remplace une route IPv6 existante ou en ajoute une nouvelle si elle n'existe pas.
# Exemple : remplacer la route pour le réseau fd00::/64 avec une nouvelle passerelle
ip -6 route replace fd00::/64 via fe80::2

# Ajouter une route par défaut IPv6
# Définit une route par défaut IPv6 pour tout le trafic non correspondant à d'autres routes.
# Exemple : ajouter une route par défaut via la passerelle fe80::1
ip -6 route add default via fe80::1

# Supprimer une route par défaut IPv6
# Supprime la route par défaut IPv6 existante.
# Exemple : supprimer la route par défaut
ip -6 route del default
```

### Description des Commandes

- **ip route show** : Affiche la table de routage actuelle. Utilisé pour vérifier quelles routes sont configurées.
- **ip route add** : Ajoute une route statique pour atteindre un réseau spécifique ou une passerelle.
- **ip route add [network] dev [interface]** : Ajoute une route via une interface réseau spécifique.
- **ip route del** : Supprime une route statique existante.
- **ip route replace** : Remplace une route existante ou ajoute une nouvelle route si elle n'existe pas.
- **ip route change** : Modifie les attributs d'une route existante.
- **ip route add default via [gateway]** : Ajoute une route par défaut pour tout le trafic non correspondant à d'autres routes.
- **ip route del default** : Supprime la route par défaut existante.
- **ip route show table [table]** : Affiche les routes configurées dans une table de routage spécifique.
- **ip route add [network] via [gateway] table [table]** : Ajoute une route dans une table de routage spécifique.
- **ip route del [network] table [table]** : Supprime une route d'une table de routage spécifique.
- **ip route show table all** : Affiche toutes les routes configurées sur le système, y compris celles des tables de routage spéciales.
- **ip route add [network] via [gateway] metric [value]** : Ajoute une route avec une métrique spécifique.
- **ip -6 route show** : Affiche la table de routage IPv6 actuelle.
- **ip -6 route add** : Ajoute une route IPv6 statique.
- **ip -6 route del** : Supprime une route IPv6 statique existante.
- **ip -6 route replace** : Remplace une route IPv6 existante ou ajoute une nouvelle route si elle n'existe pas.
- **ip -6 route add default via [gateway]** : Ajoute une route par défaut IPv6.
- **ip -6 route del default** : Supprime la route par défaut IPv6 existante.

Ce script couvre les commandes de base et avancées pour manipuler les routes IP et IPv6 sous Debian. Adaptez et exécutez ces commandes en fonction de vos besoins spécifiques en matière de routage réseau.