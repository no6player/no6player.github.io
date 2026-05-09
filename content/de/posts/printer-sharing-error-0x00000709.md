---
title: "Druckerfreigabe-Fehler 0x00000709 unter Windows beheben"
date: 2026-04-29
description: "Erhalten Sie Fehler 0x00000709 beim Verbinden mit einem freigegebenen Drucker unter Windows 10 oder Windows 11? Diese Schritt-für-Schritt-Anleitung behebt das Problem durch Hinzufügen von Windows-Anmeldeinformationen und Anpassen eines Registrierungswerts."
tags: ["windows", "fehler 0x00000709", "druckerfreigabe", "anmeldeinformationsverwaltung", "registrierung", "windows 10", "windows 11", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x00000709 — Der Vorgang konnte nicht abgeschlossen werden"
  caption: "Fehler 0x00000709 erscheint, wenn Windows keine Verbindung zum freigegebenen Drucker im Netzwerk herstellen kann"
  relative: false
---

# Druckerfreigabe-Fehler 0x00000709 unter Windows beheben

Fehler 0x00000709 ist einer der häufigsten Fehler bei freigegebenen Druckern unter Windows 10 und Windows 11. Er verhindert die Verbindung zu einem netzwerkweit freigegebenen Drucker und tritt oft plötzlich nach einem Windows-Update auf. Die gute Nachricht: Das Problem lässt sich in wenigen Minuten ohne Drittanbieter-Software lösen.

---

## Was ist Fehler 0x00000709?

Wenn Sie versuchen, eine Verbindung zu einem freigegebenen Drucker im lokalen Netzwerk herzustellen, zeigt Windows möglicherweise folgende Meldung an:

> **Der Vorgang konnte nicht abgeschlossen werden (Fehler 0x00000709). Überprüfen Sie den Druckernamen und stellen Sie sicher, dass der Drucker mit dem Netzwerk verbunden ist.**

![Fehler 0x00000709 unter Windows](/images/printer-error-0x00000709/step1.png)

Fehler 0x00000709 ist ein Berechtigungs- oder Authentifizierungsfehler. Windows versucht, über das Netzwerk mit dem Drucker-Host zu kommunizieren, wird jedoch aufgrund fehlender Anmeldeinformationen oder einer RPC-Protokollbeschränkung blockiert.

---

## Warum tritt Fehler 0x00000709 auf?

Es gibt drei Hauptursachen:

- **Fehlende Netzwerkanmeldeinformationen** — Windows hat keinen gespeicherten Benutzernamen/kein Kennwort für den Drucker-Host, weshalb der Zugriff verweigert wird.
- **RPC-Protokollbeschränkung** — Nach bestimmten Windows-Sicherheitsupdates hat sich die Standard-RPC-Kommunikationsmethode zwischen Computern geändert, was Druckerfreigabe-Verbindungen unterbricht.
- **Gastkonto nicht erkannt** — Das Gastkonto des Drucker-Hosts wird vom Client-PC nicht als vertrauenswürdig eingestuft, wodurch die Authentifizierung still fehlschlägt.

Dieser Fehler tritt häufig auf **Windows 10**- und **Windows 11**-Clients auf, die sich mit einem von einem anderen PC freigegebenen Drucker verbinden.

---

## Lösung

Die Lösung besteht aus zwei Teilen: Zuerst die Anmeldeinformationen des Drucker-Hosts zur Windows-Anmeldeinformationsverwaltung hinzufügen, dann einen RPC-Registrierungswert auf dem Client-PC anpassen.

### Teil 1 — Windows-Anmeldeinformationen hinzufügen

**1.** Öffnen Sie die **Systemsteuerung**, setzen Sie die Ansicht auf **Kleine Symbole** und klicken Sie auf **Anmeldeinformationsverwaltung**.

![Systemsteuerung — Anmeldeinformationsverwaltung](/images/printer-error-0x00000709/step2.png)

**2.** Wählen Sie **Windows-Anmeldeinformationen** und klicken Sie auf **Windows-Anmeldeinformationen hinzufügen**.

![Anmeldeinformationsverwaltung — Windows-Anmeldeinformationen hinzufügen](/images/printer-error-0x00000709/step3.png)

**3.** Geben Sie im Feld **Internetadresse oder Netzwerkadresse** die IP-Adresse des Host-PCs ein (z. B. `192.168.1.100`). Tragen Sie als **Benutzernamen** `guest` ein und lassen Sie das **Kennwortfeld** leer. Klicken Sie auf **OK**.

![Windows-Anmeldeinformationen hinzufügen — Host-IP und Gastbenutzername eingeben](/images/printer-error-0x00000709/step4.png)

> **Tipp:** Die IP-Adresse des Host-PCs finden Sie, indem Sie auf dem Host-PC in einer Eingabeaufforderung `ipconfig` ausführen.

### Teil 2 — Registrierung anpassen

**4.** Drücken Sie **Win + R**, geben Sie `regedit` ein und drücken Sie Enter, um den **Registrierungseditor** zu öffnen.

![Ausführen-Dialog mit regedit](/images/printer-error-0x00000709/step5.png)

**5.** Navigieren Sie zum folgenden Schlüssel. Falls der Unterschlüssel `RPC` nicht vorhanden ist, erstellen Sie ihn manuell unter `Printers`:

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Klicken Sie im rechten Bereich mit der rechten Maustaste und wählen Sie **Neu → DWORD-Wert (32-Bit)**.

![Registrierungseditor — Neuen DWORD-Wert unter dem RPC-Schlüssel erstellen](/images/printer-error-0x00000709/step6.png)

**6.** Benennen Sie den neuen Wert in `RpcUseNamedPipeProtocol` um und setzen Sie die **Wertdaten** auf `1`. Klicken Sie auf **OK**.

![Registrierungseditor — RpcUseNamedPipeProtocol auf 1 gesetzt](/images/printer-error-0x00000709/step7.png)

**7.** Nach Abschluss aller Schritte **starten Sie den Computer neu** und versuchen Sie erneut, eine Verbindung zum freigegebenen Drucker herzustellen. Fehler 0x00000709 sollte nicht mehr auftreten.

---

## Häufig gestellte Fragen

**F: Funktioniert diese Lösung sowohl unter Windows 10 als auch Windows 11?**
Ja. Fehler 0x00000709 tritt sowohl unter Windows 10 als auch unter Windows 11 auf. Die beschriebenen Schritte gelten für beide Versionen.

**F: Muss ich auch auf dem Drucker-Host-PC Änderungen vornehmen?**
In der Regel nicht. Die beschriebene Lösung wird auf dem **Client**-PC (dem verbindenden PC) angewendet. Stellen Sie jedoch sicher, dass der Drucker auf dem Host korrekt freigegeben ist und das Gastkonto dort aktiviert ist.

**F: Der Fehler tritt weiterhin auf. Was kann ich noch versuchen?**
- Überprüfen Sie, ob die IP-Adresse des Host-PCs in der Anmeldeinformationsverwaltung korrekt ist.
- Stellen Sie sicher, dass die Firewall des Host-PCs die Druckerfreigabe nicht blockiert.
- Versuchen Sie, den Druckernamen statt der IP-Adresse zu verwenden (z. B. `\\DESKTOP-ABC123`).
- Deinstallieren und reinstallieren Sie den Druckertreiber auf dem Client-PC.

---

## Zusammenfassung

Fehler 0x00000709 beim Verbinden mit einem freigegebenen Drucker unter Windows 10 oder Windows 11 wird typischerweise durch fehlende Netzwerkanmeldeinformationen oder eine RPC-Protokollbeschränkung verursacht. Die zweiteilige Lösung:

1. IP-Adresse des Drucker-Hosts und **guest**-Konto zur **Windows-Anmeldeinformationsverwaltung** hinzufügen
2. Registrierungswert **RpcUseNamedPipeProtocol** = `1` unter `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC` erstellen

---

## Verwandte Artikel

- [Druckerfreigabe-Fehler 0x0000011b unter Windows beheben](/de/posts/printer-sharing-error-0x0000011b/)
- [Druckerfreigabe-Fehler 0x000003e3 unter Windows 10 beheben](/de/posts/printer-sharing-error-0x000003e3/)
- [Druckerfreigabe-Fehler 0x00000040 unter Windows beheben](/de/posts/printer-sharing-error-0x00000040/)
