### Cours : Introduction au Shell Scripting pour l'Installation de GLPI

#### Objectifs du Cours
- Comprendre les bases du shell scripting sur Linux.
- Apprendre à automatiser des tâches système en utilisant des scripts shell.
- Appliquer les concepts appris pour installer et configurer GLPI sur un serveur Linux.

---

### 1. Introduction au Shell Scripting

#### 1.1. Qu'est-ce qu'un Shell ?
Le *shell* est un interpréteur de commandes qui permet d'interagir avec le système d'exploitation via une interface en ligne de commande. Sur les systèmes Linux, les shells les plus courants sont `bash`, `sh`, `zsh`, etc.

#### 1.2. Qu'est-ce qu'un Script Shell ?
Un *script shell* est un fichier texte contenant une série de commandes shell exécutées séquentiellement. C'est un outil puissant pour automatiser des tâches répétitives, telles que l'installation de logiciels, la gestion des systèmes, et plus encore.

---

### 2. Les Bases du Shell Scripting

#### 2.1. Structure d'un Script Shell
Un script shell commence généralement par un *shebang* (`#!/bin/bash`), qui indique au système quel interpréteur utiliser pour exécuter le script.

Exemple :
```bash
#!/bin/bash
echo "Bonjour, ceci est un script shell simple."
```

#### 2.2. Exécution d'un Script Shell
Pour exécuter un script shell, il faut d'abord le rendre exécutable puis le lancer.
```bash
chmod +x mon_script.sh
./mon_script.sh
```

---

### 3. Concepts Clés en Shell Scripting

#### 3.1. Variables
Les variables sont utilisées pour stocker des valeurs qui peuvent être réutilisées dans un script.
```bash
nom="GLPI"
echo "Installation de $nom"
```

#### 3.2. Conditions et Boucles
Les conditions (`if`, `else`) et les boucles (`for`, `while`) permettent de contrôler le flux d'exécution du script.
```bash
if [ "$nom" = "GLPI" ]; then
  echo "Nous installons GLPI"
fi
```

---

### 4. Application : Installation de GLPI via un Script Shell

#### 4.1. Présentation du Script d'Installation

Le script suivant automatise l'installation de GLPI sur un serveur Linux. Il met à jour les paquets, installe les composants nécessaires, configure la base de données, et prépare le serveur web.

**Script : install_glpi.sh**
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

echo "Finalisation :"
echo "Une fois l'installation terminée, connectez-vous avec les identifiants par défaut (glpi/glpi pour l'administrateur)"
echo "Changez immédiatement les mots de passe par défaut"
echo "Supprimez le fichier install.php pour des raisons de sécurité :"
echo "sudo rm /var/www/html/glpi/install/install.php"

echo "Votre instance GLPI est maintenant installée et prête à être utilisée."
echo "N'oubliez pas de configurer les sauvegardes régulières et de maintenir le système à jour."
```

#### 4.2. Explication du Script

- **Mise à jour et installation** : Mise à jour des paquets et installation des composants nécessaires pour GLPI, Apache, PHP, et MariaDB.
- **Sécurisation de MariaDB** : Utilisation de `mysql_secure_installation` pour sécuriser l'installation de MariaDB.
- **Création de la base de données** : Création de la base de données GLPI et d'un utilisateur dédié avec les privilèges appropriés.
- **Téléchargement de GLPI** : Téléchargement de la dernière version de GLPI et extraction dans le répertoire web.
- **Configuration d'Apache** : Création d'un fichier de configuration pour Apache afin de servir GLPI.
- **Permissions** : Configuration des permissions nécessaires pour Apache.
- **Instructions de finalisation** : Indications pour finaliser l'installation via l'interface web.

---

### 5. Pratique : Exécuter le Script d'Installation

#### 5.1. Créer et Exécuter le Script
- **Créer le script** : `nano install_glpi.sh`
- **Rendre le script exécutable** : `chmod +x install_glpi.sh`
- **Exécuter le script** : `./install_glpi.sh`

#### 5.2. Répondre aux Questions de Sécurisation
Lors de l'exécution, vous devrez répondre à quelques questions pour sécuriser l'installation de MariaDB. Les réponses recommandées sont fournies dans le guide.

---

### 6. Conclusion

En suivant ce cours, vous avez appris les bases du shell scripting, et comment l'appliquer pour automatiser l'installation d'une application web comme GLPI sur un serveur Linux. Vous devriez maintenant être capable de modifier et personnaliser ce script pour répondre à vos besoins spécifiques.

---

### Références Complémentaires
- [Documentation GLPI](https://glpi-project.org/documentation)
- [Guide d'Installation GLPI](https://glpi-project.org/installation)
