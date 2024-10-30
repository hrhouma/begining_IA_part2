
# 1. Erreur 1 (sans les guillemets )

```bash
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
curl -XGET localhost:9200
```
## ➔ 1.1. ERREUR

```bash
curl: (52) Empty reply from server
```
## ➔ 1.2. RÉSOLUTION

```bash
curl -k -XGET "https://localhost:9200"
```

## ➔ 1.2.1 EXPLICATION

Dans la commande `curl -k -XGET "https://localhost:9200"`, les options `-k` et les guillemets (`"`) autour de l'URL ont des significations spécifiques :

1. **Option `-k`** : Cette option indique à `curl` d'ignorer les erreurs de validation du certificat SSL. Elle est utile lorsqu'on accède à un serveur HTTPS avec un certificat auto-signé ou non validé par une autorité de certification reconnue. En utilisant `-k`, `curl` accepte la connexion même si le certificat SSL n'est pas considéré comme sûr par défaut.

2. **Les guillemets autour de l'URL (`"`)** : Ils permettent de s'assurer que l'URL complète est interprétée comme un seul argument de la commande. Ceci est particulièrement important si l'URL contient des caractères spéciaux, des espaces ou des variables qui pourraient être mal interprétés sans guillemets.

En résumé, cette commande permet d'envoyer une requête `GET` vers `https://localhost:9200` en ignorant les erreurs SSL, ce qui est fréquent lors de tests locaux ou de connexions à des serveurs avec des certificats non validés.


## ➔ 1.3. AUTRES
###### ➔ AUTRES COMMANDES INTÉRESSANTES: ANENXE 01 - partie 01
###### ➔  AUTRES COMMANDES INTÉRESSANTES: ANENXE 01 - partie 02
######  ➔ AUTRES COMMANDES INTÉRESSANTES: ANENXE 01 - partie 03
######  ➔ AUTRES COMMANDES INTÉRESSANTES: ANENXE 01 - partie 04
######  ➔ AUTRES COMMANDES INTÉRESSANTES: ANENXE 01 - partie 05


-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# Erreur 2 (sans le -u , sans le password, sans le -k)


```bash
curl -XGET localhost:9200
```


## ➔ 2.1. ERREUR

```bash
curl: (52) Empty reply from server
```
## ➔ 2.2. RÉSOLUTION

```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic  (pour générer un nouveau mot de passe)
curl -u elastic:c+vdv5FUzys5hft5*8Fs -X GET 'https://localhost:9200/' (sans le -k , encore une erreur)
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/' (ette option indique à curl d'ignorer les erreurs de validation du certificat SSL)
```

## ➔ 2.2.1 RÉSOLUTION FINALE

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/' (ette option indique à curl d'ignorer les erreurs de validation du certificat SSL)
```


## ➔ 2.3. AUTRES
###### ➔ Pas d'annexe 2 :(



-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

# Erreur 3 (sans le -k)

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -X GET 'https://localhost:9200/'
```

## ➔ 3.1. ERREUR

```bash
curl: (60) SSL certificate problem: self-signed certificate in certificate chain
```

```bash
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```


