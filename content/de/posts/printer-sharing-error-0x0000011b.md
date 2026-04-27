---
title: "Druckerfreigabe-Fehler 0x0000011b unter Windows beheben"
date: 2026-04-27
description: "Erhalten Sie die Meldung 'Windows kann keine Verbindung mit dem Drucker herstellen, Fehler 0x0000011b'? Diese Anleitung zeigt Ihnen die bewährte Registrierungslösung zur sofortigen Wiederherstellung der Druckerverbindung."
tags: ["windows", "fehler 0x0000011b", "druckerfreigabe", "registrierung", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x0000011b/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x0000011b"
  caption: "Der Fehler 0x0000011b erscheint, wenn Windows keine Verbindung zum freigegebenen Drucker herstellen kann"
  relative: false
---

Freigegebene Drucker sind in Büro- und Heimnetzwerken unverzichtbar. Doch eine hartnäckige Fehlermeldung hindert viele Windows-Nutzer an der Verbindung:

> **„Windows kann keine Verbindung mit dem Drucker herstellen. Fehler beim Ausführen des Vorgangs: 0x0000011b."**

![Fehlerdialog 0x0000011b unter Windows](/images/printer-error-0x0000011b/step1.png)

Dieser Fehler trat massenhaft nach einem Microsoft-Sicherheitsupdate (KB5005565, Ende 2021) auf, das die RPC-Authentifizierungsanforderungen verschärfte. Die Lösung dauert weniger als zwei Minuten.

---

## Ursache des Fehlers 0x0000011b

Die Ursache liegt in einer Änderung der **RPC-Authentifizierungsprivatsphäre** bei der Kommunikation mit freigegebenen Druckern. Nach dem Update fordert Windows standardmäßig eine strengere Authentifizierung — ältere oder anders konfigurierte Systeme im selben Netzwerk können diese nicht immer aushandeln, was zum Fehler `0x0000011b` führt.

---

## Lösung — Windows-Registrierung bearbeiten

> ⚠️ **Hinweis:** Erstellen Sie vor dem Bearbeiten der Registrierung eine Sicherung: **Datei → Exportieren**.

### Schritt 1 — Registrierungseditor öffnen

Drücken Sie **Win + R**, geben Sie `regedit` ein und drücken Sie **Enter**.

![Suche nach regedit im Windows-Startmenü](/images/printer-error-0x0000011b/step2.png)

### Schritt 2 — Zum Print-Schlüssel navigieren

Fügen Sie folgenden Pfad in die Adressleiste ein und drücken Sie **Enter**:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print
```

![Registrierungseditor mit dem Pfad Control\Print](/images/printer-error-0x0000011b/step3.png)

### Schritt 3 — Neuen DWORD-Wert erstellen

1. Rechtsklick auf einen leeren Bereich im rechten Fenster.
2. **Neu → DWORD-Wert (32-Bit)** auswählen.

![Kontextmenü zum Erstellen eines neuen DWORD-Werts](/images/printer-error-0x0000011b/step4.png)

3. Den Wert genau benennen: `RpcAuthnLevelPrivacyEnabled`

### Schritt 4 — Wert auf 0 setzen

1. Doppelklick auf `RpcAuthnLevelPrivacyEnabled`.
2. Wert `0` eingeben, Basis **Hexadezimal** belassen.
3. **OK** klicken.

![DWORD-Wert auf 0 setzen](/images/printer-error-0x0000011b/step5.png)

### Schritt 5 — Neu starten und prüfen

PC neu starten und erneut mit dem freigegebenen Drucker verbinden.

![Druckerwarteschlange zeigt erfolgreiche Verbindung](/images/printer-error-0x0000011b/step6.png)

---

## Fehler tritt weiterhin auf?

- **Lösung auch auf dem Host-PC anwenden** — dieselbe Registrierungsänderung auf dem PC durchführen, der den Drucker freigibt.
- **Windows-Firewall prüfen** — Datei- und Druckerfreigabe auf beiden Computern erlauben.
- **Gleiches Netzwerk bestätigen** — beide PCs müssen im selben Subnetz sein.
- **Druckwarteschlange neu starten** — Win + R → `services.msc` → **Druckwarteschlange** → **Neu starten**.

---

## Zusammenfassung

| Schritt | Aktion |
|---|---|
| 1 | Registrierungseditor öffnen (`Win + R` → `regedit`) |
| 2 | Zu `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print` navigieren |
| 3 | Neuen **DWORD-Wert (32-Bit)** erstellen |
| 4 | `RpcAuthnLevelPrivacyEnabled` benennen und auf `0` setzen |
| 5 | Computer neu starten |
