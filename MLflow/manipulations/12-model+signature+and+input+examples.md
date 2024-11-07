
# 0 - Nouveaux ajouts par rapport au script précédent 💀🚨

Dans cette version, **nous avons introduit les concepts de signature de modèle et d’exemple d’entrée pour MLflow**. Ces fonctionnalités permettent de spécifier la structure des entrées et sorties d’un modèle, ainsi que de fournir un exemple d’entrée pour aider les utilisateurs à comprendre les types de données attendus par le modèle.

💀🚨 **Importation des modules `ModelSignature` et `Schema`** : Nécessaire pour définir la signature du modèle, indiquant les types de données en entrée et en sortie.

💀🚨 **Création d'un `input_example`** : Un exemple d’entrée réel est fourni, montrant les valeurs typiques pour chaque caractéristique d'entrée, ce qui est utile pour la documentation et le déploiement.

💀🚨 **Ajout de la signature et de l’exemple d’entrée à `mlflow.sklearn.log_model()`** : La signature et l'exemple sont spécifiés lors de l'enregistrement du modèle pour que MLflow puisse générer une documentation plus claire.

---

# 1 - Script avec les nouveaux ajouts 🎛️

Ce script inclut les ajouts en **highlight** dans les commentaires.

```python
# Importation des bibliothèques nécessaires
import warnings
import argparse
import logging
import pandas as pd
import numpy as np
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
from sklearn.model_selection import train_test_split
from sklearn.linear_model import ElasticNet
import mlflow
import mlflow.sklearn
from pathlib import Path
import os
from mlflow.models.signature import ModelSignature, infer_signature  # 💀🚨 Pour définir la signature du modèle
from mlflow.types.schema import Schema, ColSpec  # 💀🚨 Pour spécifier les types de colonnes dans la signature

logging.basicConfig(level=logging.WARN)
logger = logging.getLogger(__name__)

# get arguments from command
parser = argparse.ArgumentParser()
parser.add_argument("--alpha", type=float, required=False, default=0.7)
parser.add_argument("--l1_ratio", type=float, required=False, default=0.7)
args = parser.parse_args()

# evaluation function
def eval_metrics(actual, pred):
    rmse = np.sqrt(mean_squared_error(actual, pred))
    mae = mean_absolute_error(actual, pred)
    r2 = r2_score(actual, pred)
    return rmse, mae, r2


if __name__ == "__main__":
    warnings.filterwarnings("ignore")
    np.random.seed(40)

    # Read the wine-quality csv file from the URL
    data = pd.read_csv("red-wine-quality.csv")
    data.to_csv("data/red-wine-quality.csv", index=False)

    # Split the data into training and test sets. (0.75, 0.25) split.
    train, test = train_test_split(data)
    train.to_csv("data/train.csv")
    test.to_csv("data/test.csv")

    train_x = train.drop(["quality"], axis=1)
    test_x = test.drop(["quality"], axis=1)
    train_y = train[["quality"]]
    test_y = test[["quality"]]

    alpha = args.alpha
    l1_ratio = args.l1_ratio

    mlflow.set_tracking_uri(uri="")
    print("The set tracking uri is ", mlflow.get_tracking_uri())

    exp = mlflow.set_experiment(experiment_name="experiment_signature")
    print("Name: {}".format(exp.name))
    print("Experiment_id: {}".format(exp.experiment_id))
    print("Artifact Location: {}".format(exp.artifact_location))
    print("Tags: {}".format(exp.tags))
    print("Lifecycle_stage: {}".format(exp.lifecycle_stage))
    print("Creation timestamp: {}".format(exp.creation_time))

    mlflow.start_run()
    tags = {
        "engineering": "ML platform",
        "release.candidate": "RC1",
        "release.version": "2.0"
    }
    mlflow.set_tags(tags)
    
    mlflow.sklearn.autolog(
        log_input_examples=False,
        log_model_signatures=False,
        log_models=False
    )

    lr = ElasticNet(alpha=alpha, l1_ratio=l1_ratio, random_state=42)
    lr.fit(train_x, train_y)
    predicted_qualities = lr.predict(test_x)
    (rmse, mae, r2) = eval_metrics(test_y, predicted_qualities)

    print("Elasticnet model (alpha={:f}, l1_ratio={:f}):".format(alpha, l1_ratio))
    print("  RMSE: %s" % rmse)
    print("  MAE: %s" % mae)
    print("  R2: %s" % r2)

    # 💀🚨 Création d'un schéma de signature d'entrée
    input_data = [
        {"name": "fixed acidity", "type": "double"},
        {"name": "volatile acidity", "type": "double"},
        {"name": "citric acid", "type": "double"},
        {"name": "residual sugar", "type": "double"},
        {"name": "chlorides", "type": "double"},
        {"name": "free sulfur dioxide", "type": "double"},
        {"name": "total sulfur dioxide", "type": "double"},
        {"name": "density", "type": "double"},
        {"name": "pH", "type": "double"},
        {"name": "sulphates", "type": "double"},
        {"name": "alcohol", "type": "double"},
        {"name": "quality", "type": "double"}
    ]
    output_data = [{'type': 'long'}]

    input_schema = Schema([ColSpec(col["type"], col['name']) for col in input_data])
    output_schema = Schema([ColSpec(col['type']) for col in output_data])
    signature = ModelSignature(inputs=input_schema, outputs=output_schema)  # 💀🚨 Création de la signature du modèle

    # 💀🚨 Exemple d'entrée pour le modèle
    input_example = {
        "fixed acidity": np.array([7.2, 7.5, 7.0, 6.8, 6.9]),
        "volatile acidity": np.array([0.35, 0.3, 0.28, 0.38, 0.25]),
        "citric acid": np.array([0.45, 0.5, 0.55, 0.4, 0.42]),
        "residual sugar": np.array([8.5, 9.0, 8.2, 7.8, 8.1]),
        "chlorides": np.array([0.045, 0.04, 0.035, 0.05, 0.042]),
        "free sulfur dioxide": np.array([30, 35, 40, 28, 32]),
        "total sulfur dioxide": np.array([120, 125, 130, 115, 110]),
        "density": np.array([0.997, 0.996, 0.995, 0.998, 0.994]),
        "pH": np.array([3.2, 3.1, 3.0, 3.3, 3.2]),
        "sulphates": np.array([0.65, 0.7, 0.68, 0.72, 0.62]),
        "alcohol": np.array([9.2, 9.5, 9.0, 9.8, 9.4]),
        "quality": np.array([6, 7, 6, 8, 7])
    }

    mlflow.log_artifact("red-wine-quality.csv")
    # 💀🚨 Enregistrement du modèle avec la signature et l'exemple d'entrée
    mlflow.sklearn.log_model(lr, "model", signature=signature, input_example=input_example)
    
    artifacts_uri = mlflow.get_artifact_uri()
    print("The artifact path is", artifacts_uri)
    mlflow.end_run()

    run = mlflow.last_active_run()
    print("Active run id is {}".format(run.info.run_id))
    print("Active run name is {}".format(run.info.run_name))
```

