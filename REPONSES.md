Réponses au TP Git & GitHub

# Partie 1
Q1. L'option `--global` enregistre la configuration dans le fichier de configuration global situé dans le répertoire personnel de l'utilisateur (par exemple, `~/.gitconfig` sous Linux/Git Bash ou `C:\Users\<nom>\.gitconfig` sous Windows). 
* **Différence :** Une configuration globale s'applique à tous les dépôts de l'utilisateur sur la machine, tandis qu'une configuration locale (sans `--global`, faite directement dans un dépôt) s'applique uniquement à ce dépôt spécifique et prend le pas sur la configuration globale.
Q2.GitHub utilise l'adresse email associée au commit pour identifier son auteur. Si l'email configuré dans Git ne correspond pas à celui de votre compte GitHub, vos commits ne seront pas reliés à votre profil et n'apparaîtront pas dans vos contributions (graphique de contribution).

# Partie 2
Q3.Le dossier `.git` contient toute l'infrastructure de contrôle de version : l'historique des commits, les branches, les tags, les objets (fichiers compressés), la configuration du dépôt local et l'index (staging area).
Conséquence de sa suppression : On perdrait tout l'historique des modifications, les branches et l'état du dépôt. Le dossier ne serait plus qu'un simple répertoire ordinaire non versionné.
Q4.* `git diff` montre les modifications apportées dans la copie de travail qui n'ont **pas encore été indexées** (non ajoutées par `git add`).
   `git diff --staged` (ou `--cached`) montre les modifications qui ont été **indexées** (ajoutées avec `git add`) et qui sont prêtes à partir dans le prochain commit.
Q5.Il s'agit du hachage (ou *hash* / identifiant unique) SHA-1 abrégé du commit (généralement les 7 premiers caractères). Il sert d'empreinte numérique pour identifier et cibler précisément ce commit dans l'historique.
# Partie 3 : Corriger et annuler

*Q6.
 On ne versionne pas les fichiers compilés (`.class`) car ce sont des fichiers générés automatiquement à partir du code source (`Main.java`). Ils sont redondants et alourdiraient inutilement l'historique.
*Rôle de `.gitignore` :** Il indique à Git quels fichiers ou dossiers ignorer automatiquement, ce qui évite de les inclure par erreur lors d'un `git add .` global.

*Q7. 
* Réponse :** 
  * `git revert` crée un *nouveau* commit qui annule les effets d'un commit précédent, en préservant l'historique.
  * `git reset` supprime purement et simplement des commits de l'historique local en reculant le pointeur HEAD.
*Sur une branche partagé Il faut utiliser **`git revert`**, car `git reset` réécrit l'historique et pose de graves problèmes de synchronisation si d'autres collaborateurs ont déjà récupéré les commits.

Q8. Que signifie `HEAD~1` ? Que veut dire le message « detached HEAD » vu à l'étape 3.8 ?**
Réponse :
  * `HEAD~1` désigne le parent direct du commit actuel (le commit juste avant le HEAD).
  * « Detached HEAD » (HEAD détaché) signifie que le pointeur HEAD ne pointe plus sur une branche active, mais directement sur un commit précis du passé via son identifiant.
Partie4
Q9. Pourquoi la multiplication est-elle absente de `Main.java` à l'étape 4.2 alors que le fichier est le même ?**
Réponse : Parce que vous vous trouvez sur la branche `main` et que la modification (la méthode de multiplication) a été développée et enregistrée sur une autre branche (`multiplication`). Les branches isolent les modifications tant qu'elles ne sont pas fusionnées.

Q10. Qu'est-ce qu'une fusion fast-forward ? Pourquoi la fusion de l'étape 4.4 a-t-elle créé un commit de fusion ?**
Réponse :
  * Une fusion *fast-forward* se produit lorsque la branche principale (`main`) n'a subi aucun changement depuis la création de la branche secondaire : Git se contente de déplacer le pointeur `main` vers le dernier commit de la branche.
  * À l'étape 4.4, les deux branches avaient divergé (des modifications ont été faites sur les deux côtés). Git a donc dû créer un **commit de fusion** (*merge commit*) pour combiner les deux historiques.

