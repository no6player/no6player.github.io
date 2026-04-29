---
title: "Druckerfreigabe-Fehler 0x00000040 unter Windows beheben"
date: 2026-04-29
description: "Windows 7-Clients erhalten beim Verbinden mit einem auf Windows 11 freigegebenen Drucker den Fehler 0x00000040? Diese Anleitung erklärt, wie Sie SMB 1.0 aktivieren und die Registrierung anpassen, um das Problem zu lösen."
tags: ["windows", "fehler 0x00000040", "druckerfreigabe", "windows 7", "windows 11", "registrierung", "SMB", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x00000040/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x00000040 — Der angegebene Netzwerkname ist nicht mehr verfügbar"
  caption: "Fehler 0x00000040 erscheint, wenn ein Windows 7-Client keine Verbindung zu einem auf Windows 11 freigegebenen Drucker herstellen kann"
  relative: false
---

## Problembeschreibung

Wenn ein Windows 11-PC als Druckerfreigabe-Host verwendet wird, erhalten Windows 7-Clients beim Verbindungsversuch folgende Fehlermeldung:

> **Der Vorgang konnte nicht abgeschlossen werden (Fehler 0x00000040). Der angegebene Netzwerkname ist nicht mehr verfügbar.**

![Fehler 0x00000040 unter Windows](/images/printer-error-0x00000040/step1.png)

Dieser Fehler wird häufig durch einen deaktivierten SMB-Dienst, einen abgelaufenen Freigabenamen, eine unterbrochene Netzwerkverbindung oder eine RPC-Protokoll-Inkompatibilität zwischen den beiden Betriebssystemen verursacht. Die Lösung umfasst die Aktivierung von SMB 1.0 und die Anpassung zweier Registrierungswerte auf dem Windows 11-Host.

---

## Lösung

**1.** Öffnen Sie **Systemsteuerung → Programme → Windows-Features aktivieren oder deaktivieren** und aktivieren Sie **SMB 1.0/CIFS-Dateifreigabe-Unterstützung**.

![SMB 1.0/CIFS in Windows-Features aktivieren](/images/printer-error-0x00000040/step2.png)

**2.** Drücken Sie **Win + R**, geben Sie `regedit` ein und drücken Sie Enter, um den **Registrierungseditor** zu öffnen.

![Ausführen-Dialog mit regedit](/images/printer-error-0x00000040/step3.png)

**3.** Navigieren Sie zum folgenden Schlüssel (erstellen Sie ihn, falls er nicht vorhanden ist):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Klicken Sie im rechten Bereich mit der rechten Maustaste und erstellen Sie einen neuen **DWORD-Wert (32-Bit)**.

![Registrierungseditor — Neuen DWORD-Wert unter dem RPC-Schlüssel erstellen](/images/printer-error-0x00000040/step4.png)

**4.** Benennen Sie den neuen Wert in `RpcProtocols` um und setzen Sie die **Wertdaten** auf `7`.

![Registrierungseditor — RpcProtocols auf 7 gesetzt](/images/printer-error-0x00000040/step5.png)

**5.** Klicken Sie erneut mit der rechten Maustaste im rechten Bereich und erstellen Sie einen weiteren **DWORD-Wert (32-Bit)**. Benennen Sie ihn in `ForceKerberosForRpc` um und setzen Sie die **Wertdaten** auf `1`.

![Registrierungseditor — ForceKerberosForRpc auf 1 gesetzt](/images/printer-error-0x00000040/step6.png)

**6.** Starten Sie nach Abschluss aller Schritte den Computer **neu** und versuchen Sie erneut, eine Verbindung zum freigegebenen Drucker herzustellen.

---

## Zusammenfassung

Fehler 0x00000040 beim Verbinden mit einem freigegebenen Drucker ist ein häufiges Problem in gemischten Windows 7/Windows 11-Umgebungen. Er wird in der Regel durch einen deaktivierten SMB-Dienst oder eine RPC-Protokoll-Inkompatibilität verursacht. Die Lösung umfasst zwei Schritte:

- **SMB 1.0/CIFS-Dateifreigabe-Unterstützung** in den Windows-Features aktivieren
- Die Registrierungswerte **RpcProtocols** (`7`) und **ForceKerberosForRpc** (`1`) unter `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC` hinzufügen

Nach dem Neustart sollte die Verbindung zum freigegebenen Drucker problemlos funktionieren.