---

# 2 - Exécution du Script 🏃‍♂️

Pour exécuter le script, utilisez simplement la commande suivante dans le terminal :

```bash
python <nom_du_script>.py
```

Ce script :
- 🚀 Entraîne un modèle ElasticNet avec les paramètres `alpha` et `l1_ratio`.
- 🚀 Enregistre les métriques de performance.
- 🚀 Enregistre un modèle avec une signature et un exemple d’entrée pour une documentation claire et prête à être déployée.

---

# 3 - Annexe des Explications 📖

### Explications des Ajouts :

1. **Signature du Modèle avec `ModelSignature`** : 
   - **`Schema`** et **`ColSpec`** définissent les types de chaque colonne en entrée et en sortie.
   - **Utilité** : La signature spécifie clairement les attentes en termes de format d'entrée et de sortie, ce qui est essentiel pour le déploiement.

2. **Exemple d'Entrée (`input_example`)** :

 
   - Fournit des valeurs réelles pour chaque caractéristique d’entrée du modèle.
   - **Utilité** : Facilite la compréhension des entrées attendues par le modèle et aide lors du déploiement.

3. **Enregistrement avec Signature et Exemple** :
   - **Commandes** : `mlflow.sklearn.log_model()` prend en argument `signature` et `input_example`.
   - **Bénéfices** : Améliore la traçabilité et la documentation des modèles MLflow, surtout dans un environnement collaboratif ou en production.


