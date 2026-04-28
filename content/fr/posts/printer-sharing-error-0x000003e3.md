---
title: "Comment corriger l'erreur 0x000003e3 de l'imprimante partagée sous Windows 10"
date: 2026-04-28
description: "Vous obtenez l'erreur 0x000003e3 en vous connectant à une imprimante partagée sous Windows 10 ? Ce guide vous explique comment modifier les paramètres de stratégie de sécurité locale et de stratégie de groupe pour rétablir la connexion."
tags: ["windows 10", "erreur 0x000003e3", "imprimante partagée", "stratégie de sécurité", "stratégie de groupe", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x000003e3/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x000003e3 lors de la connexion à une imprimante partagée"
  caption: "L'erreur 0x000003e3 apparaît quand Windows 10 ne peut pas se connecter à une imprimante partagée sur le réseau local"
  relative: false
---

## Description du problème

Sous Windows 10, la connexion à une imprimante partagée sur le réseau local échoue avec le code d'erreur suivant :

> **Erreur 0x000003e3**

![Erreur 0x000003e3 sous Windows 10](/images/printer-error-0x000003e3/step1.png)

Il s'agit d'une erreur courante de permissions ou d'authentification lors de la connexion à des imprimantes partagées. Elle est généralement causée par un accès refusé, une anomalie de service ou un problème de protocole réseau. Suivez les étapes ci-dessous pour résoudre le problème.

---

## Solution

### Étape 1 — Modifier la stratégie de sécurité locale

**1.** Ouvrez **Panneau de configuration → Programmes → Activer ou désactiver des fonctionnalités Windows**, puis activez **Prise en charge du partage de fichiers SMB 1.0/CIFS**.

![Activer la prise en charge SMB 1.0/CIFS dans les fonctionnalités Windows](/images/printer-error-0x000003e3/step2.png)

**2.** Appuyez sur **Win + R**, tapez `secpol.msc` et appuyez sur Entrée pour ouvrir l'éditeur de **Stratégie de sécurité locale**.

![Boîte de dialogue Exécuter avec secpol.msc](/images/printer-error-0x000003e3/step3.png)

**3.** Accédez à **Stratégies locales → Attribution des droits utilisateur → Accéder à cet ordinateur à partir du réseau**. Ajoutez le compte **Invité** local.  
Ensuite, allez dans **Refuser l'accès à cet ordinateur à partir du réseau** et **supprimez** le compte Invité de cette liste.

![Stratégie de sécurité locale — Attribution des droits utilisateur](/images/printer-error-0x000003e3/step4.png)

**4.** Accédez à **Stratégies locales → Options de sécurité → Accès réseau : modèle de partage et de sécurité pour les comptes locaux** et définissez la valeur sur **Invité uniquement – les utilisateurs locaux s'authentifient en tant qu'invités**.

![Stratégie de sécurité locale — Modèle de partage défini sur Invité uniquement](/images/printer-error-0x000003e3/step5.png)

---

### Étape 2 — Modifier la stratégie de groupe locale

**1.** Appuyez sur **Win + R**, tapez `gpedit.msc` et appuyez sur Entrée pour ouvrir l'**Éditeur de stratégie de groupe locale**.

![Boîte de dialogue Exécuter avec gpedit.msc](/images/printer-error-0x000003e3/step6.png)

**2.** Accédez à **Configuration de l'ordinateur → Paramètres Windows → Paramètres de sécurité → Stratégies locales → Options de sécurité**. Recherchez la stratégie **Comptes : limiter l'utilisation de mots de passe vides par le compte local à l'ouverture de session console**, double-cliquez dessus, sélectionnez **Désactivé** et cliquez sur **OK**.

![Stratégie de groupe — Désactiver la restriction de mot de passe vide](/images/printer-error-0x000003e3/step7.png)

**3.** Ouvrez **Gestion de l'ordinateur**, puis accédez à **Outils système → Utilisateurs et groupes locaux → Utilisateurs**. Double-cliquez sur **Invité**, cliquez sur l'onglet **Membre de**, puis sur **Ajouter**. Dans la boîte de dialogue, tapez `users` et cliquez sur **OK**.

![Gestion de l'ordinateur — Ajout d'Invité au groupe Utilisateurs](/images/printer-error-0x000003e3/step8.png)

**4.** De retour dans la fenêtre Propriétés de l'invité, sous **Membre de**, sélectionnez **Utilisateurs**, cliquez sur **Ajouter**, puis sur **Appliquer** et **OK**.

![Propriétés de l'invité — Onglet Membre de affichant le groupe Utilisateurs](/images/printer-error-0x000003e3/step9.png)

**5.** Une fois toutes les étapes terminées, **redémarrez votre ordinateur** et essayez à nouveau d'ajouter l'imprimante partagée.

---

## Résumé

L'erreur 0x000003e3 lors de la connexion à une imprimante partagée sous Windows 10 est généralement causée par des paramètres de stratégie de sécurité ou de groupe incorrects. La solution comprend :

- L'activation de la prise en charge du partage de fichiers SMB 1.0/CIFS
- L'octroi des droits d'accès réseau au compte Invité
- La définition du modèle de sécurité de partage sur « Invité uniquement »
- La désactivation de la restriction de connexion console pour les mots de passe vides
- L'ajout du compte Invité au groupe local Utilisateurs

Si l'erreur persiste après avoir suivi toutes les étapes, vérifiez également :

- Que la connexion LAN fonctionne correctement (l'hôte et le client doivent être sur le même sous-réseau et pouvoir se pinger mutuellement)
- Que l'hôte de l'imprimante partagée dispose de paramètres correspondants
- Désinstaller et réinstaller le pilote d'imprimante, puis réessayer
