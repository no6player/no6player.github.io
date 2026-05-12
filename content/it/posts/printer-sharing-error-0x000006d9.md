---
title: "Come correggere l'errore 0x000006d9 nella condivisione della stampante su Windows"
date: 2026-05-12
description: "Viene visualizzato l'errore 0x000006d9 durante la condivisione di una stampante su Windows 10 o 11? La soluzione è semplice: avviare il servizio Windows Firewall. Questa guida spiega le cause e i passaggi necessari."
tags: ["windows", "errore 0x000006d9", "condivisione stampante", "servizio windows firewall", "services.msc", "windows 10", "windows 11", "risoluzione problemi"]
categories: ["Risoluzione Problemi"]
cover:
  image: "/images/printer-error-0x000006d9/step1.png"
  alt: "Finestra di dialogo di errore di Windows con il codice 0x000006d9 — Impossibile salvare le impostazioni della stampante"
  caption: "L'errore 0x000006d9 appare quando il servizio Windows Firewall non è in esecuzione, impedendo l'attivazione della condivisione della stampante"
  relative: false
---

# Come correggere l'errore 0x000006d9 nella condivisione della stampante su Windows

L'errore 0x000006d9 è un errore Windows comune che compare specificamente quando si tenta di **condividere** una stampante — non durante il suo utilizzo normale. La stampante si connette e stampa normalmente, ma nel momento in cui si aprono le impostazioni di condivisione e si fa clic su Applica, Windows visualizza:

> **Impossibile salvare le impostazioni della stampante. Impossibile completare l'operazione (Errore 0x000006d9).**

Questa guida spiega esattamente perché si verifica l'errore 0x000006d9 e fornisce una correzione chiara passo dopo passo per Windows 10 e Windows 11.

---

## Cos'è l'errore 0x000006d9?

Quando si attiva la condivisione della stampante su Windows, il sistema si affida al **servizio Windows Firewall** per registrare ed esporre la stampante condivisa sulla rete — anche se il firewall stesso è disabilitato. Se il *servizio* Windows Firewall è arrestato o disabilitato, Windows non riesce a completare la configurazione della condivisione e restituisce l'errore 0x000006d9.

![Errore 0x000006d9 su Windows — Impossibile salvare le impostazioni della stampante](/images/printer-error-0x000006d9/step1.png)

Il messaggio di errore menziona "spazio di archiviazione insufficiente", il che è fuorviante. Nel contesto della condivisione delle stampanti, l'errore 0x000006d9 non ha nulla a che fare con lo spazio su disco — indica sempre che il servizio Windows Firewall non è in esecuzione.

---

## Perché si verifica l'errore 0x000006d9?

Esistono quattro cause comuni:

1. **Il servizio Windows Firewall è arrestato** — La condivisione della stampante richiede che questo servizio sia *in esecuzione*, indipendentemente dal fatto che il firewall sia abilitato o disabilitato.
2. **Il servizio Windows Firewall è disabilitato** — Strumenti di ottimizzazione del sistema, installazioni Windows ridotte o modifiche manuali spesso disabilitano completamente questo servizio.
3. **I servizi dipendenti non sono in esecuzione** — I servizi che dipendono da Windows Firewall possono anch'essi essere arrestati o in stato anomalo.
4. **File di sistema corrotti** — In rari casi, file di sistema mancanti o danneggiati impediscono l'avvio del servizio.

---

## Soluzione

### Passaggio 1 — Aprire il Gestore dei servizi

Premere **Win + R**, digitare `services.msc` e premere Invio per aprire la finestra **Servizi**.

![Finestra di dialogo Esegui con services.msc](/images/printer-error-0x000006d9/step2.png)

### Passaggio 2 — Trovare il servizio Windows Firewall

Scorrere l'elenco per individuare **Windows Firewall** (elencato anche come *MpsSvc* in alcune versioni).

![Finestra Servizi — individuazione del servizio Windows Firewall](/images/printer-error-0x000006d9/step3.png)