# 4 - Explication détaillée


Notre script Python effectue une série de tâches pour entraîner et suivre un modèle de régression ElasticNet sur un ensemble de données de qualité de vin rouge. Voici une explication détaillée de chaque section :

1. **Import des bibliothèques** :  
   Le script utilise diverses bibliothèques pour la gestion des données (`pandas`, `numpy`), l'évaluation du modèle (`sklearn.metrics`), et l'enregistrement des informations de suivi avec `mlflow`.

2. **Configuration des journaux et des arguments** :
   ```python
   logging.basicConfig(level=logging.WARN)
   logger = logging.getLogger(__name__)
   parser = argparse.ArgumentParser()
   parser.add_argument("--alpha", type=float, required=False, default=0.7)
   parser.add_argument("--l1_ratio", type=float, required=False, default=0.7)
   args = parser.parse_args()
   ```
   - Définit le niveau de journalisation et initialise un analyseur d'arguments pour capturer les hyperparamètres `alpha` et `l1_ratio` depuis la ligne de commande.

3. **Fonction d’évaluation des métriques** :
   ```python
   def eval_metrics(actual, pred):
       rmse = np.sqrt(mean_squared_error(actual, pred))
       mae = mean_absolute_error(actual, pred)
       r2 = r2_score(actual, pred)
       return rmse, mae, r2
   ```
   - Cette fonction calcule les principales métriques d'évaluation : l'erreur quadratique moyenne (RMSE), l'erreur absolue moyenne (MAE), et le coefficient de détermination \( R^2 \).

4. **Chargement des données et division en ensembles de formation et de test** :
   ```python
   data = pd.read_csv("red-wine-quality.csv")
   train, test = train_test_split(data)
   train_x = train.drop(["quality"], axis=1)
   test_x = test.drop(["quality"], axis=1)
   train_y = train[["quality"]]
   test_y = test[["quality"]]
   ```
   - Charge le fichier CSV de qualité du vin rouge, sauvegarde les ensembles de formation et de test, et sépare les caractéristiques (`X`) de la cible (`y`), ici la colonne `quality`.

5. **Configuration de l'expérimentation MLflow** :
   ```python
   mlflow.set_tracking_uri(uri="")
   exp = mlflow.set_experiment(experiment_name="experiment_signature")
   ```
   - Configure MLflow pour suivre l'expérimentation sous le nom `experiment_signature`.

6. **Démarrage de l'exécution MLflow et enregistrement des hyperparamètres** :
   ```python
   mlflow.start_run()
   tags = { "engineering": "ML platform", "release.candidate":"RC1", "release.version": "2.0" }
   mlflow.set_tags(tags)
   ```
   - Démarre une exécution dans MLflow, ajoute des tags pour décrire l'exécution (comme version de publication, plateforme d'ingénierie), et enregistre les hyperparamètres du modèle ElasticNet.

7. **Entraînement du modèle ElasticNet** :
   ```python
   lr = ElasticNet(alpha=alpha, l1_ratio=l1_ratio, random_state=42)
   lr.fit(train_x, train_y)
   predicted_qualities = lr.predict(test_x)
   ```
   - Initialise le modèle ElasticNet avec les paramètres spécifiés, entraîne le modèle sur les données de formation et effectue des prédictions sur l’ensemble de test.

8. **Calcul et affichage des métriques** :
   ```python
   (rmse, mae, r2) = eval_metrics(test_y, predicted_qualities)
   print("Elasticnet model (alpha={:f}, l1_ratio={:f}):".format(alpha, l1_ratio))
   print("  RMSE: %s" % rmse)
   print("  MAE: %s" % mae)
   print("  R2: %s" % r2)
   ```
   - Évalue les performances du modèle en utilisant la fonction `eval_metrics` et affiche les métriques calculées.

