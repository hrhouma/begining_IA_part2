# Guide d'Installation de GLPI sur un Serveur Linux

Ce guide fournit les instructions et un script shell pour installer GLPI sur un serveur Linux (Ubuntu ou Debian recommandé).

## Prérequis
- Un serveur Linux (Ubuntu ou Debian recommandé)
- Un serveur web (Apache)
- PHP et ses extensions
- Une base de données (MariaDB ou MySQL)

## Étapes d'Installation

### 1. Créez et exécutez le script shell

Enregistrez le script suivant dans un fichier, par exemple `install_glpi.sh` :

```bash
nano install_glpi.sh
```
### 2. Copier/coller le contenu de ce script, et ensuite faite CTL + X , tapez Y

```bash
#!/bin/bash

# Mise à jour des paquets et installation des composants nécessaires
sudo apt update
sudo apt install -y apache2 php php-{apcu,cli,common,curl,gd,imap,ldap,mysql,xmlrpc,xml,mbstring,bcmath,intl,zip,bz2} libapache2-mod-php mariadb-server

# Sécurisation de l'installation de MariaDB
sudo mysql_secure_installation

# Connexion à MariaDB et création de la base de données GLPI et de l'utilisateur
sudo mysql -u root <<EOF
CREATE DATABASE glpidb;
CREATE USER 'glpiuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON glpidb.* TO 'glpiuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
EOF

# Téléchargement et extraction de GLPI
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/10.0.7/glpi-10.0.7.tgz
sudo tar xvf glpi-10.0.7.tgz -C /var/www/html/

# Configuration des permissions
sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 775 /var/www/html/glpi

# Création du fichier de configuration pour GLPI dans Apache
sudo bash -c 'cat <<EOT > /etc/apache2/sites-available/glpi.conf
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot /var/www/html/glpi/public

    <Directory /var/www/html/glpi/public>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog \${APACHE_LOG_DIR}/glpi_error.log
    CustomLog \${APACHE_LOG_DIR}/glpi_access.log combined
</VirtualHost>
EOT'

# Activation de la configuration et redémarrage d'Apache
sudo a2ensite glpi.conf
sudo a2enmod rewrite
sudo systemctl restart apache2

# Instructions pour l'installation via l'interface web
echo "Installation via l'interface web :"
echo "Ouvrez un navigateur et accédez à http://127.0.0.1/glpi"
echo "Suivez l'assistant d'installation en sélectionnant la langue, acceptant la licence, et en fournissant les informations de la base de données :"
echo "Base de données : localhost"
echo "Utilisateur : glpiuser"
echo "Mot de passe : password"



# Instructions pour la finalisation de l'installation
echo "Finalisation :"
echo "Une fois l'installation terminée, connectez-vous avec les identifiants par défaut (glpi/glpi pour l'administrateur)"
echo "Changez immédiatement les mots de passe par défaut"
echo "Supprimez le fichier install.php pour des raisons de sécurité :"
echo "sudo rm /var/www/html/glpi/install/install.php"

echo "Votre instance GLPI est maintenant installée et prête à être utilisée."
echo "N'oubliez pas de configurer les sauvegardes régulières et de maintenir le système à jour."
```

Rendez le script exécutable et exécutez-le :

```bash
chmod +x install_glpi.sh
./install_glpi.sh
```

## 3. Réponses aux configurations demandées lors de l'exécution de ./install_glpi.sh
 ```bash
Enter current password for root : password
Switch to unix_socket authentication [Y/n] Y
Change the root password? [Y/n] n
Remove anonymous users? [Y/n] n
Disallow root login remotely? [Y/n] n
Remove test database and access to it? [Y/n] n
Reload privilege tables now? [Y/n] Y
 ```

## 4. Installation via l'interface web

1. Ouvrez un navigateur et accédez à [http://127.0.0.1/glpi](http://127.0.0.1/glpi)
2. Suivez l'assistant d'installation en sélectionnant la langue, acceptant la licence, et en fournissant les informations de la base de données :
   - Base de données : localhost
   - Utilisateur : glpiuser
   - Mot de passe : password

## 5. Finalisation

1. Une fois l'installation terminée, connectez-vous avec les identifiants par défaut (glpi/glpi pour l'administrateur)
2. Changez immédiatement les mots de passe par défaut
3. Supprimez le fichier `install.php` pour des raisons de sécurité :
   ```bash
   sudo rm /var/www/html/glpi/install/install.php
   ```

Votre instance GLPI est maintenant installée et prête à être utilisée. N'oubliez pas de configurer les sauvegardes régulières et de maintenir le système à jour.

## Références

1. [GLPI Documentation](https://glpi-install.readthedocs.io/en/latest/)
2. [GLPI Installation Guide](https://glpi-install.readthedocs.io/en/latest/install/index.html)
3. [GLPI Project Documentation](https://glpi-project.org/fr/glpi-documentation/)
4. [Installation de GLPI sur Ubuntu](https://glpi-project.org/how-to-install-glpi-on-ubuntu/)
5. [Installation de GLPI sur RHEL](https://www.tecmint.com/install-glpi-asset-management-rhel/)
6. [Comment installer GLPI sur Ubuntu](https://glpi-project.org/fr/comment-installer-glpi-sur-ubuntu/)
7. [Procédures d'installation GLPI](https://faq.teclib.com/03_knowledgebase/procedures/install_glpi/)
8. [Tutoriel Fedora Server GLPI](https://ipv6.rs/tutorial/Fedora_Server_Latest/GLPI/)
9. [OpenClassrooms - Installation de GLPI](https://openclassrooms.com/fr/courses/1730516-gerez-votre-parc-informatique-avec-glpi/5993816-installez-votre-serveur-glpi)
10. [YouTube - Installation de GLPI](https://www.youtube.com/watch?v=Dc0dy1Z6MyM)
11. [AwoUI - Installation de GLPI](https://www.awoui.com/post/installation-de-glpi)
12. [YouTube - Installation de GLPI](https://www.youtube.com/watch?v=AF5pJaQJXvU)
13. [Installation de GLPI 10 sur Debian 12](https://www.it-connect.fr/installation-pas-a-pas-de-glpi-10-sur-debian-12/)
14. [GLPI Project Documentation](https://glpi-project.org/documentation/)
15. [GLPI Project GitHub Documentation](https://github.com/glpi-project/doc-install)

- Ce tutoriel inclut un script shell qui automatise l'installation de GLPI. Il guide l'utilisateur à travers les étapes d'installation et de configuration, assurant une installation correcte et sécurisée.
