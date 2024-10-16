# Histoire : L’Enquête de l’Administrateur Système

#### Contexte

Vous êtes un administrateur système dans une grande entreprise appelée **TechCorp**. Un jour, vous recevez un appel urgent du département de sécurité. Ils vous informent qu'un incident potentiel de sécurité a été détecté, et ils vous demandent de mener une enquête approfondie sur les journaux systèmes pour identifier toute activité suspecte.

**TechCorp** stocke des journaux volumineux de tous les serveurs de l'entreprise. Votre mission est de créer des fichiers volumineux simulant ces journaux, puis d’utiliser `grep` pour rechercher des informations critiques qui pourraient révéler des indices sur l'incident de sécurité.

### Étape 1 : Création de Fichiers Volumineux

Pour simuler les journaux des serveurs, vous allez créer un fichier volumineux contenant des informations fictives.

1. **Créer un fichier de journal volumineux**

   Ouvrez un terminal sur votre machine Ubuntu 22.04. Vous allez créer un fichier de journal appelé `server_log.txt` qui contient 1 million de lignes. Utilisez la commande suivante pour générer ce fichier, avec des anomalies insérées au milieu du fichier :

   ```bash
   for i in {1..500000}; do echo "Info: Server is running. Timestamp: $(date)" >> server_log.txt; done
   echo "Error: Unauthorized access attempt detected. Timestamp: $(date)" >> server_log.txt
   echo "Warning: Possible data breach detected. Timestamp: $(date)" >> server_log.txt
   echo "Alert: Server overload detected. Timestamp: $(date)" >> server_log.txt
   for i in {500001..1000000}; do echo "Info: Server is running. Timestamp: $(date)" >> server_log.txt; done
   ```

   - **Explication :** Cette commande crée un fichier contenant des messages répétitifs simulant des journaux réguliers du serveur, avec des anomalies critiques insérées au milieu. 

### Étape 2 : Utilisation de grep pour Rechercher des Anomalies

Maintenant que vous avez créé un fichier volumineux, l'étape suivante consiste à utiliser `grep` pour rechercher des informations spécifiques dans ce fichier.

1. **Rechercher les messages d'erreur**

   Utilisez `grep` pour rechercher tous les messages contenant le mot "Error" dans le fichier `server_log.txt` :

   ```bash
   grep "Error" server_log.txt
   ```

   - **Explication :** Cette commande permet d'identifier rapidement toutes les erreurs dans un fichier volumineux.

2. **Filtrer les messages d'alerte**

   Recherchez tous les messages contenant le mot "Alert" pour trouver des événements critiques :

   ```bash
   grep "Alert" server_log.txt
   ```

   - **Explication :** Cette étape montre comment isoler les événements les plus graves dans les journaux.

3. **Recherche insensible à la casse**

   Les journaux peuvent utiliser des capitalisations différentes. Utilisez `grep` avec l'option `-i` pour rechercher tous les avertissements, indépendamment de la casse :

   ```bash
   grep -i "warning" server_log.txt
   ```

   - **Explication :** Cette commande est utile pour s'assurer qu'aucun avertissement n'est manqué, même si la casse est différente.

4. **Recherche de plusieurs motifs simultanément**

   Parfois, il est nécessaire de rechercher plusieurs termes à la fois. Utilisez `grep` avec l'option `-E` pour rechercher des messages contenant "Error" ou "Warning" :

   ```bash
   grep -E "Error|Warning" server_log.txt
   ```

   - **Explication :** Cette méthode est efficace pour filtrer plusieurs types d'événements en une seule commande.

5. **Compter les occurrences**

   Pour obtenir un aperçu du nombre total d'occurrences d'un certain type d'événement, utilisez l'option `-c` pour compter les messages "Error" :

   ```bash
   grep -c "Error" server_log.txt
   ```

   - **Explication :** Cette commande montre combien de fois une erreur spécifique s'est produite, ce qui est crucial pour évaluer la gravité de l'incident.

### Étape 3 : Analyse Avancée avec grep

Pour une analyse plus approfondie, vous allez combiner plusieurs options de `grep` pour affiner votre recherche.

1. **Recherche de lignes contenant une adresse IP suspecte**

   Si vous suspectez une adresse IP particulière, utilisez `grep` pour la rechercher dans les journaux :

   ```bash
   grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" server_log.txt
   ```

   - **Explication :** Les expressions régulières permettent de rechercher des motifs complexes comme des adresses IP. Cette commande extrait toutes les lignes contenant une adresse IP dans le fichier.

2. **Inverser la recherche**

   Parfois, vous voulez voir toutes les lignes qui **ne** contiennent **pas** un certain mot. Utilisez `grep` avec l'option `-v` pour exclure les lignes contenant "Info" :

   ```bash
   grep -v "Info" server_log.txt
   ```

   - **Explication :** Cela permet de se concentrer sur les événements importants en excluant les messages non critiques.

### Étape 4 : Automatisation avec des Scripts

Enfin, pour rendre l'enquête plus efficace, vous allez automatiser certaines de ces recherches en écrivant un script shell.

1. **Créer un script d'automatisation**

   Créez un script appelé `analyze_logs.sh` qui exécute toutes les commandes `grep` nécessaires pour identifier les anomalies :

   ```bash
   nano analyze_logs.sh
   ```

   Ajoutez-y les commandes suivantes :

   ```bash
   #!/bin/bash
   echo "Rechercher les erreurs :"
   grep "Error" server_log.txt
   echo "Rechercher les alertes :"
   grep "Alert" server_log.txt
   echo "Rechercher les avertissements :"
   grep -i "warning" server_log.txt
   echo "Rechercher les adresses IP suspectes :"
   grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" server_log.txt
   ```

   - **Explication :** Les scripts shell permettent d'automatiser les tâches répétitives et de gagner du temps.

2. **Exécuter le script**

   Rendez le script exécutable et exécutez-le :

   ```bash
   chmod +x analyze_logs.sh
   ./analyze_logs.sh
   ```

   - **Explication :** Cette étape montre comment automatiser l’analyse des journaux en une seule commande.

---

### Conclusion de l’Histoire

Après avoir exécuté le script, vous avez trouvé plusieurs anomalies dans les journaux du serveur, notamment des tentatives d'accès non autorisées et des alertes de surcharge du serveur. Grâce à votre expertise en utilisation de `grep` et à l'automatisation avec des scripts shell, vous avez pu identifier rapidement les problèmes et les signaler au département de sécurité pour qu'ils prennent des mesures correctives.

---

### Annexes

#### Annexe 01 - Commandes grep Avancées

**Explications détaillées sur les options `grep`** :
- Description de `grep -r`, `grep -v`, `grep -i`, `grep -E`, et comment les utiliser ensemble pour des recherches puissantes.

#### Annexe 02 - Utilisation des Expressions Régulières

**Introduction aux expressions régulières** :
- Concepts de base et avancés des expressions régulières, avec des exemples pratiques d'utilisation dans `grep`.

#### Annexe 03 - Sécurité et Analyse des Journaux

**Meilleures pratiques pour l'analyse des journaux** :
- Techniques et conseils pour utiliser `grep` efficacement dans l'analyse des journaux systèmes, avec un focus sur la sécurité.
