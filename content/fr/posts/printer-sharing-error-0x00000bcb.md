---
title: "Comment corriger l'erreur 0x00000bcb de l'imprimante partagée sous Windows 10 et 11"
date: 2026-05-11
description: "Vous obtenez l'erreur 0x00000bcb lors de la connexion à une imprimante partagée sous Windows 10 ou 11 ? Ce guide couvre toutes les causes et solutions — modification du registre, nettoyage des pilotes et réinitialisation du spouleur."
tags: ["windows", "erreur 0x00000bcb", "imprimante partagée", "registre", "point and print", "pilote imprimante", "windows 10", "windows 11", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x00000bcb/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x00000bcb — Le pilote d'imprimante spécifié n'a pas été trouvé sur ce système"
  caption: "L'erreur 0x00000bcb apparaît quand Windows ne peut pas installer le pilote de l'imprimante partagée en raison des restrictions de politique RPC"
  relative: false
---

# Comment corriger l'erreur 0x00000bcb de l'imprimante partagée sous Windows 10 et 11

L'erreur 0x00000bcb est une défaillance courante de connexion à une imprimante partagée sous Windows 10 et Windows 11. Windows affiche un message indiquant que le pilote d'imprimante spécifié n'a pas été trouvé et doit être téléchargé — mais le téléchargement échoue. Cet article explique précisément pourquoi l'erreur 0x00000bcb se produit et comment la corriger étape par étape.

---

## Qu'est-ce que l'erreur 0x00000bcb ?

Lorsque vous tentez de vous connecter à une imprimante partagée sur votre réseau local, Windows affiche :

> **L'opération n'a pas pu être effectuée (Erreur 0x00000bcb). Le pilote d'imprimante spécifié n'a pas été trouvé sur ce système et doit être téléchargé.**

![Erreur 0x00000bcb sous Windows](/images/printer-error-0x00000bcb/step1.png)

Windows tente automatiquement d'installer le pilote d'imprimante depuis le PC hôte lors de la connexion à une imprimante partagée. L'erreur 0x00000bcb signifie que cette installation automatique a été bloquée — le plus souvent par une politique de sécurité introduite dans les mises à jour récentes de Windows.

---

## Pourquoi l'erreur 0x00000bcb se produit-elle ?

Il existe cinq causes courantes :

1. **La mise à jour Windows a renforcé la sécurité RPC** — Les correctifs de sécurité de Microsoft ont restreint l'installation des pilotes Point and Print aux administrateurs uniquement, bloquant le téléchargement automatique depuis les imprimantes partagées.
2. **Pilote d'imprimante manquant ou incompatible** — Le PC client ne dispose pas du pilote correct, ou la version installée ne correspond pas à celle du PC hôte.
3. **Erreur du service Spouleur d'impression ou cache d'impression corrompu** — Un travail d'impression bloqué peut empêcher l'installation de nouveaux pilotes.
4. **Droits LAN insuffisants** — Le PC client ne dispose pas des droits nécessaires pour récupérer et installer les pilotes depuis l'hôte.
5. **Pare-feu ou logiciel de sécurité bloquant la communication partagée** — Des règles antivirus tierces ou du Pare-feu Windows peuvent bloquer le trafic de partage d'imprimante.

---

## Solution

### Étape 1 — Modifier le registre (restriction Point and Print)

**1.** Appuyez sur **Win + R**, tapez `regedit` et appuyez sur Entrée pour ouvrir l'**Éditeur du Registre**.

![Boîte de dialogue Exécuter avec regedit](/images/printer-error-0x00000bcb/step2.png)

**2.** Accédez au chemin suivant. Si une partie du chemin n'existe pas, créez les sous-clés manquantes manuellement : clic droit sur la clé parente → **Nouveau → Clé**.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint
```

![Éditeur du Registre — navigation vers la clé PointAndPrint](/images/printer-error-0x00000bcb/step3.png)

**3.** Dans le volet de droite, faites un clic droit et sélectionnez **Nouveau → Valeur DWORD (32 bits)**. Nommez-la `RestrictDriverInstallationToAdministrators`.

![Éditeur du Registre — création du DWORD RestrictDriverInstallationToAdministrators](/images/printer-error-0x00000bcb/step4.png)

**4.** Double-cliquez sur la nouvelle valeur et définissez ses **données de valeur** sur `0`. Cliquez sur **OK**.

![Éditeur du Registre — RestrictDriverInstallationToAdministrators défini sur 0](/images/printer-error-0x00000bcb/step5.png)

### Étape 2 — Supprimer l'ancien pilote d'imprimante

**5.** Ouvrez **Panneau de configuration → Matériel et audio → Périphériques et imprimantes**. Cliquez sur **Propriétés du serveur d'impression** dans la barre de menu supérieure, puis accédez à l'onglet **Pilotes**. Sélectionnez l'ancien pilote de l'imprimante partagée et cliquez sur **Supprimer**.

![Propriétés du serveur d'impression — onglet Pilotes](/images/printer-error-0x00000bcb/step6.png)

### Étape 3 — Redémarrer et reconnecter

**6.** Redémarrez l'ordinateur. Après le redémarrage, essayez d'ajouter à nouveau l'imprimante partagée. Windows devrait maintenant pouvoir télécharger et installer le pilote sans afficher l'erreur 0x00000bcb.

---

## Foire aux questions

**Q : L'erreur 0x00000bcb concerne-t-elle uniquement Windows 10 ou aussi Windows 11 ?**
Les deux. L'erreur 0x00000bcb apparaît sur Windows 10 et Windows 11, car la cause sous-jacente — une politique de sécurité Point and Print renforcée — a été appliquée aux deux systèmes via Windows Update.

**Q : Est-il sûr de définir RestrictDriverInstallationToAdministrators sur 0 ?**
Oui, dans un environnement LAN domestique ou de bureau. Cette valeur de registre ne fait qu'assouplir la permission d'installation des pilotes pour les imprimantes partagées sur votre réseau local. Elle ne désactive aucune autre fonction de sécurité.

**Q : J'ai suivi toutes les étapes mais l'erreur 0x00000bcb persiste. Que puis-je faire d'autre ?**
Installez manuellement le pilote d'imprimante sur le PC client (téléchargez-le depuis le site du fabricant), puis connectez l'imprimante partagée. Cela contourne entièrement le téléchargement automatique du pilote.

**Q : L'erreur 0x00000bcb est-elle la même que l'erreur 0x00000709 ?**
Non, mais les causes sont très similaires — toutes deux impliquent des restrictions de politique RPC et des problèmes de permissions de pilotes. Si les étapes ci-dessus ne résolvent pas 0x00000bcb, consultez le [guide de correction de 0x00000709](/fr/posts/printer-sharing-error-0x00000709/).

---

## Résumé

L'erreur 0x00000bcb lors de la connexion à une imprimante partagée sous Windows 10 ou Windows 11 est principalement causée par une politique de sécurité Windows Update qui restreint l'installation automatique des pilotes d'imprimante. La correction en trois étapes :

1. Définir `RestrictDriverInstallationToAdministrators` = `0` dans le registre sous `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint`
2. Supprimer l'ancien pilote d'imprimante dans **Propriétés du serveur d'impression → Pilotes**
3. Redémarrer l'ordinateur et reconnecter l'imprimante partagée

---

## Articles connexes

- [Corriger l'erreur d'imprimante partagée 0x00000709 sous Windows](/fr/posts/printer-sharing-error-0x00000709/)
- [Corriger l'erreur d'imprimante partagée 0x0000011b sous Windows](/fr/posts/printer-sharing-error-0x0000011b/)
- [Corriger l'erreur d'imprimante partagée 0x000003e3 sous Windows 10](/fr/posts/printer-sharing-error-0x000003e3/)
