# 🚀 Tutoriel pour Importer un Jeu de Données en Bulk dans Elasticsearch

Avant de commencer, assurez-vous d'avoir le fichier **`News_Category_Dataset_v2.json`** dans le même dossier où vous allez exécuter les commandes.

---

# 🔍 Commandes Initiales

1. **🛠️ Vérifier l'état d'Elasticsearch :**

    ```bash
    curl -k -X GET 'https://localhost:9200/'
    ```

    Cette commande vérifie que votre instance Elasticsearch fonctionne. Le paramètre `-k` ignore les erreurs de certificat SSL, utile pour une configuration en local.

2. **📁 Créer un index "news" :**

    ```bash
    curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X PUT "https://localhost:9200/news"
    ```

    Ici, vous créez un nouvel index nommé `news`. Le paramètre `-u elastic:c+vdv5FUzys5hft5*8Fs` indique les informations de connexion pour le compte `elastic`.

3. **⚡ Tenter un Import Direct avec la Commande Bulk :**

    ```bash
    curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_v2.json"
    ```

    Vous tentez ici d'importer directement le fichier JSON dans Elasticsearch. Si vous rencontrez une erreur, cela signifie probablement que le fichier **`News_Category_Dataset_v2.json`** n'est pas dans le format requis par l'API Bulk d'Elasticsearch.

---

# ❗ Erreur de Format avec l'API Bulk d'Elasticsearch

- L'API Bulk nécessite une ligne d'action avant chaque document JSON pour indiquer l'opération à effectuer. 
- Dans notre cas, nous ajoutons la ligne `{"index": {}}` pour chaque document, ce qui indique qu'il s'agit d'une opération d'indexation.

---

# 📝 Étape 1 : Préparer le Fichier `News_Category_Dataset_v2.json` pour l'API Bulk

##### 🐍 Option 1 : Utiliser un Script Python

Voici un script Python simple pour ajouter la ligne d'action nécessaire avant chaque document JSON :

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

1. Sauvegardez ce script sous le nom **`prepare_bulk.py`**.
2. Exécutez-le avec la commande suivante :

    ```bash
    nano prepare_bulk.py
    python3 prepare_bulk.py
    ```

Cela créera un fichier **`News_Category_Dataset_bulk.json`** qui est maintenant compatible avec l'API Bulk d'Elasticsearch.

---

##### 🖥️ Option 2 : Utiliser la Commande `sed` dans le Terminal

Si vous préférez une méthode rapide dans le terminal, utilisez `sed` pour ajouter la ligne d'index avant chaque document JSON :

```bash
sed 's/^/{\"index\": {}}\n&/' News_Category_Dataset_v2.json > News_Category_Dataset_bulk.json
```

Cette commande créera également un fichier **`News_Category_Dataset_bulk.json`** avec les lignes d'action appropriées.

---

# 📥 Étape 2 : Importer le Fichier Transformé dans Elasticsearch

Une fois le fichier **`News_Category_Dataset_bulk.json`** prêt, vous pouvez l'importer dans Elasticsearch avec la commande suivante :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_bulk.json"
```

- **`{"index": {}}`** : Cette ligne d'action informe Elasticsearch qu'il s'agit d'une opération d'indexation pour chaque document.
- **`News_Category_Dataset_bulk.json`** : Ce fichier contient désormais les lignes d'action nécessaires.

---

# ✅ Étape 3 : Vérifier l'Importation des Données

Pour vérifier que les documents ont été importés, exécutez une requête de recherche simple :

```bash
curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty"
```

Si l'importation a réussi, vous devriez voir les documents importés dans l'index `news`.

---

# 🔎 Recherche d'un Document Spécifique

Enfin, pour effectuer une recherche spécifique (par exemple, les documents avec le mot "Trump" dans le champ `headline`), utilisez la commande suivante :

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

Cela vous renvoie tous les documents dans l'index **`news`** contenant le mot **"Trump"** dans le champ `headline`.

---

# 📚 Résumé

Ce tutoriel couvre la préparation d'un fichier JSON pour l'importation en bulk dans Elasticsearch, le formatage du fichier pour l'API Bulk et la vérification de l'importation. Utilisez cette méthode pour tout fichier JSON nécessitant une ligne d'action pour chaque document, afin de garantir la compatibilité avec Elasticsearch.
