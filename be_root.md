# Comment être root ?

## Introduction

> Un grand pouvoir implique de grandes reponsabilités.

En effet, être root sur le dump ce n'est pas rien.

Cependant, si vous savez ce que vous faites, que vous promettez de ne pas utiliser 'sudo' et 'su' à tout bout de champ ou devant des commandes que vous ne comprenez pas, je vous invite à lire la suite du tuto.

## Démarrer en mode single-user

Tout d'abord, il faut démarrer sur le dump en mode single-user, c'est à dire que vous pourrez passer root dès le démarrage.

Pour cela, quand vous êtes sur GRUB, appuyez sur 'e'.

Rajoutez `init=/bin/sh` à la fin de la ligne commençant par `linux`, puis appuyez sur <key>F10</key> pour démarrer.

Au bout d'un moment, plus rien ne va s'afficher. Appuyez sur Entrée.

Vous arrivez sur un shell, tapez `su -`, puis `loadkeys fr-pc`.

Vous utilisez maintenant un shell en super-utilisateur.

Tapez cette commande afin de remonter le système de fichiers en lecture/écriture:

```shell
mount -o rw,remount /
```

## Changer le mot de passe root

Pour pouvoir changer le mot de passe, il faudra d'abord désactiver un service systemd qui change le mot de passe tout seul.

Ce service s'appelle `salt-minion`, et pour le désactiver il suffit de taper cette commande:

```shell
rm /etc/systemd/system/multi-user.target.wants/salt-minion.service
```

Une fois que vous avez fait ça, vous n'avez plus qu'à changer le mot de passe avec la commande `passwd`.

Pour finir, tapez deux fois `exit`, puis redémarrez de force votre ordinateur.

Et voilà, vous êtes root !
