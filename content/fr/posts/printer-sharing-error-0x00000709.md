---
title: "Comment corriger l'erreur 0x00000709 de l'imprimante partagée sous Windows"
date: 2026-04-29
description: "Vous obtenez l'erreur 0x00000709 lors de la connexion à une imprimante partagée sous Windows 10 ou Windows 11 ? Ce guide étape par étape résout le problème en ajoutant des informations d'identification Windows et en modifiant une valeur de registre."
tags: ["windows", "erreur 0x00000709", "imprimante partagée", "gestionnaire d'informations d'identification", "registre", "windows 10", "windows 11", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x00000709 — L'opération n'a pas pu être effectuée"
  caption: "L'erreur 0x00000709 apparaît quand Windows ne peut pas se connecter à une imprimante partagée sur le réseau"
  relative: false
---

# Comment corriger l'erreur 0x00000709 de l'imprimante partagée sous Windows

L'erreur 0x00000709 est l'une des erreurs d'imprimante partagée les plus courantes sous Windows 10 et Windows 11. Elle empêche la connexion à une imprimante partagée sur le réseau et peut apparaître soudainement — souvent après une mise à jour Windows. La bonne nouvelle : ce problème se résout en quelques minutes sans logiciel tiers.

---

## Qu'est-ce que l'erreur 0x00000709 ?

Lorsque vous tentez de vous connecter à une imprimante partagée sur votre réseau local, Windows peut afficher :

> **L'opération n'a pas pu être effectuée (Erreur 0x00000709). Vérifiez le nom de l'imprimante et assurez-vous qu'elle est connectée au réseau.**

![Erreur 0x00000709 sous Windows](/images/printer-error-0x00000709/step1.png)

L'erreur 0x00000709 est une erreur de permission ou d'authentification. Windows tente de communiquer avec l'hôte de l'imprimante via le réseau, mais est bloqué en raison d'informations d'identification manquantes ou d'une restriction du protocole RPC.

---

## Pourquoi l'erreur 0x00000709 se produit-elle ?

Il existe trois causes principales :

- **Informations d'identification réseau manquantes** — Windows ne dispose pas d'un nom d'utilisateur/mot de passe enregistré pour la machine hôte de l'imprimante, ce qui entraîne un refus d'accès.
- **Restriction du protocole RPC** — Après certaines mises à jour de sécurité Windows, la méthode de communication RPC par défaut entre les ordinateurs a changé, interrompant les connexions aux imprimantes partagées.
- **Compte Invité non reconnu** — Le compte Invité de l'hôte n'est pas approuvé par la machine cliente, ce qui entraîne silencieusement un échec d'authentification.

Cette erreur apparaît fréquemment sur les clients **Windows 10** et **Windows 11** se connectant à une imprimante partagée depuis un autre PC.

---

## Solution

La correction comporte deux parties : d'abord ajouter les informations d'identification de l'hôte au Gestionnaire d'informations d'identification Windows, puis ajuster une valeur de registre RPC sur la machine cliente.

### Partie 1 — Ajouter les informations d'identification Windows

**1.** Ouvrez le **Panneau de configuration**, réglez l'affichage sur **Petites icônes**, puis cliquez sur **Gestionnaire d'informations d'identification**.

![Panneau de configuration — Gestionnaire d'informations d'identification](/images/printer-error-0x00000709/step2.png)

**2.** Sélectionnez **Informations d'identification Windows**, puis cliquez sur **Ajouter des informations d'identification Windows**.

![Gestionnaire d'informations d'identification — Ajouter des informations d'identification Windows](/images/printer-error-0x00000709/step3.png)

**3.** Dans le champ **Adresse Internet ou réseau**, saisissez l'adresse IP du PC hôte (ex. `192.168.1.100`). Définissez le **Nom d'utilisateur** sur `guest` et laissez le champ **Mot de passe** vide. Cliquez sur **OK**.

![Ajouter des informations d'identification Windows — saisir l'IP hôte et le nom d'utilisateur invité](/images/printer-error-0x00000709/step4.png)

> **Conseil :** Vous pouvez trouver l'adresse IP du PC hôte en exécutant `ipconfig` dans une invite de commandes sur ce PC.

### Partie 2 — Modifier le registre

**4.** Appuyez sur **Win + R**, tapez `regedit` et appuyez sur Entrée pour ouvrir l'**Éditeur du Registre**.

![Boîte de dialogue Exécuter avec regedit](/images/printer-error-0x00000709/step5.png)

**5.** Accédez à la clé suivante. Si la sous-clé `RPC` n'existe pas, créez-la manuellement sous `Printers` :

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Cliquez avec le bouton droit dans le volet de droite et sélectionnez **Nouveau → Valeur DWORD (32 bits)**.

![Éditeur du Registre — Création d'une nouvelle valeur DWORD sous la clé RPC](/images/printer-error-0x00000709/step6.png)

**6.** Renommez la nouvelle valeur en `RpcUseNamedPipeProtocol` et définissez ses **données de valeur** sur `1`. Cliquez sur **OK**.

![Éditeur du Registre — RpcUseNamedPipeProtocol défini sur 1](/images/printer-error-0x00000709/step7.png)

**7.** Une fois toutes les étapes terminées, **redémarrez l'ordinateur** et essayez à nouveau de vous connecter à l'imprimante partagée. L'erreur 0x00000709 ne devrait plus apparaître.

---

## Foire aux questions

**Q : Cette solution fonctionne-t-elle sous Windows 10 et Windows 11 ?**
Oui. L'erreur 0x00000709 se produit sur Windows 10 et Windows 11. Les étapes décrites s'appliquent aux deux versions.

**Q : Dois-je effectuer des modifications sur le PC hôte de l'imprimante ?**
En général non. La correction s'applique sur le PC **client**. Assurez-vous toutefois que l'imprimante est correctement partagée sur l'hôte et que le compte Invité y est activé.

**Q : L'erreur persiste malgré toutes les étapes. Que puis-je faire ?**
- Vérifiez que l'adresse IP du PC hôte est correcte dans le Gestionnaire d'informations d'identification.
- Assurez-vous que le pare-feu du PC hôte ne bloque pas le partage de fichiers et d'imprimantes.
- Essayez de vous connecter en utilisant le nom du PC hôte plutôt que son adresse IP.
- Désinstallez et réinstallez le pilote d'imprimante sur le PC client.

---

## Résumé

L'erreur 0x00000709 lors de la connexion à une imprimante partagée sous Windows 10 ou Windows 11 est généralement causée par des informations d'identification réseau manquantes ou une restriction du protocole RPC. La correction en deux étapes :

1. Ajouter l'adresse IP de l'hôte et le compte **guest** au **Gestionnaire d'informations d'identification Windows**
2. Créer la valeur de registre **RpcUseNamedPipeProtocol** = `1` sous `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

---

## Articles connexes

- [Corriger l'erreur d'imprimante partagée 0x0000011b sous Windows](/fr/posts/printer-sharing-error-0x0000011b/)
- [Corriger l'erreur d'imprimante partagée 0x000003e3 sous Windows 10](/fr/posts/printer-sharing-error-0x000003e3/)
- [Corriger l'erreur d'imprimante partagée 0x00000040 sous Windows](/fr/posts/printer-sharing-error-0x00000040/)
