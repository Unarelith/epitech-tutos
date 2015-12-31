# Comment utiliser git correctement ?

## Faire des bons commits

### Nommer ses commits

La première chose à savoir faire, c'est de bien nommer ses commits.

Par exemple, si on ajoute des fichiers error.h et error.c, comment le commit pourrait-il se nommer ?

De bons noms:

> Fichiers error.c et error.h ajoutés.

> Error handling: OK.

> [error] Added.

De mauvais noms:

> error

> lol

Et si on rajoute player_movement.c et player_movement.h:

> Error handling: OK. Player movement: OK.

> [error] Added. [player_movement] Added.

> [error|player_movement] Added.

### Le contenu des commits

Essayez au maximum de faire des petits commits.

Par exemple, vous venez de terminer une fonctionnalité ou une partie de celle-ci, faites un commit.

Ces conseils vous permetteront de mieux vous retrouver dans vos commits et d'éviter des pertes de données inutiles.

### Réparer un commit foireux

Vous avez fait une faute de frappe dans le nom ou ajouté des fichiers que vous ne vouliez pas ajouter ? Il est possible de régler ça sans avoir à faire un autre commit.

Si vous voulez supprimer un fichier uniquement dans l'index de git et pas sur votre disque, utiliser `git rm --cached`.

Si vous avez ajouté des fichiers, pensez à utiliser `git add`.

La commande `git commit --amend` vous permet d'éditer le dernier commit dans un éditeur de texte.

Attention, cela ne marchera pas avec emacs, si vous l'utilisez rajoutez `EDITOR='emacs -nw'` devant la commande.

Si vous voulez annuler l'édition, supprimez le message de commit.

Vous n'avez plus qu'à sauvegarder et quitter, les modifications seront prises en compte.

Si vous avez déjà push le commit, vous allez devoir forcer le push: `git push origin +master`. Il y a cependant un problème avec cette commande, c'est que si des utilisateurs ont un clone avec le commit foireux, ils ne pourront pas pull normalement.

## Vérifier ce que je fais

### Afficher l’état actuel du dépot

Il est très important de se servir de `git status` pour avoir une vision
globale de l’état du dépot. Cette commande affiche ce qui est prêt à être
commit, ce qui n’est pas prêt d’être commit, les fichiers non suivis et
beacoup d’autres informations. À utiliser sans modération.

### Afficher les modifications faites

`git diff` est une merveilleuse commande permettant de comparer n’importe
quel commit à n’importe quel autre commit. Sans paramètre, il affiche les
modifications entre l’état actuel du dépot et le dernier commit.

Je vous invite à consulter [la documentation][git diff] pour plus de détails.

## Les remotes

### Qu'est ce qu'une remote ?

Une remote contient une ou plusieurs adresses que git va utiliser pour pull/push.

Pour voir la liste de vos remotes utilisez `git remote -v`.

### Les commandes utiles

Pour ajouter une remote:

```shell
git remote add [remote name] [url]
```

Pour supprimer une remote:

```shell
git remote remove [remote name]
```

Pour modifier l'url d'une remote:

```shell
git remote set-url [remote-name] [new url]
```

Pour push sur deux url à la fois:

```shell
git remote set-url --add --push [remote_name] [url 1]
git remote set-url --add --push [remote_name] [url 2]
```

## Obtenir de l’aide

Internet ne manque pas d’informations sur Git. Vous pouvez consulter [la
documentation sur le site officiel][gitscm], ou faire une recherche
[Google][google].

[git diff]: https://git-scm.com/docs/git-diff
[gitscm]: https://git-scm.com/doc
[google]: https://google.com/
