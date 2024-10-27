-------------------------------------------
# Étape 1 - Installation de Kibana
-------------------------------------------

1. **Mettre à jour les packages** :
   ```bash
   sudo apt update
   ```

2. **Importer la clé GPG de la suite Elastic** :
   ```bash
   wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   ```

3. **Ajouter le dépôt Elastic** (*regardez l'annexe 1*):
   Ajoutez le dépôt Elastic à votre liste de sources APT :
   ```bash
   echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
   ```

# (*Regardez l'annexe 1*):
*Assurez-vous de remplacer `8.x` par la version spécifique de votre installation Elasticsearch si nécessaire.*

4. **Mettre à jour les dépôts** :
   ```bash
   sudo apt update
   ```

5. **Installer Kibana** :
   Maintenant, vous pouvez installer Kibana avec :
   ```bash
   sudo apt install kibana
   ```


6. **Configurer et démarrer Kibana** :
   Une fois installé, activez et démarrez Kibana :
   ```bash
   sudo systemctl status kibana
   sudo systemctl enable kibana
   sudo systemctl start kibana
   sudo systemctl status kibana
   ```


7. **Configurer Kibana** :
   Éditez le fichier de configuration :
   ```bash
   sudo nano /etc/kibana/kibana.yml
   ```
   Modifiez les paramètres suivants :
   ```yaml
   server.port: 5601
   server.host: "localhost"
   elasticsearch.hosts: ["http://localhost:9200"]
   ```
8 **Démarrer Kibana** :
   ```bash
   sudo systemctl stop kibana
   sudo systemctl enable kibana
   sudo systemctl start kibana
   ```


9. **Vérifiez l'installation d'Elasticsearch**

Assurez-vous qu'Elasticsearch est bien installé et que son binaire est dans le bon répertoire. Essayez cette commande pour vérifier l'état d'Elasticsearch :

```bash
sudo apt update
sudo apt install elasticsearch
sudo systemctl status elasticsearch
sudo systemctl enable kibana
sudo systemctl start kibana
sudo systemctl status kibana
```

*Cela vous permettra de voir si Elasticsearch est bien installé et en cours d'exécution.*


9. *Vérifiez http://localhost:5601*

- L'écran que vous voyez demande un "Enrollment Token" pour initialiser Kibana.
- Ce token est nécessaire pour connecter Kibana à Elasticsearch, et il est généré lors de l'installation d'Elasticsearch.

### Pour obtenir le token d'enrollment, suivez ces étapes :

1. **Accédez à votre terminal** sur le serveur où Elasticsearch est installé.
   
2. **Générer un token d'enrollment** pour Kibana :
   Utilisez cette commande pour générer un token d'enrollment si vous ne l'avez pas encore fait :
   ```bash
   sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
   ```

3. **Copiez le token généré** dans le terminal.

4. **Collez-le dans Kibana** dans la boîte de dialogue que vous avez montrée, sous "Enrollment token".

# ===> Cela permettra à Kibana de se connecter à votre cluster Elasticsearch et de démarrer la configuration.


------------------
# Annexe 1 :
------------------

# (*Regardez l'annexe 1*):
*Assurez-vous de remplacer `8.x` par la version spécifique de votre installation Elasticsearch si nécessaire.*

- Il est recommandé d'utiliser une version spécifique de Kibana compatible avec celle d'Elasticsearch pour garantir la stabilité et la compatibilité entre les deux services.
- La série de versions doit correspondre, donc si vous utilisez Elasticsearch version 8.x, il est préférable d'installer Kibana de la même série 8.x pour éviter les incompatibilités. Cela s'applique aussi aux configurations et aux fonctionnalités avancées qui peuvent ne pas être prises en charge si les versions diffèrent.
- Pour notre étape 3, vous pourriez ainsi adapter le dépôt Elastic en fonction de la version exacte d'Elasticsearch (par exemple, `8.8` si vous avez cette version spécifique), en modifiant l’URL :

```bash
echo "deb https://artifacts.elastic.co/packages/8.8/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
``` 

Ensuite, vous pourrez continuer les étapes d’installation.

