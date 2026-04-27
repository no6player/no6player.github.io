---
title: "Comment corriger l'erreur de partage d'imprimante 0x0000011b sur Windows"
date: 2026-04-27
description: "Vous obtenez le message 'Windows ne peut pas se connecter à l'imprimante, erreur 0x0000011b' ? Ce guide vous explique le correctif de registre éprouvé pour rétablir la connexion instantanément."
tags: ["windows", "erreur 0x0000011b", "partage imprimante", "registre", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x0000011b/step1.png"
  alt: "Boîte de dialogue d'erreur Windows 0x0000011b"
  caption: "L'erreur 0x0000011b apparaît quand Windows ne peut pas se connecter à l'imprimante partagée"
  relative: false
---

Les imprimantes partagées sont essentielles dans les environnements de bureau et les réseaux domestiques. Cependant, une erreur frustrante empêche de nombreux utilisateurs de se connecter :

> **« Windows ne peut pas se connecter à l'imprimante. L'opération a échoué avec l'erreur 0x0000011b. »**

![Boîte de dialogue d'erreur 0x0000011b sous Windows](/images/printer-error-0x0000011b/step1.png)

Cette erreur est apparue massivement après une mise à jour de sécurité Microsoft (KB5005565, fin 2021). La correction prend moins de deux minutes.

---

## Pourquoi l'erreur 0x0000011b se produit-elle ?

La cause est une modification de la **confidentialité d'authentification RPC**. Après la mise à jour, Windows exige un niveau d'authentification plus strict que les anciens systèmes ne peuvent pas toujours négocier, provoquant l'erreur `0x0000011b`.

---

## Correction — Modifier le registre Windows

> ⚠️ **Avant de commencer :** Sauvegardez le registre : **Fichier → Exporter**.

### Étape 1 — Ouvrir l'Éditeur du Registre

Appuyez sur **Win + R**, tapez `regedit` et appuyez sur **Entrée**.

![Recherche de regedit dans le menu Démarrer de Windows](/images/printer-error-0x0000011b/step2.png)

### Étape 2 — Naviguer vers la clé Print

Collez le chemin suivant dans la barre d'adresse et appuyez sur **Entrée** :

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print
```

![Éditeur du Registre avec le chemin Control\Print](/images/printer-error-0x0000011b/step3.png)

### Étape 3 — Créer une nouvelle valeur DWORD

1. Clic droit sur une zone vide du volet de droite.
2. Sélectionnez **Nouveau → Valeur DWORD (32 bits)**.

![Menu contextuel pour créer une valeur DWORD](/images/printer-error-0x0000011b/step4.png)

3. Nommez la valeur exactement : `RpcAuthnLevelPrivacyEnabled`

### Étape 4 — Définir la valeur sur 0

1. Double-cliquez sur `RpcAuthnLevelPrivacyEnabled`.
2. Entrez `0` dans le champ **Données de la valeur**.
3. Cliquez sur **OK**.

![Définition de RpcAuthnLevelPrivacyEnabled sur 0](/images/printer-error-0x0000011b/step5.png)

### Étape 5 — Redémarrer et vérifier

Redémarrez le PC et reconnectez-vous à l'imprimante partagée.

![File d'attente d'impression montrant une connexion réussie](/images/printer-error-0x0000011b/step6.png)

---

## L'erreur persiste ?

- **Appliquez la correction sur l'ordinateur hôte** — même modification sur le PC qui partage l'imprimante.
- **Vérifiez le pare-feu Windows** — autoriser le partage de fichiers et d'imprimantes des deux côtés.
- **Même réseau** — les deux appareils doivent être sur le même sous-réseau.
- **Redémarrez le Spouleur d'impression** — `services.msc` → **Spouleur d'impression** → **Redémarrer**.

---

## Récapitulatif

| Étape | Action |
|---|---|
| 1 | Ouvrir l'Éditeur du Registre (`Win + R` → `regedit`) |
| 2 | Naviguer vers `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print` |
| 3 | Créer une nouvelle valeur **DWORD (32 bits)** |
| 4 | Nommer `RpcAuthnLevelPrivacyEnabled` et définir sur `0` |
| 5 | Redémarrer l'ordinateur |
