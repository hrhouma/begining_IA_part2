
# 1. Erreur 1 

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

# Erreur 2


```bash
curl -XGET localhost:9200
curl -u elastic:c+vdv5FUzys5hft5*8Fs -X GET 'https://localhost:9200/'

```


## ➔ 2.1. ERREUR

```bash
curl: (52) Empty reply from server
```
## ➔ 2.2. RÉSOLUTION

```bash

curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
```


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


