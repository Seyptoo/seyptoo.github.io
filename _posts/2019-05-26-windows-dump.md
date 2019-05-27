---
title: "[SAM] Dumping des informations d'identification Microsoft"
description: sam.png
tags: ["Dans cet article je vous présente le fonctionnement d'un fichier Security Account Manage (SAM), et de vous montrer comment dump un fichier SAM grâce au fichier SYSTEM sous une machine Windows."]
---

![Flower](../sam.png)

Introduction
----
Dans ce cours à but éducatif nous allons voir comment dumper et casser des mots de passe Microsoft grâce au système de fichier `Security Account Manager` (SAM) ou en français `Gestionnaire de compte de sécurité`.

SAM
----
Alors commençons par la base des bases c'est quoi `SAM` concrètement ? <br />

La SAM (Security Account Manager ou gestionnaire des comptes de sécurité) est la base de données des comptes locaux sur Windows Server 2003, Windows XP, Windows 2000. C'est l'un des composants de la base de registre. Elle contient les mots de passe locaux.

La SAM est stockée physiquement dans le fichier `%SystemRoot%\system32\Config\SAM`. C'est un fichier de ruche inclus dans `HKEY_LOCAL_MACHINE`, lui-même inclus dans la base de registre. Si vous regardez bien, nous voyons très bien que le fichier `SAM` et `SYSTEM` sont dans le registre de Windows.

![Flower](https://image.noelshack.com/fichiers/2019/21/7/1558894108-screenshot-5.png)

Nous allons commencer par copier le fichier `SAM` et `SYSTEM` via la commande `reg`. La commande `reg` est une commande Microsoft et le but du programme est de gérer le registre de Windows (Très utile pour les personnes qui n'aiment pas le GUI).

(Documentation de `REG`) : ![REG](https://windows.developpez.com/cours/ligne-commande/?page=page_17)

![Flower](https://image.noelshack.com/fichiers/2019/21/7/1558894619-screenshot-2.png)

Le fichier `SAM` et `SYSTEM` ont été copier avec succès donc maintenant nous allons installer quelques paquets pour dump les deux fichiers en question.

IMPACKET
----
Donc avant de dump le fichier, on va s'assurer que `impacket` est installer dans notre machine, voici le lien pour l'installer [impacket](https://github.com/SecureAuthCorp/impacket)

Impacket est un ensemble de classes Python permettant de travailler avec des protocoles réseau. Impacket se concentre sur la fourniture d'un accès programmatique de bas niveau aux paquets et à certains protocoles (par exemple, SMB1-3 et MSRPC), la mise en œuvre du protocole elle-même.

Pour l'installation de `impacket` : <br />

![Flower](https://image.noelshack.com/fichiers/2019/21/7/1558905335-screenshot-1.png)

Donc pour dump le fichier en question vous allez dans le dossier `examples` dans le dossier `impacket`. Et ensuite d'exécuter le fichier `secretsdump.py`.

> -sam    : Spécifier le fichier `SAM`. <br />
> -system : Spécifier le fichier `SYSTEM`. <br />
> LOCAL   : Si vous voulez analyser les fichiers locaux.<br />

![Flower](https://image.noelshack.com/fichiers/2019/21/7/1558905838-screenshot-2.png)

Le dumping à fonctionner avec succès donc maintenant nous allons passer au casse du HASH avec john. Il faut avant tout identifier le HASH cela ressemble grandement à du `NT Lan Manager` (NTLM). 

Vous pouvez très bien ne pas identifier le HASH pour casser le HASH ça reste facultatif.. Le programme john va chercher par lui même pour identifier le HASH en question (Avantage par rapport à `hashcat`).

