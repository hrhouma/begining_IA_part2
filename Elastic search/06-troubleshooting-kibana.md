
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


## ➔ 3.3. EXPLICATION DELA RÉSOLUTION

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
# ANENXE 04 - partie 01 - automatiser le tous via un script version initiale à ignorer
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

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------


# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------


# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------




# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------



# erreur 1 


```bash
root@elasticSrv1:/home/eleve# history

```

-----------------------------------------------------------
-----------------------------------------------------------
-----------------------------------------------------------


