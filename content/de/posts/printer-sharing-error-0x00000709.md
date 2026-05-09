---
title: "Druckerfreigabe-Fehler 0x00000709 unter Windows beheben"
date: 2026-04-29
description: "Erhalten Sie Fehler 0x00000709 beim Verbinden mit einem freigegebenen Drucker unter Windows? Diese Anleitung zeigt, wie Sie das Problem durch Hinzufügen von Windows-Anmeldeinformationen und Ändern eines Registrierungswerts lösen."
tags: ["windows", "fehler 0x00000709", "druckerfreigabe", "anmeldeinformationsverwaltung", "registrierung", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x00000709 — Der Vorgang konnte nicht abgeschlossen werden"
  caption: "Fehler 0x00000709 erscheint, wenn Windows keine Verbindung zum freigegebenen Drucker im Netzwerk herstellen kann"
  relative: false
---

## Problembeschreibung

Beim Versuch, eine Verbindung zu einem freigegebenen Drucker im Netzwerk herzustellen, schlägt diese mit folgender Fehlermeldung fehl:

> **Der Vorgang konnte nicht abgeschlossen werden (Fehler 0x00000709). Überprüfen Sie den Druckernamen und stellen Sie sicher, dass der Drucker mit dem Netzwerk verbunden ist.**

![Fehler 0x00000709 unter Windows](/images/printer-error-0x00000709/step1.png)

Fehler 0x00000709 ist ein häufiger Verbindungsfehler bei freigegebenen Druckern unter Windows. Er wird typischerweise durch ein Systemupdate, eine falsche RPC-Protokollkonfiguration oder ein Problem mit der Anmeldeinformations-Authentifizierung verursacht. Die Lösung umfasst das Hinzufügen von Windows-Anmeldeinformationen und das Ändern eines Registrierungswerts.

---

## Lösung

**1.** Öffnen Sie die **Systemsteuerung**, setzen Sie die Ansicht auf **Kleine Symbole** und klicken Sie auf **Anmeldeinformationsverwaltung**.

![Systemsteuerung — Anmeldeinformationsverwaltung](/images/printer-error-0x00000709/step2.png)

**2.** Wählen Sie **Windows-Anmeldeinformationen** und klicken Sie auf **Windows-Anmeldeinformationen hinzufügen**.

![Anmeldeinformationsverwaltung — Windows-Anmeldeinformationen hinzufügen](/images/printer-error-0x00000709/step3.png)

**3.** Geben Sie im Feld **Internetadresse oder Netzwerkadresse** die IP-Adresse des Host-PCs ein. Tragen Sie als **Benutzernamen** `guest` ein und lassen Sie das **Kennwortfeld** leer. Klicken Sie auf **OK**.

![Windows-Anmeldeinformationen hinzufügen — Host-IP und Gastbenutzername eingeben](/images/printer-error-0x00000709/step4.png)

**4.** Drücken Sie **Win + R**, geben Sie `regedit` ein und drücken Sie Enter, um den **Registrierungseditor** zu öffnen.

![Ausführen-Dialog mit regedit](/images/printer-error-0x00000709/step5.png)

**5.** Navigieren Sie zum folgenden Schlüssel (erstellen Sie ihn, falls er nicht vorhanden ist):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Klicken Sie im rechten Bereich mit der rechten Maustaste und erstellen Sie einen neuen **DWORD-Wert (32-Bit)**.

![Registrierungseditor — Neuen DWORD-Wert unter dem RPC-Schlüssel erstellen](/images/printer-error-0x00000709/step6.png)

**6.** Benennen Sie den neuen Wert in `RpcUseNamedPipeProtocol` um und setzen Sie die **Wertdaten** auf `1`.

![Registrierungseditor — RpcUseNamedPipeProtocol auf 1 gesetzt](/images/printer-error-0x00000709/step7.png)

**7.** Nach Abschluss aller Schritte **starten Sie den Computer neu** und versuchen Sie erneut, eine Verbindung zum freigegebenen Drucker herzustellen.

---

## Zusammenfassung

Fehler 0x00000709 beim Verbinden mit einem freigegebenen Drucker ist ein häufiges Windows-Problem, das durch ein Systemupdate, eine RPC-Protokollkonfiguration oder ein Authentifizierungsproblem verursacht wird. Die Lösung umfasst zwei Schritte:

- Hinzufügen der IP-Adresse des Host-PCs und des **guest**-Kontos in der **Windows-Anmeldeinformationsverwaltung**
- Erstellen des Registrierungswerts **RpcUseNamedPipeProtocol** (`1`) unter `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Nach dem Neustart sollte die Verbindung zum freigegebenen Drucker problemlos funktionieren.
