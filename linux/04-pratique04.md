# Histoire : L’Enquête de l’Administrateur Système

#### Contexte

Vous êtes un administrateur système chez **TechCorp**, une grande entreprise technologique. Un jour, vous recevez un appel urgent du département de sécurité. Ils suspectent une intrusion sur un de leurs serveurs critiques et vous demandent de mener une enquête en profondeur. Votre mission est de parcourir les journaux du système pour identifier toute activité suspecte et prendre des mesures correctives.

---

### Étape 1 : Préparation de l’Enquête

Pour commencer, vous devez vous assurer que vous avez accès aux journaux système. Vous décidez de créer un répertoire dédié pour cette enquête.

1. **Création d’un répertoire pour les journaux**

   Utilisez la commande suivante pour créer un répertoire nommé `investigation_logs` où vous stockerez tous les fichiers de journaux relatifs à l'incident :

   ```bash
   mkdir investigation_logs
   cd investigation_logs
   ```

   - **Explication :** Cette commande crée un espace de travail propre pour votre enquête.

2. **Copie des fichiers de journaux dans le répertoire**

   Vous savez que les journaux critiques sont stockés dans `/var/log`. Vous copiez les fichiers `auth.log` et `syslog` pour les analyser :

   ```bash
   cp /var/log/auth.log /var/log/syslog .
   ```

   - **Explication :** `cp` permet de copier les fichiers de journaux dans votre répertoire d’enquête.

### Étape 2 : Identification des Activités Suspectes

Maintenant que les fichiers sont prêts, vous devez les analyser pour identifier les signes d’une intrusion.

1. **Recherche de tentatives de connexion échouées**

   Pour détecter les tentatives d'accès non autorisées, vous utilisez `grep` pour rechercher des occurrences de "Failed password" dans le fichier `auth.log` :

   ```bash
   grep "Failed password" auth.log
   ```

   - **Explication :** Cette commande affiche toutes les tentatives de connexion échouées, ce qui peut indiquer une tentative de piratage.

2. **Filtrer les événements critiques**

   Vous savez que des événements importants sont marqués comme "CRITICAL" dans le fichier `syslog`. Utilisez `grep` pour les isoler :

   ```bash
   grep "CRITICAL" syslog
   ```

   - **Explication :** `grep` vous aide à trouver rapidement les événements critiques à ne pas négliger.

3. **Recherche d’IP suspectes**

   Vous soupçonnez qu'une adresse IP particulière a été utilisée pour l'intrusion. Utilisez `grep` pour rechercher cette adresse IP dans `auth.log` :

   ```bash
   grep "192.168.1.100" auth.log
   ```

   - **Explication :** La recherche par adresse IP vous permet de traquer les activités spécifiques liées à cette IP.

4. **Comptabilisation des tentatives d’accès**

   Pour avoir une idée de l’ampleur des tentatives d’accès, vous comptez le nombre de lignes correspondant à "Failed password" :

   ```bash
   grep -c "Failed password" auth.log
   ```

   - **Explication :** Cette commande vous donne un nombre exact de tentatives échouées.

5. **Exclure les activités non pertinentes**

   Pour vous concentrer sur les événements importants, vous décidez d'exclure les lignes contenant "Accepted password" :

   ```bash
   grep -v "Accepted password" auth.log
   ```

   - **Explication :** `grep -v` exclut les lignes non pertinentes, facilitant ainsi l'analyse.

### Étape 3 : Investigation Avancée

Pour affiner davantage votre recherche, vous combinez plusieurs commandes et options.

1. **Recherche de plusieurs termes à la fois**

   Vous recherchez à la fois les mots "Failed" et "CRITICAL" dans un seul fichier :

   ```bash
   grep -E "Failed|CRITICAL" syslog
   ```

   - **Explication :** `grep -E` vous permet de rechercher plusieurs termes en même temps.

2. **Recherche insensible à la casse**

   Vous soupçonnez que certains événements critiques sont écrits en majuscules. Pour les trouver, utilisez l'option `-i` :

   ```bash
   grep -i "critical" syslog
   ```

   - **Explication :** L'option `-i` permet de rechercher un terme sans tenir compte de la casse, garantissant que vous n'ignorez aucun événement critique.

3. **Filtrer les erreurs d'une date spécifique**

   Vous voulez voir les erreurs qui se sont produites le 15 août. Utilisez `grep` pour rechercher cette date spécifique dans les journaux :

   ```bash
   grep "Aug 15" syslog
   ```

   - **Explication :** Cette commande filtre les événements par date, vous aidant à concentrer votre enquête sur une période spécifique.

