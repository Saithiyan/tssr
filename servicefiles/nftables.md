### Fichier de commandes `nftables` avec descriptions et exemples

```sh
# --- Initialisation de nftables ---
# Vider les règles actuelles
nft flush ruleset

# Charger une configuration depuis un fichier
nft -f /etc/nftables.conf

# Sauvegarder la configuration actuelle dans un fichier
nft list ruleset > /etc/nftables.conf

# --- Affichage des règles et des tables ---
# Afficher toutes les règles
nft list ruleset

# Afficher une table spécifique
nft list table ip [nom_de_la_table]

# Afficher une chaîne spécifique dans une table
nft list chain ip [nom_de_la_table] [nom_de_la_chaîne]

# --- Création et suppression de tables ---
# Créer une table IPv4
nft add table ip [nom_de_la_table]

# Créer une table IPv6
nft add table ip6 [nom_de_la_table]

# Supprimer une table
nft delete table ip [nom_de_la_table]

# --- Création et suppression de chaînes ---
# Créer une chaîne dans une table (chaîne de type filter)
nft add chain ip [nom_de_la_table] [nom_de_la_chaîne] { type filter hook input priority 0 \; }

# Supprimer une chaîne
nft delete chain ip [nom_de_la_table] [nom_de_la_chaîne]

# --- Ajout de règles dans une chaîne ---
# Autoriser le trafic entrant sur l'interface lo (loopback)
nft add rule ip [nom_de_la_table] [nom_de_la_chaîne] iifname "lo" accept

# Autoriser le trafic sortant vers une adresse IP spécifique
nft add rule ip [nom_de_la_table] [nom_de_la_chaîne] ip daddr [adresse_ip] accept

# Bloquer le trafic entrant d'une adresse IP spécifique
nft add rule ip [nom_de_la_table] [nom_de_la_chaîne] ip saddr [adresse_ip] drop

# Autoriser le trafic entrant sur le port 22 (SSH)
nft add rule ip [nom_de_la_table] [nom_de_la_chaîne] tcp dport 22 accept

# Bloquer tout le trafic entrant par défaut
nft add rule ip [nom_de_la_table] [nom_de_la_chaîne] drop

# --- Exemples de configuration de pare-feu ---
# Créer une table et des chaînes pour un pare-feu simple
nft add table ip filter
nft add chain ip filter input { type filter hook input priority 0 \; }
nft add chain ip filter forward { type filter hook forward priority 0 \; }
nft add chain ip filter output { type filter hook output priority 0 \; }

# Ajouter des règles de base pour le pare-feu
# Autoriser le trafic sur l'interface loopback
nft add rule ip filter input iifname "lo" accept
# Autoriser les connexions déjà établies
nft add rule ip filter input ct state established,related accept
# Autoriser le trafic ICMP (ping)
nft add rule ip filter input icmp type echo-request accept
# Autoriser le trafic SSH
nft add rule ip filter input tcp dport 22 ct state new accept
# Bloquer tout le reste du trafic entrant
nft add rule ip filter input drop

# --- Exemples avancés ---
# Limiter le nombre de nouvelles connexions SSH à 10 par minute
nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept

# Journaliser les paquets abandonnés
nft add rule ip filter input drop log prefix "nftables drop: "

# --- Suppression de règles ---
# Supprimer une règle spécifique
nft delete rule ip [nom_de_la_table] [nom_de_la_chaîne] handle [numéro_du_handle]

# Exemple pour supprimer une règle spécifique:
# Trouver le handle de la règle à supprimer
nft list chain ip filter input
# La commande renverra quelque chose comme :
# table ip filter {
#  chain input {
#      type filter hook input priority 0; policy accept;
#      handle 1 ...
#      handle 2 ...
#      ...
#  }
# }
# Ensuite, utiliser le handle pour supprimer la règle
nft delete rule ip filter input handle [numéro_du_handle]

# --- Sauvegarder et restaurer la configuration ---
# Sauvegarder la configuration actuelle
nft list ruleset > /etc/nftables.conf

# Restaurer la configuration depuis un fichier
nft -f /etc/nftables.conf
```

