---
title: "[SAM] Dumping des informations d'identification Microsoft"
description: sam.png
tags: ["Dans cet article je vous présente le fonctionnement d'un fichier Security Account Manage (SAM), et de vous montrer comment dump un fichier SAM grâce au fichier SYSTEM sous une machine Windows."]
---

![Flower](../sam.png)

Introduction
----
Dans ce cours à but éducatif nous allons voir comment dumper et cracker des mots de passes Microsoft grâce au système de fichier `Security Account Manager` (SAM) en français `Gestionnaire de compte de sécurité`.

SAM
----
Alors commençons par la base des bases c'est quoi `SAM` concrètement ? <br />

La SAM (Security Account Manager ou gestionnaire des comptes de sécurité) est la base de données des comptes locaux sur Windows Server 2003, Windows XP, Windows 2000. C'est l'un des composants de la base de registre. Elle contient les mots de passe locaux.

La SAM est stockée physiquement dans le fichier `%SystemRoot%\system32\Config\SAM`. C'est un fichier de ruche inclus dans `HKEY_LOCAL_MACHINE`, lui-même inclus dans la base de registre. Comme vous pouvez le voir ils sont stockés dans le registre.

![Flower](https://image.noelshack.com/fichiers/2019/21/7/1558894108-screenshot-5.png)

Nous allons commencer par copier les fichiers `SAM` et `SYSTEM` via la commande `reg`. La commande `reg` est une commande Microsoft. Et la commande `reg` est un moyen pour gérer le registre de Windows.

![Flower](https://image.noelshack.com/fichiers/2019/21/7/1558894619-screenshot-2.png)

Le fichier `SAM` et `SYSTEM` ont été copier avec succès donc maintenant nous allons aller sous Linux pour installer quelques paquets pour dump les deux fichiers en question.

IMPACKET
----
Donc avant de dump le fichier, on va s'assurer que `impacket` est installer dans notre machine, voici le lien pour l'installer https://github.com/SecureAuthCorp/impacket.

Impacket est un ensemble de classes Python permettant de travailler avec des protocoles réseau. Impacket se concentre sur la fourniture d'un accès programmatique de bas niveau aux paquets et à certains protocoles (par exemple, SMB1-3 et MSRPC), la mise en œuvre du protocole elle-même.

Pour l'installation de `impacket` :
![Flower](https://image.noelshack.com/fichiers/2019/21/7/1558905335-screenshot-1.png)

Il faut d'abord spécifier le fichier `SYSTEM` et enfin le fichier `SAM` pour dumper les fichiers.

![Flower](https://image.noelshack.com/fichiers/2019/21/7/1558896664-capture-du-2019-05-26-20-50-31.png)

Le dumping à fonctionner avec succès donc maintenant nous allons passer au crack du hash avec john. Il faut avant tout identifier le hash cela ressemble grandement à du `NT Lan Manager` (NTLM). Vous pouvez très bien ne pas identifier le hash pour cracker le hash ça reste facultatif.. le programme john va chercher par lui même pour identifier le hash en question. Nous allons avant tout mettre les deux hashs dans un fichier.

    $ echo aad3b435b51404eeaad3b435b51404ee > hashs.log
    $ echo 26112010952d963c8dc4217daec986d9 >> hashs.log

