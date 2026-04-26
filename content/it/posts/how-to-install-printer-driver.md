---
title: "Come installare un driver per stampante su Windows 10/11"
date: 2026-04-26
description: "Guida completa passo dopo passo per installare i driver della stampante su Windows 10 e Windows 11 — USB, rete e installazione manuale con suggerimenti per la risoluzione dei problemi."
tags: ["windows", "driver", "installazione", "USB", "stampante di rete"]
categories: ["Installazione"]
cover:
  image: "/images/printer-driver/connection-types.svg"
  alt: "Panoramica dei metodi di connessione della stampante"
  caption: "Due modi principali per collegare una stampante: USB e rete/Wi-Fi"
  relative: false
---

Installare correttamente un driver per stampante è la base di una configurazione di stampa funzionante. Questa guida illustra tutti i metodi — dal semplice plug-and-play all'installazione completamente manuale — e mostra come risolvere i problemi più comuni.

---

## Prima di iniziare

Raccogliere le seguenti informazioni prima di iniziare:

| Cosa serve | Dove trovarlo |
|---|---|
| Numero di modello della stampante | Etichetta sulla parte anteriore o inferiore della stampante |
| Versione di Windows (10 o 11) | Impostazioni → Sistema → Informazioni su |
| Tipo di sistema (32 bit / 64 bit) | Impostazioni → Sistema → Informazioni su → Tipo di sistema |
| Tipo di connessione (USB o Wi-Fi) | Decidere prima di iniziare |

---

## Metodo 1 — USB Plug-and-Play (Il più semplice)

Questo metodo funziona per la maggior parte delle stampanti moderne.

**Passaggi:**

1. Assicurarsi che la stampante sia **spenta**.
2. Collegare il cavo USB dalla stampante a una porta USB del computer.
3. Accendere la stampante.
4. Windows rileva automaticamente la stampante e scarica il driver.
5. Attendere la notifica: *«Il dispositivo è pronto»* nell'angolo in basso a destra.

> **Suggerimento:** Utilizzare un cavo USB più corto di 3 metri. I cavi più lunghi possono causare errori di connessione.

---

## Metodo 2 — Impostazioni di Windows (USB o rete)

Utilizzare questo metodo se il plug-and-play non ha funzionato, o per aggiungere una stampante di rete.

![Impostazioni di Windows — Stampanti e scanner](/images/printer-driver/windows-settings.svg)

**Passaggi:**

1. Aprire **Start** → **Impostazioni** (icona ⚙).
2. Fare clic su **Bluetooth e dispositivi** → **Stampanti e scanner**.
3. Fare clic su **Aggiungi una stampante o uno scanner** (pulsante blu in alto).
4. Windows cerca le stampanti nelle vicinanze. Attendere 20–30 secondi.

![Procedura guidata per l'aggiunta di stampante — selezionare la stampante dall'elenco](/images/printer-driver/add-printer-wizard.svg)

5. Quando la stampante appare nell'elenco, fare clic su di essa.
6. Fare clic su **Aggiungi dispositivo**.
7. Windows installa il driver automaticamente.

> **La stampante di rete non appare?** Assicurarsi che la stampante e il computer siano collegati alla **stessa rete Wi-Fi**.

---

## Metodo 3 — Download manuale del driver

Utilizzare questo metodo quando Windows non riesce a trovare il driver automaticamente.

![Download manuale del driver — quattro passaggi](/images/printer-driver/manual-driver-download.svg)

**Passaggi:**

1. **Trovare il modello della stampante** — controllare l'etichetta sulla stampante.
2. **Andare al sito web del produttore:**
   - HP: `support.hp.com`
   - Canon: `canon.com/support`
   - Epson: `epson.com/support`
   - Brother: `support.brother.com`
3. **Cercare** il numero di modello esatto.
4. In **Software e driver**, selezionare il sistema operativo.
5. Scaricare il **driver completo** (consigliato) o il driver di base.
6. Aprire il file `.exe` scaricato e seguire la procedura guidata di installazione.

---

## Metodo 4 — Stampante di rete tramite indirizzo IP

Utilizzare questo metodo quando la stampante di rete non viene rilevata automaticamente.

**Passaggi:**

1. Stampare una **pagina di configurazione** dal pannello di controllo della stampante. Mostra l'indirizzo IP.
2. Aprire **Impostazioni** → **Bluetooth e dispositivi** → **Stampanti e scanner** → **Aggiungi una stampante o uno scanner**.
3. Fare clic su **«La stampante desiderata non è nell'elenco»**.
4. Selezionare **«Aggiungi una stampante usando un indirizzo TCP/IP o un nome host»** → **Avanti**.
5. Inserire l'indirizzo IP della stampante (es. `192.168.1.105`).
6. Fare clic su **Avanti** — Windows si connette e installa il driver.

---

## Risoluzione dei problemi

![Problemi comuni e soluzioni](/images/printer-driver/troubleshooting.svg)

### Stampante non trovata durante l'installazione

- Verificare che la stampante sia **accesa** (non in modalità risparmio energetico).
- Per USB: provare una porta USB diversa; evitare gli hub USB.
- Per la rete: confermare che entrambi i dispositivi siano sulla stessa rete Wi-Fi.
- Disabilitare temporaneamente il firewall e riprovare.

### La stampante appare come «Non in linea»

1. Premere `Win + R`, digitare `services.msc` e premere **Invio**.
2. Scorrere fino a **Spooler di stampa**.
3. Fare clic con il tasto destro → **Riavvia**.
4. Aprire **Dispositivi e stampanti**, fare clic con il tasto destro sulla stampante → **Visualizza cosa si sta stampando**.
5. Nel menu, fare clic su **Stampante** e deselezionare **Usa stampante offline**.

### Errore durante l'installazione del driver

- Fare clic con il tasto destro sul programma di installazione → **Esegui come amministratore**.
- Disabilitare temporaneamente l'antivirus e riprovare.
- Verificare di aver scaricato il driver corretto (64 bit o 32 bit).

---

## Riepilogo

| Metodo | Ideale per | Fonte del driver |
|---|---|---|
| USB Plug-and-Play | Stampanti nuove, configurazione rapida | Windows Update |
| Impostazioni di Windows | Stampante USB o di rete | Windows Update |
| Download manuale | Funzioni complete, errore installazione automatica | Sito del produttore |
| Indirizzo IP | Stampanti di rete non rilevate automaticamente | Windows / Produttore |

Dopo l'installazione, stampare sempre una **pagina di prova**: fare clic con il tasto destro sulla stampante → **Proprietà stampante** → **Stampa pagina di prova**.