### Explications et exemples

#### Initialisation de nftables
- **Vider les règles actuelles :** Cette commande réinitialise la configuration nftables.
  ```sh
  nft flush ruleset
  ```
- **Charger une configuration depuis un fichier :** Charge la configuration spécifiée dans le fichier.
  ```sh
  nft -f /etc/nftables.conf
  ```
- **Sauvegarder la configuration actuelle :** Sauvegarde la configuration actuelle dans un fichier pour une utilisation ultérieure.
  ```sh
  nft list ruleset > /etc/nftables.conf
  ```

#### Affichage des règles et des tables
- **Afficher toutes les règles :** Affiche la configuration complète actuelle de nftables.
  ```sh
  nft list ruleset
  ```

#### Création et suppression de tables
- **Créer une table IPv4 :** Crée une nouvelle table pour les règles IPv4.
  ```sh
  nft add table ip filter
  ```

#### Création et suppression de chaînes
- **Créer une chaîne dans une table :** Crée une nouvelle chaîne pour filtrer les paquets entrants.
  ```sh
  nft add chain ip filter input { type filter hook input priority 0 \; }
  ```

#### Ajout de règles dans une chaîne
- **Autoriser le trafic entrant sur l'interface loopback :** Autorise les paquets entrants sur l'interface `lo`.
  ```sh
  nft add rule ip filter input iifname "lo" accept
  ```
- **Bloquer le trafic entrant d'une adresse IP spécifique :** Bloque les paquets entrants provenant de l'adresse IP spécifiée.
  ```sh
  nft add rule ip filter input ip saddr 192.168.1.100 drop
  ```

#### Exemples de configuration de pare-feu
- **Ajouter des règles de base pour un pare-feu simple :** Configure un pare-feu avec des règles de base.
  ```sh
  nft add table ip filter
  nft add chain ip filter input { type filter hook input priority 0 \; }
  nft add chain ip filter forward { type filter hook forward priority 0 \; }
  nft add chain ip filter output { type filter hook output priority 0 \; }
  nft add rule ip filter input iifname "lo" accept
  nft add rule ip filter input ct state established,related accept
  nft add rule ip filter input icmp type echo-request accept
  nft add rule ip filter input tcp dport 22 ct state new accept
  nft add rule ip filter input drop
  ```

#### Exemples avancés
- **Limiter le nombre de nouvelles connexions SSH :** Limite le nombre de nouvelles connexions SSH à 10 par minute pour éviter les attaques par force brute.
  ```sh
  nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
  ```

#### Suppression de règles
- **Supprimer une règle spécifique :** Supprime une règle en utilisant son handle.
  ```sh
  nft delete rule ip filter input handle [numéro_du_handle]
  ```

#### Sauvegarder et restaurer la configuration
- **Sauvegarder la configuration actuelle :** Sauvegarde la configuration actuelle dans un fichier.
  ```sh
  nft list ruleset > /etc/nftables.conf
  ```
- **Restaurer la configuration depuis un fichier :** Charge la configuration à partir d'un fichier.
  ```sh
  nft -f /etc/nftables.conf
  ```

Remplace les éléments entre crochets (`[ ]`) par les valeurs appropriées pour ton contexte spécifique. Utilise les commandes avec des privilèges super-utilisateur (souvent avec `sudo`) pour qu'elles soient appliquées correctement.



---
### Exemple de fichier `nftables.conf` avec explications et exemples de règles de pare-feu

Le fichier `nftables.conf` contient les configurations complètes pour nftables. Voici un exemple de configuration avec des explications et des exemples de règles de pare-feu :

