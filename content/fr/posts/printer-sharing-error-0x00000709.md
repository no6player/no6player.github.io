---
title: "Comment corriger l'erreur 0x00000709 de l'imprimante partagée sous Windows"
date: 2026-04-29
description: "Vous obtenez l'erreur 0x00000709 lors de la connexion à une imprimante partagée sous Windows ? Ce guide explique comment résoudre le problème en ajoutant des informations d'identification Windows et en modifiant une valeur de registre."
tags: ["windows", "erreur 0x00000709", "imprimante partagée", "gestionnaire d'informations d'identification", "registre", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x00000709 — L'opération n'a pas pu être effectuée"
  caption: "L'erreur 0x00000709 apparaît quand Windows ne peut pas se connecter à une imprimante partagée sur le réseau"
  relative: false
---

## Description du problème

Lors de la tentative de connexion à une imprimante partagée sur le réseau, la connexion échoue avec le message d'erreur suivant :

> **L'opération n'a pas pu être effectuée (Erreur 0x00000709). Vérifiez le nom de l'imprimante et assurez-vous qu'elle est connectée au réseau.**

![Erreur 0x00000709 sous Windows](/images/printer-error-0x00000709/step1.png)

L'erreur 0x00000709 est une erreur courante de connexion à une imprimante partagée sous Windows. Elle est généralement causée par une mise à jour système, une mauvaise configuration du protocole RPC ou un problème d'authentification des informations d'identification. La correction consiste à ajouter des informations d'identification Windows et à modifier une valeur de registre.

---

## Solution

**1.** Ouvrez le **Panneau de configuration**, réglez l'affichage sur **Petites icônes**, puis cliquez sur **Gestionnaire d'informations d'identification**.

![Panneau de configuration — Gestionnaire d'informations d'identification](/images/printer-error-0x00000709/step2.png)

**2.** Sélectionnez **Informations d'identification Windows**, puis cliquez sur **Ajouter des informations d'identification Windows**.

![Gestionnaire d'informations d'identification — Ajouter des informations d'identification Windows](/images/printer-error-0x00000709/step3.png)

**3.** Dans le champ **Adresse Internet ou réseau**, saisissez l'adresse IP du PC hôte. Définissez le **Nom d'utilisateur** sur `guest` et laissez le champ **Mot de passe** vide. Cliquez sur **OK**.

![Ajouter des informations d'identification Windows — saisir l'IP hôte et le nom d'utilisateur invité](/images/printer-error-0x00000709/step4.png)

**4.** Appuyez sur **Win + R**, tapez `regedit` et appuyez sur Entrée pour ouvrir l'**Éditeur du Registre**.

![Boîte de dialogue Exécuter avec regedit](/images/printer-error-0x00000709/step5.png)

**5.** Accédez à la clé suivante (créez-la si elle n'existe pas) :

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Cliquez avec le bouton droit dans le volet de droite et créez une nouvelle **valeur DWORD (32 bits)**.

![Éditeur du Registre — Création d'une nouvelle valeur DWORD sous la clé RPC](/images/printer-error-0x00000709/step6.png)

**6.** Renommez la nouvelle valeur en `RpcUseNamedPipeProtocol` et définissez ses **données de valeur** sur `1`.

![Éditeur du Registre — RpcUseNamedPipeProtocol défini sur 1](/images/printer-error-0x00000709/step7.png)

**7.** Une fois toutes les étapes terminées, **redémarrez l'ordinateur** et essayez à nouveau de vous connecter à l'imprimante partagée.

---

## Résumé

L'erreur 0x00000709 lors de la connexion à une imprimante partagée est un problème courant sous Windows, généralement causé par une mise à jour système, une configuration du protocole RPC incorrecte ou un échec d'authentification. La correction comprend deux étapes :

- Ajouter l'adresse IP du PC hôte et le compte **guest** dans le **Gestionnaire d'informations d'identification Windows**
- Créer la valeur de registre **RpcUseNamedPipeProtocol** (`1`) sous `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Après le redémarrage, la connexion à l'imprimante partagée devrait fonctionner normalement.
