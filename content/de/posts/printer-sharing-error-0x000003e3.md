---
title: "Druckerfreigabe-Fehler 0x000003e3 unter Windows 10 beheben"
date: 2026-04-28
description: "Erhalten Sie Fehler 0x000003e3 beim Verbinden mit einem freigegebenen Drucker unter Windows 10? Diese Anleitung zeigt, wie Sie lokale Sicherheitsrichtlinien und Gruppenrichtlinien anpassen, um die Verbindung wiederherzustellen."
tags: ["windows 10", "fehler 0x000003e3", "druckerfreigabe", "sicherheitsrichtlinie", "gruppenrichtlinie", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x000003e3/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x000003e3 beim Verbinden mit einem freigegebenen Drucker"
  caption: "Fehler 0x000003e3 erscheint, wenn Windows 10 keine Verbindung zum freigegebenen Drucker im lokalen Netzwerk herstellen kann"
  relative: false
---

## Problembeschreibung

Unter Windows 10 schlägt die Verbindung zu einem freigegebenen Drucker im lokalen Netzwerk mit folgendem Fehlercode fehl:

> **Fehler 0x000003e3**

![Fehler 0x000003e3 unter Windows 10](/images/printer-error-0x000003e3/step1.png)

Dieser Fehler ist ein häufiger Berechtigungs- oder Authentifizierungsfehler beim Verbinden mit freigegebenen Druckern. Er wird typischerweise durch verweigerten Zugriff, eine Dienstanomalie oder ein Netzwerkprotokollproblem verursacht. Befolgen Sie die nachstehenden Schritte, um das Problem zu beheben.

---

## Lösung

### Schritt 1 — Lokale Sicherheitsrichtlinie anpassen

**1.** Öffnen Sie **Systemsteuerung → Programme → Windows-Features aktivieren oder deaktivieren** und aktivieren Sie **SMB 1.0/CIFS-Dateifreigabe-Unterstützung**.

![SMB 1.0/CIFS-Dateifreigabe in Windows-Features aktivieren](/images/printer-error-0x000003e3/step2.png)

**2.** Drücken Sie **Win + R**, geben Sie `secpol.msc` ein und drücken Sie Enter, um den Editor für **Lokale Sicherheitsrichtlinien** zu öffnen.

![Ausführen-Dialog mit secpol.msc](/images/printer-error-0x000003e3/step3.png)

**3.** Navigieren Sie zu **Lokale Richtlinien → Zuweisen von Benutzerrechten → Auf diesen Computer vom Netzwerk aus zugreifen**. Fügen Sie das lokale **Gastkonto** hinzu.  
Gehen Sie dann zu **Zugriff auf diesen Computer vom Netzwerk aus verweigern** und **entfernen** Sie das Gastkonto aus dieser Liste.

![Lokale Sicherheitsrichtlinie — Benutzerrechte](/images/printer-error-0x000003e3/step4.png)

**4.** Navigieren Sie zu **Lokale Richtlinien → Sicherheitsoptionen → Netzwerkzugriff: Freigabe- und Sicherheitsmodell für lokale Konten** und setzen Sie die Einstellung auf **Nur Gast – lokale Benutzer authentifizieren sich als Gast**.

![Lokale Sicherheitsrichtlinie — Freigabemodell auf Nur Gast setzen](/images/printer-error-0x000003e3/step5.png)

---

### Schritt 2 — Lokale Gruppenrichtlinie anpassen

**1.** Drücken Sie **Win + R**, geben Sie `gpedit.msc` ein und drücken Sie Enter, um den **Editor für lokale Gruppenrichtlinien** zu öffnen.

![Ausführen-Dialog mit gpedit.msc](/images/printer-error-0x000003e3/step6.png)

**2.** Navigieren Sie zu **Computerkonfiguration → Windows-Einstellungen → Sicherheitseinstellungen → Lokale Richtlinien → Sicherheitsoptionen**. Suchen Sie die Richtlinie **Konten: Verwendung von leeren Kennwörtern auf Konsolenanmeldung beschränken**, doppelklicken Sie darauf, wählen Sie **Deaktiviert** und klicken Sie **OK**.

![Gruppenrichtlinie — Einschränkung für leere Kennwörter deaktivieren](/images/printer-error-0x000003e3/step7.png)

**3.** Öffnen Sie die **Computerverwaltung**, gehen Sie zu **Systemprogramme → Lokale Benutzer und Gruppen → Benutzer**. Doppelklicken Sie auf **Gast**, klicken Sie auf die Registerkarte **Mitglied von**, dann auf **Hinzufügen**. Geben Sie `users` ein und klicken Sie **OK**.

![Computerverwaltung — Gast zur Gruppe Benutzer hinzufügen](/images/printer-error-0x000003e3/step8.png)

**4.** Zurück im Fenster „Gasteigenschaften" unter **Mitglied von** wählen Sie **Benutzer**, klicken auf **Hinzufügen**, dann auf **Übernehmen** und **OK**.

![Gasteigenschaften — Mitglied von zeigt Gruppe Benutzer](/images/printer-error-0x000003e3/step9.png)

**5.** Nach Abschluss aller Schritte **starten Sie den Computer neu** und versuchen Sie erneut, den freigegebenen Drucker hinzuzufügen.

---

## Zusammenfassung

Fehler 0x000003e3 beim Verbinden mit einem freigegebenen Drucker unter Windows 10 wird typischerweise durch falsch konfigurierte Sicherheits- oder Gruppenrichtlinien verursacht. Die Lösung umfasst:

- Aktivierung der SMB 1.0/CIFS-Dateifreigabe
- Gewährung des Netzwerkzugriffs für das Gastkonto
- Setzen des Freigabe-Sicherheitsmodells auf „Nur Gast"
- Deaktivierung der Konsolenanmeldungs-Einschränkung für leere Kennwörter
- Hinzufügen des Gastkontos zur lokalen Gruppe „Benutzer"

Sollte der Fehler weiterhin auftreten, prüfen Sie außerdem:

- Ob die LAN-Verbindung funktioniert (Host und Client müssen im gleichen Subnetz sein und sich gegenseitig anpingen können)
- Ob der freigegebene Drucker-Host entsprechende Einstellungen besitzt
- Deinstallieren und Neuinstallieren des Druckertreibers und erneuter Versuch
