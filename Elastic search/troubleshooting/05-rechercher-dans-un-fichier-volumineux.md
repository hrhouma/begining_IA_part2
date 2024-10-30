------------------------------
# Introduction: 
------------------------------

Pour rechercher ces lignes dans un fichier volumineux, vous pouvez utiliser des commandes de recherche comme `grep` sous Linux, ou l’éditeur de texte de votre choix. 
- Voici comment procéder :

![image](https://github.com/user-attachments/assets/172c7f10-2ba2-4e01-b0a3-67499fa1e2d2)

# Sur Linux avec `grep` :
Pour rechercher les lignes contenant `network.host` et `http.port`, utilisez :
```bash
grep -E 'network\.host|http\.port' nom_du_fichier
```
Cela retournera toutes les lignes contenant l'une ou l'autre des expressions.

# Avec un éditeur de texte comme VS Code :
1. Ouvrez le fichier dans Visual Studio Code.
2. Utilisez la fonction de recherche (généralement accessible avec **Ctrl+F**).
3. Tapez `network.host` pour trouver toutes les occurrences de cette ligne, puis `http.port` pour la seconde.

# Avec Notepad++ (sur Windows) :
1. Ouvrez le fichier.
2. Utilisez **Ctrl+F** pour ouvrir la recherche, puis entrez `network.host` et `http.port` pour les trouver respectivement.

Ces méthodes vous permettront de localiser facilement les lignes spécifiques dans un fichier volumineux.
