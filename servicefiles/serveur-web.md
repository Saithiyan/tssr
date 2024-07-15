# Serveur Web

On installe __apache2__ et __openssl__ pour la clé sécurisé et le certificat autosigné ( HTTPS ) :
```sh
apt update ; apt install apache2 openssl -y
```
Dans le dossier ***/var/www/html/***  créer un fichier __index.html__ dans un nouveau dossier site :
```sh
cd /var/www/html/ ; mkdir one
```

```sh
echo "<html><body><h1 style="color:red; text-align:center; margin-top 10%;" >Welcome www.one.piece</h1></body></html>" > one/index.html
``` 

On crée des __Certificats__ et des __clés SSL__ avec __openssl__  :
On crée un nouveau dossier :
```sh
mkdir /etc/ssl/mescertifs
```
La commande crée un fichier certificat __one.crt__ et un fichier __one.key__ dans ***/etc/ssl/mescertifs/*** :
```sh
openssl req -new -x509 -nodes -out /etc/ssl/mescertifs/one.crt -keyout
/etc/ssl/mescertifs/one.key
```
On créer les fichiers de configuration de chaque site __one.conf__ dans ***/etc/apache2/sites-available*** :
```sh
cd  /etc/apache2/sites-available
```

```sh
nano one.conf
```

```
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
```

Ensuite il faut activer le site et le SSL dans ***/etc/apache2/sites-enabled*** :
```sh
cd /etc/apache2/sites-enabled
```
( si plusieurs sites: cmd: a2ensite one.conf two.piece )
```sh
 a2enmod ssl && a2ensite one.conf
```

```sh
systemctl enable apache2 && systemctl restart apache2
```
ou un reload pour rafraichir uniquement une partie qui a été modifié
```
systemctl reload apache2
```


---
Pour avoir une __Authentification Basique__ sur le site :
Il faut créer un fichier __htpasswd__ contenant les utilisateurs et leurs mots de passe hachés à l'aide de la commande __htpasswd__ :
```sh
 htpasswd -c /chemin/vers/fichier.htpasswd nom_utilisateur
```
__Exemple :__
```sh
htpasswd -c /etc/apache2/one.htpasswd admin
```
ou
```sh
cmd: htpasswd -c /etc/apache2/.htpasswd admin
```
Il demandera le mot de passe 2 fois à confirmer.

Ensuite intégrer le __Directory__ dans le fichier conf du site ( __one.conf__ ) dans ***/etc/apache2/sites-available/*** :
```
<VirtualHost *:443>
   	ServerName one.piece
	ServerAlias www.one.piece
	DocumentRoot /var/www/html/one
 
	SSLEngine on
	SSLCertificateFile /etc/ssl/mescertifs/one.crt
	SSLCertificateKeyFile /etc/ssl/mescertifs/one.key

	<Directory "/var/www/html/one">
                 AuthType Basic
                 AuthName "Accès Restreint ! Veuillez-vous authentifier !"
                 AuthUserFile /etc/apache2/poiss.htpasswd
                 <limit GET>
                         Require valid-user
                 </limit>
         </Directory>
</VirtualHost>
```