### Étape 4 : Automatisation de l’Analyse

Pour rendre l’enquête plus efficace, vous automatisez le processus en créant un script.

1. **Écriture du script d'analyse**

   Créez un script appelé `log_analysis.sh` qui exécutera toutes les commandes d’analyse automatiquement :

   ```bash
   nano log_analysis.sh
   ```

   Ajoutez-y les commandes suivantes :

   ```bash
   #!/bin/bash
   echo "Recherche de tentatives de connexion échouées :"
   grep "Failed password" auth.log
   echo "Recherche des événements critiques :"
   grep "CRITICAL" syslog
   echo "Recherche d'adresses IP suspectes :"
   grep "192.168.1.100" auth.log
   echo "Comptage des tentatives de connexion échouées :"
   grep -c "Failed password" auth.log
   ```

   - **Explication :** Ce script regroupe toutes les commandes de recherche dans un seul fichier, rendant l'analyse plus rapide et plus simple.

2. **Exécution du script**

   Rendez le script exécutable et exécutez-le :

   ```bash
   chmod +x log_analysis.sh
   ./log_analysis.sh
   ```

   - **Explication :** Cette commande exécute toutes les recherches en une seule fois, vous faisant gagner du temps et assurant la cohérence de l’analyse.

---

### Conclusion de l’Histoire

Grâce à votre expertise et aux outils comme `grep`, vous avez pu identifier une tentative d'intrusion sur le serveur de **TechCorp**. Votre analyse approfondie a permis de révéler les failles dans la sécurité, et vous avez immédiatement pris des mesures pour les corriger. Le département de sécurité vous félicite pour votre réactivité et votre efficacité.

---

### Annexes

#### Annexe 01 - Commandes grep Avancées

**Explications détaillées sur les options `grep`** :
- Utilisation de `grep -r`, `grep -v`, `grep -i`, `grep -E` pour des recherches avancées.
- Exemples pratiques pour combiner ces options.

#### Annexe 02 - Écriture de Scripts Shell

**Guide pour écrire des scripts shell** :
- Concepts de base pour l'automatisation des tâches avec des scripts.
- Exemples de scripts pour l’analyse des journaux.

#### Annexe 03 - Analyse des Journaux Systèmes

**Meilleures pratiques pour l'analyse des journaux** :
- Techniques pour analyser efficacement les journaux et détecter les anomalies de sécurité.
- Conseils pour optimiser vos recherches avec `grep`.

---

### Questions pour l'Évaluation

1. Quelle commande utilisez-vous pour créer un répertoire appelé `investigation_logs` ?
2. Comment copieriez-vous un fichier `auth.log` dans le répertoire `investigation_logs` ?
3. Quelle commande rechercherait les tentatives de connexion échouées dans `auth.log` ?
4. Comment isoleriez-vous les événements critiques dans le fichier `syslog` ?
5. Comment rechercheriez-vous une adresse IP spécifique dans `auth.log` ?
6. Comment compteriez-vous le nombre de tentatives de connexion échouées dans `auth.log` ?
7. Quelle commande exclurait toutes les lignes contenant "Accepted password" de `auth.log` ?
8. Comment rechercheriez-vous à la fois "Failed" et "CRITICAL" dans `syslog` ?
9. Comment rechercheriez-vous des termes sans tenir compte de la casse dans `syslog` ?
10. Quelle commande filtrerait les événements du 15 août dans `syslog` ?
11. Comment automatiseriez-vous toutes les recherches dans un script shell ?
12. Comment rendriez-vous un script shell exécutable ?
13. Quelle est l'option `-i` dans `grep` et pourquoi est-elle utile ?
14. Pourquoi est-il important d'exclure certaines lignes lors de l'analyse de journaux ?
15. Comment pouvez-vous utiliser `grep` pour rechercher des motifs complexes comme des adresses IP ?
16. Que fait la commande `chmod +x` ?
17. Quelle est la différence entre `grep` et `grep -E` ?
18. Pourquoi est-il utile de compter les occurrences d'un motif dans un fichier de journal ?
19. Comment pouvez-vous combiner plusieurs options `grep` dans une seule commande ?
20. Quelle est l'importance de l'automatisation dans l'analyse des journaux ?
21. Comment feriez-vous pour filtrer des résultats par date dans `grep` ?
22. Pourquoi utiliseriez-vous `grep -v` dans l'analyse des journaux ?
23. Comment rechercheriez-vous plusieurs termes