```sh
#!/usr/sbin/nft -f

# --- Initialisation de nftables ---
# Vider les règles actuelles
flush ruleset

# --- Création des tables ---
# Créer une table pour les règles IPv4
table ip filter {
    # --- Création des chaînes ---
    # Chaîne pour les paquets entrants
    chain input {
        type filter hook input priority 0; policy drop;
        
        # --- Règles de base pour le pare-feu ---
        # Autoriser tout le trafic sur l'interface loopback
        iifname "lo" accept
        
        # Autoriser les connexions déjà établies et les connexions associées
        ct state established,related accept
        
        # Autoriser le trafic ICMP (ping)
        icmp type echo-request accept
        
        # Autoriser le trafic SSH (port 22)
        tcp dport 22 ct state new accept
        
        # Journaliser et bloquer tout le reste du trafic entrant
        log prefix "nftables drop: " flags all counter drop
    }

    # Chaîne pour les paquets sortants
    chain output {
        type filter hook output priority 0; policy accept;
    }

    # Chaîne pour les paquets forwardés
    chain forward {
        type filter hook forward priority 0; policy drop;
    }
}

# --- Exemples avancés ---
# Limiter le nombre de nouvelles connexions SSH à 10 par minute
nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
```

### Explications

#### Initialisation de nftables
- **Vider les règles actuelles :** Cette commande réinitialise la configuration nftables pour s'assurer qu'il n'y a pas de règles résiduelles.
  ```sh
  flush ruleset
  ```

#### Création des tables
- **Table pour les règles IPv4 :** Crée une table appelée `filter` pour les règles IPv4.
  ```sh
  table ip filter {
  ```

#### Création des chaînes
- **Chaîne pour les paquets entrants :** Crée une chaîne appelée `input` pour filtrer les paquets entrants. La politique par défaut (`policy drop`) est de bloquer tous les paquets qui ne correspondent à aucune règle.
  ```sh
  chain input {
      type filter hook input priority 0; policy drop;
  ```

#### Règles de base pour le pare-feu
- **Autoriser tout le trafic sur l'interface loopback :** Cette règle permet tout le trafic entrant et sortant sur l'interface loopback (lo).
  ```sh
  iifname "lo" accept
  ```

- **Autoriser les connexions déjà établies et les connexions associées :** Cette règle permet aux paquets faisant partie de connexions déjà établies ou connexions associées.
  ```sh
  ct state established,related accept
  ```

- **Autoriser le trafic ICMP (ping) :** Permet les requêtes ICMP de type `echo-request` (ping).
  ```sh
  icmp type echo-request accept
  ```

- **Autoriser le trafic SSH (port 22) :** Permet les nouvelles connexions SSH sur le port 22.
  ```sh
  tcp dport 22 ct state new accept
  ```

- **Journaliser et bloquer tout le reste du trafic entrant :** Journalise et bloque tous les autres paquets qui ne correspondent pas aux règles précédentes.
  ```sh
  log prefix "nftables drop: " flags all counter drop
  ```

#### Chaîne pour les paquets sortants
- **Chaîne pour les paquets sortants :** Crée une chaîne appelée `output` pour filtrer les paquets sortants. La politique par défaut est d'accepter tous les paquets.
  ```sh
  chain output {
      type filter hook output priority 0; policy accept;
  ```

#### Chaîne pour les paquets forwardés
- **Chaîne pour les paquets forwardés :** Crée une chaîne appelée `forward` pour filtrer les paquets qui traversent le routeur. La politique par défaut est de bloquer tous les paquets.
  ```sh
  chain forward {
      type filter hook forward priority 0; policy drop;
  ```

#### Exemples avancés
- **Limiter le nombre de nouvelles connexions SSH :** Limite le nombre de nouvelles connexions SSH à 10 par minute pour éviter les attaques par force brute.
  ```sh
  nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
  ```

### Exemple complet de configuration

Voici un exemple complet de fichier `nftables.conf` :

