---
title: "Come correggere l'errore 0x00000709 della stampante condivisa su Windows"
date: 2026-04-29
description: "Viene visualizzato l'errore 0x00000709 durante la connessione a una stampante condivisa su Windows? Questa guida spiega come risolvere il problema aggiungendo credenziali Windows e modificando un valore del registro."
tags: ["windows", "errore 0x00000709", "stampante condivisa", "gestione credenziali", "registro", "risoluzione problemi"]
categories: ["Risoluzione Problemi"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Finestra di dialogo di errore di Windows con il codice 0x00000709 — Impossibile completare l'operazione"
  caption: "L'errore 0x00000709 appare quando Windows non riesce a connettersi a una stampante condivisa sulla rete"
  relative: false
---

## Descrizione del problema

Quando si tenta di connettersi a una stampante condivisa sulla rete, la connessione non riesce con il seguente messaggio di errore:

> **Impossibile completare l'operazione (Errore 0x00000709). Verificare il nome della stampante e assicurarsi che sia connessa alla rete.**

![Errore 0x00000709 su Windows](/images/printer-error-0x00000709/step1.png)

L'errore 0x00000709 è un errore comune di connessione alle stampanti condivise su Windows. È generalmente causato da un aggiornamento di sistema, una configurazione errata del protocollo RPC o un problema di autenticazione delle credenziali. La correzione prevede l'aggiunta di credenziali Windows e la modifica di un valore del registro.

---

## Soluzione

**1.** Aprire il **Pannello di controllo**, impostare la visualizzazione su **Icone piccole** e fare clic su **Gestione credenziali**.

![Pannello di controllo — Gestione credenziali](/images/printer-error-0x00000709/step2.png)

**2.** Selezionare **Credenziali Windows** e fare clic su **Aggiungi credenziali Windows**.

![Gestione credenziali — Aggiungi credenziali Windows](/images/printer-error-0x00000709/step3.png)

**3.** Nel campo **Indirizzo Internet o di rete**, inserire l'indirizzo IP del PC host. Impostare il **Nome utente** su `guest` e lasciare il campo **Password** vuoto. Fare clic su **OK**.

![Aggiungi credenziali Windows — inserire IP host e nome utente guest](/images/printer-error-0x00000709/step4.png)

**4.** Premere **Win + R**, digitare `regedit` e premere Invio per aprire l'**Editor del Registro di sistema**.

![Finestra di dialogo Esegui con regedit](/images/printer-error-0x00000709/step5.png)

**5.** Passare alla chiave seguente (crearla se non esiste):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Fare clic con il pulsante destro nel riquadro di destra e creare un nuovo **Valore DWORD (32 bit)**.

![Editor del Registro — Creazione di un nuovo valore DWORD sotto la chiave RPC](/images/printer-error-0x00000709/step6.png)

**6.** Rinominare il nuovo valore in `RpcUseNamedPipeProtocol` e impostare i **dati del valore** su `1`.

![Editor del Registro — RpcUseNamedPipeProtocol impostato su 1](/images/printer-error-0x00000709/step7.png)

**7.** Dopo aver completato tutti i passaggi, **riavviare il computer** e provare a connettersi nuovamente alla stampante condivisa.

---

## Riepilogo

L'errore 0x00000709 durante la connessione a una stampante condivisa è un problema comune su Windows, generalmente causato da un aggiornamento del sistema, una configurazione errata del protocollo RPC o un errore di autenticazione. La correzione comprende due passaggi:

- Aggiungere l'indirizzo IP del PC host e l'account **guest** nella **Gestione credenziali Windows**
- Creare il valore di registro **RpcUseNamedPipeProtocol** (`1`) in `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Dopo il riavvio, la connessione alla stampante condivisa dovrebbe funzionare normalmente.
