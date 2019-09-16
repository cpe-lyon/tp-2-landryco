# Exercice 1. Variables d’environnement

1. __Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur?__ <br>
`printenv PATH`<br>
`/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin`

2. __Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel?__
`$HOME`

3. __Explicitez le rôle des variables LANG,PWD,OLDPWD,SHELL et \_.__ <br>
`LANG`: La langue utilisé par l'utilisateur connecté<br>
`PWD` : Le chemin actuel de l'utilisateur connecté<br>
`OLDPWD` : Le chemin précédent de l'utilisateur connecté<br>
`SHELL` : Le shell utilisé par l'utilisateur connecté<br>
`_` : La dernière commande saisie par l'utilisateur connecté<br>

4. __Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.__ <br>
`MY_VAR=OK`<br>
`echo $MY_VAR`<br>`OK`

5. __Tapez ensuite la commande bash. Que fait-elle? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale.__<br>
Même si cela semble ne rien faire, en réalité, un nouveau shell est exécuté (on peut le voir en tapant `exit`).
La variable MY_VAR n'existe pas car il s'agit d'une variable locale qui n'est valide que dans le shell où elle a été créée.

6. __Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.__
`export MY_VAR=OK`
La variable MY_VAR est désormait une variable d'environnement. Elle sera donc valide sur tous les shells tant que le shell créateur n'est pas fermée.

7. __Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.__
`NOMS='Coud & Landry'`

8. __Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2!” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS.__
`echo "Bonjour à vous deux, $NOMS"`

9. __Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande unset?__
Avec la commande `unset`, la variable sera totalement supprimée de la mémoire du shell.

10. __Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash)__
`echo '$HOME = '"$HOME"`

11. __

