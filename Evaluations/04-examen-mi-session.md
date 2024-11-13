# Examen de mi-session 

---

### **Nom de l'étudiant :** _____________________________   **Groupe :** ________________   **Date :** ___________________

---

## **Partie 01 : Quiz (20%)**

> Répondez à chaque question en sélectionnant **la meilleure option** parmi les choix proposés.



1. **Dans un écosystème d’intelligence artificielle, quel est le principal rôle de MLflow ?**
   - a) Automatiser le déploiement de modèles sur des serveurs de production
   - b) Fournir un environnement collaboratif pour le suivi, l’enregistrement et la gestion des expérimentations de machine learning
   - c) Générer automatiquement des modèles de machine learning
   - d) Permettre le calcul distribué des modèles d’intelligence artificielle

2. **Pourquoi est-il essentiel pour une équipe de data scientists d’utiliser un outil comme MLflow pour suivre les expérimentations de modèles ?**
   - a) Pour optimiser l’utilisation de la mémoire
   - b) Pour documenter, comparer et retrouver les configurations et résultats des expérimentations facilement
   - c) Pour accélérer l’entraînement des modèles
   - d) Pour garantir que chaque exécution produit les mêmes résultats

3. **Quelle caractéristique de MLflow facilite l’interopérabilité des modèles de machine learning entre différents frameworks ?**
   - a) La gestion des logs
   - b) Les artefacts stockés en JSON
   - c) Les modèles enregistrés en tant que MLflow Model format, compatible avec plusieurs librairies et outils
   - d) Les fichiers d’hyperparamètres stockés en YAML

4. **Dans un flux de travail d’IA, quel avantage offre le paramètre `--default-artifact-root` spécifié lors du démarrage du serveur MLflow ?**
   - a) Il permet de spécifier un stockage centralisé pour les artefacts, rendant plus simple l’accès aux données et modèles partagés
   - b) Il limite les utilisateurs autorisés à accéder aux expériences
   - c) Il optimise la vitesse d’entraînement des modèles
   - d) Il permet d’entraîner plusieurs modèles en parallèle

5. **Dans une organisation, quel avantage y a-t-il à déployer `mlflow server` sur une machine centralisée plutôt que `mlflow ui` en local ?**
   - a) Permet de dédier les ressources au calcul intensif
   - b) Facilite le travail collaboratif et le suivi d’expériences partagées entre les membres de l’équipe
   - c) Empêche les utilisateurs d’accéder aux résultats des expérimentations
   - d) Aucune différence notable pour les utilisateurs finaux

6. **En IA, pourquoi est-il important de pouvoir visualiser et comparer les performances de plusieurs expérimentations sur un même modèle dans MLflow ?**
   - a) Pour déterminer la configuration exacte du modèle qui donne les meilleurs résultats
   - b) Pour identifier rapidement le modèle le plus rapide en entraînement
   - c) Pour remplacer les modèles qui donnent des erreurs de compilation
   - d) Pour visualiser l’évolution des logs système

7. **Comment MLflow aide-t-il à la reproductibilité dans le domaine de l’IA ?**
   - a) En stockant les données d’entraînement de chaque modèle
   - b) En permettant de sauvegarder les paramètres, métriques et configurations de chaque expérimentation, facilitant la reprise ou le partage des expériences
   - c) En optimisant automatiquement les paramètres du modèle
   - d) En supprimant automatiquement les expériences non concluantes

8. **Dans le cadre d’une gestion de projet en IA, qu’apporte la fonctionnalité de `model signature` de MLflow ?**
   - a) Une sécurité accrue pour les modèles de production
   - b) Une description des types de données attendues en entrée et en sortie, assurant la compatibilité des modèles avec d’autres systèmes
   - c) La possibilité de restreindre les utilisateurs autorisés à consulter le modèle
   - d) Un résumé des erreurs générées par le modèle

9. **Pourquoi MLflow est-il utile dans un environnement de déploiement continu de modèles (MLOps) ?**
   - a) Il permet de visualiser les logs d’erreurs
   - b) Il assure la continuité de la chaîne d’entraînement et de déploiement, en permettant de suivre, valider et déployer automatiquement les versions des modèles
   - c) Il automatise les mises à jour des frameworks de machine learning
   - d) Il améliore la vitesse des calculs

