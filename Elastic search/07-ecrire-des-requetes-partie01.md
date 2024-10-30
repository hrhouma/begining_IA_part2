-----------------------------
# Introduction:
-----------------------------

*L'objectif de ce document est de vous fournir un ensemble détaillé de commandes `curl` pour interagir facilement avec Elasticsearch.* 
> Vous y trouverez des explications claires et des exemples concrets utilisant des noms d'index et des données, afin de vous guider dans l'exploration et la manipulation des données au sein d'Elasticsearch.


---

### 1. **Vérifier les informations générales du cluster**
   Cette commande permet de vérifier si Elasticsearch fonctionne et d'obtenir des informations de base sur le cluster (nom, version, UUID, etc.).
   ```bash
   curl -k -XGET "https://localhost:9200/"
   ```
   **Explication :** Cette commande envoie une requête GET à Elasticsearch sans préciser de chemin spécifique, ce qui renvoie des informations générales sur le cluster.

---

### 2. **Vérifier l'état de santé du cluster**
   Utilisez cette commande pour voir l'état de santé global du cluster (`green`, `yellow`, ou `red`).
   ```bash
   curl -k -XGET "https://localhost:9200/_cluster/health"
   ```
   **Explication :** L'état `green` signifie que le cluster est entièrement opérationnel, `yellow` signifie que certaines répliques manquent, et `red` indique que des données ou des shards ne sont pas disponibles.

---

### 3. **Lister tous les indices dans le cluster**
   Cette commande affiche tous les indices disponibles avec des informations comme l'état, le nombre de documents, et l'espace disque utilisé.
   ```bash
   curl -k -XGET "https://localhost:9200/_cat/indices?v"
   ```
   **Exemple de résultat :**
   ```
   health status index      uuid                   pri rep docs.count store.size pri.store.size
   green  open   products   xaZV1...aV            1   1       10000       1.2gb           600mb
   green  open   customers  AaQ20...2v            1   1       5000        300mb           150mb
   ```

---

### 4. **Créer un index nommé `products`**
   Cette commande crée un index vide nommé `products` pour y stocker des documents de produits.
   ```bash
   curl -k -XPUT "https://localhost:9200/products"
   ```
   **Explication :** Les indices sont similaires aux tables en bases de données relationnelles. Cet index `products` pourra contenir des documents sur différents produits.

---

### 5. **Vérifier les informations de mappage d’un index (structure des données)**
   Cette commande affiche le mappage d’un index, montrant comment les champs et les types de données sont structurés.
   ```bash
   curl -k -XGET "https://localhost:9200/products/_mapping"
   ```
   **Explication :** Le mappage détermine la structure des documents stockés dans un index, comme les types de données (texte, nombre, etc.) et les paramètres de champ.

---

### 6. **Ajouter un document dans l’index `products`**
   Ajoutons un produit de test dans l'index `products` avec un ID unique.
   ```bash
   curl -k -XPOST "https://localhost:9200/products/_doc/1" -H 'Content-Type: application/json' -d'
   {
     "name": "Laptop",
     "category": "Electronics",
     "price": 1200,
     "stock": 50
   }'
   ```
   **Explication :** Ce document contient les informations d'un produit (`Laptop`) avec des champs tels que `name`, `category`, `price`, et `stock`.

---

### 7. **Rechercher des documents dans l’index `products`**
   Effectuons une recherche pour tous les produits de la catégorie `Electronics`.
   ```bash
   curl -k -XGET "https://localhost:9200/products/_search?q=category:Electronics"
   ```
   **Explication :** Cette commande utilise une recherche basée sur une requête (`q`) pour trouver des documents où le champ `category` est égal à `Electronics`.

---

### 8. **Lister les documents de l’index `products` avec un filtre par prix**
   Cette commande recherche les produits ayant un prix inférieur à 1500.
   ```bash
   curl -k -XGET "https://localhost:9200/products/_search" -H 'Content-Type: application/json' -d'
   {
     "query": {
       "range": {
         "price": {
           "lt": 1500
         }
       }
     }
   }'
   ```
   **Explication :** Utilisation d’une requête de `range` pour filtrer les documents dont le champ `price` est inférieur (`lt`) à 1500.

---

### 9. **Ajouter un document dans un nouvel index `customers`**
   Ajoutons un document client avec des informations de base dans un nouvel index nommé `customers`.
   ```bash
   curl -k -XPOST "https://localhost:9200/customers/_doc/1" -H 'Content-Type: application/json' -d'
   {
     "name": "Alice",
     "age": 30,
     "membership": "gold",
     "email": "alice@example.com"
   }'
   ```
   **Explication :** Le document contient des informations de base sur un client nommé `Alice` avec des champs comme `name`, `age`, `membership`, et `email`.

---

