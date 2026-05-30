---
title: "Druckerfehler 0x00000032 unter Windows 10 und 11 beheben"
date: 2026-05-30
description: "Erhalten Sie Fehler 0x00000032 (Die Anforderung wird nicht unterstützt) beim Installieren oder Verbinden eines Druckers unter Windows 10 oder 11? Diese Anleitung behandelt alle Ursachen und Lösungen — Treiberbereinigung, Druckwarteschlangen-Reset, Gruppenrichtlinie und Druckerfreigabe-Reparatur."
tags: ["windows", "fehler 0x00000032", "druckerfehler", "die anforderung wird nicht unterstützt", "druckwarteschlange", "druckertreiber", "gruppenrichtlinie", "windows 10", "windows 11", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x00000032/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x00000032 — Die Anforderung wird nicht unterstützt"
  caption: "Fehler 0x00000032 erscheint, wenn Windows das Laden oder Installieren des Druckertreibers aufgrund von Inkompatibilität oder einem gesperrten alten Treiber verweigert"
  relative: false
---

# Druckerfehler 0x00000032 unter Windows 10 und 11 beheben

Fehler 0x00000032 — „Die Anforderung wird nicht unterstützt" — ist einer der häufigsten Druckertreiberfehler unter Windows 10 und Windows 11. Er tritt beim Installieren eines lokalen USB-Druckers, beim Verbinden mit einem freigegebenen Netzwerkdrucker oder beim Aktualisieren eines Druckertreibers auf. Im Gegensatz zu Netzwerkfehlern handelt es sich bei Fehler 0x00000032 um einen Treiberklassen-Fehler: Windows verweigert das Laden, Ersetzen oder Installieren des aktuellen Druckertreibers. Diese Anleitung erklärt alle Ursachen und führt durch jede Lösung in logischer Reihenfolge.

---

## Was ist Fehler 0x00000032?

Wenn Sie einen Drucker installieren oder eine Verbindung zu einem Drucker herstellen, zeigt Windows folgende Meldung:

> **Der Vorgang konnte nicht abgeschlossen werden (Fehler 0x00000032). Die Anforderung wird nicht unterstützt.**

![Windows-Fehlerdialog mit Fehler 0x00000032 — Die Anforderung wird nicht unterstützt](/images/printer-error-0x00000032/step1.png)

**Typische Symptome:**
- Das Hinzufügen eines lokalen USB-Druckers schlägt sofort mit diesem Fehler fehl
- Das Verbinden mit einem LAN-freigegebenen Drucker schlägt auf einem Client-PC fehl, funktioniert aber auf anderen
- Das Aktualisieren des Druckertreibers löst den Fehler aus, selbst nach einem frischen Download
- Der generische/universelle Treiber installiert sich problemlos, aber der offizielle Treiber gibt 0x00000032 zurück

---

## Warum tritt Fehler 0x00000032 auf?

Es gibt sechs häufige Ursachen:

1. **Treiberinkompatibilität (häufigste Ursache)** — Die Treiberversion stimmt nicht mit dem Treiber des Host-PCs überein, ein 32-Bit-Treiber wird mit einem 64-Bit-System gemischt (oder umgekehrt), oder der Treiber unterstützt die Windows-Version nicht. Windows stuft die Installationsanforderung als ungültig ein und blockiert sie.
2. **Verbleibende Treiberdateien sperren die Installation** — Dateien und Registrierungseinträge eines zuvor installierten Treibers wurden nicht vollständig entfernt. Der Treiber wird vom System gehalten und kann nicht überschrieben werden, was 0x00000032 auslöst.
3. **Ausfall des Druckwarteschlangendienstes** — Der Spooler-Prozess ist eingefroren oder hält Treiberdateien geöffnet und verhindert, dass ein neuer Treiber geschrieben oder geladen wird.
4. **Systemrichtlinienblock für Treiberinstallation** — Die Windows-Sicherheitsrichtlinie blockiert die Installation unsignierter oder Drittanbieter-Treiber.
5. **Fehler bei der freigegebenen Treibersynchronisierung** — Beim Verbinden mit einem LAN-freigegebenen Drucker hat der Host-PC keinen passenden Treiber für die Windows-Version des Client-PCs (insbesondere 32-Bit vs. 64-Bit-Unterschiede). Windows verweigert die Treiberdownload-Anforderung.
6. **Geringfügige Systemdatei-Beschädigung** — Fehlende oder beschädigte druckbezogene Windows-Komponenten verhindern die Funktion der Treiberinstallationsschnittstelle.

---

## Lösung

Führen Sie die folgenden Schritte der Reihe nach durch. Die meisten Fälle werden durch Schritt 1 und 2 gelöst.

### Schritt 1 — Druckwarteschlange neu starten und Druckercache leeren

**1.** Drücken Sie **Win + R**, geben Sie `services.msc` ein und drücken Sie Enter.

**2.** Suchen Sie **Druckwarteschlange (Print Spooler)** in der Liste, klicken Sie mit der rechten Maustaste darauf und wählen Sie **Beenden**.

![Fenster „Dienste" — Druckwarteschlangendienst beenden](/images/printer-error-0x00000032/step2.png)

**3.** Drücken Sie **Win + R**, geben Sie `C:\Windows\System32\spool\PRINTERS` ein und drücken Sie Enter. Löschen Sie **alle Dateien** im Ordner (löschen Sie nicht den Ordner selbst).

![Datei-Explorer — alle Dateien im PRINTERS-Spoolordner löschen](/images/printer-error-0x00000032/step3.png)

**4.** Kehren Sie zu Dienste zurück, klicken Sie mit der rechten Maustaste auf **Druckwarteschlange (Print Spooler)**, wählen Sie **Starten** und setzen Sie den **Starttyp** auf **Automatisch**.

---

### Schritt 2 — Verbleibendes Treiberpaket vollständig entfernen

Fehler 0x00000032 wird am häufigsten durch einen gesperrten alten Treiber verursacht. Sie müssen das vollständige Treiberpaket entfernen — nicht nur das Druckergerät:

**1.** Öffnen Sie **Systemsteuerung → Geräte und Drucker**. Klicken Sie mit der rechten Maustaste auf jeden fehlerhaften oder doppelten Drucker und wählen Sie **Gerät entfernen**.

**2.** Drücken Sie **Win + R**, geben Sie `printui /s /t2` ein und drücken Sie Enter, um die **Druckservereigenschaften** zu öffnen. Wechseln Sie zur Registerkarte **Treiber**. Wählen Sie den Treiber für den Problemdrucker aus, klicken Sie auf **Entfernen** und wählen Sie dann **Treiber und Treiberpaket entfernen**, um alle Treiberdateien zu löschen.

![Druckservereigenschaften — Treiber und Treiberpaket vollständig entfernen](/images/printer-error-0x00000032/step4.png)

**3.** Starten Sie den Computer neu. Laden Sie dann den korrekten vollständigen Treiber von der Website des Herstellers herunter und installieren Sie ihn.

---

### Schritt 3 — Systemrichtlinie anpassen, um Treiberinstallation zu erlauben

Bei Windows Professional und Enterprise Editionen kann die Gruppenrichtlinie die Treiberinstallation blockieren:

**1.** Drücken Sie **Win + R**, geben Sie `gpedit.msc` ein und drücken Sie Enter. Navigieren Sie zu **Computerkonfiguration → Administrative Vorlagen → Drucker**.

**2.** Deaktivieren Sie **Installation von Druckern mit Kernelmodustreibern nicht zulassen** (falls aktiviert).

**3.** Aktivieren Sie **Druckwarteschlange darf Clientverbindungen annehmen**.

![Gruppenrichtlinien-Editor — Richtlinien für die Druckertreiberinstallation anpassen](/images/printer-error-0x00000032/step5.png)

---

### Schritt 4 — Probleme mit freigegebenen Druckern beheben (LAN-Verbindung)

Wenn Fehler 0x00000032 nur beim Verbinden mit einem freigegebenen Drucker im lokalen Netzwerk auftritt:

**1.** Fügen Sie auf dem **Host-PC** das zusätzliche Treiberpaket hinzu, das der **Windows-Version des Client-PCs** entspricht (32-Bit oder 64-Bit). Tun Sie dies unter **Druckservereigenschaften → Treiber → Hinzufügen**.

**2.** Aktivieren Sie auf dem **Client-PC** **Unsichere Gastanmeldung aktivieren**: Öffnen Sie **Lokale Gruppenrichtlinie → Computerkonfiguration → Administrative Vorlagen → Netzwerk → Lanman-Arbeitsstation** und setzen Sie **Unsichere Gastanmeldungen aktivieren** auf **Aktiviert**.

**3.** Löschen Sie veraltete Netzwerkanmeldeinformationen auf dem Client-PC: Drücken Sie **Win + R**, geben Sie `control keymgr.dll` ein, öffnen Sie den **Anmeldeinformations-Manager** und löschen Sie alle Einträge, die mit der IP-Adresse oder dem Computernamen des Host-PCs zusammenhängen.

![Anmeldeinformations-Manager — veraltete Netzwerkanmeldeinformationen auf dem Client-PC löschen](/images/printer-error-0x00000032/step6.png)

**4.** Verbinden Sie sich erneut über die IP-Adresse des Host-PCs direkt: `\\192.168.1.x`.

---

### Schritt 5 — Systemdateien reparieren

**1.** Suchen Sie nach **cmd**, klicken Sie mit der rechten Maustaste darauf und wählen Sie **Als Administrator ausführen**.

**2.** Führen Sie den folgenden Befehl aus und warten Sie, bis er abgeschlossen ist:

```
sfc /scannow
```

![Eingabeaufforderung — sfc /scannow zur Reparatur von Systemdateien ausführen](/images/printer-error-0x00000032/step7.png)

**3.** Starten Sie den Computer neu und versuchen Sie erneut, den Druckertreiber zu installieren.

---

## Häufig gestellte Fragen

**F: Was bedeutet Fehler 0x00000032 „Die Anforderung wird nicht unterstützt" genau?**
Er bedeutet, dass Windows die Treiberinstallationsanforderung verweigert. Das System hat festgestellt, dass die Anforderung ungültig ist — meistens weil der Treiber nicht mit der Systemversion übereinstimmt, ein widersprüchlicher alter Treiber gesperrt ist oder eine Sicherheitsrichtlinie die Installation blockiert.

**F: Der generische Treiber installiert sich problemlos, aber der offizielle Treiber gibt Fehler 0x00000032. Warum?**
Dies wird fast immer durch ein gesperrtes verbleibendes Treiberpaket von einer früheren Installation verursacht. Der generische Treiber verwendet einen anderen Installationspfad, weshalb er funktioniert. Löschen Sie das vollständige Treiberpaket über `printui /s /t2`, starten Sie neu und installieren Sie dann den offiziellen Treiber.

**F: Andere Computer können sich mit dem freigegebenen Drucker verbinden — nur mein PC zeigt Fehler 0x00000032. Was soll ich tun?**
Das Problem ist spezifisch für die Treiberumgebung Ihres Client-PCs. Andere PCs haben kompatible Treiber; Ihres hat entweder einen alten Treiberkonflikt oder einen Versionskonflikt. Bereinigen Sie den Treiber nur auf Ihrem PC (Schritte 1 und 2) — auf dem Host-PC oder Drucker sind keine Änderungen erforderlich.

**F: Ich habe den Treiber mehrmals neu installiert, bekomme aber immer noch den Fehler. Was mache ich falsch?**
Sie haben höchstwahrscheinlich das Druckergerät gelöscht, aber nicht das **Treiberpaket**. Das bloße Entfernen des Geräts aus „Geräte und Drucker" lässt die Treiberdateien an Ort und Stelle. Öffnen Sie `printui /s /t2`, wechseln Sie zur Registerkarte Treiber und löschen Sie das Treiberpaket vollständig, bevor Sie neu installieren.

**F: Kann ein 32-Bit/64-Bit-Unterschied zwischen Host und Client Fehler 0x00000032 verursachen?**
Ja. Wenn der Host-PC 64-Bit ist und der Client 32-Bit, und der Host kein 32-Bit-Treiberpaket hinzugefügt hat, kann der Client keinen passenden Treiber herunterladen und Windows gibt „Die Anforderung wird nicht unterstützt" zurück. Fügen Sie den fehlenden Architekturtreiber auf dem Host-PC in den Druckservereigenschaften hinzu.

---

## Zusammenfassung

Fehler 0x00000032 beim Installieren oder Verbinden eines Druckers unter Windows 10 oder Windows 11 bedeutet, dass Windows die Treiberinstallationsanforderung verweigert. Er hängt nicht mit dem Netzwerk, der Druckerhardware oder größeren Systemschäden zusammen. Beheben Sie ihn in dieser Reihenfolge:

1. **Druckwarteschlange neu starten und Druckercache leeren**
2. **Altes Treiberpaket vollständig entfernen** über `printui /s /t2` — dies ist die häufigste Lösung
3. **Gruppenrichtlinie anpassen**, um die Treiberinstallation zu erlauben (Professional/Enterprise Editionen)
4. **Für freigegebene Drucker**: passende Architekturtreiber auf dem Host hinzufügen, Gastanmeldung auf dem Client aktivieren und veraltete Anmeldeinformationen löschen
5. **`sfc /scannow` ausführen**, um beschädigte Systemdateien zu reparieren

---

## Verwandte Artikel

- [Druckerfreigabe-Fehler 0x00000006 unter Windows beheben](/de/posts/printer-sharing-error-0x00000006/)
- [Druckerfreigabe-Fehler 0x00000035 unter Windows beheben](/de/posts/printer-sharing-error-0x00000035/)
- [Druckerfreigabe-Fehler 0x00000709 unter Windows beheben](/de/posts/printer-sharing-error-0x00000709/)