```sh
#!/usr/sbin/nft -f

flush ruleset

table ip filter {
    chain input {
        type filter hook input priority 0; policy drop;
        iifname "lo" accept
        ct state established,related accept
        icmp type echo-request accept
        tcp dport 22 ct state new accept
        log prefix "nftables drop: " flags all counter drop
    }

    chain output {
        type filter hook output priority 0; policy accept;
    }

    chain forward {
        type filter hook forward priority 0; policy drop;
    }
}

# Limiter le nombre de nouvelles connexions SSH à 10 par minute
nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
```

### Explications et exemples de règles de pare-feu

1. **Autoriser tout le trafic sur l'interface loopback :** Cette règle est cruciale pour le bon fonctionnement des applications locales.
   ```sh
   iifname "lo" accept
   ```

2. **Autoriser les connexions déjà établies et les connexions associées :** Cela permet aux paquets qui font partie de connexions existantes ou connexions associées de passer sans être bloqués.
   ```sh
   ct state established,related accept
   ```

3. **Autoriser le trafic ICMP (ping) :** Permet aux paquets ICMP de type `echo-request` de passer, ce qui est utile pour les tests de connectivité réseau.
   ```sh
   icmp type echo-request accept
   ```

4. **Autoriser le trafic SSH (port 22) :** Permet aux nouvelles connexions SSH d'entrer, ce qui est nécessaire pour l'administration à distance.
   ```sh
   tcp dport 22 ct state new accept
   ```

5. **Journaliser et bloquer tout le reste du trafic entrant :** Bloque tous les autres paquets entrants et les journalise pour les analyses futures.
   ```sh
   log prefix "nftables drop: " flags all counter drop
   ```

6. **Limiter le nombre de nouvelles connexions SSH à 10 par minute :** Limite les tentatives de connexion SSH à 10 par minute pour réduire les risques d'attaques par force brute.
   ```sh
   nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
   ```

Utilise ce fichier comme base pour configurer tes règles de pare-feu nftables et personnalise-le en fonction de tes besoins spécifiques.





---
RE: a voir

J'ai tout terminé. Voici un fichier `nftables.conf` complet avec des explications et des exemples de règles de pare-feu :

```sh
#!/usr/sbin/nft -f

# --- Initialisation de nftables ---
# Vider les règles actuelles
flush ruleset

# --- Création des tables ---
# Créer une table pour les règles IPv4
table ip filter {
    # --- Création des chaînes ---
    # Chaîne pour les paquets entrants
    chain input {
        type filter hook input priority 0; policy drop;
        
        # --- Règles de base pour le pare-feu ---
        # Autoriser tout le trafic sur l'interface loopback
        iifname "lo" accept
        
        # Autoriser les connexions déjà établies et les connexions associées
        ct state established,related accept
        
        # Autoriser le trafic ICMP (ping)
        icmp type echo-request accept
        
        # Autoriser le trafic SSH (port 22)
        tcp dport 22 ct state new accept
        
        # Journaliser et bloquer tout le reste du trafic entrant
        log prefix "nftables drop: " flags all counter drop
    }

    # Chaîne pour les paquets sortants
    chain output {
        type filter hook output priority 0; policy accept;
    }

    # Chaîne pour les paquets forwardés
    chain forward {
        type filter hook forward priority 0; policy drop;
    }
}

# --- Exemples avancés ---
# Limiter le nombre de nouvelles connexions SSH à 10 par minute
nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
```

### Explications et exemples de règles de pare-feu

1. **Initialisation de nftables :** Vider les règles actuelles pour s'assurer d'une configuration propre.
   ```sh
   flush ruleset
   ```

2. **Création de la table `filter` pour les règles IPv4 :** Définir une table pour organiser les règles de filtrage.
   ```sh
   table ip filter {
   ```

