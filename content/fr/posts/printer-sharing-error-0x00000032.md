---
title: "Comment corriger l'erreur d'imprimante 0x00000032 sur Windows 10 et 11"
date: 2026-05-30
description: "Vous obtenez l'erreur 0x00000032 (La demande n'est pas prise en charge) lors de l'installation ou de la connexion d'une imprimante sur Windows 10 ou 11 ? Ce guide couvre toutes les causes et les solutions — nettoyage des pilotes, réinitialisation du spouleur d'impression, stratégie de groupe et réparation de l'imprimante partagée."
tags: ["windows", "erreur 0x00000032", "erreur imprimante", "la demande n'est pas prise en charge", "spouleur d'impression", "pilote d'imprimante", "stratégie de groupe", "windows 10", "windows 11", "dépannage"]
categories: ["Dépannage"]
cover:
  image: "/images/printer-error-0x00000032/step1.png"
  alt: "Boîte de dialogue d'erreur Windows affichant l'erreur 0x00000032 — La demande n'est pas prise en charge"
  caption: "L'erreur 0x00000032 s'affiche lorsque Windows refuse de charger ou d'installer le pilote d'imprimante en raison d'une incompatibilité ou d'un ancien pilote verrouillé"
  relative: false
---

# Comment corriger l'erreur d'imprimante 0x00000032 sur Windows 10 et 11

L'erreur 0x00000032 — « La demande n'est pas prise en charge » — est l'une des erreurs de pilote d'imprimante les plus courantes sur Windows 10 et Windows 11. Elle survient lors de l'installation d'une imprimante USB locale, de la connexion à une imprimante réseau partagée ou de la mise à jour d'un pilote d'imprimante. Contrairement aux erreurs réseau, l'erreur 0x00000032 est un échec de classe de pilote : Windows refuse de charger, remplacer ou installer le pilote d'imprimante actuel. Ce guide explique chaque cause et détaille chaque solution dans un ordre logique.

---

## Qu'est-ce que l'erreur 0x00000032 ?

Lorsque vous installez une imprimante ou vous y connectez, Windows affiche :

> **L'opération n'a pas pu être effectuée (Erreur 0x00000032). La demande n'est pas prise en charge.**

