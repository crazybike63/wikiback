---
title: Homeassistant
description: purge logs
published: true
date: 2025-09-08T09:16:59.894Z
tags: 
editor: markdown
dateCreated: 2025-09-08T09:09:00.096Z
---

# Homeassistant
## But

Il est nécessaire de purger les logs car ils saturent le disque de la VM.

## Étapes
1. Se connecter à l'interface de la VM en ligne de commande (ou en SSH ou à travers proxmox)
```
ha>login
```
> Le clavier est en anglais QWERTY
{.is-warning}

1. Vérifier le taux de remplissage des disques
```
df -f
```

![df.png](/df.png)
Aucune partition ne doit être à 100%

Dans le cas des logs saturant la VM, la partition overlay est à 100%

1. Pour vérifier s'il s'agit des logs il faut se rendre dans le répertoire de homeassistant:
```
cd /mnt/data/supervisor/homeassistant/
````
1. on regarde la taille des fichiers:
```
ls -lh
```
![ls.png](/ls.png)
1. on supprime les fichiers qui posent problème, ici home-assistant.log.1
```
rm home-assistant.log.1
```
> La commande rm est puissante et irreversible! Il faut réfléchir plusieurs fois avant de lancer cette commande.
{.is-warning}

1. On lance df -h pour regarder la taille récupérer et on va sur homeassistant pour vérifier que le service redémarre.

> On ne redémarre pas la VM sans avoir vérifier que le service fontionne car on est actuellement connecté sur un système sur lequel on a la main. Si une erreur a été commise on risque de perdre complétement l'accès au système.
{.is-warning}

## TODO
Limiter la taille d'écriture des logs, 5G semble amplement suffisant.
