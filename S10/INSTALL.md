# Configuration d'un serveur GLPI sur Debian pour gérer le ticketing

---

- [Pré-requis](https://github.com/WildCodeSchool/TSSR-2405-P3-G2-BuildYourInfra-Pharmgreen/blob/main/S10/INSTALL.md#installation-des-pr%C3%A9-requis)
- [Configuration de GLPI](https://github.com/WildCodeSchool/TSSR-2405-P3-G2-BuildYourInfra-Pharmgreen/blob/main/S10/INSTALL.md#configuration-de-glpi)

---

## Installation des pré-requis

**1. Vérifier les mises à jour et les installer**
   
   `sudo apt-get update && sudo apt-get upgrade`

**2. Installer Apache, qui hébergera le serveur GLPI, et l'activer**
   
   `sudo apt-get install apache2 -y`
   `sudo systemctl enable apache2`

**3. Installer MariaDB, qui servira de base de données à GLPI**
   
   `sudo apt-get install mariadb-server -y`

**4. Installer le module PHP et ses annexes**
   
   `sudo apt-get install php libapache2-mod-php -y`

**5. Vérifier la version de PHP (7.4 à 8.2.0 pour être compatible avec GLPI)**
    
   `sudo php --version`

   Si votre version n'est pas comprise entre la 7.4 et la 8.2.0, il est nécessaire de la modifier.
   Les versions antérieures à la 8.3 ne sont pas natives à Debian, il faut donc installer une librairie spécifique pour les récuperer.

   a. *Modifier le fichier `/etc/apt/sources.list` pour y ajouter une nouvelle librairie de paquets*

   > deb https://packages.sury.org/php bookworm main

   ![Sources.list](https://github.com/WildCodeSchool/TSSR-2405-P3-G2-BuildYourInfra-Pharmgreen/blob/main/S10/Annexes/sources.list.png)

   b. *Mettre à jour les paquets*

   `sudo apt-get update && sudo apt-get upgrade`

   c. *Installer PHP 7.4 ou 8.1*

   `sudo apt install php7.4` or `sudo apt install php8.1`

   d. *Modifier la version de PHP utilisée (ainsi que phar et phar.phar) en selectionnant les versions souhaitées*

   `sudo update-alternatives --config php` / `sudo update-alternatives --config phar` / `sudo update-alternatives --config phar.phar`

   ![Update-alternatives](https://github.com/WildCodeSchool/TSSR-2405-P3-G2-BuildYourInfra-Pharmgreen/blob/main/S10/Annexes/update_alternatives.png)

   e. *Modifier la version de PHP utilisée par Apache*

   `sudo a2dismod php8.3`

   `sudo a2enmod php7.4`

   f. *Redémarrer Apache*

   `sudo systemctl restart apache2`

**6. Installer les modules annexes de PHP pour une version spécifique**

  `sudo apt-get install php7.4-{ldap,imap,apcu,xmlrpc,curl,common,gd,json,mbstring,mysql,xml,intl,zip,bz2}`

**7. Création de la base de données avec Maria DB**

   ```bash
   # Initialisation de la base de données
   sudo mysql_secure_installation
   # Connexion à la base de données
   mysql -u root -p
   # Création de la base de données (modifier nomDeLaBd, nomDeCompte et motDePasse)
   create database "nomDeLaBd" character set utf8 collate utf8_bin;
   grant all privileges on "nomDeLaBd".* to "nomDeCompte"@localhost identified by 'motDePasse';
   flush privileges;
   quit
   ```

**8. Récupération des sources de GLPI**

   ```bash
   sudo wget https://github.com/glpi-project/glpi/releases/download/10.0.2/glpi-10.0.2.tgz
   # Remplacer "monNomDeDomaine" par votre domaine ou en créer un
   sudo mkdir /var/www/html/glpi."monNomDeDomaine"
   sudo tar -xzvf glpi-10.0.2.tgz
   sudo cp -R glpi/* /var/www/html/glpi."monNomDeDomaine"/
   sudo chown -R www-data:www-data /var/www/html/glpi."monNomDeDomaine"/
   sudo chmod -R 775 /var/www/html/glpi."monNomDeDomaine"/
   ```

## Configuration de GLPI

**1. Connexion à l'interface du serveur GLPI depuis un client**

Depuis un client du domaine, accéder à GLPI via un navigateur avec l'url **http://adresseIpDuserveur/glpi."monNomDeDomaine"**


   
