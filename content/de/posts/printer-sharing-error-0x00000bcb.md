---
title: "Druckerfreigabe-Fehler 0x00000bcb unter Windows 10 und 11 beheben"
date: 2026-05-11
description: "Erhalten Sie Fehler 0x00000bcb beim Verbinden mit einem freigegebenen Drucker unter Windows 10 oder 11? Diese Anleitung erklärt alle Ursachen und Lösungen — Registrierungsanpassung, Treiberbereinigung und Druckerwarteschlangen-Reset."
tags: ["windows", "fehler 0x00000bcb", "druckerfreigabe", "registrierung", "point and print", "druckertreiber", "windows 10", "windows 11", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x00000bcb/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x00000bcb — Der angegebene Druckertreiber wurde auf diesem System nicht gefunden"
  caption: "Fehler 0x00000bcb erscheint, wenn Windows den Druckertreiber aufgrund von RPC-Richtlinienbeschränkungen nicht installieren kann"
  relative: false
---

# Druckerfreigabe-Fehler 0x00000bcb unter Windows 10 und 11 beheben

Fehler 0x00000bcb ist ein häufiger Verbindungsfehler bei freigegebenen Druckern unter Windows 10 und Windows 11. Windows meldet, dass der angegebene Druckertreiber nicht gefunden wurde und heruntergeladen werden muss — der Download schlägt jedoch fehl. Dieser Artikel erklärt die genauen Ursachen von Fehler 0x00000bcb und zeigt, wie Sie ihn Schritt für Schritt beheben.

---

## Was ist Fehler 0x00000bcb?

Beim Versuch, eine Verbindung zu einem freigegebenen Drucker im lokalen Netzwerk herzustellen, zeigt Windows folgende Meldung an:

> **Der Vorgang konnte nicht abgeschlossen werden (Fehler 0x00000bcb). Der angegebene Druckertreiber wurde auf diesem System nicht gefunden und muss heruntergeladen werden.**

![Fehler 0x00000bcb unter Windows](/images/printer-error-0x00000bcb/step1.png)

Windows versucht beim Verbinden mit einem freigegebenen Drucker automatisch, den Treiber vom Host-PC zu installieren. Fehler 0x00000bcb bedeutet, dass diese automatische Treiberinstallation blockiert wurde — meist durch eine Sicherheitsrichtlinie aus einem neueren Windows-Update.

---

## Warum tritt Fehler 0x00000bcb auf?

Es gibt fünf häufige Ursachen:

1. **Windows-Update hat die RPC-Sicherheit verschärft** — Microsoft-Sicherheitspatches haben die Point-and-Print-Treiberinstallation auf Administratoren beschränkt, was den automatischen Treiberdownload von freigegebenen Druckern blockiert.
2. **Fehlender oder nicht übereinstimmender Druckertreiber** — Dem Client-PC fehlt der korrekte Druckertreiber, oder die installierte Version stimmt nicht mit der auf dem Host überein.
3. **Druckerwarteschlangen-Fehler oder beschädigter Druckcache** — Ein feststeckender oder beschädigter Druckauftrag kann die Installation neuer Treiber verhindern.
4. **Unzureichende LAN-Berechtigungen** — Der Client-PC hat keine Rechte, Treiber vom Host zu laden und zu installieren.
5. **Firewall oder Sicherheitssoftware blockiert die gemeinsame Kommunikation** — Drittanbieter-Antivirensoftware oder Windows-Firewallregeln können den Druckerfreigabe-Datenverkehr blockieren.

---

## Lösung

### Schritt 1 — Registrierung anpassen (Point-and-Print-Einschränkung)

**1.** Drücken Sie **Win + R**, geben Sie `regedit` ein und drücken Sie Enter, um den **Registrierungseditor** zu öffnen.

![Ausführen-Dialog mit regedit](/images/printer-error-0x00000bcb/step2.png)

**2.** Navigieren Sie zum folgenden Pfad. Falls Teile des Pfads nicht vorhanden sind, erstellen Sie die fehlenden Unterschlüssel manuell: Rechtsklick auf den übergeordneten Schlüssel → **Neu → Schlüssel**.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint
```

![Registrierungseditor — Navigation zum PointAndPrint-Schlüssel](/images/printer-error-0x00000bcb/step3.png)

**3.** Klicken Sie im rechten Bereich mit der rechten Maustaste und wählen Sie **Neu → DWORD-Wert (32-Bit)**. Benennen Sie ihn `RestrictDriverInstallationToAdministrators`.

![Registrierungseditor — DWORD RestrictDriverInstallationToAdministrators erstellen](/images/printer-error-0x00000bcb/step4.png)

**4.** Doppelklicken Sie auf den neuen Wert und setzen Sie die **Wertdaten** auf `0`. Klicken Sie auf **OK**.

![Registrierungseditor — RestrictDriverInstallationToAdministrators auf 0 setzen](/images/printer-error-0x00000bcb/step5.png)

### Schritt 2 — Alten Druckertreiber entfernen

**5.** Öffnen Sie **Systemsteuerung → Hardware und Sound → Geräte und Drucker**. Klicken Sie in der oberen Menüleiste auf **Druckservereigenschaften**, dann auf die Registerkarte **Treiber**. Wählen Sie den alten Treiber für den freigegebenen Drucker aus und klicken Sie auf **Entfernen**.

![Druckservereigenschaften — Registerkarte Treiber mit altem Druckertreiber](/images/printer-error-0x00000bcb/step6.png)

### Schritt 3 — Neustart und erneute Verbindung

**6.** Starten Sie den Computer neu. Versuchen Sie danach, den freigegebenen Drucker erneut hinzuzufügen. Windows sollte nun in der Lage sein, den Druckertreiber ohne Fehler 0x00000bcb herunterzuladen und zu installieren.

---

## Häufig gestellte Fragen

**F: Betrifft Fehler 0x00000bcb nur Windows 10 oder auch Windows 11?**
Beide. Fehler 0x00000bcb tritt unter Windows 10 und Windows 11 auf, da die zugrunde liegende Ursache — eine verschärfte Point-and-Print-Sicherheitsrichtlinie — per Windows-Update auf beide Systeme angewendet wurde.

**F: Ist es sicher, RestrictDriverInstallationToAdministrators auf 0 zu setzen?**
Ja, in einem Heim- oder Büro-LAN. Dieser Wert lockert nur die Treiberinstallationsberechtigung für freigegebene Drucker im lokalen Netzwerk. Andere Sicherheitsfunktionen werden nicht deaktiviert.

**F: Ich habe alle Schritte befolgt, erhalte aber immer noch Fehler 0x00000bcb. Was kann ich noch tun?**
Installieren Sie den Druckertreiber manuell auf dem Client-PC (laden Sie ihn von der Website des Herstellers herunter) und verbinden Sie dann den freigegebenen Drucker. Dies umgeht den automatischen Treiberdownload vollständig.

**F: Ist Fehler 0x00000bcb dasselbe wie Fehler 0x00000709?**
Nein, aber die Ursachen sind sehr ähnlich — beide betreffen RPC-Richtlinienbeschränkungen und Treiber-Berechtigungsprobleme. Wenn die obigen Schritte 0x00000bcb nicht lösen, lesen Sie die [Anleitung für 0x00000709](/de/posts/printer-sharing-error-0x00000709/).

---

## Zusammenfassung

Fehler 0x00000bcb beim Verbinden mit einem freigegebenen Drucker unter Windows 10 oder Windows 11 wird hauptsächlich durch eine Windows-Update-Sicherheitsrichtlinie verursacht, die die automatische Druckertreiberinstallation einschränkt. Die Lösung besteht aus drei Schritten:

1. `RestrictDriverInstallationToAdministrators` = `0` in der Registrierung unter `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint` setzen
2. Alten Druckertreiber unter **Druckservereigenschaften → Treiber** entfernen
3. Computer neu starten und den freigegebenen Drucker erneut verbinden

---

## Verwandte Artikel

- [Druckerfreigabe-Fehler 0x00000709 unter Windows beheben](/de/posts/printer-sharing-error-0x00000709/)
- [Druckerfreigabe-Fehler 0x0000011b unter Windows beheben](/de/posts/printer-sharing-error-0x0000011b/)
- [Druckerfreigabe-Fehler 0x000003e3 unter Windows 10 beheben](/de/posts/printer-sharing-error-0x000003e3/)