## ➔ 3.2. RÉSOLUTION

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
{
  "name" : "elasticSrv1",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "koIiPy-iTY6IuEnzMuhXEw",
  "version" : {
    "number" : "8.15.3",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "f97532e680b555c3a05e73a74c28afb666923018",
    "build_date" : "2024-10-09T22:08:00.328917561Z",
    "build_snapshot" : false,
    "lucene_version" : "9.11.1",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}

```

-k : Cette option indique à curl d'ignorer les erreurs de validation du certificat SSL


## ➔ 3.3. EXPLICATION DE LA RÉSOLUTION

Le problème rencontré ici est lié au certificat SSL auto-signé utilisé par Elasticsearch. Lorsque vous avez tenté d'accéder au serveur avec la commande `curl`, l'erreur suivante est apparue :

```
curl: (60) SSL certificate problem: self-signed certificate in certificate chain
```

### Explication de l'erreur
Cette erreur signifie que `curl` a tenté de vérifier le certificat SSL présenté par le serveur Elasticsearch mais a échoué. Le certificat est auto-signé, ce qui signifie qu'il n'a pas été émis par une autorité de certification (CA) reconnue et de confiance. Par défaut, `curl` exige que le certificat soit signé par une CA pour garantir une connexion sécurisée et éviter les connexions potentiellement compromises.

### Solutions au problème

1. **Utilisation de l'option `-k` ou `--insecure` avec `curl`** :
   En utilisant l'option `-k`, comme dans la commande suivante :
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
   ```
   Vous indiquez à `curl` d'ignorer l'erreur de vérification du certificat SSL et de procéder malgré l'absence de vérification. C'est pourquoi cette commande a fonctionné : `curl` a ignoré la vérification du certificat auto-signé et a établi une connexion.

2. **Ajout du certificat auto-signé aux certificats de confiance** (pour éviter d'utiliser `-k`) :
   Si vous souhaitez éviter d'utiliser l'option `-k` pour chaque requête et préférez que le certificat auto-signé soit considéré comme "fiable" par `curl`, vous pouvez ajouter le certificat d’Elasticsearch aux certificats de confiance de votre système. Cela rend la vérification SSL possible même avec un certificat auto-signé.

3. **Utiliser un certificat émis par une CA reconnue** :
   Si Elasticsearch est utilisé dans un environnement de production, il est recommandé d’utiliser un certificat signé par une autorité de certification reconnue pour éviter d’ignorer les erreurs de vérification. Vous pouvez obtenir un certificat SSL valide auprès d'une CA ou configurer un certificat Let's Encrypt pour les environnements accessibles publiquement.

### En résumé
L'option `-k` vous a permis d'ignorer la vérification SSL et de vous connecter avec succès à Elasticsearch. Toutefois, cette solution est principalement recommandée pour des environnements de test, car elle désactive une mesure de sécurité importante. Pour une utilisation à long terme, envisagez l'ajout du certificat auto-signé aux certificats de confiance de votre système ou l'obtention d'un certificat signé.


## ➔ 3.3. AUTRES
###### ➔ Pas d'annexe 3 :(


-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------


# Erreur 4 (Pouvons-nous automatiser le tous ?)


## ➔ 4.1. ERREUR

```
curl: (60) SSL certificate problem: self-signed certificate in certificate chain
```


## ➔ 4.2. RÉSOLUTION

```
Le script dans l'annexe 4-1 est correct et fonctionnera pour automatiser la configuration d’authentification `curl` pour Elasticsearch. 
Mais il y a quelques points importants à vérifier et des améliorations possibles pour garantir son bon fonctionnement :
```

1. **SSL et Paramètre `-k` pour Ignorer les Certificats Auto-signés** :
   Puisque Elasticsearch utilise un certificat auto-signé, il faut ajouter `-k` pour les requêtes `curl`, afin d'ignorer les erreurs SSL.

   Modifiez la ligne de test de connexion à la fin du script pour inclure `-k` :
   ```bash
   curl -k -XGET "https://localhost:9200"
   ```

2. **Mise à jour des Identifiants** :
   Assurez-vous de remplacer `your_password` par le mot de passe réel de votre utilisateur Elasticsearch avant d’exécuter le script.

3. **Sécurisation** :
   Comme le fichier `.curlrc` contiendra des informations d'identification sensibles, assurez-vous que les permissions sont bien définies (ce que le script fait avec `chmod 600`). Cela garantit que seul l’utilisateur actuel peut lire le fichier.

4. **Alternative pour des Environnements de Production** :
   Dans un environnement de production, plutôt que de désactiver la vérification SSL (`-k`), il serait préférable d'ajouter le certificat auto-signé au magasin de certificats de votre système pour éviter d'utiliser `-k` et améliorer la sécurité.

### Version Mise à Jour du Script de celui dans l'annexe 4.1

Voici la version finale du script avec l’ajout de `-k` pour la vérification SSL :

```bash
#!/bin/bash

# Variables pour le nom d'utilisateur et le mot de passe Elasticsearch
ES_USER="elastic"
ES_PASSWORD="your_password"  # Remplacez par votre mot de passe réel

# Chemin du fichier .curlrc pour stocker les identifiants
CURLRC_PATH="$HOME/.curlrc"

# Vérifie si Elasticsearch est en cours d'exécution
echo "Vérification du statut d'Elasticsearch..."
if systemctl is-active --quiet elasticsearch; then
    echo "Elasticsearch est en cours d'exécution."
else
    echo "Elasticsearch n'est pas démarré. Tentative de démarrage..."
    sudo systemctl start elasticsearch
    sleep 5  # Attendre quelques secondes pour s'assurer qu'Elasticsearch démarre
fi

# Ajoute les identifiants au fichier .curlrc
echo "Configuration de l'authentification pour les requêtes curl..."
echo "user = \"$ES_USER:$ES_PASSWORD\"" > "$CURLRC_PATH"
chmod 600 "$CURLRC_PATH"  # Sécuriser le fichier pour que seul l'utilisateur puisse y accéder

# Vérifie si les identifiants ont été ajoutés avec succès
if grep -q "$ES_USER" "$CURLRC_PATH"; then
    echo "Authentification configurée avec succès dans $CURLRC_PATH"
else
    echo "Échec de la configuration de l'authentification."
fi

# Test de connexion pour vérifier que tout fonctionne
echo "Test de connexion à Elasticsearch..."
curl -k -XGET "https://localhost:9200"
```

En utilisant ce script, vous pourrez automatiser la configuration pour `curl` avec l’authentification Elasticsearch, et la connexion devrait fonctionner même si Elasticsearch utilise un certificat auto-signé.




## ➔ 4.3. AUTRES
###### ➔ ANENXE 04 - partie 01 






-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------











# Erreur 5 (grep dans un fichier volumineux ?)


## ➔ 5.1. ERREUR

```
Je veux effectuer un grep de ca xpack.security.enabled: dans /etc/elasticsearch/elasticsearch.yml

```


## ➔ 5.2. RÉSOLUTION


- Pour rechercher la présence de `xpack.security.enabled:` dans le fichier `/etc/elasticsearch/elasticsearch.yml`, vous pouvez utiliser la commande suivante :

```bash
grep "xpack.security.enabled:" /etc/elasticsearch/elasticsearch.yml
```

### Explication

- `grep "xpack.security.enabled:"` : recherche la ligne contenant `xpack.security.enabled:`.
- `/etc/elasticsearch/elasticsearch.yml` : indique le fichier dans lequel faire la recherche.

### Variante pour Voir les Lignes Contenant “enabled” dans le Contexte d’En-têtes
Si vous voulez voir toutes les lignes contenant “enabled” dans le fichier :

```bash
grep "enabled" /etc/elasticsearch/elasticsearch.yml
```

Cela affichera toutes les occurrences du mot `enabled` dans le fichier de configuration Elasticsearch, y compris le paramètre de sécurité `xpack.security.enabled`.





## ➔ 5.3. AUTRES
###### ➔ ANENXE 05 - pas d'annexes :(




-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# Erreur 6 (grep dans un fichier volumineux + numéro de la ligne ?)


## ➔ 6.1. ERREUR

```
Je veux effectuer un grep de ca xpack.security.enabled: dans /etc/elasticsearch/elasticsearch.yml + NUMÉRO DE LA LIGNE

```


## ➔ 6.2. RÉSOLUTION

Pour afficher le numéro de ligne où `xpack.security.enabled:` se trouve dans le fichier `/etc/elasticsearch/elasticsearch.yml`, utilisez l'option `-n` avec `grep` :

```bash
grep -n "xpack.security.enabled:" /etc/elasticsearch/elasticsearch.yml
```

### Explication

- `-n` : affiche le numéro de ligne pour chaque correspondance.
- `"xpack.security.enabled:"` : la chaîne que vous recherchez.
- `/etc/elasticsearch/elasticsearch.yml` : le fichier dans lequel la recherche est effectuée.

La sortie de cette commande indiquera le numéro de ligne où le paramètre `xpack.security.enabled` est défini, ce qui vous permettra de le localiser facilement dans le fichier.




## ➔ 6.3. AUTRES
###### ➔ ANENXE 05 - pas d'annexes :(





-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

# Erreur 7

--------------------
# Objectif - Créer, configurer et exécuter le script `configure_elastic.sh` pour automatiser l’authentification `curl` avec Elasticsearch
--------------------

Comment créer, configurer et exécuter le script `configure_elastic.sh` pour automatiser l’authentification `curl` avec Elasticsearch ?

### Étape 1 : Créer le script `configure_elastic.sh`

1. Ouvrez un terminal et utilisez `nano` pour créer le fichier de script.
   ```bash
   nano configure_elastic.sh
   ```
   
2. Dans l'éditeur `nano`, copiez et collez le script suivant :

   ```bash
   #!/bin/bash

   # Variables pour le nom d'utilisateur et le mot de passe Elasticsearch
   ES_USER="elastic"
   ES_PASSWORD="your_password"  # Remplacez par votre mot de passe réel

   # Chemin du fichier .curlrc pour stocker les identifiants
   CURLRC_PATH="$HOME/.curlrc"

   # Vérifie si Elasticsearch est en cours d'exécution
   echo "Vérification du statut d'Elasticsearch..."
   if systemctl is-active --quiet elasticsearch; then
       echo "Elasticsearch est en cours d'exécution."
   else
       echo "Elasticsearch n'est pas démarré. Tentative de démarrage..."
       sudo systemctl start elasticsearch
       sleep 5  # Attendre quelques secondes pour s'assurer qu'Elasticsearch démarre
   fi

   # Ajoute les identifiants au fichier .curlrc
   echo "Configuration de l'authentification pour les requêtes curl..."
   echo "user = \"$ES_USER:$ES_PASSWORD\"" > "$CURLRC_PATH"
   chmod 600 "$CURLRC_PATH"  # Sécuriser le fichier pour que seul l'utilisateur puisse y accéder

   # Vérifie si les identifiants ont été ajoutés avec succès
   if grep -q "$ES_USER" "$CURLRC_PATH"; then
       echo "Authentification configurée avec succès dans $CURLRC_PATH"
   else
       echo "Échec de la configuration de l'authentification."
   fi

   # Test de connexion pour vérifier que tout fonctionne
   echo "Test de connexion à Elasticsearch..."
   curl -k -XGET "https://localhost:9200"
   ```

3. **Enregistrez et quittez** `nano` :
   - Appuyez sur `Ctrl + X`, puis `Y` pour confirmer et `Enter` pour sauvegarder.

### Étape 2 : Rendre le script exécutable

1. Utilisez la commande suivante pour ajouter les permissions d'exécution au script :
   ```bash
   chmod +x configure_elastic.sh
   ```

### Étape 3 : Exécuter le script

1. Lancez le script pour configurer l'authentification `curl` pour Elasticsearch :
   ```bash
   ./configure_elastic.sh
   ```

### Explications du script

- **Définition des identifiants** : 
  Le script définit les variables `ES_USER` et `ES_PASSWORD` pour les informations d’identification Elasticsearch. Remplacez `"your_password"` par le mot de passe réel.

- **Vérification du statut d'Elasticsearch** :
  Le script vérifie si le service Elasticsearch est actif et tente de le démarrer si nécessaire.

- **Création du fichier `.curlrc`** :
  Le script crée un fichier `.curlrc` contenant les informations d'identification pour que `curl` puisse se connecter automatiquement.

- **Sécurisation du fichier `.curlrc`** :
  La commande `chmod 600` garantit que seul l'utilisateur actuel peut lire le fichier `.curlrc`, car il contient des informations sensibles.

- **Test de connexion avec `curl`** :
  Le script effectue une requête `curl` vers Elasticsearch en utilisant l’option `-k` pour ignorer les erreurs de certificat auto-signé.

### Vérification

- **Test de Connexion** : Une fois le script exécuté, le dernier message devrait afficher des informations provenant d'Elasticsearch, confirmant que la connexion fonctionne.

Ce tutoriel vous guide pas à pas pour configurer et exécuter le script `configure_elastic.sh`, afin d'automatiser l'authentification de `curl` avec Elasticsearch en utilisant un certificat auto-signé.



-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

# Erreur 8 - Éviter de saisir le mot de passe à chaque requête `curl
#  nano /home/eleve/.curlrc ou  nano /root/.curlrc ?
--------------------

- En suivant ces étapes, vous éviterez de saisir le mot de passe à chaque requête `curl`, et le fichier `.curlrc` stockera les informations d'identification de manière pratique et sécurisée.

- Voici comment utiliser un fichier `.curlrc` pour stocker les informations d'identification de manière à ce que `curl` n'ait plus besoin que vous entriez manuellement l’utilisateur et le mot de passe à chaque requête.

### Étape 1 : Créer le fichier `.curlrc`

1. Utilisez `nano` pour créer le fichier `.curlrc` dans le répertoire de l’utilisateur `eleve` :
   ```bash
   nano /home/eleve/.curlrc
   ```

2. Ajoutez les informations d'identification dans le fichier `.curlrc` sous la forme suivante :
   ```bash
   user = "elastic:c+vdv5FUzys5hft5*8Fs"
   ```

   - **user** : Le nom d’utilisateur pour Elasticsearch (`elastic`).
   - **c+vdv5FUzys5hft5*8Fs** : Le mot de passe associé à l’utilisateur `elastic`.
   - Assurez-vous de remplacer ces informations si vos identifiants changent.

3. **Enregistrez et quittez** `nano` :
   - Appuyez sur `Ctrl + X`, puis `Y` pour confirmer et `Enter` pour sauvegarder.

### Étape 2 : Protéger le fichier `.curlrc`

Pour sécuriser le fichier et s'assurer qu'il est accessible uniquement par l'utilisateur `eleve`, définissez les permissions avec la commande suivante :
```bash
chmod 600 /home/eleve/.curlrc
```

### Étape 3 : Tester la configuration

Maintenant, chaque fois que vous utiliserez `curl` sans spécifier d’utilisateur, `curl` ajoutera automatiquement les informations d'identification à partir du fichier `.curlrc`. 

Par exemple, pour tester la connexion à Elasticsearch avec `curl`, vous pouvez exécuter la commande suivante :
```bash
curl -k -X GET 'https://localhost:9200/'
```

L'option `-k` est utilisée ici pour ignorer les erreurs SSL, car Elasticsearch utilise un certificat auto-signé.

---

# ==> erreur : 

```bash
curl -k -X GET 'https://localhost:9200/'
{"error":{"root_cause":[{"type":"security_exception","reason":"missing authentication credentials for REST request [/]","header":{"WWW-Authenticate":["Basic realm=\"security\", charset=\"UTF-8\"","Bearer realm=\"security\"","ApiKey"]}}],"type":"security_exception","reason":"missing authentication credentials for REST request [/]","header":{"WWW-Authenticate":["Basic realm=\"security\", charset=\"UTF-8\"","Bearer realm=\"security\"","ApiKey"]}},"status":401}root@elasticSrv1:/hroot@elasticSrv1:/home/eleve# sudo systemctl restart elasticsearch
root@elasticSrv1:/home/eleve# curl -k -X GET 'https://localhost:9200/'
{"error":{"root_cause":[{"type":"security_exception","reason":"missing authentication credentials for REST request [/]","header":{"WWW-Authenticate":["Basic realm=\"security\", charset=\"UTF-8\"","Bearer realm=\"security\"","ApiKey"]}}],"type":"security_exception","reason":"missing authentication credentials for REST request [/]","header":{"WWW-Authenticate":["Basic realm=\"security\", charset=\"UTF-8\"","Bearer realm=\"security\"","ApiKey"]}},"status":401}r
```


## ➔ 8.1. RÉSOLUTION



Il semble que malgré l'utilisation du fichier `.curlrc`, les informations d'identification ne sont pas prises en compte automatiquement par `curl`, ce qui entraîne l'erreur d'authentification. Voici quelques solutions pour résoudre ce problème :

### Solution 1 : Vérifiez le contenu et le chemin du fichier `.curlrc`

Assurez-vous que le fichier `.curlrc` est bien créé dans le répertoire de l'utilisateur qui exécute `curl`. Si vous utilisez `root`, placez le fichier dans le répertoire `/root/` au lieu de `/home/eleve/`.

1. **Vérifiez le contenu du fichier `.curlrc`** :
   ```bash
   cat /root/.curlrc
   ```
   Le fichier doit contenir exactement :
   ```bash
   user = "elastic:c+vdv5FUzys5hft5*8Fs"
   ```

2. **Vérifiez les permissions du fichier** :
   Assurez-vous que le fichier `.curlrc` a bien les permissions `600` :
   ```bash
   chmod 600 /root/.curlrc
   ```

3. **Exécutez `curl` en tant que `root`** :
   Si vous êtes connecté en tant que `root`, essayez la commande suivante :
   ```bash
   curl -k -X GET 'https://localhost:9200/'
   ```

### Solution 2 : Utiliser directement l'option `-u` avec `curl`

Si le fichier `.curlrc` ne fonctionne toujours pas, utilisez l'option `-u` pour spécifier manuellement les informations d'identification dans la commande `curl` :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
```

### Solution 3 : Vérifiez la configuration Elasticsearch

Si vous continuez à rencontrer des problèmes, il est possible qu'il y ait une configuration erronée dans `elasticsearch.yml`. Assurez-vous que la sécurité est bien activée et que les paramètres sont corrects. Vous devriez avoir dans `/etc/elasticsearch/elasticsearch.yml` :

```yaml
xpack.security.enabled: true
xpack.security.http.ssl.enabled: true
```

Après chaque modification dans `elasticsearch.yml`, redémarrez Elasticsearch :
```bash
sudo systemctl restart elasticsearch
```

### Solution 4 : Testez l'authentification avec un autre outil (comme `Postman`)

Pour vérifier si le problème vient de `curl` ou de la configuration Elasticsearch, essayez de vous connecter à `https://localhost:9200/` avec les identifiants via un outil comme **Postman** (ou un autre client HTTP) en utilisant l’authentification de base.

Ces étapes devraient aider à isoler le problème. Si cela persiste, cela peut être dû à un problème de configuration plus profond dans Elasticsearch ou dans votre environnement `curl`.




-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

# Erreur 9  - Je veux importer des donnees volumineuses avec curl dans elastic search maintenant , voici mes donnees en local News_Category_Dataset_v2.json
--------------------


Pour importer un fichier JSON volumineux, tel que `News_Category_Dataset_v2.json`, dans Elasticsearch avec `curl`, voici les étapes à suivre :

### Prérequis

1. Assurez-vous que votre fichier `News_Category_Dataset_v2.json` est dans le bon format pour Elasticsearch. Le fichier doit contenir un document JSON par ligne pour chaque enregistrement, par exemple :

   ```json
   {"category": "World", "headline": "Headline text", "authors": "Author name", "link": "URL", "short_description": "Description", "date": "2022-01-01"}
   {"category": "Politics", "headline": "Another headline", "authors": "Author name", "link": "URL", "short_description": "Description", "date": "2022-01-02"}
   ```
   Assurez-vous que chaque ligne est un objet JSON indépendant (format NDJSON ou JSON lignes).

2. **Index Elasticsearch** : Créez un index pour stocker les données, par exemple `news`. Vous pouvez utiliser la commande suivante :
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X PUT "https://localhost:9200/news"
   ```

### Étape 1 : Préparer le fichier pour le Bulk API

Elasticsearch propose une **API Bulk** pour importer des données volumineuses. Vous devez préparer le fichier en ajoutant une ligne d'action avant chaque document JSON. Cela pourrait ressembler à ceci :

   ```json
   {"index": {}}
   {"category": "World", "headline": "Headline text", "authors": "Author name", "link": "URL", "short_description": "Description", "date": "2022-01-01"}
   {"index": {}}
   {"category": "Politics", "headline": "Another headline", "authors": "Author name", "link": "URL", "short_description": "Description", "date": "2022-01-02"}
   ```

### Étape 2 : Importer le fichier avec `curl`

Une fois le fichier prêt, utilisez la commande `curl` pour le charger dans Elasticsearch via l’API Bulk :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_v2.json"
```

### Explications

- `-u elastic:c+vdv5FUzys5hft5*8Fs` : Les informations d'authentification pour Elasticsearch.
- `-k` : Ignorer les erreurs SSL (si vous utilisez un certificat auto-signé).
- `-X POST "https://localhost:9200/news/_bulk"` : Utilise l'API Bulk pour envoyer les données dans l’index `news`.
- `-H "Content-Type: application/json"` : Indique que le contenu est en JSON.
- `--data-binary "@News_Category_Dataset_v2.json"` : Charge le fichier JSON en utilisant `--data-binary` pour envoyer les données sans modifications.

### Étape 3 : Vérifier l'importation

Une fois le chargement terminé, vous pouvez vérifier si les données ont été importées correctement en exécutant une requête de recherche simple :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty"
```

Cela affichera quelques documents de l'index `news` pour confirmer que les données ont été importées.

### Remarque

Si vous avez beaucoup de données, l'importation peut être lente. Pour des volumes très importants, il est recommandé d'utiliser un client Elasticsearch comme `Logstash` ou `Elasticsearch Python client`, qui est mieux optimisé pour l'importation en masse que `curl`.


-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

# Erreur 10  - Format d'importation (correction dans le point erreur 11)
--------------------



### Commande 1
```bash
root@elasticSrv1:/home/eleve# curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X PUT "https://localhost:9200/news"
```

**Résultat 1**
```json
{"acknowledged":true,"shards_acknowledged":true,"index":"news"}
```

---

### Commande 2
```bash
root@elasticSrv1:/home/eleve# curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_v2.json"
```

**Résultat 2**
```json
{
  "error": {
    "root_cause": [
      {
        "type": "illegal_argument_exception",
        "reason": "Malformed action/metadata line [1], expected field [create], [delete], [index] or [update] but found [category]"
      }
    ],
    "type": "illegal_argument_exception",
    "reason": "Malformed action/metadata line [1], expected field [create], [delete], [index] or [update] but found [category]"
  },
  "status": 400
}
```

---

### Exemples de données envoyées

Les données envoyées dans la requête `_bulk` contiennent des lignes comme celles-ci :
```json
{"category": "CRIME", "headline": "There Were 2 Mass Shootings In Texas Last Week, But Only 1 On TV", "authors": "Melissa Jeltsen", "link": "https://www.huffingtonpost.com/entry/texas-amanda-painter-mass-shooting_us_5b081ab4e4b0802d69caad89", "short_description": "She left her husband. He killed their children. Just another day in America.", "date": "2018-05-26"}
{"category": "ENTERTAINMENT", "headline": "Will Smith Joins Diplo And Nicky Jam For The 2018 World Cup's Official Song", "authors": "Andy McDonald", "link": "https://www.huffingtonpost.com/entry/will-smith-joins-diplo-and-nicky-jam-for-the-official-2018-world-cup-song_us_5b09726fe4b0fdb2aa541201", "short_description": "Of course it has a song.", "date": "2018-05-26"}
```

Pour corriger cette erreur, chaque bloc de données doit être précédé d’une ligne d’instruction d’action, par exemple :
```json
{ "index": {} }
{"category": "CRIME", "headline": "There Were 2 Mass Shootings In Texas Last Week, But Only 1 On TV", "authors": "Melissa Jeltsen", "link": "https://www.huffingtonpost.com/entry/texas-amanda-painter-mass-shooting_us_5b081ab4e4b0802d69caad89", "short_description": "She left her husband. He killed their children. Just another day in America.", "date": "2018-05-26"}
```


-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

# Erreur 11  - Format d'importation (correction du point erreur 10)
--------------------



L'erreur rencontrée au point 10 signifie que le format du fichier `News_Category_Dataset_v2.json` n'est pas conforme à ce qu'attend l'API Bulk d'Elasticsearch. Le fichier JSON que nous utilisons contient des objets JSON sans les actions nécessaires pour le Bulk API.

Pour que l'API Bulk fonctionne, chaque document doit être précédé d'une ligne d'action (`{"index": {}}`) qui indique qu'il s'agit d'une opération d'indexation. Voici comment préparer le fichier pour un import correct.

### Étape 1 : Préparer le fichier `News_Category_Dataset_v2.json` pour l'importation en bulk

Vous pouvez utiliser un outil comme `sed`, `awk` ou un script Python pour ajouter une ligne d'action avant chaque ligne JSON.

#### Option 1 : Utiliser un script Python pour préparer le fichier

Voici un script Python simple pour transformer votre fichier JSON existant en un format compatible avec l'API Bulk d'Elasticsearch.

```python
# Script Python pour préparer le fichier Bulk
input_file = 'News_Category_Dataset_v2.json'
output_file = 'News_Category_Dataset_bulk.json'

with open(input_file, 'r') as infile, open(output_file, 'w') as outfile:
    for line in infile:
        # Écrire la ligne d'action pour chaque document
        outfile.write('{"index": {}}\n')
        # Écrire le document JSON lui-même
        outfile.write(line)

print(f"Fichier {output_file} prêt pour importation en bulk.")
```

Enregistrez ce script sous le nom `prepare_bulk.py` et exécutez-le :

```bash
python3 prepare_bulk.py
```

Cela créera un nouveau fichier `News_Category_Dataset_bulk.json` qui est prêt pour l'importation avec l'API Bulk d'Elasticsearch.

#### Option 2 : Utiliser `sed` dans le terminal pour ajouter des lignes d'index

Si vous préférez utiliser `sed` dans le terminal (si le fichier est très volumineux, cela pourrait être plus rapide), exécutez cette commande :

```bash
sed 's/^/{\"index\": {}}\n&/' News_Category_Dataset_v2.json > News_Category_Dataset_bulk.json
```

### Étape 2 : Importer le fichier transformé dans Elasticsearch

Une fois le fichier `News_Category_Dataset_bulk.json` prêt, utilisez `curl` pour l'importer dans Elasticsearch :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_bulk.json"
```

### Explications

- **`{"index": {}}`** : Cette ligne d'action est nécessaire pour chaque document JSON, elle informe Elasticsearch qu'il s'agit d'une opération d'indexation.
- **Fichier JSON** : `News_Category_Dataset_bulk.json` contient maintenant les lignes d'action nécessaires pour l'API Bulk.

### Étape 3 : Vérifier l'importation

Vous pouvez vérifier que les données ont été correctement importées avec une requête de recherche simple :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty"
```

Si tout s'est bien passé, vous devriez voir des documents dans l'index `news`.

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# Erreur 12  - Écrire des exemples de requêtes
--------------------

Pour écrire des requêtes dans Elasticsearch, vous pouvez utiliser l'API de recherche et l'API de requête pour explorer et filtrer les données que vous avez importées. Voici quelques exemples de requêtes pour explorer votre index `news` contenant les données du fichier `News_Category_Dataset_v2.json`.

### 1. Requête de Base : Rechercher Tous les Documents

Pour obtenir tous les documents de l'index `news`, utilisez la requête suivante :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty"
```

### 2. Requête de Filtrage : Rechercher par Catégorie

Pour rechercher tous les articles de la catégorie `ENTERTAINMENT`, vous pouvez utiliser un filtre :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "category": "ENTERTAINMENT"
    }
  }
}'
```

### 3. Requête de Filtrage Multiple : Rechercher par Catégorie et Auteur

Si vous voulez rechercher tous les articles dans la catégorie `ENTERTAINMENT` écrits par `Ron Dicker` :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        { "match": { "category": "ENTERTAINMENT" }},
        { "match": { "authors": "Ron Dicker" }}
      ]
    }
  }
}'
```

