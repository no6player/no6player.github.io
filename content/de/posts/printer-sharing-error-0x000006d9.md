---
title: "Druckerfreigabe-Fehler 0x000006d9 unter Windows beheben"
date: 2026-05-12
description: "Erhalten Sie Fehler 0x000006d9 beim Freigeben eines Druckers unter Windows 10 oder 11? Die Lösung ist einfach — starten Sie den Windows-Firewall-Dienst. Diese Anleitung erklärt Ursachen und Schritt-für-Schritt-Behebung."
tags: ["windows", "fehler 0x000006d9", "druckerfreigabe", "windows firewall dienst", "services.msc", "windows 10", "windows 11", "fehlerbehebung"]
categories: ["Fehlerbehebung"]
cover:
  image: "/images/printer-error-0x000006d9/step1.png"
  alt: "Windows-Fehlerdialog mit Fehler 0x000006d9 — Druckereinstellungen können nicht gespeichert werden"
  caption: "Fehler 0x000006d9 erscheint, wenn der Windows-Firewall-Dienst nicht ausgeführt wird und die Druckerfreigabe verhindert"
  relative: false
---

# Druckerfreigabe-Fehler 0x000006d9 unter Windows beheben

Fehler 0x000006d9 ist ein häufiger Windows-Fehler, der speziell beim **Freigeben** eines Druckers auftritt — nicht bei der normalen Nutzung. Der Drucker verbindet sich und druckt normal, aber beim Öffnen der Freigabeeinstellungen und Klicken auf „Übernehmen" erscheint:

> **Druckereinstellungen können nicht gespeichert werden. Der Vorgang konnte nicht abgeschlossen werden (Fehler 0x000006d9).**

Diese Anleitung erklärt genau, warum Fehler 0x000006d9 auftritt, und bietet eine klare Schritt-für-Schritt-Lösung für Windows 10 und Windows 11.

---

## Was ist Fehler 0x000006d9?

Wenn Sie die Druckerfreigabe unter Windows aktivieren, verlässt sich das System auf den **Windows-Firewall-Dienst**, um den freigegebenen Drucker im Netzwerk zu registrieren — selbst wenn die Firewall selbst deaktiviert ist. Ist der Windows-Firewall-*Dienst* gestoppt oder deaktiviert, kann Windows die Freigabe nicht abschließen und gibt Fehler 0x000006d9 zurück.

![Fehler 0x000006d9 unter Windows — Druckereinstellungen können nicht gespeichert werden](/images/printer-error-0x000006d9/step1.png)

Die Fehlermeldung erwähnt „unzureichender Speicher", was irreführend ist. Im Kontext der Druckerfreigabe hat Fehler 0x000006d9 nichts mit dem Festplattenspeicher zu tun — er zeigt immer, dass der Windows-Firewall-Dienst nicht ausgeführt wird.

---

## Warum tritt Fehler 0x000006d9 auf?

Es gibt vier häufige Ursachen:

1. **Windows-Firewall-Dienst ist gestoppt** — Die Druckerfreigabe erfordert, dass dieser Dienst *läuft*, unabhängig davon, ob die Firewall aktiviert oder deaktiviert ist.
2. **Windows-Firewall-Dienst ist deaktiviert** — Systemoptimierungstools, abgespeckte Windows-Installationen oder manuelle Anpassungen deaktivieren diesen Dienst oft vollständig.
3. **Abhängige Dienste laufen nicht** — Dienste, die vom Windows-Firewall-Dienst abhängen, können ebenfalls gestoppt oder fehlerhaft sein.
4. **Beschädigte Systemdateien** — In seltenen Fällen verhindern fehlende oder beschädigte Systemdateien den Start des Firewall-Dienstes.

---

## Lösung

### Schritt 1 — Dienste-Manager öffnen

Drücken Sie **Win + R**, geben Sie `services.msc` ein und drücken Sie Enter, um das Fenster **Dienste** zu öffnen.

![Ausführen-Dialog mit services.msc](/images/printer-error-0x000006d9/step2.png)

### Schritt 2 — Windows-Firewall-Dienst finden

Scrollen Sie in der Liste nach unten und suchen Sie **Windows-Firewall** (in einigen Windows-Versionen auch als *MpsSvc* aufgeführt).

![Dienste-Fenster — Windows-Firewall-Dienst lokalisieren](/images/printer-error-0x000006d9/step3.png)

### Schritt 3 — Dienst aktivieren und starten