![Boîte de dialogue d'erreur Windows affichant l'erreur 0x00000032 — La demande n'est pas prise en charge](/images/printer-error-0x00000032/step1.png)

**Les symptômes typiques incluent :**
- L'ajout d'une imprimante USB locale échoue immédiatement avec cette erreur
- La connexion à une imprimante partagée sur le réseau local échoue sur un PC client mais fonctionne sur d'autres
- La mise à jour du pilote d'imprimante déclenche l'erreur même après un téléchargement récent
- Le pilote générique/universel s'installe correctement, mais le pilote officiel renvoie 0x00000032

---

## Pourquoi l'erreur 0x00000032 se produit-elle ?

Il y a six causes courantes :

1. **Incompatibilité de pilote (cause la plus fréquente)** — La version du pilote ne correspond pas au pilote du PC hôte, un pilote 32 bits est mélangé avec un système 64 bits (ou inversement), ou le pilote ne prend pas en charge la version de Windows. Windows juge la demande d'installation invalide et la bloque.
2. **Fichiers de pilote résiduels bloquant l'installation** — Les fichiers et entrées de registre d'un pilote précédemment installé n'ont pas été entièrement supprimés. Le pilote est retenu par le système et ne peut pas être écrasé, déclenchant l'erreur 0x00000032.
3. **Défaillance du service Spouleur d'impression** — Le processus Spouleur est figé ou maintient des fichiers de pilote ouverts, empêchant l'écriture ou le chargement d'un nouveau pilote.
4. **Blocage de la stratégie d'installation des pilotes système** — La stratégie de sécurité de Windows bloque l'installation de pilotes non signés ou tiers.
5. **Échec de synchronisation du pilote partagé** — Lors de la connexion à une imprimante partagée sur le réseau local, le PC hôte ne dispose pas d'un pilote correspondant à la version Windows du PC client (notamment les incompatibilités 32 bits vs. 64 bits). Windows refuse la demande de téléchargement du pilote.
6. **Corruption mineure de fichiers système** — Des composants Windows liés à l'impression manquants ou endommagés provoquent l'arrêt du fonctionnement de l'interface d'installation des pilotes.

---

## Solution

Suivez les étapes ci-dessous dans l'ordre. La plupart des cas sont résolus par les étapes 1 et 2.

### Étape 1 — Redémarrer le Spouleur d'impression et vider le cache d'impression

**1.** Appuyez sur **Win + R**, tapez `services.msc` et appuyez sur Entrée.

**2.** Trouvez **Spouleur d'impression** dans la liste, faites un clic droit dessus et sélectionnez **Arrêter**.

![Fenêtre Services — arrêt du service Spouleur d'impression](/images/printer-error-0x00000032/step2.png)

**3.** Appuyez sur **Win + R**, tapez `C:\Windows\System32\spool\PRINTERS` et appuyez sur Entrée. Supprimez **tous les fichiers** dans le dossier (ne supprimez pas le dossier lui-même).

![Explorateur de fichiers — suppression de tous les fichiers dans le dossier de spoule PRINTERS](/images/printer-error-0x00000032/step3.png)

**4.** Revenez dans Services, faites un clic droit sur **Spouleur d'impression**, sélectionnez **Démarrer** et définissez le **Type de démarrage** sur **Automatique**.

---

### Étape 2 — Supprimer complètement le package de pilote résiduel

L'erreur 0x00000032 est le plus souvent causée par un ancien pilote verrouillé. Vous devez supprimer le package de pilote complet — pas seulement le périphérique imprimante :

**1.** Ouvrez **Panneau de configuration → Périphériques et imprimantes**. Faites un clic droit sur chaque imprimante défaillante ou en double et sélectionnez **Supprimer le périphérique**.

**2.** Appuyez sur **Win + R**, tapez `printui /s /t2` et appuyez sur Entrée pour ouvrir les **Propriétés du serveur d'impression**. Passez à l'onglet **Pilotes**. Sélectionnez le pilote de l'imprimante problématique, cliquez sur **Supprimer**, puis choisissez **Supprimer le pilote et le package de pilotes** pour effacer tous les fichiers de pilote.

![Propriétés du serveur d'impression — suppression complète du pilote et du package de pilotes](/images/printer-error-0x00000032/step4.png)

**3.** Redémarrez l'ordinateur. Téléchargez ensuite le pilote complet correct depuis le site Web du fabricant et installez-le.

---

### Étape 3 — Ajuster la stratégie système pour autoriser l'installation des pilotes

Sur les éditions Windows Professionnel et Entreprise, la stratégie de groupe peut bloquer l'installation des pilotes :

**1.** Appuyez sur **Win + R**, tapez `gpedit.msc` et appuyez sur Entrée. Naviguez vers **Configuration ordinateur → Modèles d'administration → Imprimantes**.

**2.** Désactivez **Ne pas autoriser l'installation d'imprimantes à l'aide de pilotes en mode noyau** (si activé).

**3.** Activez **Autoriser le spouleur d'impression à accepter les connexions des clients**.

![Éditeur de stratégie de groupe — ajustement des stratégies d'installation des pilotes d'imprimante](/images/printer-error-0x00000032/step5.png)

---

### Étape 4 — Corriger les problèmes d'imprimante partagée (connexion LAN)

Si l'erreur 0x00000032 apparaît uniquement lors de la connexion à une imprimante partagée sur le réseau local :

**1.** Sur le **PC hôte**, ajoutez le package de pilotes supplémentaire correspondant à la **version Windows du PC client** (32 bits ou 64 bits). Faites-le dans **Propriétés du serveur d'impression → Pilotes → Ajouter**.

**2.** Sur le **PC client**, activez **Activer les ouvertures de session invité non sécurisées** : ouvrez **Stratégie de groupe locale → Configuration ordinateur → Modèles d'administration → Réseau → Station de travail Lanman** et définissez **Activer les ouvertures de session invité non sécurisées** sur **Activé**.

**3.** Effacez les informations d'identification réseau obsolètes sur le PC client : appuyez sur **Win + R**, tapez `control keymgr.dll`, ouvrez le **Gestionnaire d'informations d'identification** et supprimez toutes les entrées liées à l'adresse IP ou au nom d'ordinateur du PC hôte.

![Gestionnaire d'informations d'identification — suppression des informations d'identification réseau obsolètes sur le PC client](/images/printer-error-0x00000032/step6.png)

**4.** Reconnectez-vous en utilisant directement l'adresse IP du PC hôte : `\\192.168.1.x`.

---

### Étape 5 — Réparer les fichiers système

**1.** Recherchez **cmd**, faites un clic droit dessus et sélectionnez **Exécuter en tant qu'administrateur**.

**2.** Exécutez la commande suivante et attendez qu'elle se termine :

```
sfc /scannow
```

![Invite de commandes — exécution de sfc /scannow pour réparer les fichiers système](/images/printer-error-0x00000032/step7.png)

**3.** Redémarrez l'ordinateur et essayez à nouveau d'installer le pilote d'imprimante.

---

## Foire aux questions

**Q : Que signifie réellement l'erreur 0x00000032 « La demande n'est pas prise en charge » ?**
Cela signifie que Windows refuse la demande d'installation du pilote. Le système a déterminé que la demande est invalide — le plus souvent parce que le pilote ne correspond pas à la version du système, qu'un ancien pilote en conflit est verrouillé en place, ou qu'une stratégie de sécurité bloque l'installation.

**Q : Le pilote générique s'installe correctement, mais le pilote officiel donne l'erreur 0x00000032. Pourquoi ?**
Cela est presque toujours causé par un package de pilote résiduel verrouillé d'une installation précédente. Le pilote générique utilise un chemin d'installation différent, c'est pourquoi il fonctionne. Supprimez le package de pilote complet via `printui /s /t2`, redémarrez, puis installez le pilote officiel.

**Q : Les autres ordinateurs peuvent se connecter à l'imprimante partagée — seul le mien obtient l'erreur 0x00000032. Que faire ?**
Le problème est spécifique à l'environnement de pilote de votre PC client. Les autres PC ont des pilotes compatibles ; le vôtre a soit un conflit de pilote ancien, soit une incompatibilité de version. Nettoyez le pilote sur votre PC uniquement (étapes 1 et 2) — aucune modification n'est nécessaire sur le PC hôte ou l'imprimante.

**Q : J'ai réinstallé le pilote plusieurs fois mais j'obtiens toujours l'erreur. Qu'est-ce que je rate ?**
Vous avez très probablement supprimé le périphérique imprimante mais pas le **package de pilotes**. La simple suppression du périphérique dans Périphériques et imprimantes laisse les fichiers de pilote en place. Ouvrez `printui /s /t2`, allez dans l'onglet Pilotes et supprimez complètement le package de pilotes avant de réinstaller.

**Q : Une incompatibilité 32 bits/64 bits entre l'hôte et le client peut-elle provoquer l'erreur 0x00000032 ?**
Oui. Si le PC hôte est en 64 bits et le client en 32 bits, et que l'hôte n'a pas ajouté de package de pilotes 32 bits, le client ne peut pas télécharger un pilote correspondant et Windows renvoie « La demande n'est pas prise en charge. » Ajoutez le pilote d'architecture manquant sur le PC hôte dans les Propriétés du serveur d'impression.

---

## Résumé

L'erreur 0x00000032 lors de l'installation ou de la connexion d'une imprimante sur Windows 10 ou Windows 11 signifie que Windows refuse la demande d'installation du pilote. Elle n'est pas liée au réseau, au matériel de l'imprimante ou à des dommages système majeurs. Corrigez-la dans cet ordre :

1. **Redémarrer le Spouleur d'impression et vider le cache d'impression**
2. **Supprimer complètement l'ancien package de pilotes** via `printui /s /t2` — c'est la solution la plus courante
3. **Ajuster la stratégie de groupe** pour autoriser l'installation des pilotes (éditions Professionnel/Entreprise)
4. **Pour les imprimantes partagées** : ajouter les pilotes d'architecture correspondants sur l'hôte, activer la session invité sur le client et effacer les informations d'identification obsolètes
5. **Exécuter `sfc /scannow`** pour réparer les fichiers système corrompus

---

## Articles connexes

- [Comment corriger l'erreur d'imprimante partagée 0x00000006 sur Windows](/fr/posts/printer-sharing-error-0x00000006/)
- [Comment corriger l'erreur d'imprimante partagée 0x00000035 sur Windows](/fr/posts/printer-sharing-error-0x00000035/)
- [Comment corriger l'erreur d'imprimante partagée 0x00000709 sur Windows](/fr/posts/printer-sharing-error-0x00000709/)