3. **Chaîne `input` pour les paquets entrants :**
   - **Autoriser le trafic sur l'interface loopback :**
     ```sh
     iifname "lo" accept
     ```
   - **Autoriser les connexions déjà établies et les connexions associées :**
     ```sh
     ct state established,related accept
     ```
   - **Autoriser le trafic ICMP (ping) :**
     ```sh
     icmp type echo-request accept
     ```
   - **Autoriser le trafic SSH (port 22) :**
     ```sh
     tcp dport 22 ct state new accept
     ```
   - **Journaliser et bloquer tout le reste du trafic entrant :**
     ```sh
     log prefix "nftables drop: " flags all counter drop
     ```

4. **Chaîne `output` pour les paquets sortants :** Accepter tout le trafic sortant.
   ```sh
   chain output {
       type filter hook output priority 0; policy accept;
   ```

5. **Chaîne `forward` pour les paquets forwardés :** Bloquer tout le trafic forwardé par défaut.
   ```sh
   chain forward {
       type filter hook forward priority 0; policy drop;
   ```

6. **Exemple avancé - Limiter le nombre de nouvelles connexions SSH :** Limiter les tentatives de connexion SSH à 10 par minute pour réduire les risques d'attaques par force brute.
   ```sh
   nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
   ```

Ce fichier peut être utilisé comme base pour configurer tes règles de pare-feu avec nftables. Adapte-le en fonction de tes besoins spécifiques et sauvegarde-le en tant que `/etc/nftables.conf`. Pour appliquer cette configuration, utilise la commande suivante :
```sh
sudo nft -f /etc/nftables.conf
```

Si tu as d'autres questions ou besoins supplémentaires, n'hésite pas à demander.





---
PAREFEU règles :
### Explications des Règles de Pare-feu et des Chaînes de nftables

#### Chaînes et Règles de Pare-feu

Les règles de pare-feu déterminent quels paquets réseau sont autorisés à passer et lesquels sont bloqués. Ces règles sont organisées en **chaînes**, qui à leur tour sont organisées en **tables**. Dans nftables, les chaînes couramment utilisées sont `input`, `output` et `forward`.

#### Tables

- **Tables** : Les tables contiennent les chaînes. Une table regroupe des chaînes de règles similaires ou liées à une fonction particulière. Par exemple, la table `filter` est utilisée pour les règles de filtrage des paquets.

#### Chaînes

1. **Input** : La chaîne `input` est utilisée pour les paquets entrants destinés à la machine locale.
2. **Output** : La chaîne `output` est utilisée pour les paquets sortants générés par la machine locale.
3. **Forward** : La chaîne `forward` est utilisée pour les paquets qui traversent la machine mais ne sont pas destinés à elle (par exemple, un routeur).

### Utilisation des Chaînes

#### Chaîne `input`

- **Quand l'utiliser** : Pour contrôler les paquets entrant sur la machine locale.
- **Pourquoi l'utiliser** : Pour protéger la machine des connexions indésirables ou malveillantes, autoriser des services spécifiques (SSH, HTTP, etc.), et filtrer le trafic indésirable.
  
#### Exemple :
```sh
# Autoriser les connexions sur l'interface loopback
nft add rule ip filter input iifname "lo" accept

# Autoriser les connexions déjà établies et les connexions associées
nft add rule ip filter input ct state established,related accept

# Autoriser le trafic ICMP (ping)
nft add rule ip filter input icmp type echo-request accept

# Autoriser les connexions SSH
nft add rule ip filter input tcp dport 22 ct state new accept

# Journaliser et bloquer tout le reste
nft add rule ip filter input log prefix "input drop: " flags all counter drop
```

#### Chaîne `output`

- **Quand l'utiliser** : Pour contrôler les paquets sortants de la machine locale.
- **Pourquoi l'utiliser** : Pour limiter les connexions sortantes, par exemple, empêcher un programme malveillant d'établir des connexions sortantes non autorisées.

#### Exemple :
```sh
# Autoriser toutes les connexions sortantes
nft add rule ip filter output accept
```

#### Chaîne `forward`

