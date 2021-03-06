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

## Programmation Bash

__Vous enregistrerez vos scripts dans un dossier script que vous créerez dans votre répertoire personnel.Tous les scripts sont bien entendu à tester. Ajoutez le chemin vers script à votre PATH de manière permanente.__

`PATH=$PATH:~/script`
(on ajoute cette ligne au ~/.bashrc)

# Exercice 2. Contrôle de mot de passe

__Écrivez un scripttestpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.__

```
#!/bin/sh

PASSWORD=Test123;

read -p "Saisissez un mot de passe :" -s pwd

if [ $pwd = $PASSWORD ]; then
        echo -e "\nMot de passe correct"
else
        echo -e "\nMot de passe incorrect"
fi
```

# Exercice 3. Expressions rationnelles

__Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètreest un nombre réel__

```
#!/bin/sh

function is_number()
{
        re='^[+-]?[0-9]+([.][0-9]+)?$'
        if ! [[ $1 =~ $re ]] ; then
                return 1
        else
                return 0
        fi
}

if is_number $1 ; then
        echo -e "\nCorrect"
else
        echo -e "\nErreur"
fi
```

# Exercice 4. Contrôle d’utilisateur

__Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si lescript est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation :nom_du_scriptnom_utilisateur”,oùnom_du_scriptest le nom de votre script récupéré automatiquement (si vous changez le nom de votrescript, le message doit changer automatiquement)__

```
#!/bin/sh

if [ -z "$1" ]; then
        echo -e "Utilisation : $(basename $0) nom_utilisateur";
elif id "$1" >/dev/null 2>&1 ; then
        echo -e "Exist";
else
        echo -e "Not Exist";
fi
```

# Exercice 5. Factorielle

__Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera quel’utilisateur saisit toujours un entier naturel).__

```
#!/bin/sh

fact() {
        n=$1
        if [ $n -eq 0 ]; then
                echo 1
        else
                echo $(( n * `fact $(( n - 1 ))` ))
        fi
}

echo "Resultat : $(fact $1)"
```

# Exercice 6. Le juste prix

__Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner.Le programme écrira ”C’est plus!”, ”C’est moins!” ou ”Gagné!” selon les cas (vous utiliserez$RANDOM).__

```
#!/bin/bash

RDM=$((1 + RANDOM % 1000));
REP=0;

read -p 'Saisissez un nombre entre 1 et 1000 : '

while [ $REP -ne $RDM ];
do
        if [ $REP -gt $RDM ]; then
                read -p 'Le nombre est trop grand : ' REP
        elif [ $REP -lt $RDM ]
        then
                read -p 'Le nombre est trop petit : ' REP
        fi
done

echo "Bravo ! Vous avez trouvé";
```

# Exercice 7. Statistiques

1. __Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max
et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres
sont bien des entiers.__

```
function is_number()
{
 re='^[+-]?[0-9]+([.][0-9]+)?$'
 if ! [[ $1 =~ $re ]] ; then
  return 1
 else
  return 0
 fi
}

MAX=-100;
MIN=100;
MOY=0;

for i in $(seq 1 3);
do
        if is_number ${!i} ; then
                if [ ${!i} -lt $MIN ]; then
                        MIN=${!i}
                fi
                if [ ${!i} -gt $MAX ]; then
                        MAX=${!i}
                fi
                
                MOY=$(($MOY+${!i}))
        else
                echo "${!i} n'est pas un nombre entiers"
        fi
done
MOY=$(($MOY/3))

echo "MIN: $MIN / MOY: $MOY / MAX: $MAX";
```

2. __Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT)__

```
function is_number()
{
 re='^[+-]?[0-9]+([.][0-9]+)?$'
 if ! [[ $1 =~ $re ]] ; then
  return 1
 else
  return 0
 fi
}

MAX=-100;
MIN=100;
MOY=0;
NBVALUE=0;

while (("$#"));
do
        if is_number $1 ; then
                if [ $1 -lt $MIN ]; then
                        MIN=$1
                fi
                if [ $1 -gt $MAX ]; then
                        MAX=$1
                fi
                NBVALUE=$(($NBVALUE+1));
                MOY=$((MOY+$1));
        else
                echo "$1 n'est pas un nombre entiers"
        fi
        shift
done
MOY=$(($MOY/$NBVALUE))
echo "MIN: $MIN / MOY: $MOY / MAX: $MAX";
```

3. __Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et
stockées au fur et à mesure dans un tableau.__

```
#!/bin/bash

function is_number()
{
 re='^[+-]?[0-9]+([.][0-9]+)?$'
 if ! [[ $1 =~ $re ]] ; then
  return 1
 else
  return 0
 fi
}

GET=0;
MAX=-100;
MIN=100;
MOY=0;

TAB=();

while [ $GET -lt 101 ] && [ $GET -gt -101 ]
do
        read -p 'Saisissez la note : ' GET

        if [ $GET -lt 101 ] && [ $GET -gt -101 ]; then
                if is_number $GET ; then

                        TSIZE=${#TAB[*]}
                        TAB[$TSIZE+1]=$GET

                        if [ $GET -lt $MIN ]; then
                                MIN=$GET
                        fi
                        if [ $GET -gt $MAX ]; then
                                MAX=$GET
                        fi

                        MOY=$(($MOY+$GET))
                else
                        echo "$GET n'est pas un nombre entiers"
                fi
        fi
done
TSIZE=${#TAB[*]}
echo "Generate Table ... ($TSIZE value(s))";
echo ${TAB[*]};
MOY=$(($MOY/$TSIZE))
echo "MIN: $MIN / MOY: $MOY / MAX: $MAX";
```