Q11. Que signifient `<<<<<<< HEAD` `=======` et `>>>>>>> titre` ? Dans quel cas Git ne peut-il pas fusionner seul ?**
Réponse : Ce sont des marqueurs de conflit insérés par Git dans le fichier :
  * `<<<<<<< HEAD` : début du code de votre version actuelle (sur `main`).
  * `=======` : séparateur entre les deux versions.
  * `>>>>>>> titre` : fin du code provenant de la branche fusionnée (`titre`).
git remote add origin git@github.com:VotrePseudo/tp-git-votre-nom.gitCas de blocage : Git ne peut pas fusionner seul lorsque la *même ligne* ou le *même bloc de code* a été modifié de deux manières dif férentes sur les deux branches.
# Partie 5 : Publier sur GitHub

Q12. Que désigne `origin` ? Que fait l'option `-u` de `git push` ?**
Réponse :
  * `origin` est l'alias par défaut donné au dépôt distant hébergé sur GitHub.
  * L'option `-u` (ou `--set-upstream`) mémorise le lien entre la branche locale actuelle et la branche distante, permettant d'utiliser simplement `git push` ou `git pull` par la suite sans repréciser les noms.

Q13. Quelle différence entre `git fetch` et `git pull` ?**
Réponse :
  * `git fetch` télécharge les nouveautés du dépôt distant sur votre machine sans les fusionner avec votre code actuel.
  * `git pull` fait un `git fetch` suivi immédiatement d'un `git merge` pour intégrer les modifications distantes dans votre copie de travail.

Q14. Pourquoi le `git push` de l'étape 5.5 a-t-il été refusé ? Quelle est la bonne habitude à prendre avant de pousser ?**
Réponse : Il a été refusé car le dépôt distant avait pris de l'avance (un autre poste ou une modification en ligne avait ajouté des commits). Pour éviter d'écraser le travail, Git refuse le push.
Bonne habitude :Toujours faire un `git pull` (ou `git fetch` puis `git merge`) pour récupérer les dernières modifications distantes avant de pousser ses propres commits.

# Partie 6 : Collaborer : fork, pull request et issues

Q15. Quelle différence entre un fork, un clone et une branche ?**
Réponse :
  * Un fork est une copie indépendante d'un dépôt sur le serveur GitHub d'un autre utilisateur.
  * Un clone est une copie locale sur votre machine d'un dépôt distant.
  * Une branche est une ligne de développement parallèle au sein d'un même dépôt.

Q16. Pourquoi propose-t-on une PR depuis une branche dédiée plutôt que depuis `main` ?**
Réponse : Travailler sur une branche dédiée permet de garder la branche `main` propre, de tester la fonctionnalité isolément et de faciliter la relecture de code (*code review*) ainsi que la gestion d'éventuels retours ou modifications avant la fusion.

Q17. À quoi servent les remotes `origin` et `upstream` dans le dépôt de B ?**
Réponse :
  * `origin` pointe vers le **fork personnel** du contributeur (où il a les droits en écriture pour pousser ses branches).
  * `upstream` pointe vers le **dépôt d'origine** (celui du propriétaire A), ce qui permet de récupérer les mises à jour officielles du projet.

---

# Partie 7 : Entraînement interactif LearnGitBranching

Q18. Pour trois niveaux de votre choix, notez dans REPONSES.md le nom du niveau, vos commandes et votre score par rapport à la solution optimale.**
* Réponse :
  1. Niveau 1 : Introduction à `git commit`**
     * Commandes : `git commit`, `git commit`
     * Score : Optimal (2 commandes / 2 optimal)
  2. Niveau 2 : `git branch` et `git checkout` (ou switch)**
     * Commandes : `git branch bugFix`, `git checkout bugFix`
     * Score : Optimal (2 commandes / 2 optimal)
  3. Niveau 3 : Fusion de branches avec `git merge`**
     * Commandes : `git checkout -b bugFix`, `git commit`, `git checkout main`, `git commit`, `git merge bugFix`
     * Score : Optimal (5 commandes / 5 optimal)