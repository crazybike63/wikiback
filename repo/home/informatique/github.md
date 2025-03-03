---
title: github
description: 
published: true
date: 2024-08-21T10:25:31.851Z
tags: 
editor: markdown
dateCreated: 2024-08-04T17:23:20.491Z
---

# Fonctionnement
![principe](https://th.bing.com/th/id/OIP.Ju-X7VzkCnYT7UngdT26QwHaG8?rs=1&pid=ImgDetMain)
# lien vers la page Git
[git](/https://git-scm.com/docs/git)

# Commandes
## git add  .
Commande qui permet d'enregistrer les fichiers qui vont être versionnés

NOM
git-add - Ajoute le contenu de fichiers à l’index

SYNOPSIS
git add [--verbose | -v] [--dry-run | -n] [--force | -f] [--interactive | -i] [--patch | -p]
	  [--edit | -e] [--[no-]all | --[no-]ignore-removal | [--update | -u]]
	  [--intent-to-add | -N] [--refresh] [--ignore-errors] [--ignore-missing] [--renormalize]
	  [--chmod=(+|-)x] [--pathspec-from-file=<fichier> [--pathspec-file-nul]]
	  [--] [<spécificateur de chemin>…​]
DESCRIPTION
Cette commande met à jour l’index en utilisant le contenu actuel trouvé dans l’arbre de travail, pour préparer le contenu de la prochaine validation. Typiquement, elle ajoute intégralement le contenu actuel des chemins existants, mais peut aussi n’ajouter que certaines parties des modifications au moyen d’options ou soustraire certains chemins qui n’existent plus dans l’arbre de travail.

L'« index » contient un instantané du contenu de la copie de travail et c’est cet instantané qui sera utilisé comme contenu du prochain commit. Ainsi, après avoir réalisé des modifications dans la copie de travail, et avant de lancer la commande commit, vous devez utiliser la commande add pour ajouter tout fichier nouveau ou modifié à l’index.

Cette commande peut être effectuée plusieurs fois avant la validation. Elle n’ajoute que le contenu des fichiers spécifiés au moment où la commande add est lancée ; si vous souhaitez inclure des modifications postérieures à un add dans la prochaine validation, vous devez alors lancer git add à nouveau pour ajouter le nouveau contenu à l’index.

La commande git status permet d’obtenir un résumé des fichiers modifiés qui sont préparés pour la prochaine validation.

Par défaut, la commande git add n’ajoute pas les fichiers ignorés. Si des fichiers ignorés sont spécifiés explicitement en ligne de commande, git add échouera avec la liste des fichiers ignorés. Les fichiers ignorés atteints via la récursion de répertoires ou les patrons de fichiers gérés par Git (les patrons doivent alors être échappés du shell par des quotes) seront ignorés silencieusement. La commande git add peut tout de même ajouter des fichiers ignorés avec l’option -f (force).

Référez-vous git-commit[1] pour des méthodes alternatives d’ajout de contenu à une validation.

OPTIONS
<spécificateur de chemin>…​
Fichiers dont le contenu doit être ajouté. Les patrons (ex : *.c) permettent de restreindre à tous les fichiers correspondants. Un nom de répertoire (ex : rep pour ajouter rep/fichier1 et rep/fichier2) permet d’ajouter récursivement tous les fichiers d’un répertoire (par exemple spécifier rep n’enregistrera pas seulement le fichier modifié dans l’arbre de travail rep/fichier1 ou un fichier ajouté rep/fichier2, mais aussi un fichier rep/fichier3 supprimé de l’arbre de travail). Veuillez noter que des version anciennes de Git ignoraient les fichiers supprimés ; utilisez l’option --no-all si vous souhaitez ajouter les fichiers nouveaux ou modifiés mais ignorer les fichiers supprimés.

Pour de plus amples détails sur la syntaxe <spécificateur de chemin>, référez-vous à la section pathspec dans gitglossary[7].

-n
--dry-run
Ne pas ajouter réellement les fichiers. Montrer juste ceux qui existent ou seront ignorés.

-v
--verbose
Mode bavard.

-f
--force
Permettre l’ajout de fichiers qui sont normalement ignorés.

-i
--interactive
Ajouter le contenu modifié dans l’arbre de travail à l’index. Les arguments optionnels de chemin permettent de limiter les opérations à un sous-ensemble de l’arbre de travail. Référez-vous à « Mode interactif » pour plus de détails.

-p
--patch
Choisir de manière interactive les sections de patch entre l’index et la copie de travail et les ajouter à l’index. Cela permet à l’utilisateur de passer en revue les différences avant d’ajouter le contenu modifié à l’index.

Cela lance effectivement add --interactive mais court-circuite le menu initial et saute directement à la sous-commande patch. Référez-vous à « Mode interactif » pour plus de détails.

-e
--edit
Ouvrir les différences avec l’index dans un éditeur et laisser l’utilisateur les éditer. Après la fermeture de l’éditeur, ajuster les entêtes de sections et appliquer le patch à l’index.

L’objectif de cette option est de permettre de choisir et retenir les lignes de la rustine à appliquer, ou même de modifier le contenu des lignes à indexer. Cela peut être plus rapide et plus flexible que l’utilisation du sélecteur interactif. Cependant, il est plus facile de se tromper et de créer un patch qui ne s’applique pas. Référez-vous à ÉDITER LES RUSTINES ci-dessous.

-u
--update
Mettre à jour l’index sur les seuls fichiers déjà présents et correspondant à <chemin>. Cela retire ou modifie les entrées d’index pour correspondre à la copie de travail, mais n’ajoute pas de fichier.

Si aucun <chemin> n’est spécifié quand l’option -u est utilisée, tous les fichiers suivis dans la totalité de l’arbre de travail sont mis à jour (les versions anciennes de Git limitaient la mise à jour au répertoire courant et ses sous-répertoires).

-A
--all
--no-ignore-removal
Mettre à jour l’index non seulement pour tous les fichiers de l’arbre de travail correspondant à <chemin> mais aussi pour toutes les entrées existant déjà dans l’index. Ceci ajoute, modifie et retire des entrées d’index pour correspondre à la copie de travail.

Si aucun <chemin> n’est spécifié quand l’option -A est utilisée, tous les fichiers de l’arbre de travail sont mis à jour (les versions anciennes de Git utilisaient le répertoire courant et ses sous-répertoires).

--no-all
--ignore-removal
Mettre à jour l’index en y ajoutant les nouveaux fichiers qui sont inconnus et les fichiers modifiés dans la copie de travail, mais ignore les fichiers qui ont été effacés de la copie de travail. Cette option ne fait rien quand aucun <chemin> n’est utilisé.

Cette option sert principalement à aider les utilisateurs de versions anciennes de Git pour lesquels « git add <chemin>…​ » était synonyme de « git add --no-all <chemin>…​ », c’est-à-dire qui ignorait les fichiers effacés.

-N
--intent-to-add
N’enregistrer que le fait que le chemin sera ajouté plus tard. Une entrée pour le chemin est placée en index sans contenu. C’est particulièrement utile pour, entre autres choses, montrer le contenu non indexé de ces fichiers avec git diff et les valider avec git commit -a.

--refresh
Ne pas ajouter le ou les fichiers mais rafraîchir seulement leur information de stat() dans l’index.

--ignore-errors
Si des fichiers n’ont pas pu être ajoutés à cause d’erreurs lors de leur indexation, ne pas annuler l’opération mais continuer l’ajout des autres fichiers. La commande se terminera tout de même avec un code d’erreur non nul. Le paramètre de configuration add.ignoreErrors peut être positionné à true pour que ce comportement soit celui par défaut.

--ignore-missing
Cette option ne peut être utilisée que couplée avec --dry-run. L’utilisation de cette option permet à l’utilisateur de vérifier si un des fichiers indiqués serait ignoré, qu’il soit présent ou non dans la copie de travail.

--no-warn-embedded-repo
Par défaut, git add avertit l’utilisateur lors de l’ajout d’un dépôt embarqué dans l’index sans utilisation de git submodule add pour créer une entrée dans .gitmodules. Cette option supprime cet avertissement (par exemple, si vous effectuez manuellement des opérations sur des sous-modules).

--renormalize
Forcer l’application du filtre "clean" à tous les fichiers suivis pour les ajouter en force à l’index. C’est utile après avoir changé la variable de configuration core.autocrlf ou l’attribut text pour corriger des fichiers ajoutés avec des fins de lignes erronées CRLF/LF. Cette option implique -u.

--chmod=(+|-)x
Forcer le bit exécutable des fichiers ajoutés. Le bit exécutable n’est modifié que dans l’index, les fichiers de la copie de travail ne sont pas modifiés.

--pathspec-from-file=<fichier>
Le spécificateur de chemin est passé dans <fichier> au lieu des arguments de la ligne de commande. Si <fichier> vaut - alors l’entrée standard est utilisée. Les éléments du spécificateur de chemin sont séparés par LF ou CR/LF. Les éléments du spécificateur de chemin peuvent être cités comme expliqué pour la variable de configuration core.quotePath (voir git-config[1]). Voir aussi l’option --pathspec-file-nul et l’option globale --literal-pathspecs.

--pathspec-file-nul
Uniquement significatif avec --pathspec-from-file. Les éléments du spécificateur de chemin sont séparés par le caractère NUL et tous les autres caractères sont utilisés littéralement (y compris les retours à la ligne et les guillemets).

--
Cette option permet de séparer les options de la ligne de commande de la liste des fichiers (utile si certains noms de fichiers peuvent être confondus avec des options).

EXEMPLES
Ajouter le contenu de tous les fichiers *.txt sous le répertoire Documentation et ses sous-répertoires :

$ git add Documentation/\*.txt
Remarquez que l’astérisque * est échappé du shell dans cet exemple ; cela permet d’inclure les fichiers dans les sous-répertoires du répertoire Documentation/.

Ajouter le contenu de tous les scripts git-*.sh :

$ git add git-*.sh
Comme cet exemple laisse le shell réaliser l’expansion de l’astérisque (c’est-à-dire que vous listez explicitement les fichiers du répertoire), il ne traite pas subdir/git-foo.sh.

MODE INTERACTIF
Quand la commande entre en mode interactif, elle affiche le résultat de la sous-commande status, puis entre en boucle de commande interactive.

La boucle de commande affiche la liste des sous-commandes disponibles et affiche le prompt « What now> » (Que faire maintenant). En général, lorsque le prompt se termine par un > unique, vous ne pouvez choisir qu’une seule des propositions et appuyer Entrée, comme cela :

    *** Commands ***
      1: status       2: update       3: revert       4: add untracked
      5: patch        6: diff         7: quit         8: help
    What now> 1
Vous pouvez indiquer s ou sta ou status dans le cas ci-dessus, à condition que le choix soit univoque.

La boucle de commande principale propose 6 sous-commandes (plus help (aide) et quit (quitter)).

status
Ceci affiche les modifications entre HEAD et l’index (c-à-d ce qui seraient validées si vous lanciez git commit), et entre l’index et les fichiers de la copie de travail (c-à-d ce que vous pourriez indexer au moyen de git add avant de lancer git commit) pour chaque chemin. Un exemple d’affichage ressemble à ceci :

              staged     unstaged path
     1:       binary      nothing foo.png
     2:     +403/-35        +1/-1 git-add--interactive.perl
Cela montre que foo.png contient des différences avec HEAD (mais c’est un format binaire donc le nombre de lignes ne peut pas être affiché) et qu’il n’y a pas de différence entre la copie indexée et la copie de travail (si la copie de travail aussi avait été différente, binary aurait été affiché à la place de nothing). L’autre fichier, git-add--interactive.perl, a 430 lignes ajoutées et 35 effacées si vous validez ce qui est dans l’index, mais la copie de travail contient d’autres modifications (un ajout et un retrait).

update
Ceci affiche l’information d’état et une invite "Update>>". Quand l’invite se termine par un double >, vous pouvez sélectionner plus d’une option, concaténées avec des espaces ou des virgules. Vous pouvez aussi indiquer des intervalles. Par exemple "2-5 7,9" pour choisir 2, 3, 4, 5, 7 et 9 dans la liste. Si le second nombre d’un intervalle est absent, tous les patchs restants sont sélectionnés. Par ex. "7-" choisit 7, 8 et 9 dans la liste. * permet de tout sélectionner.

Tout ce qui a été sélectionné est indiqué par un *, comme ceci :

           staged     unstaged path
  1:       binary      nothing foo.png
* 2:     +403/-35        +1/-1 git-add--interactive.perl
Pour retirer une sélection, préfixez-la avec - comme ceci :

Update>> -2
Après sélection, répondez avec une ligne vide pour indexer le contenu des fichiers sélectionnés de l’arbre de travail.

revert
Ceci présente une interface d’utilisation similaire à update, et les informations indexées des chemins sélectionnés sont ramenées à la version HEAD. Restaurer des chemins nouveaux les rend non-suivis.

add untracked
Ceci présente une interface d’utilisation très similaire à update et revert et permet d’ajouter des chemins non-suivis à l’index.

patch
Ceci permet de choisir un chemin depuis une sélection similaire à status. Après le choix du chemin, la différence entre l’index et le fichier dans l’arbre de travail est présentée et il vous est demandé si vous souhaitez indexer chaque section de modification. Vous pouvez sélectionner une des options suivantes et taper Entrée :

y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
Après avoir décidé du devenir de chaque section, l’index est mis à jour avec les sections sélectionnées.

Vous pouvez vous éviter de taper Entrée ici, en mettant la variable de configuration interactive.singlekey à true.

diff
Ceci permet de faire une revue de ce qui sera validé (c’est une différence entre HEAD et index).

ÉDITER LES CORRECTIFS
Invoquer git add -e ou sélectionner e depuis le sélecteur interactif de sections ouvre un correctif dans votre éditeur ; après avoir quitté l’éditeur, le résultat est appliqué à l’index. Vous êtes libre de modifier en tout point le correctif, mais notez cependant que certaines modifications provoquent des résultats inattendus ou même créent des correctifs inapplicables. Si vous souhaitez abandonner complètement l’opération (c’est-à-dire ne rien ajouter à l’index), effacez toutes les lignes du correctif. La liste ci-dessous décrit des formes habituelles dans les correctifs et quelles opérations d’édition peuvent être réalisées.

contenu ajouté
Le contenu ajouté est représenté par des lignes commençant par un "+". Vous pouvez empêcher l’indexation de lignes ajoutées en les supprimant.

contenu supprimé
Le contenu supprimé est représenté par des lignes commençant par "-". Vous pouvez empêcher l’indexation de ces suppressions en convertissant le "-" en " " (espace).

contenu modifié
Le contenu modifié est représenté par des lignes "-" (supprimant l’ancien contenu) suivies de lignes "+" (ajoutant le nouveau contenu). Vous pouvez empêcher l’indexation de ces modifications en convertissant les lignes "-" en lignes " " et en supprimant les lignes "+". Méfiez-vous : ne modifier que la moitié de la paire de lignes a de fortes chances de créer des modifications inattendues dans l’index.

Il existe aussi des opérations plus complexes qui peuvent être réalisées. Méfiez-vous : quand le correctif n’est appliqué que dans l’index et pas dans l’arbre de travail, l’arbre de travail semblera « défaire » les modifications de l’index. Par exemple, l’introduction dans l’index d’une nouvelle ligne qui n’apparaît ni dans HEAD ni dans l’arbre de travail indexera la nouvelle ligne pour validation, mais cette ligne semblera être supprimée dans l’arbre de travail.

Évitez d’utiliser ces constructions, ou faites-le avec une extrême précaution.

suppression de contenu intact
Le contenu qui ne diffère pas entre l’index et l’arbre de travail peut être visible dans des lignes de contexte commençant par un " " (espace). Vous pouvez indexer l’élimination de lignes de contexte en convertissant l’espace en "-". Le fichier dans l’arbre de travail semblera ré-ajouter le contenu.

modification de contenu existant
On peut aussi modifier le contenu de lignes de contexte en indexant leur suppression (en convertissant " " en "-") et en ajoutant dessous une ligne "+" avec le nouveau contenu. On peut modifier des lignes "+" dans des ajouts ou des modifications de contenu. Dans tous les cas, la nouvelle modification indexée semblera être annulée dans l’arbre de travail.

contenu nouveau
Vous pouvez aussi ajouter du contenu nouveau qui n’existe pas dans le correctif ; ajoutez simplement des nouvelles lignes, chacune commençant par "+". L’ajout semblera annulé dans l’arbre de travail.

Il existe aussi quelques opérations à éviter complètement car celles-ci rendent le correctif inapplicable :

ajout de contexte (" ") ou lignes de suppression ("-")

suppression de contexte ou de lignes supprimées

modification de contenu de contexte ou de lignes supprimées


## git commit -m "message du commit"
Enregistrement d'une version des fichiers avec un message pour identifier les changements
## git push
envoie des fichiers sur la branche sur laquelle on se trouve
## git pull
Récupération des fichiers