### 10. **Mettre à jour un document existant dans `customers`**
   Mettons à jour l'âge d'un client existant.
   ```bash
   curl -k -XPOST "https://localhost:9200/customers/_update/1" -H 'Content-Type: application/json' -d'
   {
     "doc": {
       "age": 31
     }
   }'
   ```
   **Explication :** La commande met à jour uniquement le champ `age` d'un document existant, en laissant les autres champs intacts.

---

### 11. **Supprimer un document spécifique d’un index**
   Supposons que nous voulons supprimer le document avec l'ID `1` dans l'index `products`.
   ```bash
   curl -k -XDELETE "https://localhost:9200/products/_doc/1"
   ```
   **Explication :** Cette commande supprime le document identifié par `1` dans l'index `products`.

---

### 12. **Supprimer un index complet**
   Pour supprimer complètement l'index `products` (attention, cela supprimera tous les documents dans cet index).
   ```bash
   curl -k -XDELETE "https://localhost:9200/products"
   ```
   **Explication :** Supprimer un index signifie supprimer tous les documents et le mappage de cet index dans Elasticsearch.

---

### 13. **Obtenir les statistiques d'un index**
   Cette commande fournit des statistiques comme le nombre de documents, la taille de l'index, et d'autres informations de stockage pour l’index `customers`.
   ```bash
   curl -k -XGET "https://localhost:9200/customers/_stats"
   ```
   **Explication :** Les statistiques d’index donnent un aperçu de la quantité de données et de l’utilisation de l’espace disque pour un index particulier.

---

### 14. **Lister les informations des shards pour un index**
   Cette commande affiche des informations détaillées sur les shards utilisés par l'index `customers`.
   ```bash
   curl -k -XGET "https://localhost:9200/_cat/shards/customers?v"
   ```
   **Explication :** Chaque index est divisé en shards, qui sont répartis entre les nœuds du cluster. Cette commande permet de voir la distribution des shards pour un index spécifique.

---

### 15. **Vérifier l'état de récupération de tous les indices**
   Utilisez cette commande pour obtenir des informations sur les processus de récupération en cours pour tous les indices.
   ```bash
   curl -k -XGET "https://localhost:9200/_recovery?pretty"
   ```
   **Explication :** Lors de la restauration de snapshots ou après une panne, il est possible de suivre la progression de la récupération des shards.

---

