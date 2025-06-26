

Projet de Mise en Place d'un Site Web avec Architecture 3-Tiers

Description du Projet

Dans le cadre de ma formation Administrateur Système, Réseau et Sécurité sur OpenClassrooms, j'ai réalisé un projet portant sur la mise en place d'un site web sécurisé en utilisant une architecture 3-tiers et une stack LAMP (Linux, Apache, MySQL/MariaDB, PHP).

Le site web déployé repose sur trois machines virtuelles interconnectées :

1. Serveur DNS: Héberge le service DNS avec BIND9 (IP : `192.168.68.51`)
2. Serveur Web : Héberge le serveur Apache2 et le site web PHP (IP : `192.168.68.53`)
3. Serveur BDD : Héberge MariaDB pour la gestion de la base de données (IP : `192.168.68.52`)

Objectifs

* Configurer un serveur web Apache2 pour héberger le site.
* Configurer un serveur de base de données MariaDB pour stocker les données du site.
* Configurer un serveur DNS pour résoudre le domaine `www.beesafe.co`.
* Mettre en place une sécurité de base avec des permissions d'accès à la base de données.


 Prérequis

* Debian 12 pour chaque machine virtuelle.
* Apache2 pour le serveur web.
* MariaDB pour la base de données.
* BIND9 pour le serveur DNS.

 Installation

1. Configurer le Serveur DNS (BIND9)
   Installer et configurer BIND9 pour résoudre le domaine `www.beesafe.co` vers l'IP du serveur web.

2. Configurer le Serveur Web (Apache2)
   Installer Apache2 et configurer le fichier de virtual host pour le domaine `www.beesafe.co`. Voir ci-dessous un exemple de configuration :

   ```apache
   <VirtualHost *:80>
       ServerName www.beesafe.co
       DocumentRoot /var/www/html/ASR-P4-Beesafe

       <Directory /var/www/html/ASR-P4-Beesafe>
           Options Indexes FollowSymlinks
           AllowOverride All
           Require all granted
       </Directory>

       ErrorLog ${APACHE_LOG_DIR}/beesafe_error.log
       CustomLog ${APACHE_LOG_DIR}/beesafe_access.log combined

       <FilesMatch \.php$>
           SetHandler application/x-httpd-php
       </FilesMatch>
   </VirtualHost>
   ```

3. Configurer le Serveur de Base de Données (MariaDB)
   Installer MariaDB et configurer la base de données `beesafe_db`. Exemple de création d'un utilisateur et des privilèges :

   ```sql
   CREATE USER 'svc_beesafe_dbuser'@'%' IDENTIFIED BY 'mot_de_passe_complexe';
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE ON *.* TO 'svc_beesafe_dbuser'@'%';
   ```

   Ensuite, assurez-vous que la base de données `beesafe_db` est correctement initialisée.

4. Installer le Site Web

 Configuration de la Connexion à la Base de Données

Voici un exemple de code PHP pour la connexion à la base de données :

```php
<?php
$servername = "192.168.68.52"; // Adresse IP du serveur BDD
$username = "svc_beesafe_dbuser"; // Nom d'utilisateur
$password = "mot_de_passe_complexe"; // Mot de passe
$dbname = "beesafe_db"; // Nom de la base de données
$port = 3306; // Port par défaut MySQL

// Création de la connexion
$conn = new mysqli($servername, $username, $password, $dbname, $port);

// Vérification de la connexion
if ($conn->connect_error) {
    die("Connexion échouée: " . $conn->connect_error);
}
?>
```

Sécurité

* Vérifier que les permissions d'accès à MariaDB sont correctement configurées pour éviter les accès non autorisés.
* Mettre en place fail2ban et iptables pour sécuriser les serveurs.
* Les connexions à la base de données et aux autres services doivent être chiffrées via SSL/TLS si nécessaire.

 Conclusion

Ce projet représente la mise en place d'une infrastructure simple mais fonctionnelle pour héberger un site web sécurisé avec une architecture 3-tiers. Cela permet de séparer les différentes couches de l'application pour améliorer la sécurité et la gestion des ressources.




