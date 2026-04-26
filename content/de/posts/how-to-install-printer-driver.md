---
title: "Druckertreiber unter Windows 10/11 installieren"
date: 2026-04-26
description: "Eine vollständige Schritt-für-Schritt-Anleitung zur Installation von Druckertreibern unter Windows 10 und Windows 11 – USB, Netzwerk und manuelle Installation mit Problemlösungen."
tags: ["windows", "treiber", "installation", "USB", "netzwerkdrucker"]
categories: ["Installation"]
cover:
  image: "/images/printer-driver/connection-types.svg"
  alt: "Übersicht der Druckeranschlussmethoden"
  caption: "Zwei Hauptwege zum Anschließen eines Druckers: USB und Netzwerk/WLAN"
  relative: false
---

Die korrekte Installation eines Druckertreibers ist die Grundlage für ein funktionierendes Druckersystem. Diese Anleitung führt Sie durch alle Methoden – vom einfachen Plug-and-Play bis zur vollständig manuellen Installation – und zeigt Ihnen, wie Sie die häufigsten Probleme beheben.

---

## Vor dem Start

Sammeln Sie folgende Informationen, bevor Sie beginnen:

| Was Sie benötigen | Wo Sie es finden |
|---|---|
| Druckermodellnummer | Aufkleber vorne oder unten am Drucker |
| Windows-Version (10 oder 11) | Einstellungen → System → Info |
| Systemtyp (32-Bit / 64-Bit) | Einstellungen → System → Info → Systemtyp |
| Verbindungstyp (USB oder WLAN) | Entscheiden Sie sich vor dem Start |

---

## Methode 1 — USB Plug-and-Play (Einfachste Methode)

Diese Methode funktioniert bei den meisten modernen Druckern sofort.

**Schritte:**

1. Stellen Sie sicher, dass der Drucker **ausgeschaltet** ist.
2. Schließen Sie das USB-Kabel vom Drucker an einen USB-Anschluss des Computers an.
3. Schalten Sie den Drucker **ein**.
4. Windows erkennt den Drucker automatisch und lädt den Treiber herunter.
5. Warten Sie auf die Benachrichtigung: *„Gerät ist bereit"* in der unteren rechten Ecke.

> **Tipp:** Verwenden Sie ein USB-Kabel mit weniger als 3 Metern Länge. Längere Kabel können Verbindungsfehler verursachen.

---

## Methode 2 — Windows-Einstellungen (USB oder Netzwerk)

Verwenden Sie diese Methode, wenn Plug-and-Play nicht funktioniert hat, oder um einen Netzwerkdrucker hinzuzufügen.

![Windows-Einstellungen — Drucker und Scanner](/images/printer-driver/windows-settings.svg)

**Schritte:**

1. Öffnen Sie **Start** → **Einstellungen** (⚙-Symbol).
2. Klicken Sie auf **Bluetooth und Geräte** → **Drucker und Scanner**.
3. Klicken Sie auf **Drucker oder Scanner hinzufügen** (blauer Button oben).
4. Windows sucht nach Druckern in der Nähe. Warten Sie 20–30 Sekunden.

![Drucker-Assistent — Drucker aus der Liste auswählen](/images/printer-driver/add-printer-wizard.svg)

5. Wenn Ihr Drucker in der Liste erscheint, klicken Sie darauf.
6. Klicken Sie auf **Gerät hinzufügen**.
7. Windows installiert den Treiber automatisch.

> **Netzwerkdrucker wird nicht angezeigt?** Stellen Sie sicher, dass Drucker und Computer mit dem **gleichen WLAN-Netzwerk** verbunden sind.

---

## Methode 3 — Manueller Treiber-Download

Verwenden Sie diese Methode, wenn Windows den Treiber nicht automatisch findet.

![Manueller Treiber-Download – vier Schritte](/images/printer-driver/manual-driver-download.svg)

**Schritte:**

1. **Druckermodell ermitteln** — überprüfen Sie den Aufkleber am Drucker.
2. **Website des Herstellers aufrufen:**
   - HP: `support.hp.com`
   - Canon: `canon.com/support`
   - Epson: `epson.com/support`
   - Brother: `support.brother.com`
3. **Suchen** Sie nach Ihrer genauen Modellnummer.
4. Wählen Sie unter **Software und Treiber** Ihr Betriebssystem aus.
5. Laden Sie den **Vollfunktions-Treiber** (empfohlen) oder den Basistreiber herunter.
6. Öffnen Sie die heruntergeladene `.exe`-Datei und folgen Sie dem Installationsassistenten.

---

## Methode 4 — Netzwerkdrucker über IP-Adresse

Verwenden Sie diese Methode, wenn Ihr Netzwerkdrucker nicht automatisch erkannt wird.

**Schritte:**

1. Drucken Sie eine **Konfigurationsseite** über das Druckerbedienfeld aus. Diese zeigt die IP-Adresse des Druckers.
2. Öffnen Sie **Einstellungen** → **Bluetooth und Geräte** → **Drucker und Scanner** → **Drucker oder Scanner hinzufügen**.
3. Klicken Sie auf **„Der gesuchte Drucker ist nicht aufgeführt"**.
4. Wählen Sie **„Drucker mit TCP/IP-Adresse oder Hostname hinzufügen"** → **Weiter**.
5. Geben Sie die IP-Adresse des Druckers ein (z. B. `192.168.1.105`).
6. Klicken Sie auf **Weiter** — Windows stellt die Verbindung her und installiert den Treiber.

---

## Fehlerbehebung

![Häufige Druckerprobleme und Lösungen](/images/printer-driver/troubleshooting.svg)

### Drucker wird bei der Installation nicht gefunden

- Überprüfen Sie, ob der Drucker **eingeschaltet** ist (nicht im Energiesparmodus).
- Bei USB: Versuchen Sie einen anderen USB-Anschluss; vermeiden Sie USB-Hubs.
- Bei Netzwerk: Stellen Sie sicher, dass beide Geräte im gleichen WLAN sind.
- Deaktivieren Sie vorübergehend Ihre Firewall und versuchen Sie es erneut.

### Drucker wird als „Offline" angezeigt

1. Drücken Sie `Win + R`, geben Sie `services.msc` ein und drücken Sie **Enter**.
2. Scrollen Sie zu **Druckwarteschlange**.
3. Rechtsklick → **Neu starten**.
4. Öffnen Sie **Geräte und Drucker**, rechtsklicken Sie auf Ihren Drucker → **Druckerwarteschlange anzeigen**.
5. Klicken Sie im Menü auf **Drucker** und deaktivieren Sie **Drucker offline verwenden**.

### Treiberinstallation schlägt fehl

- Rechtsklick auf den Installer → **Als Administrator ausführen**.
- Deaktivieren Sie vorübergehend Ihr Antivirenprogramm und versuchen Sie es erneut.
- Stellen Sie sicher, dass Sie den Treiber für die richtige Betriebssystemversion heruntergeladen haben.

---

## Zusammenfassung

| Methode | Am besten für | Treiberquelle |
|---|---|---|
| USB Plug-and-Play | Neue Drucker, schnelle Einrichtung | Windows Update |
| Windows-Einstellungen | USB- oder Netzwerkdrucker | Windows Update |
| Manueller Download | Vollfunktionen, fehlgeschlagene Auto-Installation | Hersteller-Website |
| IP-Adresse | Nicht automatisch erkannte Netzwerkdrucker | Windows / Hersteller |

Drucken Sie nach der Installation immer eine **Testseite**: Rechtsklick auf den Drucker → **Druckereigenschaften** → **Testseite drucken**.
