---
title: "Comment corriger l'erreur 0x00000040 de l'imprimante partagée sous Windows"
date: 2026-04-29
description: "Les clients Windows 7 reçoivent l'erreur 0x00000040 en se connectant à une imprimante partagée depuis Windows 11 ? Ce guide explique comment activer SMB 1.0 et modifier le registre pour résoudre le problème."
tags: ["windows", "erreur 0x00000040", "imprimante partagée", "windows 7", "windows 11", "registre", "SMB", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x00000040/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x00000040 — Le nom réseau spécifié n'est plus disponible"
  caption: "L'erreur 0x00000040 apparaît quand un client Windows 7 ne peut pas se connecter à une imprimante partagée depuis Windows 11"
  relative: false
---

## Description du problème

Lorsqu'un PC Windows 11 est utilisé comme hôte d'imprimante partagée, les clients Windows 7 qui tentent de s'y connecter reçoivent le message d'erreur suivant :

> **L'opération n'a pas pu être effectuée (Erreur 0x00000040). Le nom réseau spécifié n'est plus disponible.**

![Erreur 0x00000040 sous Windows](/images/printer-error-0x00000040/step1.png)

Cette erreur est généralement causée par un service SMB désactivé, un nom de partage expiré, une connexion réseau interrompue ou une incompatibilité de protocole RPC entre les deux systèmes d'exploitation. La correction consiste à activer SMB 1.0 et à modifier deux valeurs de registre sur l'hôte Windows 11.

---

## Solution

**1.** Ouvrez **Panneau de configuration → Programmes → Activer ou désactiver des fonctionnalités Windows** et activez **Prise en charge du partage de fichiers SMB 1.0/CIFS**.

![Activer SMB 1.0/CIFS dans les fonctionnalités Windows](/images/printer-error-0x00000040/step2.png)

**2.** Appuyez sur **Win + R**, tapez `regedit` et appuyez sur Entrée pour ouvrir l'**Éditeur du Registre**.

![Boîte de dialogue Exécuter avec regedit](/images/printer-error-0x00000040/step3.png)

**3.** Accédez à la clé suivante (créez-la si elle n'existe pas) :

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Cliquez avec le bouton droit dans le volet de droite et créez une nouvelle **valeur DWORD (32 bits)**.

![Éditeur du Registre — Création d'une nouvelle valeur DWORD sous la clé RPC](/images/printer-error-0x00000040/step4.png)

**4.** Renommez la nouvelle valeur en `RpcProtocols` et définissez ses **données de valeur** sur `7`.

![Éditeur du Registre — RpcProtocols défini sur 7](/images/printer-error-0x00000040/step5.png)

**5.** Cliquez à nouveau avec le bouton droit dans le volet de droite et créez une autre **valeur DWORD (32 bits)**. Renommez-la en `ForceKerberosForRpc` et définissez ses **données de valeur** sur `1`.

![Éditeur du Registre — ForceKerberosForRpc défini sur 1](/images/printer-error-0x00000040/step6.png)

**6.** Une fois toutes les étapes terminées, **redémarrez l'ordinateur** et essayez à nouveau de vous connecter à l'imprimante partagée.

---

## Résumé

L'erreur 0x00000040 lors de la connexion à une imprimante partagée est un problème courant dans les environnements mixtes Windows 7/Windows 11. Elle est généralement causée par un service SMB désactivé ou une incompatibilité de protocole RPC. La correction comprend deux étapes :

- Activer la **prise en charge du partage de fichiers SMB 1.0/CIFS** dans les fonctionnalités Windows
- Ajouter les valeurs de registre **RpcProtocols** (`7`) et **ForceKerberosForRpc** (`1`) sous `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Après le redémarrage, la connexion à l'imprimante partagée devrait fonctionner normalement.