### 4. Requête de Texte Complet : Rechercher par Titre (Headline)

Pour rechercher des articles contenant un mot spécifique dans le titre (par exemple, `Trump`), utilisez `match` sur le champ `headline` :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "headline": "Trump"
    }
  }
}'
```

### 5. Requête de Plage : Rechercher par Date

Si vous souhaitez rechercher des articles publiés après une certaine date (par exemple, après le 1er mai 2018) :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "range": {
      "date": {
        "gte": "2018-05-01"
      }
    }
  }
}'
```

### 6. Requête de Texte Complet : Rechercher par Description

Pour rechercher des articles contenant un mot spécifique dans la description courte (`short_description`), utilisez `match` sur ce champ. Par exemple, pour rechercher le mot `song` dans la description :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "short_description": "song"
    }
  }
}'
```

### 7. Requête de Comptage : Compter le Nombre d’Articles par Catégorie

Vous pouvez utiliser une agrégation pour compter le nombre d'articles par catégorie :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "size": 0,
  "aggs": {
    "categories_count": {
      "terms": {
        "field": "category.keyword"
      }
    }
  }
}'
```

### 8. Trier les Résultats : Rechercher et Trier par Date

Pour rechercher des articles dans la catégorie `ENTERTAINMENT` et les trier par date dans l’ordre décroissant (du plus récent au plus ancien) :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "category": "ENTERTAINMENT"
    }
  },
  "sort": [
    { "date": "desc" }
  ]
}'
```

### 9. Pagination des Résultats : Limiter et Paginer les Résultats

Pour obtenir les 5 premiers résultats, puis paginer avec `from` et `size` :

- **Première page (documents 0 à 4)** :
  ```bash
  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
  {
    "from": 0,
    "size": 5,
    "query": {
      "match_all": {}
    }
  }'
  ```

- **Deuxième page (documents 5 à 9)** :
  ```bash
  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
  {
    "from": 5,
    "size": 5,
    "query": {
      "match_all": {}
    }
  }'
  ```

Ces exemples couvrent les requêtes de base pour interagir avec vos données dans Elasticsearch. Vous pouvez adapter ces requêtes pour explorer différentes parties de vos données selon vos besoins.


-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------


--------------------
# ANENXE 01 - partie 01
--------------------

Le message d'erreur `curl: (52) Empty reply from server` signifie qu'il n'y a pas de réponse de la part du serveur Elasticsearch sur le port 9200. Voici quelques étapes de dépannage :

1. **Vérifier le statut du service Elasticsearch** :
   Assurez-vous qu'Elasticsearch est bien en cours d'exécution.
   ```bash
   sudo systemctl status elasticsearch
   ```
   Si le statut est "inactive" ou "failed", essayez de redémarrer le service :
   ```bash
   sudo systemctl restart elasticsearch
   ```

2. **Vérifier les logs d’Elasticsearch** :
   Consultez les journaux pour plus de détails sur la raison pour laquelle le service pourrait ne pas répondre.
   ```bash
   sudo journalctl -u elasticsearch
   ```
   Ou regardez dans le fichier de log d’Elasticsearch, souvent situé dans `/var/log/elasticsearch/`.

3. **Vérifier la configuration réseau** :
   Assurez-vous qu'Elasticsearch est configuré pour écouter les bonnes interfaces réseau. Ouvrez le fichier de configuration :
   ```bash
   sudo nano /etc/elasticsearch/elasticsearch.yml
   ```
   Recherchez les paramètres `network.host` et `http.port`. Si `network.host` est configuré sur `localhost` ou `127.0.0.1`, cela signifie qu'Elasticsearch n'est accessible que localement, ce qui est correct si vous testez depuis le même serveur.

4. **Redémarrer et tester de nouveau** :
   Après avoir appliqué les corrections éventuelles, redémarrez Elasticsearch :
   ```bash
   sudo systemctl restart elasticsearch
   ```
   Puis, réessayez la commande `curl` :
   ```bash
   curl -XGET localhost:9200
   ```





--------------------
# ANENXE 01 - partie 02
--------------------


Dans le fichier de configuration `/etc/elasticsearch/elasticsearch.yml`, vous pouvez ajuster les paramètres réseau en modifiant les options suivantes pour qu’Elasticsearch écoute sur la bonne interface réseau :

1. **Paramètre `network.host`** : Définit l’interface réseau sur laquelle Elasticsearch écoute. Les valeurs courantes sont :
   - `localhost` ou `127.0.0.1` : pour autoriser uniquement les connexions locales.
   - `0.0.0.0` : pour écouter sur toutes les interfaces réseau (nécessaire si vous voulez que le serveur soit accessible à distance).
   - L’adresse IP de l’interface spécifique : pour limiter Elasticsearch à une adresse IP particulière sur le serveur.

   ```yaml
   network.host: localhost     # Ou bien 0.0.0.0 pour des connexions distantes
   ```

2. **Paramètre `http.port`** : Définit le port HTTP pour Elasticsearch, qui est par défaut `9200`. Vous pouvez le changer si nécessaire, mais dans la plupart des cas, laisser le port par défaut est suffisant.

   ```yaml
   http.port: 9200
   ```

3. **Paramètres supplémentaires pour un accès distant** (optionnel) :
   Si vous ouvrez Elasticsearch pour un accès à distance (par exemple, avec `network.host: 0.0.0.0`), pensez à configurer des restrictions de pare-feu et à utiliser des paramètres de sécurité comme les `xpack.security`.

   **Exemple de configuration dans `/etc/elasticsearch/elasticsearch.yml`** :
   ```yaml
   network.host: 0.0.0.0   # Permet l’accès à distance
   http.port: 9200         # Port par défaut d’Elasticsearch
   ```

4. **Redémarrez Elasticsearch** après avoir modifié la configuration :
   ```bash
   sudo systemctl restart elasticsearch
   ```

Ces paramètres devraient vous aider à vérifier ou ajuster la configuration réseau de votre instance Elasticsearch.


--------------------
# ANENXE 01 - partie 03
--------------------

L'erreur `curl: (52) Empty reply from server` persiste, ce qui peut indiquer un problème au niveau de la configuration ou du démarrage d'Elasticsearch. Voici quelques étapes supplémentaires pour résoudre ce problème :

1. **Vérifiez les journaux d'Elasticsearch** pour tout message d'erreur spécifique, car ils peuvent fournir des indices sur ce qui empêche le service de fonctionner correctement. Exécutez cette commande pour voir les journaux récents :
   ```bash
   sudo journalctl -u elasticsearch
   ```

2. **Vérifiez les erreurs de configuration** dans le fichier `/etc/elasticsearch/elasticsearch.yml` :
   - Assurez-vous que toutes les lignes sont correctement formatées, sans espaces ou caractères inattendus.
   - Si vous avez modifié `network.host`, essayez de le rétablir temporairement sur `localhost` pour vérifier si cela résout le problème.

3. **Vérifiez les ressources système** :
   Parfois, Elasticsearch peut échouer au démarrage en raison d'un manque de mémoire. Assurez-vous que votre serveur dispose de suffisamment de RAM pour démarrer Elasticsearch (1 à 2 Go de RAM minimum pour des tests basiques).

4. **Vérifiez l'état du service Elasticsearch** pour confirmer s'il fonctionne ou s'il a échoué :
   ```bash
   sudo systemctl status elasticsearch
   ```

5. **Redémarrez Elasticsearch** en utilisant les commandes suivantes et vérifiez s'il y a des messages d'erreur spécifiques au redémarrage :
   ```bash
   sudo systemctl restart elasticsearch
   ```

6. **Vérifiez les paramètres de pare-feu** : Assurez-vous que le port `9200` est ouvert pour les connexions locales si vous testez depuis le même serveur.

Après avoir appliqué ces vérifications et corrections, essayez de nouveau avec la commande `curl` :

```bash
curl -XGET http://localhost:9200
```

Si le problème persiste, les journaux fourniront probablement des informations clés pour identifier la source de l'erreur.


--------------------
# ANENXE 01 - partie 04
--------------------


L’erreur `missing authentication credentials for REST request` indique qu’Elasticsearch a une sécurité activée, ce qui nécessite une authentification pour accéder à l’API. Voici comment résoudre ce problème :

1. **Utiliser l’authentification de base** :
   Si vous avez un utilisateur configuré pour accéder à Elasticsearch (comme `elastic`), vous pouvez utiliser l’authentification de base avec `curl` :
   ```bash
   curl -u elastic:your_password -X GET 'https://localhost:9200/'
   ```
   Remplacez `your_password` par le mot de passe de l’utilisateur `elastic` ou un autre utilisateur administrateur.

2. **Désactiver temporairement la sécurité pour des tests** (optionnel) :
   Si vous souhaitez désactiver la sécurité pour simplifier les tests, vous pouvez modifier le fichier de configuration `/etc/elasticsearch/elasticsearch.yml` et ajouter ou commenter les lignes suivantes :
   ```yaml
   xpack.security.enabled: false
   ```
   Ensuite, redémarrez Elasticsearch pour appliquer la modification :
   ```bash
   sudo systemctl restart elasticsearch
   ```

3. **Créer un utilisateur pour accéder à l’API** (si aucun utilisateur n’est configuré) :
   Si vous utilisez une version d’Elasticsearch qui impose la sécurité par défaut, vous devrez probablement créer un utilisateur. Utilisez la commande suivante pour générer un mot de passe pour `elastic` ou un autre utilisateur administrateur :
   ```bash
   sudo /usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
   ```

Après avoir configuré l’authentification, essayez à nouveau avec `curl` en fournissant les identifiants corrects :
```bash
curl -u elastic:your_password -X GET 'https://localhost:9200/'
```




--------------------
# ANENXE 01 - partie 05
--------------------


Pour qu'Elasticsearch ne vous demande plus d'identifiants, il est déconseillé de stocker le mot de passe en clair dans le fichier de configuration pour des raisons de sécurité. Cependant, voici des alternatives et des solutions possibles :

### Option 1 : Désactiver temporairement la sécurité pour des tests
Si c'est pour un environnement de développement ou de test, vous pouvez désactiver la sécurité :

1. **Modifier le fichier de configuration** :
   Ouvrez le fichier `/etc/elasticsearch/elasticsearch.yml` et ajoutez ou modifiez cette ligne :
   ```yaml
   xpack.security.enabled: false
   ```
2. **Redémarrer Elasticsearch** pour appliquer les changements :
   ```bash
   sudo systemctl restart elasticsearch
   ```

Cela supprimera toutes les exigences d'authentification pour les tests.

### Option 2 : Utiliser un fichier `curlrc` pour stocker les informations d'identification
Si vous voulez continuer à utiliser la sécurité mais éviter d'entrer votre mot de passe à chaque requête `curl`, vous pouvez créer un fichier de configuration `.curlrc` dans le répertoire utilisateur :

1. **Créer un fichier `.curlrc`** dans le répertoire de l'utilisateur `eleve` :
   ```bash
   nano /home/eleve/.curlrc
   ```
2. **Ajouter les informations d'identification** :
   Ajoutez les informations d'identification sous la forme suivante :
   ```bash
   user = "elastic:your_password"
   ```
   Remplacez `your_password` par le mot de passe réel.

3. **Vérifier la configuration** :
   Maintenant, chaque fois que vous utiliserez `curl` sans spécifier d’utilisateur, le mot de passe sera automatiquement ajouté.

### Option 3 : Configurer un proxy ou un outil d’authentification externe
Pour des environnements de production, envisagez des solutions plus robustes, comme un proxy inverse (Nginx ou Apache) configuré pour gérer l’authentification, ou des outils de gestion des identités pour un accès sécurisé sans stocker de mot de passe en clair.

En utilisant une des méthodes ci-dessus, vous pourrez éviter l'authentification répétitive lors des requêtes `curl` vers Elasticsearch.




-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

--------------------
# ANENXE 02 - pas d'annexe 02
--------------------


-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

--------------------
# ANENXE 03 - pas d'annexe 03
--------------------


-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

--------------------
# ANENXE 04 - partie 01 - automatiser le tous via un script version initiale à ignorer (correction dans la partie --> Erreur 4 (Pouvons-nous automatiser le tous ?))
--------------------

### Pouvons-nous automatiser le tous via un script ?


Oui, vous pouvez automatiser ce processus via un script shell. Voici un exemple de script qui configure automatiquement `curl` avec les informations d'identification pour Elasticsearch en créant un fichier `.curlrc` dans le répertoire de l'utilisateur. 

Ce script vérifie également si Elasticsearch est en cours d'exécution et configure temporairement la sécurité en fonction de vos besoins.

### Script Shell pour automatiser l'ajout des identifiants `curl`

```bash
#!/bin/bash

