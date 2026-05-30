---
title: "Comment corriger l'erreur d'imprimante partagée 0x0000007c sur Windows 10 et 11"
date: 2026-05-30
description: "L'erreur 0x0000007c apparaît lors de la connexion à une imprimante partagée sous Windows 10 ou 11 ? Ce guide couvre toutes les causes et solutions — nettoyage des pilotes, réinitialisation du spouleur d'impression, protocole SMB 1.0 et ajout de pilotes bidirectionnels sur l'hôte."
tags: ["windows", "error 0x0000007c", "imprimante partagée", "pilote d'imprimante", "SMB", "spouleur d'impression", "windows 10", "windows 11", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x0000007c/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x0000007c — L'opération n'a pas pu être effectuée"
  caption: "L'erreur 0x0000007c apparaît lorsque le client ne peut pas charger le pilote d'imprimante distant en raison d'une incompatibilité de pilote ou de protocole SMB"
  relative: false
---

# Comment corriger l'erreur d'imprimante partagée 0x0000007c sur Windows 10 et 11

L'erreur 0x0000007c — « L'opération n'a pas pu être effectuée » — est une erreur courante liée aux imprimantes partagées sur Windows 10 et Windows 11. Elle survient lorsque le PC client se connecte à une imprimante partagée sur le réseau local et que Windows ne parvient pas à charger le pilote d'imprimante distant. Il s'agit d'un problème de **compatibilité des pilotes et du protocole SMB**, et non d'une défaillance matérielle ou d'une panne réseau. Ce guide explique toutes les causes et détaille chaque solution dans l'ordre.

---

## Qu'est-ce que l'erreur 0x0000007c ?

Lors de la connexion à une imprimante partagée sur le réseau local, Windows affiche :

> **L'opération n'a pas pu être effectuée (Erreur 0x0000007c).**

![Boîte de dialogue d'erreur Windows affichant l'erreur 0x0000007c — L'opération n'a pas pu être effectuée](/images/printer-error-0x0000007c/step1.png)

**Les symptômes typiques incluent :**
- L'erreur apparaît immédiatement lors de la tentative d'ajout d'une imprimante partagée sur le réseau local
- Les partages de fichiers réseau sur le même PC hôte sont accessibles — seule l'imprimante échoue
- Les anciens PC se connectent sans problème à l'imprimante partagée ; seuls les nouveaux PC Windows 10/11 obtiennent l'erreur
- La suppression et le rajout de l'imprimante ne résolvent pas le problème

---

## Pourquoi l'erreur 0x0000007c se produit-elle ?

Il existe six causes courantes :

1. **Incompatibilité du pilote distant** — La version ou l'architecture du pilote d'imprimante (32 bits contre 64 bits) sur le PC hôte ne correspond pas à ce que le PC client requiert. Le client rejette le pilote transmis comme non valide.
2. **Incompatibilité de la version du protocole SMB** — Windows 10/11 utilise par défaut SMB 3.0, tandis que les anciens PC hôtes ou les pilotes hérités dépendent de SMB 1.0. Cette incompatibilité de protocole entraîne l'échec de la validation du transfert de pilote.
3. **Blocage par la vérification de la signature du pilote** — Les nouvelles versions de Windows appliquent strictement les exigences de signature numérique des pilotes. Les anciens pilotes d'imprimante partagée sans signature valide sont bloqués au chargement.
4. **Conflit de pilote résiduel sur le client** — Un pilote précédemment installé pour le même modèle d'imprimante a laissé des fichiers résiduels qui empêchent le chargement du nouveau pilote distant.
5. **Corruption du cache du spouleur d'impression** — Des données corrompues dans le cache du spouleur empêchent le service de recevoir et d'installer le pilote partagé distant.
6. **Pilotes bidirectionnels manquants sur le PC hôte** — L'hôte n'a installé qu'un pilote pour sa propre version de Windows et n'a pas ajouté de package de pilotes correspondant à l'architecture du PC client.

---

## Solution

Suivez les étapes ci-dessous dans l'ordre. La plupart des cas sont résolus par les étapes 1 et 2.

### Étape 1 — Vider le cache d'impression et redémarrer le spouleur d'impression

**1.** Appuyez sur **Win + R**, tapez `services.msc` et appuyez sur Entrée. Recherchez **Spouleur d'impression** dans la liste, faites un clic droit dessus et sélectionnez **Arrêter**.

![Fenêtre Services — arrêt du service Spouleur d'impression](/images/printer-error-0x0000007c/step2.png)

**2.** Appuyez sur **Win + R**, tapez `C:\Windows\System32\spool\PRINTERS` et appuyez sur Entrée. Supprimez **tous les fichiers** dans le dossier (ne supprimez pas le dossier lui-même). Revenez ensuite aux Services et redémarrez le **Spouleur d'impression**, en définissant le **Type de démarrage** sur **Automatique**.

![Explorateur de fichiers — suppression de tous les fichiers dans le dossier du spouleur PRINTERS](/images/printer-error-0x0000007c/step3.png)

---

### Étape 2 — Supprimer complètement l'ancien pilote du PC client

**1.** Ouvrez **Panneau de configuration → Périphériques et imprimantes**. Faites un clic droit sur chaque imprimante défaillante et sélectionnez **Supprimer le périphérique**.

**2.** Appuyez sur **Win + R**, tapez `printui /s /t2` et appuyez sur Entrée pour ouvrir les **Propriétés du serveur d'impression**. Accédez à l'onglet **Pilotes**, sélectionnez le pilote d'imprimante correspondant, cliquez sur **Supprimer** et choisissez **Supprimer le pilote et le package de pilotes** pour effacer complètement tous les fichiers du pilote.

![Propriétés du serveur d'impression — suppression complète du pilote et du package de pilotes](/images/printer-error-0x0000007c/step4.png)

**3.** Redémarrez l'ordinateur. Téléchargez le pilote complet et correct depuis le site Web du fabricant et installez-le manuellement avant de vous reconnecter à l'imprimante partagée.

---

### Étape 3 — Activer le protocole SMB 1.0/CIFS

Si le PC hôte exécute une ancienne version de Windows ou utilise des packages de pilotes plus anciens qui dépendent de SMB 1.0 :

**1.** Ouvrez **Panneau de configuration → Programmes → Activer ou désactiver des fonctionnalités Windows**.

**2.** Faites défiler vers le bas et cochez **Prise en charge du partage de fichiers SMB 1.0/CIFS**. Cliquez sur **OK** et redémarrez l'ordinateur.

![Fonctionnalités Windows — activation de la prise en charge du partage de fichiers SMB 1.0/CIFS](/images/printer-error-0x0000007c/step5.png)

> **Note de sécurité :** SMB 1.0 présente des vulnérabilités connues. Activez-le uniquement comme mesure temporaire et désactivez-le une fois le problème d'imprimante résolu, ou mettez à jour le pilote du PC hôte vers une version compatible avec SMB 2.0/3.0.

---

### Étape 4 — Ajouter des pilotes bidirectionnels sur le PC hôte

Si le PC client a une architecture Windows différente (32 bits contre 64 bits) de celle de l'hôte :

**1.** Sur le **PC hôte**, ouvrez **Périphériques et imprimantes**, faites un clic droit sur l'imprimante partagée et sélectionnez **Propriétés de l'imprimante → Onglet Partage → Pilotes supplémentaires**.

**2.** Cochez l'architecture correspondant au **PC client** (par exemple, cochez **x86** si le client est en 32 bits et l'hôte en 64 bits), puis cliquez sur **OK** et installez le package de pilotes correspondant.

Une fois que l'hôte dispose du pilote correspondant, reconnectez-vous à l'imprimante partagée depuis le PC client.

---

## Foire aux questions

**Q : Que signifie l'erreur 0x0000007c ?**
La signification officielle est « le pilote d'imprimante est non valide ou le pilote d'imprimante distant ne peut pas être chargé ». En termes simples, le système Windows du PC client n'accepte pas le pilote transmis par l'hôte. Cela est causé par une incompatibilité de protocole ou une incompatibilité de version de pilote, et non par une défaillance matérielle.

**Q : Je peux envoyer une requête ping à l'hôte et accéder aux dossiers partagés, mais l'imprimante donne l'erreur 0x0000007c. Pourquoi ?**
L'accès aux dossiers partagés nécessite uniquement une connectivité réseau SMB. Les connexions à une imprimante partagée nécessitent à la fois la **compatibilité du protocole et la compatibilité des pilotes**. Un réseau fonctionnel ne garantit pas la compatibilité des pilotes. Activez SMB 1.0 et installez manuellement le pilote d'imprimante local pour résoudre ce problème.

**Q : Les anciens PC se connectent sans problème à l'imprimante partagée. Pourquoi mon nouveau PC Windows 10/11 obtient-il l'erreur 0x0000007c ?**
Windows 10/11 désactive SMB 1.0 par défaut et applique une validation de signature de pilote plus stricte que les anciennes versions de Windows. Les anciens PC fonctionnent parce qu'ils utilisent des paramètres de compatibilité plus permissifs. Le nouveau système bloque le pilote partagé hérité.

**Q : Je supprime et rajoute l'imprimante sans cesse, mais l'erreur persiste. Que me manque-t-il ?**
La suppression du périphérique imprimante seul ne supprime pas les fichiers du pilote. Vous devez supprimer le **package de pilotes** en utilisant `printui /s /t2`. Redémarrez l'ordinateur, puis préinstallez manuellement le pilote correct avant de vous reconnecter à l'imprimante partagée.

**Q : Dois-je modifier le registre pour corriger l'erreur 0x0000007c ?**
Non, dans la grande majorité des cas. L'erreur 0x0000007c est résolue en activant SMB 1.0, en nettoyant l'ancien package de pilotes et en installant manuellement le pilote correct — aucune modification du registre n'est requise.

---

## Résumé

L'erreur 0x0000007c lors de la connexion à une imprimante partagée sous Windows 10 ou Windows 11 signifie que le client ne peut pas charger le pilote d'imprimante distant en raison d'une incompatibilité de pilote ou de protocole SMB. Cela n'est pas lié au matériel de l'imprimante ni à un endommagement du système. Résolvez-la dans cet ordre :

1. **Vider le cache d'impression et redémarrer le spouleur d'impression**
2. **Supprimer complètement l'ancien package de pilotes** via `printui /s /t2` et réinstaller manuellement
3. **Activer SMB 1.0/CIFS** si l'hôte utilise des packages de pilotes hérités
4. **Ajouter des pilotes bidirectionnels sur l'hôte** si le client et l'hôte ont des architectures Windows différentes

---

## Articles connexes

- [Comment corriger l'erreur d'imprimante partagée 0x00000035 sur Windows](/fr/posts/printer-sharing-error-0x00000035/)
- [Comment corriger l'erreur d'imprimante partagée 0x00000032 sur Windows](/fr/posts/printer-sharing-error-0x00000032/)
- [Comment corriger l'erreur d'imprimante partagée 0x00000bcb sur Windows](/fr/posts/printer-sharing-error-0x00000bcb/)