### Passaggio 3 — Abilitare e avviare il servizio

Fare doppio clic su **Windows Firewall** per aprire le proprietà. Impostare il **Tipo di avvio** su **Automatico**, quindi fare clic su **Avvia**. Fare clic su **OK** per salvare.

![Proprietà del servizio Windows Firewall — tipo di avvio Automatico e avvio del servizio](/images/printer-error-0x000006d9/step4.png)

> **Importante:** Avviare il *servizio* Windows Firewall non attiva il firewall stesso. È possibile mantenere il firewall disabilitato nelle impostazioni di Sicurezza di Windows — il servizio deve semplicemente essere in esecuzione affinché la condivisione della stampante funzioni.

### Passaggio 4 — Se Windows Firewall non è presente nell'elenco

In alcune installazioni Windows personalizzate o ridotte, il servizio Windows Firewall potrebbe non comparire nell'elenco. In tal caso, avviare prima questi due servizi correlati:

**Windows Update:** Fare doppio clic su **Windows Update**, impostare il tipo di avvio su **Automatico**, fare clic su **Avvia** e poi su **OK**.

![Proprietà del servizio Windows Update](/images/printer-error-0x000006d9/step5.png)

**Windows Installer:** Individuare **Windows Installer**, fare doppio clic, impostare il tipo di avvio su **Automatico**, fare clic su **Avvia** e poi su **OK**.

![Proprietà del servizio Windows Installer](/images/printer-error-0x000006d9/step6.png)

### Passaggio 5 — Riavviare e riprovare

Riavviare il computer. Dopo il riavvio, aprire **Servizi** e verificare se **Windows Firewall** compare ora nell'elenco. In caso affermativo, impostarlo su **Automatico** e avviarlo (come al passaggio 3). Tornare quindi alle impostazioni della stampante e abilitare nuovamente la condivisione — l'errore 0x000006d9 dovrebbe essere scomparso.

---

## Domande frequenti

**D: L'errore 0x000006d9 significa che il disco è pieno?**
No. Sebbene il messaggio menzioni "spazio di archiviazione insufficiente", nel contesto della condivisione delle stampanti questo codice di errore indica esclusivamente che il servizio Windows Firewall non è in esecuzione. Non ha nulla a che fare con lo spazio su disco.

**D: Ho già disabilitato il firewall — perché l'errore compare ugualmente?**
Perché la condivisione della stampante non verifica se il firewall è attivato o meno, ma solo se il *servizio* Windows Firewall è in esecuzione. Il servizio deve essere avviato anche se il firewall è disabilitato.

**D: Avviare il servizio Windows Firewall influirà sulle mie impostazioni di sicurezza?**
No. È possibile mantenere il firewall disabilitato in **Sicurezza di Windows → Firewall e protezione della rete**. Avviare il servizio non attiva automaticamente il firewall.

**D: Il servizio Windows Firewall deve rimanere in esecuzione in modo permanente?**
Sì, affinché la condivisione della stampante continui a funzionare. Impostando il tipo di avvio su **Automatico** si garantisce che il servizio venga avviato con Windows ogni volta.

---

## Riepilogo

L'errore 0x000006d9 durante la condivisione di una stampante su Windows ha un'unica causa: **il servizio Windows Firewall non è in esecuzione**. La correzione:

1. Aprire **Servizi** (`services.msc`)
2. Trovare **Windows Firewall**, impostare il tipo di avvio su **Automatico** e fare clic su **Avvia**
3. Riavviare il computer e riabilitare la condivisione della stampante

---

## Articoli correlati

- [Correggere l'errore stampante condivisa 0x00000bcb su Windows](/it/posts/printer-sharing-error-0x00000bcb/)
- [Correggere l'errore stampante condivisa 0x00000709 su Windows](/it/posts/printer-sharing-error-0x00000709/)
- [Correggere l'errore stampante condivisa 0x0000011b su Windows](/it/posts/printer-sharing-error-0x0000011b/)