10. **Comment MLflow simplifie-t-il la comparaison des modèles lors de l’entraînement ?**
    - a) En permettant de visualiser les métriques de performance de différents modèles sur une interface centralisée
    - b) En ajustant automatiquement les hyperparamètres
    - c) En réduisant le temps de calcul de chaque modèle
    - d) En enregistrant uniquement les meilleurs résultats

11. **Quelle est l’importance de la compatibilité multi-frameworks pour un outil comme MLflow dans un écosystème d’IA ?**
    - a) Elle permet de changer de framework pour le déploiement sans modifier le code de MLflow
    - b) Elle permet aux data scientists d’utiliser différents frameworks tout en centralisant les suivis et en favorisant la flexibilité dans le choix des outils
    - c) Elle restreint l’accès aux configurations internes des modèles
    - d) Elle garantit une conversion automatique des modèles vers TensorFlow

12. **Dans MLflow, quel est l’objectif d’ajouter un `input_example` lors de l’enregistrement d’un modèle ?**
    - a) Fournir un exemple concret des types de données attendues, utile pour les étapes de déploiement et de documentation
    - b) Diminuer la consommation de mémoire pendant le déploiement
    - c) Stocker les résultats d’entraînement dans un format compressé
    - d) Aucun des choix

13. **En MLOps, pourquoi l’utilisation de MLflow pour gérer les versions de modèles est-elle cruciale ?**
    - a) Elle permet de reconstruire facilement l’historique et de déployer la version optimale en production
    - b) Elle assure une cohérence des logs d’erreur entre les versions
    - c) Elle crée automatiquement des versions alternatives pour comparaison
    - d) Elle permet de contrôler l’accès aux hyperparamètres

14. **Quel est le bénéfice de définir des `tags` pour une expérimentation ou un modèle dans MLflow ?**
    - a) Faciliter la recherche et le filtrage des modèles ou expériences selon des catégories spécifiques (ex. version, priorités)
    - b) Optimiser le temps d’entraînement
    - c) Réduire la consommation d’espace pour chaque expérimentation
    - d) Sécuriser les informations sensibles d’un modèle

15. **Comment MLflow favorise-t-il l’intégration de l’IA dans des pipelines d’entreprise ?**
    - a) En générant des rapports automatisés pour les parties prenantes
    - b) En centralisant le suivi, l’enregistrement et la gestion de modèles et d’artefacts, facilitant leur intégration dans des systèmes existants
    - c) En effectuant des tests de charge sur les modèles
    - d) En simplifiant la création de modèles génératifs

16. **Pourquoi est-il pertinent d’utiliser SQLite en tant que backend pour MLflow dans un environnement de test ou de développement ?**
    - a) Il facilite l’intégration avec des frameworks d’IA avancés
    - b) Il offre une solution simple et légère pour stocker les données d’expérimentation dans des environnements où une base de données robuste n'est pas requise
    - c) Il assure une rapidité d’accès aux logs système
    - d) Il fournit des configurations avancées de sécurité des données

17. **Dans quel cas une équipe de machine learning pourrait-elle préférer utiliser `mlflow ui` au lieu de `mlflow server` ?**
    - a) Lorsque le suivi des expérimentations doit être effectué uniquement en local, sans besoin de centralisation
    - b) Lorsque le modèle nécessite une intégration avec des bases de données externes
    - c) Lorsque les expérimentations doivent être partagées entre plusieurs utilisateurs
    - d) Lorsque l’objectif est d’optimiser les temps de calcul

18. **Lorsqu’on déploie MLflow en production, pourquoi utiliser une base de données comme PostgreSQL en tant que backend ?**
    - a) Pour permettre un stockage sécurisé et performant des métadonnées d’expérimentation, avec des capacités de traitement plus élevées qu’un système de fichiers local
    - b) Pour accélérer l’entraînement des modèles
    - c) Pour limiter l’accès aux ressources de calcul
    - d) Pour déployer plusieurs modèles en simultané

19. **Quel rôle joue la commande `mlflow.evaluate` dans un flux de travail IA ?**
    - a) Elle exécute une série de tests pour valider les performances des modèles en production
    - b) Elle permet d’évaluer les modèles en comparant les prédictions aux données réelles, avec des métriques personnalisées et artefacts visuels
    - c) Elle installe les dépendances nécessaires aux modèles MLflow
    - d) Elle optimise le stockage des artefacts de MLflow

