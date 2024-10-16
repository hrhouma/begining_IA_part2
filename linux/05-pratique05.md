# `~/.bashrc` et `~/.bash_profile`
- Les fichiers `~/.bashrc` et `~/.bash_profile` sont tous deux des fichiers de configuration pour le shell Bash, mais ils sont utilisés dans des contextes différents. Voici les principales différences entre les deux :

### 1. **Contexte d'Exécution :**
   - **`~/.bashrc` :**
     - Ce fichier est exécuté chaque fois qu'un shell **interactif non-login** est lancé, c'est-à-dire lorsque vous ouvrez un nouveau terminal à partir de votre environnement graphique ou lorsque vous ouvrez un terminal depuis une session déjà connectée.
     - Il est couramment utilisé pour configurer l'environnement de l'utilisateur, comme les alias, les fonctions, l'historique des commandes, et d'autres options spécifiques au shell.

   - **`~/.bash_profile` :**
     - Ce fichier est exécuté chaque fois qu'un shell **interactif login** est lancé, c'est-à-dire lorsque vous vous connectez à une session en ligne de commande (par exemple, en SSH ou lors de la connexion à une session tty).
     - Ce fichier est souvent utilisé pour définir des variables d'environnement (comme `PATH`), lancer des scripts spécifiques à la session de login, ou configurer d'autres paramètres qui ne doivent être définis qu'une seule fois lors de la connexion.

### 2. **Ordre de Priorité :**
   - Si les deux fichiers existent, **`~/.bash_profile`** est lu et exécuté lors du démarrage d'une session de login. Si `~/.bash_profile` existe, il est exécuté à la place de `~/.bashrc` (c'est-à-dire que `~/.bashrc` ne sera pas lu directement).
   - Cependant, il est courant que `~/.bash_profile` contienne une ligne pour appeler `~/.bashrc` afin que les configurations dans ce fichier soient également chargées, même lors des sessions de login. Cette ligne ressemble à ceci :
   
   ```sh
   if [ -f ~/.bashrc ]; then
       source ~/.bashrc
   fi
   ```

### 3. **Utilisation Commune :**
   - **`~/.bashrc`** est généralement préféré pour configurer l'environnement du shell interactif parce qu'il est exécuté plus fréquemment (chaque fois qu'un terminal est ouvert).
   - **`~/.bash_profile`** est plus adapté pour les configurations qui ne doivent être appliquées qu'une seule fois par session de login.

### **Résumé :**
- **`~/.bashrc` :** Utilisé pour configurer des shells interactifs non-login (ouverture d'un nouveau terminal).
- **`~/.bash_profile` :** Utilisé pour configurer des shells interactifs login (connexion à une nouvelle session). 

**Remarque :** Sur certaines distributions, vous pouvez aussi trouver d'autres fichiers similaires comme `~/.bash_login` ou `~/.profile`, mais les fichiers `~/.bashrc` et `~/.bash_profile` sont les plus couramment utilisés.
