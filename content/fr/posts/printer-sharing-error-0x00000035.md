---
title: "Comment corriger l'erreur 0x00000035 de l'imprimante partagée sous Windows 10 et 11"
date: 2026-05-28
description: "Vous obtenez l'erreur 0x00000035 lors de la connexion à une imprimante partagée sous Windows 10 ou 11 ? Ce guide couvre toutes les causes et solutions — connectivité réseau, paramètres de partage, pare-feu, protocole SMB et nettoyage des identifiants."
tags: ["windows", "erreur 0x00000035", "erreur 0x80070035", "imprimante partagée", "chemin réseau introuvable", "SMB", "pare-feu windows", "windows 10", "windows 11", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x00000035/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x00000035 — Le chemin d'accès réseau est introuvable"
  caption: "L'erreur 0x00000035 apparaît quand Windows ne trouve pas le chemin partagé de l'imprimante sur le réseau local"
  relative: false
---

# Comment corriger l'erreur 0x00000035 de l'imprimante partagée sous Windows 10 et 11

L'erreur 0x00000035 — « Le chemin d'accès réseau est introuvable » — est l'une des erreurs d'imprimante partagée les plus courantes sous Windows 10 et Windows 11. Contrairement à d'autres erreurs d'imprimante causées par un seul paramètre, l'erreur 0x00000035 peut avoir plusieurs causes. Ce guide parcourt chaque cause et sa correction dans un ordre logique.

---

## Qu'est-ce que l'erreur 0x00000035 ?

Lorsque vous tentez de vous connecter à une imprimante partagée sur votre réseau local, Windows affiche :

> **L'opération n'a pas pu être effectuée (Erreur 0x00000035). Le chemin d'accès réseau est introuvable.**

![Erreur 0x00000035 sous Windows — Le chemin d'accès réseau est introuvable](/images/printer-error-0x00000035/step1.png)

**Symptômes typiques :**
- La saisie de l'IP ou du nom d'ordinateur de l'hôte dans la boîte Exécuter renvoie immédiatement l'erreur
- Certains appareils peuvent pinger l'IP hôte avec succès mais ne peuvent pas accéder aux ressources partagées
- Redémarrer le PC ou reconfigurer le partage d'imprimante ne résout pas le problème
- D'autres appareils du même réseau se connectent normalement — seuls certains PC déclenchent l'erreur

L'erreur 0x00000035 est aussi signalée comme **0x80070035** sur certains systèmes ; les deux codes signifient « chemin réseau introuvable » et se corrigent de la même façon.

---

## Pourquoi l'erreur 0x00000035 se produit-elle ?

Il existe six causes courantes :

1. **Problème d'environnement réseau** — L'hôte et le client ne sont pas sur le même réseau local ou sous-réseau.
2. **Découverte réseau et partage de fichiers désactivés** — L'hôte ou le client n'a pas activé la découverte réseau ou le partage de fichiers et d'imprimantes.
3. **Les services de partage essentiels ne fonctionnent pas** — Les services Server, Workstation ou TCP/IP NetBIOS Helper sont arrêtés ou désactivés.
4. **Le pare-feu ou le logiciel de sécurité bloque le trafic** — Le Pare-feu Windows ou un antivirus tiers bloque le port SMB 445.
5. **Échec du protocole réseau ou de la résolution de noms** — NetBIOS est désactivé, SMB 1.0/CIFS n'est pas installé, ou la résolution du nom d'hôte échoue.
6. **Identifiants réseau obsolètes** — Le client a mis en cache des identifiants incorrects pour le PC hôte.

---

## Solution

### Étape 1 — Vérifier la connectivité réseau

Sur le **PC hôte**, ouvrez l'invite de commandes et exécutez `ipconfig`. Notez l'adresse IPv4.

Sur le **PC client**, ouvrez l'invite de commandes et exécutez :
```
ping <adresse IP hôte>
```

![Invite de commandes — ipconfig et ping pour vérifier la connectivité](/images/printer-error-0x00000035/step2.png)

Si le ping échoue, vérifiez le routeur et les câbles réseau, et assurez-vous que les deux appareils sont connectés au même réseau et sous-réseau.

---

### Étape 2 — Activer la découverte réseau et le partage de fichiers

Sur l'hôte et le client, allez dans **Panneau de configuration → Centre Réseau et partage → Modifier les paramètres de partage avancés**. Sous **Réseau privé**, activez :
- **Activer la découverte du réseau**
- **Activer le partage de fichiers et d'imprimantes**

Enregistrez les paramètres, puis essayez à nouveau de vous connecter à l'imprimante partagée.

![Paramètres de partage avancés — activation de la découverte réseau et du partage](/images/printer-error-0x00000035/step3.png)

---

### Étape 3 — Autoriser le partage d'imprimante dans le pare-feu

Ouvrez **Pare-feu Windows Defender → Autoriser une application ou une fonctionnalité via le Pare-feu Windows Defender**. Trouvez **Partage de fichiers et d'imprimantes** et cochez les cases **Privé** et **Public**.

Si vous utilisez un antivirus tiers, désactivez-le temporairement pour vérifier s'il bloque la connexion.

![Pare-feu Windows — autorisation du partage de fichiers et d'imprimantes](/images/printer-error-0x00000035/step4.png)

---

### Étape 4 — Corriger les protocoles réseau et les paramètres d'accès

Ouvrez les **propriétés de la carte réseau** et assurez-vous que **Client pour les réseaux Microsoft** et **Partage de fichiers et d'imprimantes pour les réseaux Microsoft** sont cochés.

Ouvrez également **Activer ou désactiver des fonctionnalités Windows** et activez **Prise en charge du partage de fichiers SMB 1.0/CIFS**, puis redémarrez.

![Propriétés de la carte réseau — activation de SMB 1.0/CIFS](/images/printer-error-0x00000035/step5.png)

---

### Étape 5 — Supprimer les identifiants réseau obsolètes

Appuyez sur **Win + R**, tapez `control keymgr.dll` et ouvrez le **Gestionnaire d'informations d'identification**. Sous **Informations d'identification Windows**, supprimez toutes les entrées liées à l'adresse IP ou au nom d'ordinateur du PC hôte.

![Gestionnaire d'informations d'identification — suppression des identifiants obsolètes](/images/printer-error-0x00000035/step6.png)

---

## Foire aux questions

**Q : Le ping réussit mais l'erreur 0x00000035 persiste. Pourquoi ?**
Un ping réussi ne confirme que la connectivité IP de base. L'erreur 0x00000035 est causée par des services de partage, des règles de pare-feu ou des problèmes de protocole. Vérifiez le service Server, les règles de pare-feu et le protocole SMB.

**Q : L'erreur 0x00000035 est-elle la même que l'erreur 0x80070035 ?**
Oui. Les deux signifient « Le chemin d'accès réseau est introuvable ». La seule différence est le format du code d'erreur. Les causes et les corrections sont identiques.

**Q : Seul un PC client a l'erreur — tous les autres fonctionnent. Que faire ?**
Le problème est spécifique à ce PC client. Supprimez ses identifiants obsolètes, redémarrez ses services de partage locaux et activez le protocole SMB. Aucune modification n'est nécessaire sur le PC hôte.

**Q : Comment éviter que l'erreur ne réapparaisse ?**
Configurez les services **Server**, **Workstation** et **Assistance TCP/IP NetBIOS** pour démarrer automatiquement. Évitez également les caractères spéciaux dans le nom de partage de l'imprimante.

---

## Résumé

L'erreur 0x00000035 lors de la connexion à une imprimante partagée sous Windows 10 ou Windows 11 signifie que Windows ne peut pas localiser le chemin réseau partagé. Ce n'est pas lié au matériel ou au pilote de l'imprimante. Corrigez-la dans cet ordre :

1. **Vérifier la connectivité réseau** — pinger l'IP hôte ; s'assurer que les deux appareils sont sur le même sous-réseau
2. **Activer la découverte réseau et le partage de fichiers et d'imprimantes**
3. **Autoriser le partage de fichiers et d'imprimantes dans le Pare-feu Windows**
4. **Activer SMB 1.0/CIFS** et vérifier les paramètres de protocole de la carte réseau
5. **Supprimer les identifiants obsolètes** dans le Gestionnaire d'informations d'identification

---

## Articles connexes

- [Corriger l'erreur d'imprimante partagée 0x000006d9 sous Windows](/fr/posts/printer-sharing-error-0x000006d9/)
- [Corriger l'erreur d'imprimante partagée 0x00000bcb sous Windows](/fr/posts/printer-sharing-error-0x00000bcb/)
- [Corriger l'erreur d'imprimante partagée 0x00000709 sous Windows](/fr/posts/printer-sharing-error-0x00000709/)
