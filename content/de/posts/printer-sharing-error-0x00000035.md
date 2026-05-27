---
title: "Druckerfreigabe-Fehler 0x00000035 unter Windows 10 und 11 beheben"
date: 2026-05-28
description: "Erhalten Sie Fehler 0x00000035 beim Verbinden mit einem freigegebenen Drucker unter Windows 10 oder 11? Diese Anleitung behandelt alle Ursachen und Lösungen — Netzwerkverbindung, Freigabeeinstellungen, Firewall, SMB-Protokoll und Anmeldeinformationen."
tags: ["windows", "fehler 0x00000035", "fehler 0x80070035", "druckerfreigabe", "netzwerkpfad nicht gefunden", "SMB", "windows firewall", "windows 10", "windows 11", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x00000035/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x00000035 — Der Netzwerkpfad wurde nicht gefunden"
  caption: "Fehler 0x00000035 erscheint, wenn Windows den Freigabepfad des Druckers im lokalen Netzwerk nicht finden kann"
  relative: false
---

# Druckerfreigabe-Fehler 0x00000035 unter Windows 10 und 11 beheben

Fehler 0x00000035 — „Der Netzwerkpfad wurde nicht gefunden" — ist einer der häufigsten Fehler bei freigegebenen Druckern unter Windows 10 und Windows 11. Im Gegensatz zu anderen Druckerfehlern, die eine einzige Ursache haben, kann Fehler 0x00000035 mehrere Ursachen haben. Diese Anleitung geht alle Ursachen und Lösungen in logischer Reihenfolge durch.

---

## Was ist Fehler 0x00000035?

Beim Versuch, eine Verbindung zu einem freigegebenen Drucker im lokalen Netzwerk herzustellen, zeigt Windows folgende Meldung an:

> **Der Vorgang konnte nicht abgeschlossen werden (Fehler 0x00000035). Der Netzwerkpfad wurde nicht gefunden.**

![Fehler 0x00000035 unter Windows — Der Netzwerkpfad wurde nicht gefunden](/images/printer-error-0x00000035/step1.png)

**Typische Symptome:**
- Eingabe der Host-IP oder des Computernamens im Ausführen-Fenster gibt sofort einen Fehler zurück
- Manche Geräte können die Host-IP erfolgreich anpingen, aber nicht auf freigegebene Inhalte zugreifen
- Neustart oder erneutes Einrichten der Druckerfreigabe löst das Problem nicht
- Andere Geräte im gleichen LAN verbinden sich problemlos — nur einzelne PCs lösen den Fehler aus

Fehler 0x00000035 wird auf manchen Systemen auch als **0x80070035** gemeldet; beide Codes bedeuten „Netzwerkpfad nicht gefunden" und werden auf die gleiche Weise behoben.

---

## Warum tritt Fehler 0x00000035 auf?

Es gibt sechs häufige Ursachen:

1. **Netzwerkproblem** — Host und Client befinden sich nicht im selben LAN oder Subnetz, oder das Netzwerk-Routing ist gestört.
2. **Netzwerkerkennung und Dateifreigabe deaktiviert** — Host oder Client haben Netzwerkerkennung oder Datei- und Druckerfreigabe nicht aktiviert.
3. **Kerndienste für die Freigabe laufen nicht** — Server-, Workstation- oder TCP/IP-NetBIOS-Hilfsdienst ist gestoppt oder deaktiviert.
4. **Firewall oder Sicherheitssoftware blockiert den Datenverkehr** — Windows-Firewall oder Drittanbieter-Antivirensoftware blockiert SMB-Port 445.
5. **Netzwerkprotokoll- oder Namensauflösungsfehler** — NetBIOS ist deaktiviert, SMB 1.0/CIFS ist nicht installiert, oder die Hostnamensauflösung schlägt fehl.
6. **Veraltete Netzwerkanmeldeinformationen** — Der Client hat falsche gespeicherte Anmeldeinformationen für den Host-PC, die zu Konflikten führen.

---

## Lösung

### Schritt 1 — Netzwerkverbindung prüfen

Öffnen Sie auf dem **Host-PC** die Eingabeaufforderung und führen Sie `ipconfig` aus. Notieren Sie die IPv4-Adresse.

Öffnen Sie auf dem **Client-PC** die Eingabeaufforderung und führen Sie aus:
```
ping <Host-IP-Adresse>
```

![Eingabeaufforderung — ipconfig und ping zur Überprüfung der Netzwerkverbindung](/images/printer-error-0x00000035/step2.png)

Wenn der Ping fehlschlägt, überprüfen Sie Router und Netzwerkkabel und stellen Sie sicher, dass beide Geräte mit demselben Netzwerk und Subnetz verbunden sind.

---

### Schritt 2 — Netzwerkerkennung und Dateifreigabe aktivieren

Gehen Sie auf Host und Client zu **Systemsteuerung → Netzwerk- und Freigabecenter → Erweiterte Freigabeeinstellungen ändern**. Aktivieren Sie unter **Privates Netzwerk**:
- **Netzwerkerkennung einschalten**
- **Datei- und Druckerfreigabe einschalten**

Speichern Sie die Einstellungen und versuchen Sie erneut, eine Verbindung zum freigegebenen Drucker herzustellen.

![Erweiterte Freigabeeinstellungen — Netzwerkerkennung und Dateifreigabe aktivieren](/images/printer-error-0x00000035/step3.png)

---

### Schritt 3 — Druckerfreigabe durch die Firewall zulassen

Öffnen Sie **Windows Defender-Firewall → Eine App oder ein Feature durch die Windows Defender-Firewall zulassen**. Suchen Sie **Datei- und Druckerfreigabe** und aktivieren Sie sowohl **Privat** als auch **Öffentlich**.

Falls Sie Drittanbieter-Antivirensoftware verwenden, deaktivieren Sie diese vorübergehend, um zu prüfen, ob sie die Verbindung blockiert.

![Windows Firewall — Datei- und Druckerfreigabe zulassen](/images/printer-error-0x00000035/step4.png)

---

### Schritt 4 — Netzwerkprotokolle und Zugriffseinstellungen korrigieren

Öffnen Sie die **Netzwerkadaptereigenschaften** und stellen Sie sicher, dass **Client für Microsoft-Netzwerke** und **Datei- und Druckerfreigabe für Microsoft-Netzwerke** aktiviert sind.

Öffnen Sie außerdem **Windows-Features aktivieren oder deaktivieren** und aktivieren Sie **SMB 1.0/CIFS-Dateifreigabe-Unterstützung**, dann starten Sie den Computer neu.

![Netzwerkadaptereigenschaften — SMB 1.0/CIFS aktivieren](/images/printer-error-0x00000035/step5.png)

---

### Schritt 5 — Veraltete Netzwerkanmeldeinformationen löschen

Drücken Sie **Win + R**, geben Sie `control keymgr.dll` ein und öffnen Sie die **Anmeldeinformationsverwaltung**. Löschen Sie unter **Windows-Anmeldeinformationen** alle Einträge, die sich auf die IP-Adresse oder den Computernamen des Host-PCs beziehen.

![Anmeldeinformationsverwaltung — veraltete Windows-Anmeldeinformationen löschen](/images/printer-error-0x00000035/step6.png)

---

## Häufig gestellte Fragen

**F: Ping funktioniert, aber Fehler 0x00000035 erscheint trotzdem. Warum?**
Ein erfolgreicher Ping bestätigt nur die grundlegende IP-Verbindung. Fehler 0x00000035 wird durch Freigabedienste, Firewallregeln oder Protokollprobleme verursacht. Überprüfen Sie den Server-Dienst, Firewallregeln und das SMB-Protokoll.

**F: Ist Fehler 0x00000035 dasselbe wie Fehler 0x80070035?**
Ja. Beide bedeuten „Netzwerkpfad wurde nicht gefunden". Der einzige Unterschied ist das Format, in dem Windows den Fehlercode meldet. Ursachen und Lösungen sind identisch.

**F: Nur ein Client-PC hat den Fehler — alle anderen Geräte funktionieren. Was soll ich tun?**
Das Problem liegt an diesem Client-PC. Löschen Sie dessen veraltete Anmeldeinformationen, starten Sie die lokalen Freigabedienste (Server und Workstation) neu und aktivieren Sie das SMB-Protokoll. Am Host-PC sind keine Änderungen erforderlich.

**F: Wie verhindere ich, dass der Fehler erneut auftritt?**
Setzen Sie die Dienste **Server**, **Workstation** und **TCP/IP-NetBIOS-Hilfsdienst** auf automatischen Start. Vermeiden Sie außerdem Sonderzeichen oder Leerzeichen im Druckerfreigabenamen.

---

## Zusammenfassung

Fehler 0x00000035 beim Verbinden mit einem freigegebenen Drucker unter Windows 10 oder Windows 11 bedeutet, dass Windows den freigegebenen Netzwerkpfad nicht finden kann. Das Problem hat nichts mit der Druckerhardware oder dem Treiber zu tun. Beheben Sie es in dieser Reihenfolge:

1. **Netzwerkverbindung prüfen** — Host-IP anpingen; sicherstellen, dass beide Geräte im gleichen Subnetz sind
2. **Netzwerkerkennung und Datei- und Druckerfreigabe** in den erweiterten Freigabeeinstellungen aktivieren
3. **Datei- und Druckerfreigabe durch die Windows-Firewall zulassen**
4. **SMB 1.0/CIFS aktivieren** und Netzwerkadapterprotokolleinstellungen prüfen
5. **Veraltete Anmeldeinformationen** in der Anmeldeinformationsverwaltung löschen

---

## Verwandte Artikel

- [Druckerfreigabe-Fehler 0x000006d9 unter Windows beheben](/de/posts/printer-sharing-error-0x000006d9/)
- [Druckerfreigabe-Fehler 0x00000bcb unter Windows beheben](/de/posts/printer-sharing-error-0x00000bcb/)
- [Druckerfreigabe-Fehler 0x00000709 unter Windows beheben](/de/posts/printer-sharing-error-0x00000709/)
