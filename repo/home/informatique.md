---
title: Accès ssh proxmox
description: accès de secours sans VPN
published: true
date: 2024-08-21T10:19:41.833Z
tags: 
editor: markdown
dateCreated: 2023-01-15T09:28:39.242Z
---

# But
Accès de secours en ipv6 sur le proxmox en ssh
# Création de la clé publique/privée
1. depuis powershell
`ssh-keygen -b 4096`
1. valider toutes les options par défaut
ceci va créer une clé privée dans le répertoire `C:\Users\christophe\.ssh\` le fichier id_rsa (clé privée) et id_rsa_pub (clé publique)

> Seule la clé publique doit être diffusée!
{.is-warning}

1. on entre dans le répertoire `C:\Users\christophe\.ssh\` (commande cd)
on affiche la clé publique
`cat id_rsa_pub`
1. ça affiche dans powershell: `ssh-rsa blahblahblah christophe@pc
`
# Intégration de la clé publique
on copie le résultat du fichier à la fin de `.ssh/authorized_keys` sur les appareils qui ont un accès ssh

# Connexion
1. depuis powershell
`ssh root@2a01:cb14:855a:1b00::beef`

on accède directement depuis l'extérieur à l'adresse ipv6 du serveur proxmox.
C'est possible car la livebox dans Parefeu permet l'accès sur le port 22 à cette adresse. Elle est fixe. Si on veut y accéder avec un nom de domaine il faut créer une nouvelle entrée dans duckdns et lui donner cette adresse ipv6. Par exemple crazybikeproxmox et on y accèdera avec ssh root@crazybikeproxmox.duckdns.org