---
title: "Comment corriger l'erreur 0x000006d9 du partage d'imprimante sous Windows"
date: 2026-05-12
description: "Vous obtenez l'erreur 0x000006d9 lors du partage d'une imprimante sous Windows 10 ou 11 ? La solution est simple — démarrez le service Pare-feu Windows. Ce guide explique les causes et la procédure étape par étape."
tags: ["windows", "erreur 0x000006d9", "partage imprimante", "service pare-feu windows", "services.msc", "windows 10", "windows 11", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x000006d9/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x000006d9 — Impossible d'enregistrer les paramètres de l'imprimante"
  caption: "L'erreur 0x000006d9 apparaît quand le service Pare-feu Windows n'est pas en cours d'exécution, empêchant l'activation du partage d'imprimante"
  relative: false
---

# Comment corriger l'erreur 0x000006d9 du partage d'imprimante sous Windows

L'erreur 0x000006d9 est une erreur Windows courante qui apparaît spécifiquement lorsque vous essayez de **partager** une imprimante — pas lors de son utilisation normale. L'imprimante se connecte et imprime normalement, mais dès que vous ouvrez les paramètres de partage et cliquez sur Appliquer, Windows affiche :

> **Impossible d'enregistrer les paramètres de l'imprimante. L'opération n'a pas pu être effectuée (Erreur 0x000006d9).**

Ce guide explique exactement pourquoi l'erreur 0x000006d9 se produit et fournit une correction claire étape par étape pour Windows 10 et Windows 11.

---

## Qu'est-ce que l'erreur 0x000006d9 ?

Lorsque vous activez le partage d'imprimante sous Windows, le système s'appuie sur le **service Pare-feu Windows** pour enregistrer et exposer l'imprimante partagée sur le réseau — même si le pare-feu lui-même est désactivé. Si le *service* Pare-feu Windows est arrêté ou désactivé, Windows ne peut pas terminer la configuration du partage et retourne l'erreur 0x000006d9.

![Erreur 0x000006d9 sous Windows — Impossible d'enregistrer les paramètres de l'imprimante](/images/printer-error-0x000006d9/step1.png)

Le message d'erreur mentionne un « stockage insuffisant », ce qui est trompeur. Dans le contexte du partage d'imprimante, l'erreur 0x000006d9 n'a rien à voir avec l'espace disque — elle indique toujours que le service Pare-feu Windows n'est pas en cours d'exécution.

---

## Pourquoi l'erreur 0x000006d9 se produit-elle ?

Il existe quatre causes courantes :

1. **Le service Pare-feu Windows est arrêté** — Le partage d'imprimante nécessite que ce service soit *en cours d'exécution*, que le pare-feu soit activé ou non.
2. **Le service Pare-feu Windows est désactivé** — Les outils d'optimisation système, les installations Windows allégées ou les modifications manuelles désactivent souvent ce service.
3. **Les services dépendants ne fonctionnent pas** — Des services qui dépendent du Pare-feu Windows peuvent aussi être arrêtés ou en anomalie.
4. **Fichiers système corrompus** — Dans de rares cas, des fichiers système manquants ou endommagés empêchent le démarrage du service.

---

## Solution

### Étape 1 — Ouvrir le Gestionnaire de services

Appuyez sur **Win + R**, tapez `services.msc` et appuyez sur Entrée pour ouvrir la fenêtre **Services**.

![Boîte de dialogue Exécuter avec services.msc](/images/printer-error-0x000006d9/step2.png)

### Étape 2 — Trouver le service Pare-feu Windows

Faites défiler la liste pour localiser **Pare-feu Windows** (également répertorié sous *MpsSvc* dans certaines versions).

![Fenêtre Services — localisation du service Pare-feu Windows](/images/printer-error-0x000006d9/step3.png)

### Étape 3 — Activer et démarrer le service

Double-cliquez sur **Pare-feu Windows** pour ouvrir ses propriétés. Définissez le **Type de démarrage** sur **Automatique**, puis cliquez sur **Démarrer**. Cliquez sur **OK** pour enregistrer.

![Propriétés du service Pare-feu Windows — type de démarrage Automatique et démarrage du service](/images/printer-error-0x000006d9/step4.png)

> **Important :** Démarrer le *service* Pare-feu Windows n'active pas le pare-feu lui-même. Vous pouvez toujours garder le pare-feu désactivé dans les paramètres de Sécurité Windows — le service doit simplement être en cours d'exécution pour que le partage d'imprimante fonctionne.

### Étape 4 — Si Pare-feu Windows n'apparaît pas dans la liste

Dans certaines installations Windows personnalisées, le service Pare-feu Windows peut être absent. Dans ce cas, démarrez d'abord ces deux services connexes :

**Windows Update :** Double-cliquez sur **Windows Update**, définissez le type de démarrage sur **Automatique**, cliquez sur **Démarrer**, puis sur **OK**.

![Propriétés du service Windows Update](/images/printer-error-0x000006d9/step5.png)

**Windows Installer :** Localisez **Windows Installer**, double-cliquez dessus, définissez le type de démarrage sur **Automatique**, cliquez sur **Démarrer**, puis sur **OK**.

![Propriétés du service Windows Installer](/images/printer-error-0x000006d9/step6.png)

### Étape 5 — Redémarrer et réessayer

Redémarrez l'ordinateur. Après le redémarrage, vérifiez dans **Services** si **Pare-feu Windows** apparaît maintenant. Si c'est le cas, définissez-le sur **Automatique** et démarrez-le (comme à l'étape 3). Réactivez ensuite le partage d'imprimante — l'erreur 0x000006d9 devrait avoir disparu.

---

## Foire aux questions

**Q : L'erreur 0x000006d9 signifie-t-elle que le disque est plein ?**
Non. Bien que le message mentionne un « stockage insuffisant », ce code d'erreur dans le contexte du partage d'imprimante indique exclusivement que le service Pare-feu Windows n'est pas en cours d'exécution. Cela n'a rien à voir avec l'espace disque.

**Q : J'ai déjà désactivé mon pare-feu — pourquoi l'erreur apparaît-elle quand même ?**
Parce que le partage d'imprimante ne vérifie pas si le pare-feu est activé ou non, mais seulement si le *service* Pare-feu Windows est en cours d'exécution. Le service doit être démarré même si le pare-feu est désactivé.

**Q : Démarrer le service Pare-feu Windows affectera-t-il mes paramètres de sécurité ?**
Non. Vous pouvez toujours garder le pare-feu désactivé dans **Sécurité Windows → Pare-feu et protection du réseau**. Démarrer le service n'active pas automatiquement le pare-feu.

**Q : Le service Pare-feu Windows doit-il rester en cours d'exécution en permanence ?**
Oui, pour que le partage d'imprimante continue de fonctionner. En définissant le type de démarrage sur **Automatique**, il démarrera avec Windows à chaque fois.

---

## Résumé

L'erreur 0x000006d9 lors du partage d'une imprimante sous Windows est causée par une seule chose : **le service Pare-feu Windows n'est pas en cours d'exécution**. La correction :

1. Ouvrir **Services** (`services.msc`)
2. Trouver **Pare-feu Windows**, définir le type de démarrage sur **Automatique** et cliquer sur **Démarrer**
3. Redémarrer l'ordinateur et réactiver le partage d'imprimante

---

## Articles connexes

- [Corriger l'erreur d'imprimante partagée 0x00000bcb sous Windows](/fr/posts/printer-sharing-error-0x00000bcb/)
- [Corriger l'erreur d'imprimante partagée 0x00000709 sous Windows](/fr/posts/printer-sharing-error-0x00000709/)
- [Corriger l'erreur d'imprimante partagée 0x0000011b sous Windows](/fr/posts/printer-sharing-error-0x0000011b/)
