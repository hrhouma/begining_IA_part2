L'erreur `Unable to locate package kibana` signifie que le paquet Kibana n'est pas trouvé dans les dépôts par défaut d'APT. Voici comment l'ajouter correctement au dépôt pour Ubuntu/Debian :

### Étapes d'installation de Kibana

1. **Importer la clé GPG de la suite Elastic** :
   ```bash
   wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   ```

2. **Ajouter le dépôt Elastic** :
   Ajoutez le dépôt Elastic à votre liste de sources APT :
   ```bash
   echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
   ```
   Assurez-vous de remplacer `8.x` par la version spécifique de votre installation Elasticsearch si nécessaire.

3. **Mettre à jour les dépôts** :
   ```bash
   sudo apt update
   ```

4. **Installer Kibana** :
   Maintenant, vous pouvez installer Kibana avec :
   ```bash
   sudo apt install kibana
   ```

5. **Configurer et démarrer Kibana** :
   Une fois installé, activez et démarrez Kibana :
   ```bash
   sudo systemctl enable kibana
   sudo systemctl start kibana
   ```

Après cela, vous devriez pouvoir accéder à Kibana via `http://localhost:5601`. Si vous avez besoin d'une autre version de Kibana, vous pouvez ajuster le dépôt Elastic en fonction de la version de la suite que vous utilisez.