# Variables pour le nom d'utilisateur et le mot de passe Elasticsearch
ES_USER="elastic"
ES_PASSWORD="your_password"  # Remplacez par votre mot de passe réel

# Chemin du fichier .curlrc pour stocker les identifiants
CURLRC_PATH="$HOME/.curlrc"

# Vérifie si Elasticsearch est en cours d'exécution
echo "Vérification du statut d'Elasticsearch..."
if systemctl is-active --quiet elasticsearch; then
    echo "Elasticsearch est en cours d'exécution."
else
    echo "Elasticsearch n'est pas démarré. Tentative de démarrage..."
    sudo systemctl start elasticsearch
    sleep 5  # Attendre quelques secondes pour s'assurer qu'Elasticsearch démarre
fi

# Ajoute les identifiants au fichier .curlrc
echo "Configuration de l'authentification pour les requêtes curl..."
echo "user = \"$ES_USER:$ES_PASSWORD\"" > "$CURLRC_PATH"
chmod 600 "$CURLRC_PATH"  # Sécuriser le fichier pour que seul l'utilisateur puisse y accéder

# Vérifie si les identifiants ont été ajoutés avec succès
if grep -q "$ES_USER" "$CURLRC_PATH"; then
    echo "Authentification configurée avec succès dans $CURLRC_PATH"