### 16. **Effectuer une sauvegarde de snapshot d’un index dans un dépôt de snapshots**
   (Nécessite une configuration préalable d'un dépôt de snapshot).
   ```bash
   curl -k -XPUT "https://localhost:9200/_snapshot/my_backup/products_snapshot" -H 'Content-Type: application/json' -d'
   {
     "indices": "products",
     "ignore_unavailable": true,
     "include_global_state": false
   }'
   ```
   **Explication :** Sauvegarde l’index `products` dans un snapshot nommé `products_snapshot` dans un dépôt de snapshots appelé `my_backup`.

------------
# Annexe 01: Récriture des requêtes avec le user:password !
------------

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
```


# Introduction:

*L'objectif de ce document est de vous fournir un ensemble détaillé de commandes `curl` pour interagir facilement avec Elasticsearch.* 
> Vous y trouverez des explications claires et des exemples concrets utilisant des noms d'index et des données, afin de vous guider dans l'exploration et la manipulation des données au sein d'Elasticsearch.


---

### 1. **Vérifier les informations générales du cluster**
   Cette commande permet de vérifier si Elasticsearch fonctionne et d'obtenir des informations de base sur le cluster (nom, version, UUID, etc.).
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https://localhost:9200/"
   ```
   **Explication :** Cette commande envoie une requête GET à Elasticsearch sans préciser de chemin spécifique, ce qui renvoie des informations générales sur le cluster.

---

### 2. **Vérifier l'état de santé du cluster**
   Utilisez cette commande pour voir l'état de santé global du cluster (`green`, `yellow`, ou `red`).
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https://localhost:9200/_cluster/health"
   ```
   **Explication :** L'état `green` signifie que le cluster est entièrement opérationnel, `yellow` signifie que certaines répliques manquent, et `red` indique que des données ou des shards ne sont pas disponibles.

---

### 3. **Lister tous les indices dans le cluster**
   Cette commande affiche tous les indices disponibles avec des informations comme l'état, le nombre de documents, et l'espace disque utilisé.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https://localhost:9200/_cat/indices?v"
   ```
   **Exemple de résultat :**
   ```
   health status index      uuid                   pri rep docs.count store.size pri.store.size
   green  open   products   xaZV1...aV            1   1       10000       1.2gb           600mb
   green  open   customers  AaQ20...2v            1   1       5000        300mb           150mb
   ```

---

### 4. **Créer un index nommé `products`**
   Cette commande crée un index vide nommé `products` pour y stocker des documents de produits.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XPUT "https://localhost:9200/products"
   ```
   **Explication :** Les indices sont similaires aux tables en bases de données relationnelles. Cet index `products` pourra contenir des documents sur différents produits.

---

### 5. **Vérifier les informations de mappage d’un index (structure des données)**
   Cette commande affiche le mappage d’un index, montrant comment les champs et les types de données sont structurés.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https://localhost:9200/products/_mapping"
   ```
   **Explication :** Le mappage détermine la structure des documents stockés dans un index, comme les types de données (texte, nombre, etc.) et les paramètres de champ.

---

### 6. **Ajouter un document dans l’index `products`**
   Ajoutons un produit de test dans l'index `products` avec un ID unique.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XPOST "https://localhost:9200/products/_doc/1" -H 'Content-Type: application/json' -d'
   {
     "name": "Laptop",
     "category": "Electronics",
     "price": 1200,
     "stock": 50
   }'
   ```
   **Explication :** Ce document contient les informations d'un produit (`Laptop`) avec des champs tels que `name`, `category`, `price`, et `stock`.

---

### 7. **Rechercher des documents dans l’index `products`**
   Effectuons une recherche pour tous les produits de la catégorie `Electronics`.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https://localhost:9200/products/_search?q=category:Electronics"
   ```
   **Explication :** Cette commande utilise une recherche basée sur une requête (`q`) pour trouver des documents où le champ `category` est égal à `Electronics`.

---

### 8. **Lister les documents de l’index `products` avec un filtre par prix**
   Cette commande recherche les produits ayant un prix inférieur à 1500.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https://localhost:9200/products/_search" -H 'Content-Type: application/json' -d'
   {
     "query": {
       "range": {
         "price": {
           "lt": 1500
         }
       }
     }
   }'
   ```
   **Explication :** Utilisation d’une requête de `range` pour filtrer les documents dont le champ `price` est inférieur (`lt`) à 1500.

---

### 9. **Ajouter un document dans un nouvel index `customers`**
   Ajoutons un document client avec des informations de base dans un nouvel index nommé `customers`.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XPOST "https://localhost:9200/customers/_doc/1" -H 'Content-Type: application/json' -d'
   {
     "name": "Alice",
     "age": 30,
     "membership": "gold",
     "email": "alice@example.com"
   }'
   ```
   **Explication :** Le document contient des informations de base sur un client nommé `Alice` avec des champs comme `name`, `age`, `membership`, et `email`.

---

### 10. **Mettre à jour un document existant dans `customers`**
   Mettons à jour l'âge d'un client existant.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XPOST "https://localhost:9200/customers/_update/1" -H 'Content-Type: application/json' -d'
   {
     "doc": {
       "age": 31
     }
   }'
   ```
   **Explication :** La commande met à jour uniquement le champ `age` d'un document existant, en laissant les autres champs intacts.

---

### 11. **Supprimer un document spécifique d’un index**
   Supposons que nous voulons supprimer le document avec l'ID `1` dans l'index `products`.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XDELETE "https://localhost:9200/products/_doc/1"
   ```
   **Explication :** Cette commande supprime le document identifié par `1` dans l'index `products`.

---

### 12. **Supprimer un index complet**
   Pour supprimer complètement l'index `products` (attention, cela supprimera tous les documents dans cet index).
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XDELETE "https://localhost:9200/products"
   ```
   **Explication :** Supprimer un index signifie supprimer tous les documents et le mappage de cet index dans Elasticsearch.

---

### 13. **Obtenir les statistiques d'un index**
   Cette commande fournit des statistiques comme le nombre de documents, la taille de l'index, et d'autres informations de stockage pour l’index `customers`.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https://localhost:9200/customers/_stats"
   ```
   **Explication :** Les statistiques d’index donnent un aperçu de la quantité de données et de l’utilisation de l’espace disque pour un index particulier.

---

### 14. **Lister les informations des shards pour un index**
   Cette commande affiche des informations détaillées sur les shards utilisés par l'index `customers`.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https://localhost:9200/_cat/shards/customers?v"
   ```
   **Explication :** Chaque index est divisé en shards, qui sont répartis entre les nœuds du cluster. Cette commande permet de voir la distribution des shards pour un index spécifique.

---

### 15. **Vérifier l'état de récupération de tous les indices**
   Utilisez cette commande pour obtenir des informations sur les processus de récupération en cours pour tous les indices.
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XGET "https

://localhost:9200/_recovery?pretty"
   ```
   **Explication :** Lors de la restauration de snapshots ou après une panne, il est possible de suivre la progression de la récupération des shards.

---

### 16. **Effectuer une sauvegarde de snapshot d’un index dans un dépôt de snapshots**
   (Nécessite une configuration préalable d'un dépôt de snapshot).
   ```bash
   curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -XPUT "https://localhost:9200/_snapshot/my_backup/products_snapshot" -H 'Content-Type: application/json' -d'
   {
     "indices": "products",
     "ignore_unavailable": true,
     "include_global_state": false
   }'
   ```
   **Explication :** Sauvegarde l’index `products` dans un snapshot nommé `products_snapshot` dans un dépôt de snapshots appelé `my_backup`.