- **Quand l'utiliser** : Pour contrôler les paquets qui transitent par la machine (utilisée comme routeur).
- **Pourquoi l'utiliser** : Pour gérer le trafic traversant un routeur ou un pare-feu de manière à appliquer des politiques de sécurité spécifiques.

#### Exemple :
```sh
# Bloquer tout le trafic forwardé par défaut
nft add rule ip filter forward drop
```

### Configuration Complète de Pare-feu

Voici une configuration complète dans un fichier `nftables.conf` pour un serveur simple :

```sh
#!/usr/sbin/nft -f

# Vider les règles actuelles
flush ruleset

# Créer une table pour les règles IPv4
table ip filter {
    # Chaîne pour les paquets entrants
    chain input {
        type filter hook input priority 0; policy drop;
        
        # Autoriser le trafic sur l'interface loopback
        iifname "lo" accept
        
        # Autoriser les connexions déjà établies et les connexions associées
        ct state established,related accept
        
        # Autoriser le trafic ICMP (ping)
        icmp type echo-request accept
        
        # Autoriser les connexions SSH
        tcp dport 22 ct state new accept
        
        # Journaliser et bloquer tout le reste
        log prefix "input drop: " flags all counter drop
    }

    # Chaîne pour les paquets sortants
    chain output {
        type filter hook output priority 0; policy accept;
    }

    # Chaîne pour les paquets forwardés
    chain forward {
        type filter hook forward priority 0; policy drop;
    }
}

# Limiter le nombre de nouvelles connexions SSH à 10 par minute
nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
```

### Autres Exemples de Règles

#### Autoriser le Trafic HTTP et HTTPS

Pour un serveur web, vous souhaiterez probablement autoriser le trafic HTTP et HTTPS.

```sh
# Autoriser le trafic HTTP
nft add rule ip filter input tcp dport 80 ct state new accept

# Autoriser le trafic HTTPS
nft add rule ip filter input tcp dport 443 ct state new accept
```

#### Bloquer une Adresse IP Spécifique

Pour bloquer tout le trafic provenant d'une adresse IP spécifique.

```sh
# Bloquer le trafic de l'adresse IP 203.0.113.1
nft add rule ip filter input ip saddr 203.0.113.1 drop
```

#### Autoriser un Port de Gestion à Distance (ex: Webmin sur 10000)

Pour autoriser l'accès à un port de gestion à distance.

```sh
# Autoriser le trafic Webmin
nft add rule ip filter input tcp dport 10000 ct state new accept
```

### Explications Supplémentaires

#### Politiques par Défaut

Les politiques par défaut (`policy accept` ou `policy drop`) sont importantes. Si la politique est `drop`, tout paquet qui ne correspond à aucune règle sera bloqué. Si la politique est `accept`, tout paquet non correspondant sera accepté.

#### Connexions Établies et Reliées

Permettre les connexions `established` et `related` est crucial pour le bon fonctionnement de nombreux services réseau, car cela permet de continuer des connexions déjà acceptées sans devoir évaluer chaque paquet individuellement.

#### Limitation des Connexions

Limiter les nouvelles connexions SSH à un certain taux permet de protéger contre les attaques par force brute. Par exemple :

```sh
nft add rule ip filter input tcp dport 22 ct state new limit rate 10/minute accept
```

#### Journalisation

La journalisation des paquets bloqués est utile pour le diagnostic et la sécurité, car elle permet de surveiller et d'analyser les tentatives de connexion indésirables.

```sh
log prefix "input drop: " flags all counter drop
```

### Conclusion

Les règles de pare-feu sont essentielles pour la sécurité d'un serveur. Les chaînes `input`, `output`, et `forward` permettent de gérer respectivement le trafic entrant, sortant et traversant. Utiliser nftables pour configurer ces règles offre une flexibilité et une puissance considérable pour contrôler le trafic réseau de manière précise et sécurisée. Adaptez les exemples donnés à vos besoins spécifiques pour protéger efficacement votre système.