9. **Signature et exemple d’entrée pour le modèle** :
   ```python
   input_schema = Schema([ColSpec(col["type"], col['name']) for col in input_data])
   output_schema = Schema([ColSpec(col['type']) for col in output_data])
   signature = ModelSignature(inputs=input_schema, outputs=output_schema)
   ```
   - Crée une signature de modèle pour indiquer les types d’entrée et de sortie, ce qui permet une meilleure gestion des entrées/sorties lors du déploiement.

10. **Enregistrement du modèle et des artefacts** :
    ```python
    mlflow.log_artifact("red-wine-quality.csv")
    mlflow.sklearn.log_model(lr, "model", signature=signature, input_example=input_example)
    ```
    - Enregistre le fichier source des données (`red-wine-quality.csv`) et le modèle entraîné dans MLflow avec un exemple d’entrée (`input_example`), facilitant l’accès à ces artefacts pour une utilisation ultérieure.

11. **Fin de l'exécution et informations sur l'exécution active** :
    ```python
    mlflow.end_run()
    run = mlflow.last_active_run()
    print("Active run id is {}".format(run.info.run_id))
    ```
    - Termine l'exécution et affiche des informations sur l'exécution active, telles que son `run_id`, utile pour le suivi des versions du modèle.

En résumé, ce code montre comment charger et traiter les données, entraîner un modèle de régression ElasticNet, évaluer ses performances, et utiliser MLflow pour suivre et enregistrer le modèle, les métriques et les artefacts associés pour une gestion complète du cycle de vie du machine learning.


# Point 9 (AJOUT)


Abordons le point 9 de manière concrète et détaillée. 
Imaginons que tu sois en train de construire une machine qui évalue la qualité du vin. Pour cela, tu dois lui montrer des exemples de ce qu’elle va mesurer : des caractéristiques spécifiques du vin, comme l’acidité, le sucre, etc.

Dans notre code, la **signature de modèle** est comme une "carte d'identité" pour ce modèle de vin. Elle indique précisément quelles caractéristiques (ou informations) le modèle attend en entrée et comment il rend son verdict en sortie.

1. **La liste des informations d'entrée (input schema)** :
   - Imagine que notre machine à évaluer reçoit une liste de caractéristiques du vin, comme si tu décrivais le vin à un expert en disant : "Ce vin a telle acidité, telle quantité de sucre, etc.".
   - Ici, chaque caractéristique est définie par son nom (comme "acidity" pour l'acidité) et son type (comme "double", un nombre décimal). Cette liste d'entrée aide la machine à savoir exactement ce qu’elle doit recevoir pour fonctionner correctement.

2. **La sortie du modèle (output schema)** :
   - Après avoir reçu les caractéristiques, la machine évalue la qualité du vin et donne une note.
   - Dans ce cas, la sortie est simplement un nombre (ici défini par "long") qui correspond à la qualité du vin, d’un score bas à un score élevé.

3. **Pourquoi c'est utile** :
   - Lorsque tu vas montrer ton modèle à d’autres personnes ou l’utiliser pour de nouvelles prédictions, la "carte d'identité" (signature) garantit que tout le monde saura quelles informations donner à la machine et ce qu’ils peuvent attendre en retour. Cela évite les erreurs, comme donner une mauvaise information (par exemple, "couleur" au lieu d’"acidité"), qui rendrait la machine confuse.

4. **Comment c’est fait dans le code** :
   - La signature est créée dans le code avec la structure d’entrée et de sortie que l’on a définie (par exemple, l’acidité est un "double").
   - Cette signature est ensuite associée au modèle pour qu’il puisse facilement comprendre les informations d'entrée et bien fournir les résultats.

En résumé, la signature du modèle garantit que la machine sait exactement quelles informations elle doit recevoir et ce qu'elle va donner en sortie, ce qui est essentiel pour que les futurs utilisateurs de la machine comprennent bien comment l'utiliser sans se tromper.