20. **Pourquoi est-il utile d’ajouter des artefacts visuels, comme des graphiques de dispersion, dans les expérimentations de MLflow ?**
    - a) Pour documenter visuellement la performance et les écarts entre les prédictions et les valeurs réelles, facilitant l’interprétation des résultats pour les parties prenantes
    - b) Pour réduire le besoin de stocker des logs texte
    - c) Pour améliorer l’intégration avec d’autres plateformes d’IA
    - d) Pour réduire l’espace de stockage requis par MLflow
   




## **Partie 02 : Question de développement (30%)**

**Contexte :**  
Vous travaillez sur un projet de machine learning en collaboration avec plusieurs data scientists. Vous devez tester différents modèles et configurations, tout en vous assurant que les versions de chaque expérimentation sont bien suivies et reproductibles.

**Question :**  
Expliquez comment l’utilisation de MLflow pourrait simplifier la gestion de ce projet et permettre une meilleure collaboration dans l’équipe. Discutez en particulier de l’utilité du suivi des versions, de la reproductibilité et de la centralisation des expérimentations pour votre équipe.

> **Éléments d'évaluation :**
> - Pertinence de l’analyse et de la compréhension de MLflow pour un projet collaboratif.
> - Capacité à expliquer la gestion des versions et la reproductibilité dans ce contexte.
> - Clarté et structure des réponses.

---

## **Partie 03 : Conception d’une plateforme de machine learning complète (50%)**

**Contexte :**  
Vous êtes mandaté pour concevoir et mettre en place une plateforme de machine learning complète qui permet de gérer l’ensemble du cycle de vie d’un projet, depuis l’ingestion et le nettoyage des données jusqu’au déploiement et à la surveillance des modèles en production. Vous devez tenir compte des contraintes suivantes : gestion de volumes de données élevés, entraînement de plusieurs modèles, optimisation des hyperparamètres sans ralentir le développement, et minimisation des coûts.

### En tant que spécialiste en intelligence artificielle, proposez une solution complète :

1. **Architecture complète** : Décrivez les technologies, infrastructures, et outils à utiliser pour développer, tester, et déployer vos modèles. Justifiez les avantages et inconvénients de vos choix pour répondre aux besoins de scalabilité, rapidité, et flexibilité.

2. **Infrastructure matérielle et gestion des coûts** : Précisez la configuration matérielle optimale (incluant l’utilisation potentielle de GPU). Si vous envisagez une infrastructure cloud, évaluez l'impact des coûts par rapport à une solution on-premise, et proposez des stratégies d'optimisation.

3. **Méthodologie de gestion des expérimentations et versions** : Proposez des solutions pour enregistrer les versions des modèles, suivre les performances, et garantir la reproductibilité.

4. **Pipeline de données et préparation** : Élaborez votre approche pour nettoyer et préparer les données, incluant des options pour automatiser ce processus pour des données volumineuses.

5. **Déploiement, surveillance, et amélioration continue** : Décrivez votre méthode pour déployer les modèles, surveiller leurs performances, et gérer les versions en production. Expliquez si vous utiliseriez des outils comme Kubernetes et comment vous géreriez l’amélioration continue des modèles.

> **Éléments d'évaluation :**
> - **Pertinence et cohérence de l'architecture** : adéquation aux objectifs et contraintes.
> - **Gestion de l'infrastructure et des coûts** : choix raisonnés et optimisation.
> - **Méthodologie de gestion des expérimentations** : capacité à assurer le suivi et la reproductibilité.
> - **Pipeline de données** : efficacité et scalabilité de la préparation des données.
> - **Déploiement et surveillance** : robustesse et adaptabilité de la solution proposée.

---

## *Grille de notation*

### **Partie 01 - Quiz (20%)**
- Vous serez notés sur l'exactitude de vos réponses. Chaque question correcte rapporte 1 point.

### **Partie 02 - Question de développement (30%)**
- **Objectif** : Montrer votre capacité à analyser et expliquer comment MLflow facilite la gestion d’un projet de machine learning collaboratif.
- **Critères** :
  - Pertinence de l’analyse
  - Clarté des explications
  - Gestion des versions et reproductibilité

### **Partie 03 - Projet de plateforme complète (50%)**
- **Objectif** : Concevoir une plateforme de machine learning de bout en bout.
- **Critères** :
  - Pertinence et cohérence de l'architecture
  - Optimisation de l'infrastructure et des coûts
  - Méthodologie de gestion des expérimentations
  - Efficacité du pipeline de données
  - Robustesse du déploiement et de la surveillance des modèles

---

> Bonne chance à tous ! Soyez clairs et précis dans vos explications, et n’hésitez pas à justifier vos choix dans les questions de développement.