Doppelklicken Sie auf **Windows-Firewall**, um die Eigenschaften zu öffnen. Setzen Sie den **Starttyp** auf **Automatisch**, klicken Sie dann auf **Starten**. Klicken Sie auf **OK**, um zu speichern.

![Eigenschaften des Windows-Firewall-Dienstes — Starttyp auf Automatisch setzen und starten](/images/printer-error-0x000006d9/step4.png)

> **Wichtig:** Das Starten des Windows-Firewall-*Dienstes* aktiviert nicht die Firewall selbst. Sie können die Firewall in den Windows-Sicherheitseinstellungen weiterhin deaktiviert lassen — der Dienst muss nur laufen, damit die Druckerfreigabe funktioniert.

### Schritt 4 — Wenn Windows-Firewall nicht in der Liste erscheint

In manchen abgespeckten oder angepassten Windows-Installationen fehlt der Windows-Firewall-Dienst möglicherweise in der Liste. Starten Sie in diesem Fall zuerst diese beiden verwandten Dienste:

**Windows Update:** Doppelklicken Sie auf **Windows Update**, setzen Sie den Starttyp auf **Automatisch**, klicken Sie auf **Starten** und dann auf **OK**.

![Eigenschaften des Windows-Update-Dienstes](/images/printer-error-0x000006d9/step5.png)

**Windows Installer:** Suchen Sie **Windows Installer**, doppelklicken Sie darauf, setzen Sie den Starttyp auf **Automatisch**, klicken Sie auf **Starten** und dann auf **OK**.

![Eigenschaften des Windows-Installer-Dienstes](/images/printer-error-0x000006d9/step6.png)

### Schritt 5 — Neustart und erneuter Versuch

Starten Sie den Computer neu. Prüfen Sie danach in **Dienste**, ob **Windows-Firewall** nun in der Liste erscheint. Falls ja, setzen Sie ihn auf **Automatisch** und starten Sie ihn (wie in Schritt 3). Aktivieren Sie dann die Druckerfreigabe erneut — Fehler 0x000006d9 sollte verschwunden sein.

---

## Häufig gestellte Fragen

**F: Bedeutet Fehler 0x000006d9, dass der Festplattenspeicher voll ist?**
Nein. Obwohl die Fehlermeldung „unzureichender Speicher" erwähnt, zeigt dieser Fehlercode im Kontext der Druckerfreigabe ausschließlich an, dass der Windows-Firewall-Dienst nicht läuft. Mit dem Festplattenspeicher hat das nichts zu tun.

**F: Ich habe die Firewall bereits deaktiviert — warum erscheint der Fehler trotzdem?**
Weil die Druckerfreigabe nicht prüft, ob die Firewall ein- oder ausgeschaltet ist, sondern nur, ob der Windows-Firewall-*Dienst* läuft. Der Dienst muss laufen, auch wenn die Firewall deaktiviert ist.

**F: Beeinträchtigt das Starten des Windows-Firewall-Dienstes meine Sicherheitseinstellungen?**
Nein. Sie können die Windows-Firewall weiterhin in **Windows-Sicherheit → Firewall & Netzwerkschutz** deaktiviert lassen. Das Starten des Dienstes aktiviert die Firewall nicht automatisch.

**F: Muss der Windows-Firewall-Dienst dauerhaft laufen?**
Ja, damit die Druckerfreigabe weiterhin funktioniert. Durch Setzen des Starttyps auf **Automatisch** wird sichergestellt, dass er bei jedem Windows-Start automatisch gestartet wird.

---

## Zusammenfassung

Fehler 0x000006d9 beim Freigeben eines Druckers unter Windows hat eine einzige Ursache: **Der Windows-Firewall-Dienst läuft nicht**. Die Lösung:

1. **Dienste** (`services.msc`) öffnen
2. **Windows-Firewall** suchen, Starttyp auf **Automatisch** setzen und **Starten** klicken
3. Computer neu starten und Druckerfreigabe erneut aktivieren

---

## Verwandte Artikel

- [Druckerfreigabe-Fehler 0x00000bcb unter Windows beheben](/de/posts/printer-sharing-error-0x00000bcb/)
- [Druckerfreigabe-Fehler 0x00000709 unter Windows beheben](/de/posts/printer-sharing-error-0x00000709/)
- [Druckerfreigabe-Fehler 0x0000011b unter Windows beheben](/de/posts/printer-sharing-error-0x0000011b/)