else
    echo "Échec de la configuration de l'authentification."
fi

# Test de connexion pour vérifier que tout fonctionne
echo "Test de connexion à Elasticsearch..."
curl -XGET "http://localhost:9200"
```

### Explications

1. **Définition des identifiants** : Modifiez `ES_USER` et `ES_PASSWORD` pour correspondre aux informations d'identification Elasticsearch.
2. **Création de `.curlrc`** : Le script ajoute les identifiants dans le fichier `.curlrc` pour automatiser les requêtes `curl` avec authentification.
3. **Test de connexion** : Après la configuration, le script exécute une requête `curl` pour vérifier qu'Elasticsearch répond.

### Exécution du script

Enregistrez ce script sous le nom `configure_elastic.sh`, puis rendez-le exécutable et lancez-le :

```bash
chmod +x configure_elastic.sh
./configure_elastic.sh
```

Ce script configure `curl` pour qu'il utilise automatiquement vos informations d'identification pour Elasticsearch.



-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------


--------------------
# ANENXE 07 - partie 01 - Créer, configurer et exécuter le script `configure_elastic.sh` pour automatiser l’authentification `curl` avec Elasticsearch
--------------------

Voici un tutoriel détaillé pour créer, configurer et exécuter le script `configure_elastic.sh` pour automatiser l’authentification `curl` avec Elasticsearch.

### Étape 1 : Créer le script `configure_elastic.sh`

1. Ouvrez un terminal et utilisez `nano` pour créer le fichier de script.
   ```bash
   nano configure_elastic.sh
   ```
   
2. Dans l'éditeur `nano`, copiez et collez le script suivant :

   ```bash
   #!/bin/bash

   # Variables pour le nom d'utilisateur et le mot de passe Elasticsearch
   ES_USER="elastic"
   ES_PASSWORD="your_password"  # Remplacez par votre mot de passe réel

   # Chemin du fichier .curlrc pour stocker les identifiants
   CURLRC_PATH="$HOME/.curlrc"

   # Vérifie si Elasticsearch est en cours d'exécution
   echo "Vérification du statut d'Elasticsearch..."
   if systemctl is-active --quiet elasticsearch; then
       echo "Elasticsearch est en cours d'exécution."
   else
       echo "Elasticsearch n'est pas démarré. Tentative de démarrage..."
       sudo systemctl start elasticsearch
       sleep 5  # Attendre quelques secondes pour s'assurer qu'Elasticsearch démarre
   fi

   # Ajoute les identifiants au fichier .curlrc
   echo "Configuration de l'authentification pour les requêtes curl..."
   echo "user = \"$ES_USER:$ES_PASSWORD\"" > "$CURLRC_PATH"
   chmod 600 "$CURLRC_PATH"  # Sécuriser le fichier pour que seul l'utilisateur puisse y accéder

   # Vérifie si les identifiants ont été ajoutés avec succès
   if grep -q "$ES_USER" "$CURLRC_PATH"; then
       echo "Authentification configurée avec succès dans $CURLRC_PATH"
   else
       echo "Échec de la configuration de l'authentification."
   fi

   # Test de connexion pour vérifier que tout fonctionne
   echo "Test de connexion à Elasticsearch..."
   curl -k -XGET "https://localhost:9200"
   ```

3. **Enregistrez et quittez** `nano` :
   - Appuyez sur `Ctrl + X`, puis `Y` pour confirmer et `Enter` pour sauvegarder.

### Étape 2 : Rendre le script exécutable

1. Utilisez la commande suivante pour ajouter les permissions d'exécution au script :
   ```bash
   chmod +x configure_elastic.sh
   ```

### Étape 3 : Exécuter le script

1. Lancez le script pour configurer l'authentification `curl` pour Elasticsearch :
   ```bash
   ./configure_elastic.sh
   ```

### Explications du script

- **Définition des identifiants** : 
  Le script définit les variables `ES_USER` et `ES_PASSWORD` pour les informations d’identification Elasticsearch. Remplacez `"your_password"` par le mot de passe réel.

- **Vérification du statut d'Elasticsearch** :
  Le script vérifie si le service Elasticsearch est actif et tente de le démarrer si nécessaire.

- **Création du fichier `.curlrc`** :
  Le script crée un fichier `.curlrc` contenant les informations d'identification pour que `curl` puisse se connecter automatiquement.

- **Sécurisation du fichier `.curlrc`** :
  La commande `chmod 600` garantit que seul l'utilisateur actuel peut lire le fichier `.curlrc`, car il contient des informations sensibles.

- **Test de connexion avec `curl`** :
  Le script effectue une requête `curl` vers Elasticsearch en utilisant l’option `-k` pour ignorer les erreurs de certificat auto-signé.

### Vérification

- **Test de Connexion** : Une fois le script exécuté, le dernier message devrait afficher des informations provenant d'Elasticsearch, confirmant que la connexion fonctionne.

Ce tutoriel vous guide pas à pas pour configurer et exécuter le script `configure_elastic.sh`, afin d'automatiser l'authentification de `curl` avec Elasticsearch en utilisant un certificat auto-signé.



-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------


--------------------
# ANENXE 08- partie 01 - cat /root/.curlrc
--------------------


Le message `No such file or directory` indique que le fichier `.curlrc` n'existe pas dans le répertoire `/root/`. Si vous êtes connecté en tant que `root` et que vous souhaitez que `curl` utilise automatiquement les informations d’identification, créez ce fichier `.curlrc` dans le répertoire `/root/` avec les informations appropriées.

Voici comment faire :

### Étape 1 : Créer le fichier `.curlrc` dans le répertoire `/root/`

1. Ouvrez un éditeur de texte pour créer le fichier `.curlrc` :
   ```bash
   nano /root/.curlrc
   ```

2. Ajoutez les informations d'identification dans le fichier sous la forme suivante :
   ```bash
   user = "elastic:c+vdv5FUzys5hft5*8Fs"
   ```
   Remplacez `c+vdv5FUzys5hft5*8Fs` par le mot de passe réel de l’utilisateur `elastic`.

3. **Enregistrez et quittez** l'éditeur :
   - Appuyez sur `Ctrl + X`, puis `Y` pour confirmer et `Enter` pour sauvegarder.

### Étape 2 : Définir les permissions du fichier `.curlrc`

Assurez-vous que seul `root` a accès au fichier en définissant les permissions :
```bash
chmod 600 /root/.curlrc
```

### Étape 3 : Tester la commande `curl`

Essayez maintenant d'exécuter la commande `curl` sans spécifier les informations d'identification. `curl` devrait automatiquement utiliser celles contenues dans le fichier `.curlrc` :
```bash
curl -k -X GET 'https://localhost:9200/'
```

En ayant le fichier `.curlrc` correctement configuré dans `/root/`, `curl` devrait désormais utiliser les informations d’identification automatiquement pour chaque requête.


















-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

-----------------------------------------------------------
# Annexe finale - History de mes commandes 
-----------------------------------------------------------


```bash
root@elasticSrv1:/home/eleve# history
```

```bash
    1  nano install_elastic_stack.sh
    2  cat install_elastic_stack.sh
    3  #!/bin/bash
    4  # Fonction pour créer un dossier de logs s'il n'existe pas
    5  create_log_directory() {     LOG_DIR="/var/log/elastic_setup";     if [ ! -d "$LOG_DIR" ]; then         sudo mkdir -p "$LOG_DIR";         sudo chmod 755 "$LOG_DIR";     fi; }
    6  # Étape 1 - Mise à jour des paquets
    7  echo "Mise à jour des paquets..."
    8  sudo apt update && sudo apt install -y
    9  # Étape 2 - Installation de Elasticsearch
   10  echo "Installation d'Elasticsearch..."
   11  sudo apt install -y elasticsearch
   12  # Configuration et activation d'Elasticsearch
   13  echo "Configuration d'Elasticsearch..."
   14  sudo systemctl enable elasticsearch
   15  sudo systemctl start elasticsearch
   16  # Étape 3 - Installation de Kibana
   17  echo "Installation de Kibana..."
   18  wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   19  echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
   20  sudo apt update
   21  sudo apt install -y kibana
   22  # Configuration de Kibana
   23  echo "Configuration de Kibana..."
   24  sudo tee /etc/kibana/kibana.yml > /dev/null <<EOL
   25  server.port: 5601
   26  server.host: "0.0.0.0"
   27  elasticsearch.hosts: ["http://localhost:9200"]
   28  EOL
   29  # Activer et démarrer Kibana
   30  echo "Activation et démarrage de Kibana..."
   31  sudo systemctl enable kibana
   32  sudo systemctl start kibana
   33  # Création du dossier de logs
   34  create_log_directory
   35  # Étape 4 - Génération et sauvegarde du jeton d'enrôlement pour Kibana
   36  echo "Génération et sauvegarde du jeton d'enrôlement pour Kibana..."
   37  ENROLLMENT_TOKEN=$(sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana)
   38  echo "$ENROLLMENT_TOKEN" | sudo tee "$LOG_DIR/kibana_enrollment_token.txt" > /dev/null
   39  # Étape 5 - Obtention et sauvegarde du code de vérification de Kibana
   40  echo "Obtention et sauvegarde du code de vérification de Kibana..."
   41  VERIFICATION_CODE=$(sudo /usr/share/kibana/bin/kibana-verification-code)
   42  echo "$VERIFICATION_CODE" | sudo tee "$LOG_DIR/kibana_verification_code.txt" > /dev/null
   43  # Étape 6 - Réinitialisation et sauvegarde du mot de passe Elasticsearch
   44  echo "Réinitialisation et sauvegarde du mot de passe pour l'utilisateur elastic..."
   45  NEW_PASSWORD=$(sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic -b)
   46  echo "$NEW_PASSWORD" | sudo tee "$LOG_DIR/elastic_password.txt" > /dev/null
   47  # Vérification du statut des services
   48  echo "Vérification des statuts de Kibana et Elasticsearch..."
   49  sudo systemctl status kibana
   50  sudo systemctl status elasticsearch
   51  echo "Installation et configuration terminées."
   52  echo "Les informations importantes ont été sauvegardées dans $LOG_DIR"
   53  sudo ./install_elastic_stack.sh
   54  sh install_elastic_stack.sh
   55  sudo apt update
   56  wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   57  echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
   58  sudo apt update
   59  sudo apt install kibana
   60  sudo systemctl status kibana
   61  sudo systemctl enable kibana
   62  sudo systemctl start kibana
   63  sudo systemctl status kibana
   64  sudo nano /etc/kibana/kibana.yml
   65  sudo systemctl stop kibana
   66  sudo systemctl enable kibana
   67  sudo systemctl start kibana
   68  sudo systemctl status elasticsearch
   69  sudo apt update
   70  sudo apt install elasticsearch
   71  sudo systemctl status elasticsearch
   72  sudo systemctl enable elasticsearch
   73  sudo systemctl start elasticsearch
   74  sudo systemctl status kibana
   75  sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
   76  systemctl status elasticsearch
   77  GET _cluster/health
   78  GET _nodes/stats
   79  POST liste_cours/_doc
   80  { "nom_professeur": "Jean Dupon", "sigle_cours": "ABC123" }
   81  sudo nano /etc/elasticsearch/elasticsearch.yml
   82  curl -XGET localhost:9200
   83  apt install curl
   84  curl -XGET localhost:9200
   85  systemctl status elasticsearch
   86  sudo systemctl start elasticsearch
   87  sudo nano /etc/elasticsearch/elasticsearch.yml
   88  sudo systemctl stop elasticsearch
   89  sudo systemctl start elasticsearch
   90  curl -XGET localhost:9200
   91  sudo nano /etc/elasticsearch/elasticsearch.yml
   92  sudo systemctl stop elasticsearch
   93  sudo systemctl start elasticsearch
   94  curl -XGET localhost:9200
   95  sudo systemctl enable elasticsearch
   96  curl -XGET localhost:9200
   97  sudo journalctl -u elasticsearch
   98  sudo nano /etc/elasticsearch/elasticsearch.yml
   99  sudo systemctl stop elasticsearch
  100  sudo systemctl start elasticsearch
  101  curl -XGET http://localhost:9200
  102  root@elasticSrv1:/home/eleve# ^C
  103  root@elasticSrv1:/home/eleve# sudo nano /etc/elasticsearch/elasticsearch.yml
  104  root@elasticSrv1:/home/eleve# sudo systemctl stop elasticsearch
  105  root@elasticSrv1:/home/eleve# sudo systemctl start elasticsearch
  106  root@elasticSrv1:/home/eleve#
  107  root@elasticSrv1:/home/eleve# curl -XGET http://localhost:9200
  108  curl: (52) Empty reply from server
  109  curl -XGET http://localhost:9200
  110  curl -k -X GET 'https://localhost:9200/'
  111  sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
  112  curl -u elastic:c+vdv5FUzys5hft5*8Fs -X GET 'https://localhost:9200/'
  113  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  114  root@elasticSrv1:/home/eleve# sudo nano /etc/elasticsearch/elasticsearch.yml
  115  nano /etc/elasticsearch/elasticsearch.yml
  116  sudo systemctl restart elasticsearch
  117  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  118  curl -k -X GET 'https://localhost:9200/'
  119  curl -X GET 'https://localhost:9200/'
  120  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  121  sudo systemctl status elasticsearch
  122  nano /etc/elasticsearch/elasticsearch.yml
  123  sudo systemctl restart elasticsearch
  124  sudo systemctl status elasticsearch
  125  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  126  grep "xpack.security.enabled:" /etc/elasticsearch/elasticsearch.yml
  127  grep -n "xpack.security.enabled:" /etc/elasticsearch/elasticsearch.yml
  128  nano /etc/elasticsearch/elasticsearch.yml
  129  sudo systemctl restart elasticsearch
  130  sudo systemctl status elasticsearch
  131  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  132  curl -X GET 'https://localhost:9200/'
  133  curl -k -X GET 'https://localhost:9200/'
  134  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  135  nano /etc/elasticsearch/elasticsearch.yml
  136  sudo systemctl restart elasticsearch
  137  sudo systemctl status elasticsearch
  138  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  139  nano /home/eleve/.curlrc
  140  chmod 600 /home/eleve/.curlrc
  141  curl -k -X GET 'https://localhost:9200/'
  142  nano configure_elastic.sh
  143  curl -k -X GET 'https://localhost:9200/'
  144  sudo systemctl restart elasticsearch
  145  curl -k -X GET 'https://localhost:9200/'
  146  history
  147  cat /root/.curlrc
  148  ls
  149  nano /root/.curlrc
  150  chmod 600 /root/.curlrc
  151  curl -k -X GET 'https://localhost:9200/'
  152  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X PUT "https://localhost:9200/news"
  153  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_v2.json"
  154  nano prepare_bulk.py
  155  python3 prepare_bulk.py
  156  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_bulk.json"
  157  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty"
  158  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "headline": "Trump"
    }
  }
}'
  159  history
root@elasticSrv1:/home/eleve#

```

